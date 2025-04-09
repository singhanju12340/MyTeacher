---
Creation Time: Monday, April 7th 2025
Modified Time: Tuesday, April 8th 2025
---
Ticketmaster's ticket browsing UI. This is the UI that shows an event venue's available seats, and allows a user to select seats and then enter a seat checkout and payment flow.


The Ticketmaster ticket browsing UI is a UI that doesn't need strict consistency. Event ticket availability changes, even as a user is viewing the UI. If a seat is selected and a purchase flow is attempted, the system can check a consistent database to determine if the seat is _actually_ available. Additionally, always showing the browsing UI is important, as a majority of users will browse, but a minority of users will actually enter a checkout flow.

### Functional Requirements:

1. book ticket of event
2. Users should be able to view events
3. Users should be able to search for events


**Below the line (out of scope):**
1. Users should be able to view their booked events
2. Admins or event coordinators should be able to add events
3. Popular events should have dynamic pricing

### Non-Functional Requirements
1. The system should prioritize availability for searching & viewing events, but should prioritize consistency for booking events (no double booking)
2. The system should be scalable and able to handle high throughput in the form of popular events (10 million users, one event)
3. The system should have low latency search (< 500ms)
4. The system is read heavy, and thus needs to be able to support high read throughput (100:1)

system should be fault tolerant
5. provide secure transactions for purchases
6. regular backup of bookings


### Entities:
Event
```Java
id
name
date
lastRegisterDate
venueId
performerId
tickets[]
```

Venue
```Java
id
name
addressLocation
seatMap

```

Ticket
```Java
id
eventId
seat
fare
bookingDate
bookingStatus: available/booked
bookingId : link from booking table

Cassandra schema for ticket table
CREATE TABLE tickets ( 
	event_id bigint, 
	seat_id bigint, //-- seat_id is added as a clustering key to ensure primary key uniqueness; 
	price bigint, 
	//order of seat no doesn't matter for the app access patterns as different seat layout mapping is already defined
	PRIMARY KEY (event_id, seat_id) );

//given that the primary key has a partition key of event_id. The app can query the partition for price information about the event, for ticket availability totals

```
Booking
``` Java
id
userid
ticketid
totalPrice
bookingStatus: in progress/ confirmed
```

### APIs
GET endpoint that takes in an eventId and return the details of that event
```
GET v1/events/:eventId 
response -> Event & Venue & Performer & Ticket[]
```


```
GET v1/events/search?keyword={keyword}&start={start_date}&end={end_date}&pageSize={page_size}&page={page_number} 

response -> Event[]`
```

```
POST /bookings/:eventId 
Response -> bookingId { "ticketIds": string[], "paymentDetails": ... }
```

#### HLD

[[Excalidraw/TicketMaster|TicketMaster]]


_**Problems:

## Only 1 user should be able to book 1 seat

The main thing we are trying to avoid is two (or more) users paying for the same ticket.
we need to implement proper isolation levels and either row-level locking or Optimistic Concurrency Control (OCC) to fully prevent double bookings.

Also we do not want user to fail their txn  and inform them at the end, just because someone else was able to fill card details faster.

### How do we improve the booking experience by reserving tickets?
**1 way:** Timer counting down the time you have to complete your purchase. This is a common technique to reserve the tickets for a user while they are checking out. We need to ensure that the ticket is locked for the user while they are checking out. We also need to ensure that if the user abandons the checkout process, the ticket is released for other users to purchase.

	Approach to achieve the same.
**Pessimistic lock:** use long-running database locks
This is typically done using the SELECT FOR UPDATE statement in PostgreSQL, which locks the selected row(s) as part of a database transaction.
`database locks are meant to be used for short periods of time (a single, near-instant, transaction). Keeping a transaction open for a long period (like the 5-minute lock duration) is generally not advisable. It can strain database resources and increase the risk of lock contention and deadlocks.
`
`Also If the user takes too long or abandons the purchase, the system has to rely on their subsequent actions or session timeouts to release the lock. This introduces the risk of tickets being locked indefinitely if not appropriately handled

this approach may not scale well under high load, as prolonged locks can lead to increased wait times for other users and potential performance bottlenecks. Handling edge cases, such as application crashes or network issues, becomes more challenging, as these could leave locks in an uncertain state.

**Use Status  and Expiration time with Cron:**
Lock the ticket by adding a status field and expiration time on the ticket table.
The ticket can then be in 1 of 3 states: available, reserved, booked.
When a user selects a ticket, the status changes from "available" to "reserved", and the current timestamp is recorded.
This allows us to track the status of each ticket and automatically release the lock once the expiration time is reached. 
1. If the user finalizes the purchase, the status changes to "booked", and the lock is released.
2. The status changes back to "available" once the expiration time is reached.

`Challanges`
but the lag between the elapsed time and the time the row is reverted by cron is not ideal for popular events. Ideally, the lock should be removed almost exactly after expiration. Tickets might remain unavailable for purchase even after the expiration time, reducing booking opportunities for high demand events


**Implicit Status with Status and expiration time:**
To handle challanges of not reverting to availbale even after its expired. We can change implementation like. Instead of waiting for cron to reset status, next txn can check reserved and expired status and mark it to reserve again with new expiration time.
1. We begin a transaction
2. We check to see if the current ticket is either AVAILABLE or (RESERVED but expired)
3. We update the ticket to RESERVED with expiration now + 10 minutes.
4. We commit the transaction.
`Challanges`

Our read operations are going to be be slightly slower by needing to filter on two values.
But this can be sorted using compound index or materialised view concept of RDBMS.

**Use Redis distributed locks**
Once request to book ticket received by booking service, bellow steps will get executed.
1. The Booking Service will lock that ticket by adding it to our Redis Distributed Lock with a TTL of 10 minutes (this is how long we will hold the ticket for).
2. The Booking Service will also write a new booking entry in the DB with a status of `in-progress.
3. We will then respond to the user with their newly created bookingId and route the client to a the payment page.
    1. If the user stops here, then after 10 minutes the lock is auto-released and the ticket is available for another user to purchase.
4. once user enters payment details and click "Purchase."  the payment (along with the bookingId) gets sent to Stripe for processing and Stripe responds via web hook that the payment was successful.
5. Upon successful payment confirmation from Stripe, our system's web hook retrieves the bookingId embedded within the Stripe metadata. With this bookingId, the web hook initiates a database transaction to concurrently update the Ticket and Booking tables. Specifically, the status of the ticket linked to the booking is changed to "sold" in the Ticket table. Simultaneously, the corresponding booking entry in the Booking table is marked as "confirmed."
`What is locks goes down`
Realistically, you'd immediately bring a new instance up and then you'd have a 10 minute period where multiple users can progress to the payment page for the same ticket. Given we still have our status in the DB and writes are atomic, we will not fail our consistency guarantee, instead, those unlucky users will get an error and have a bad experience. This is a good trade off between Slight bad experience vs. larger consistency challenge in replicating lock.

## How to support 10s of millions of concurrent events during popular events.

**Caching**: Prioritise caching for data with high read rates and low update frequency, such as event details (names, dates, venue information), performer bios, and static venue details like location and capacity. Because this data does not change frequently.

**Load Balancing**: Use algorithms like Round Robin or Least Connections for even traffic distribution across server instances. Implement load balancing for all horizontally scaled services and databases.

**Horizontal Scaling:** The Event Service is stateless which allows us to horizontally scale it to meet demand. We can do this by adding more instances of the service and load balancing between them.


## How ensure a good user experience during high-demand events with millions simultaneously booking tickets?

With popular events, the loaded seat map will go stale quickly.
We need to ensure that the seat map is always up to date and that users are notified of changes in real-time.

_**Approach  Server Sent events:** update the seat map as soon as a seat is booked (or reserved) by another user without needing to refresh the page. SSE is a unidirectional communication channel between the server and the client. It allows the server to push data to the client without the client having to request it.

`While this approach works well for moderately popular events, the user experience will still suffer during extremely popular events`
"Taylor Swift case," for example, the seat map will immediately fill up, and users will be left with a disorienting and overwhelming experience as available seats disappear in an instant.

_**Virtual Waiting queue for popular events:**  using Redis sorted set(priority queue based on income timestamp)
For extremely popular events, we can implement an admin enabled virtual waiting queue system to manage user access during times of exceptionally high demand. Users are placed in this queue before even being able to see the booking page
1. When a user requests to view the booking page, they are placed in a virtual queue. We establish a WebSocket connection with their client and add the user to the queue using their unique WebSocket connection.
2. Periodically or based on certain criteria (like tickets booked), dequeue users from the front of the queue. Notify these users via their WebSocket connection that they can proceed to purchase tickets.
3. At the same time, update the database to reflect that this user is now allowed to access the ticket purchasing system.
`Long wait times in the queue might lead to user frustration, especially if the estimated wait times are not accurate or if the queue moves slower than expected.` By pushing updates to the client in real-time, we can mitigate this risk by providing users with constant feedback on their queue position and estimated wait time.


## How can you improve search to ensure we meet our low latency requirements?

Create indexes on the Event, Performer, and Venues tables to improve query performance. Indexes allow for faster data retrieval by mapping the values in specific columns to their corresponding rows in the table.

- Optimize queries to improve performance. This includes techniques like using EXPLAIN to analyze query execution plans, avoiding SELECT * queries, and using LIMIT to restrict the number of rows returned. Additionally, using UNION instead of OR for combining multiple queries can improve performance.

 `While indexes improve query performance, they can also increase storage requirements and slow down write operations, as each insert or update may necessitate an index update.

**Full Text Indexes**

We can extend the basic indexing strategy above to utilize full-text indexes in our database, if available. For popular SQL databases like MySQL or Postgres, full text extensions are available which utilize search engines like Lucene under the covers. These make queries for specific strings like "Taylor" or "Swift" much faster than doing a full table scan using LIKE.

###### Challenges
- Full text indexes require additional storage space and can be slower to query than standard indexes.
- Full text indexes can be more difficult to maintain, as they require special handling in both queries and in maintaining the database.


**Use Full text search Engine elastic search**
- To make sure the data in Elasticsearch is always in sync with the data in our SQL DB, we can use change data capture (CDC) for real-time or near-real-time data synchronization from PostgreSQL to Elasticsearch. This setup captures changes in the PostgreSQL database, such as inserts, updates, and deletes, and replicates them to the Elasticsearch index.
- We can enable fuzzy search functionality with Elasticsearch, which allows for error tolerance in search queries. This is way we can handle typos and slight variations in spellings such as "Taylor Swift" vs "Tayler Swift". This is something that would be very difficult to do with SQL alone.


## How can you speed up frequently repeated search queries and reduce load on our search infrastructure?
**Redis or Memcached** 
Use caching mechanisms like Redis or Memcached to store the results of frequently executed search queries. This reduces the load on the search infrastructure by serving repeated queries from the cache instead of hitting the database or search engine repeatedly.

- Key Design: Construct cache keys based on search query parameters to uniquely identify each query.
- Time-To-Live (TTL): Set appropriate TTLs for cached data to ensure freshness and relevance of the information.
```
{ 
	"key": "search:keyword=Taylor Swift&start=2021-01-01&end=2021-12-31",
	"value": [event1, event2, event3], 
	"ttl": 60 * 60 * 24 // 24 hours
}
```

`Efficiently managing cache invalidation can be challenging.
`Frequent cache misses can lead to increased load on the search infrastructure, especially during peak times`

**Elastic search buildIn caching capabilities:**
Elasticsearch has built-in caching capabilities that can be leveraged to store results of frequent queries. This reduces the query processing load on the search engine itself.

1. The node query cache in Elasticsearch is an LRU cache shared by all shards on a node, caching results of queries used in the filter.
2. Elasticsearch's shard-level request cache is used for caching search responses consisting of aggregation.

**CDN**
We can also utilize CDNs to cache search results geographically closer to the user, reducing latency and improving response times


## For events with `10,000+` tickets, the database needs to perform work to summarize information based on the user's query (price total, ticket total). Additionally, this work might be performed _a lot_ for events that are very popular and have users frequently entering the ticket browsing UI. How might we resolve these problems?

We can devide tickets into section as when a user starts browsing tickets. They see a venue map with sections. Each section might have a popover with high-level information about the ticket availability / price information for that section.

If a user clicks into a section of interest, Ticketmaster's UI then shows the individual seats and ticket information.

```
CREATE TABLE tickets ( 
	event_id bigint, 
	section_id bigint,
	seat_id bigint,
	price bigint,
	PRIMARY KEY ((event_id, section_id), seat_id)
);
```

the concept of section_id to our tickets table, and have the section_id as part of the partition key. This means the tickets table now services the query to view individual seat tickets for a given section.

_This schema allows us to distributes an event over several nodes in the Cassandra cluster, because each section of an event is in a different partition. It also means each partition is responsible for serving less data, because the number of tickets in a partition is lower

`how do we now show ticket data for the entire event?` _By keeping seperate event sections details
``` java
CREATE TABLE event_sections ( 
	event_id bigint,
	section_id bigint,
	num_tickets bigint,
	price_floor bigint, 
//-- section_id is added as a clustering key to ensure primary key uniqueness; 
//order -- doesn't matter for the app access patterns 
	PRIMARY KEY (event_id, section_id) );
```

Additionally, the section stats being queried don't need to be extremely precise - there's tolerance eventual consistency. In fact, Ticketmaster doesn't even show exact ticket numbers in their UI, they merely show a total such as 100+.