---
Creation Time: Thursday, July 25th 2024
Modified Time: Monday, February 24th 2025
---
### Requirement:
user should be able to search cab for given source and destination to book the cab for price estimates
user should be able to cancel booking if ride is not started
user should be able to book different type of available cabs 
Upon booking request, riders should be matched with driver who is nearby and available
driver can cancel/accept request and navigate to pickup and dropoff.
user should be able to track driver and ride
user should be able to watch booking history

`Out of scope problem
1. Riders should be able to rate their ride and driver post-trip.
2. Drivers should be able to rate passengers.
3. user can schedule cab
4. user can share ride details over message or whatsapp
5. user shoud get notification about cab proximity before arrival
6.  The system should have robust monitoring, logging, and alerting to quickly identify and resolve issues.
7.  The system should be resilient to failures, with redundancy and failover mechanisms in place.
8.  The system should ensure the security and privacy of user and driver data, complying with regulations like GDPR.

### NFR:
9. The system should prioritize low latency matching (< 1 minutes to match or failure)
10.  The system should ensure strong consistency in ride matching to prevent any driver from being assigned multiple rides simultaneously
11. 1. The system should be able to handle high throughput, especially during peak hours or special events (100k requests from same location)
12. System should be highly available

### Capacity estimates:
_user estimates_
daily active user 10M
user daily booking avg : 1 per day ~ 1M/86400 = 100 write/sec
daily cab search by user : 2 per day = 200 ride search/ sec
_Driver estimates:_
drivers: 10M, roughly sending location every 5 sec ~ 2M location update /sec

### API design:

GET:  /v1/vehicle/search
Header: user details in JWT token
Request: 
```
	"source": {lat, long},
	"destination": {lat, long}
```

Response:
```
	availableVehicle:[ 
		{
			"rideRequestId": "awd",
			"vehicleType":"sedan",
			"startingPrice" : 400
		},
		{
			"rideRequestId": "se1",
			"vehicleType":"mini",
			"startingPrice" : 300
		},
		{
			"rideRequestId": "gre",
			"vehicleType":"auto",
			"startingPrice" :200
		}
	]
```

POST /v1/vehicle/book
 user details in JWT token
Request
```
{
	"rideRequestId" : "awd"
}
```

Response:
```
	"status":"success",
	"data":
	{// ride data
		"cabNo":"asdas",
		"price" : "23434",
		"cabLocation" : {lat, long}
		"driverNumber": "2347u294203"
	}
	
```

POST /v1/driver/location  // send driver location update
 driver details in JWT token
Request
```
{
	"lat":"3223,
	"long":dasas
}
```

POST /v1/driver/rideRequestId  // call api to accept or reject request by driver
 driver details in JWT token
Request
```
{
	"accept" : true/false
}
```
Response
```
{
	"userLocation" : {lat, long},
	"destination" : {lat, long}
	"rideFare" : 23214,
	"userName" : sdasdjka
}

```



### DB schema
User
```
user id
name
phone no
emailid

```

UserPaymentDetails
```
userId
paymentModes:{
	wallet,
	user wallet
	gpay
	cash
	card
}
```

RegisteredVehicles
```
vehicleId
vehicle no
vehicle type
vehicle brand
```

Registered driver
```
vehicleId
driver id
driver name
driver contact
```

userBooking
````
userId
vehicleId
bookingid
soruce
destination
price
rideStatus: completed/cancelled/inprogress
````


#### HLD
[[uber HLD]]

##### Problem with the 1 design to find matching driver
**High Frequency of Writes** of driver location data:
Given we have around 10 million drivers, sending locations roughly every 5 seconds, that's about 2 million updates a second! Whether we choose something like DynamoDB or PostgreSQL (both great choices for the rest of the system), either one would either fall over under the write load, or need to be scaled up so much that it becomes prohibitively expensive for most companies.
For DynamoDB in particular, 2M writes a second of ~100 bytes would cost you about 100k a day. 
**Query Efficiency** on huge location data :
to query a table based on lat/long we would need to perform a full table scan, calculating the distance between each driver's location and the rider's location. This would be extremely inefficient, especially with millions of drivers. Even with indexing on lat/long columns, traditional B-tree indexes are not well-suited for multi-dimensional data like geographical coordinates, leading to suboptimal query performance for proximity searches

#### Sol 1:
_Batch processing while writing driver location_
For proximity searches,_using specialised geospatial database:_ with appropriate indexing is used to efficiently query nearby drivers. Geospatial databases use specialised data structures, such as` quad-trees`, to index driver locations, allowing for faster execution of proximity searches. Quad-trees are particularly well-suited for two-dimensional spatial data like geographic coordinates, as they partition the space into quadrants recursively, which can significantly speed up the process of finding all drivers within a certain area around a ride request.If we were to go with PostgreSQL, it actually support a plugin called PostGIS that allows us to use geospatial data types and functions without needing an additional database. [[Index on DB]]
###### Challenges
The interval between batch writes introduces a delay, which means the location data in the database may not reflect the drivers' most current positions. This can lead to suboptimal driver matches.

#### Sol 2 : Real time in memory GEOSpatial data store for driver location
`Redis` : which supports geospatial data types and commands. This allows us to handle real-time driver location updates and proximity searches with high throughput and low latency while minimizing storage costs with automatic data expiration.
Redis uses geo hashing to store encoded lat and long into single string key which is then indexed using sorted set.


_Redis supports bellow  Geospatial commands to provide near real time search for nearby  locations_
GEOADD key "lat" "long" timestamp
EX:
```
GEOADD driver_location 13.361389 38.115556 "driver1"
```

Find all driver within 3 KM from user location(18, 37)
```
GEORADIUS driver_location 18 37 3 km
```

Res: driver1

_Advantages:_ - - Very fast, scalable, and ideal for ephemeral real-time data. We no longer have a need for batch processing since Redis can handle the high volume of location updates in real-time. Additionally, Redis automatically expires data based on a specified time-to-live (TTL), which allows us to retain only the most recent location updates and avoid unnecessary storage costs.
_Disadvantages:_ - Data is held in memory, so if you need to persist historical data, you might need additional solutions.
However, this risk can be mitigated in a few ways:
1. Redis persistence: We could enable Redis persistence mechanisms like RDB (Redis Database) or AOF (Append-Only File) to periodically save the in-memory data to disk.
2. Redis Sentinel: We could use Redis Sentinel for high availability. Sentinel provides automatic failover if the master node goes down, ensuring that a replica is promoted to master.
3. Even if we experience data loss due to a system failure, the impact would be minimal. Since driver location updates come in every 5 seconds

	`InfluxDB or any Timeseries DB`
	
For driver location tracking Driver's continuous location data needs to be saved. These are time series data. Time series DBs are best suited for storing data based data with high right throughput and provides efficient query performance based on time range search.
- Data is stored in measurements or hypertables with a timestamp, driver ID, latitude, longitude, and other metadata.
- You can perform time-windowed queries, aggregations, and even use continuous queries for analytics.

_Advantages:_ - - Very fast, scalable for handling large volume of time stamped data with efficient compression and quering.
_Disadvantages:_ - Not too efficient for query like find driver current location, which redis can provide better.


_Recommendations is **Combined Architecture:**
4. **Driver App:** Sends periodic updates to the backend.
5. **Location Ingestion Service:** Processes updates and writes the latest location to Redis, while also storing historical data in a time-series database or Cassandra.
6. **Location Query API:** Reads from Redis for immediate location queries.
7. **Analytics and Reporting:** Reads from the historical store for aggregated reports.


##### System will be overloaded if 10M driver sends location update every 5 sec ping
_Sol: Adaptive Location update Intervals_
dynamically adjust the frequency of location updates based on contextual factors such as speed, direction of travel, proximity to pending ride requests, and driver status. This allows us to reduce the number of location updates while maintaining accuracy.
The driver's app uses ondevice sensors and algorithms to determine the optimal interval for sending location updates. For example, if the driver is stationary or moving slowly, updates can be less frequent. Conversely, if the driver is moving quickly or changing direction often, updates are sent more frequently.
_disadvantage_
The main challenge with this approach is the complexity of designing effective algorithms to determine the optimal update frequency.

##### How do we prevent multiple ride requests from being sent to the same driver simultaneously?
_Sol1:_ lock drivers to prevent multiple ride requests from being sent to the same driver simultaneously.
use application-level locking, where each instance of the Ride Matching Service marks a ride request as "locked" when it is sent to a driver. It then starts a timer for the lock duration. If the driver does not accept the ride within this period, the instance manually releases the lock and makes the request available to other drivers.
- **Lack of Coordination:** With multiple instances of the Ride Matching Service running, there's no centralized coordination, leading to potential race conditions where two instances might attempt to lock the same ride request simultaneously.
- - **Inconsistent Lock State:** If one instance sets a lock but fails before releasing it (due to a crash or network issue), other instances have no knowledge of this lock, which can leave the ride request in a locked state indefinitely.
- - **Scalability Concerns:** As the system scales and the number of instances increases, the problem of coordinating locks across instances becomes more pronounced, leading to a higher likelihood of errors and inconsistencies.

_sol 2_
 Move the lock into the database. This allows us to use the database's built-in transactional capabilities to ensure that only one instance can lock a ride request at a time. `When we send a request to a driver, we would update the status of that driver to "outstanding_request".
 If the driver accepts the request, we update the status to "accepted" and if they deny it, we update the status to "available"`.  We can then use a simple timeout mechanism within the Ride Service to ensure that the lock is released if the driver does not respond within the 10 second window.
_disadvantage_ In this solution we still run into issues with `relying on a in-memory timeout to handle unlocking if the driver does not respond. If the Ride Service crashes or is restarted, the timeout will be lost and the lock will remain indefinitely`. This is a common issue with in-memory timeouts and why they should be avoided when possible. One solution is to have a cron job that runs periodically to check for locks that have expired and release them. This works, but adds unecessary complexity and introduces a delay in unlocking the ride request.

_Sol3 _ Distributed lock with TTL

To solve the timeout issue, we can use a distributed lock implemented with an in-memory data store like Redis. When a ride request is sent to a driver, a lock is created with a unique identifier (e.g., driverId) and a TTL set to the acceptance window duration of 10 seconds
The Ride Matching Service attempts to acquire a lock on the driverId in Redis. If the lock is successfully acquired, it means no other service instance can send a ride request to the same driver until the lock expires or is released. If the driver accepts the ride within the TTL window, the Ride Matching Service updates the ride status to "accepted" in the database, and the lock is released in Redis.If the driver does not accept the ride within the TTL window, the lock in Redis expires automatically. This expiration allows the Ride Matching Service to consider the driver for new ride requests again.

_disadvantage_ This requires robust monitoring and failover strategies to ensure that the system can recover quickly from failures and that locks are not lost or corrupted. Given locks are only held for 10 seconds


##### How can we ensure no ride requests are dropped during peak demand periods?

during special events or holidays when demand is high and the system is under stress. We also need to protect against the case where an instance of the Ride Matching Service crashes or is restarted, leading to dropped rides.

With the above design where request are served based on first come first serve, will not work when there is sudden increase in demand. Horizantly scalling Ride matching service will also not be able to scale in case of sudden demand.we may not be able to scale quickly enough to prevent dropped requests.
`Additionally`, if a Ride Matching Service goes down, any ride requests that were being processed by that instance would be lost. This could result in riders waiting indefinitely for a match that will never come, leading to a poor user experience and potential loss of customers. There's no built-in mechanism for request recovery or failover in this approach.

_Sol: Introduce queuing system

When a ride request comes in, it is added to the queue. The Ride Matching Service then processes requests from the queue in a first-come, first-served manner. If the queue grows too large, the system scales horizontally by adding more instances of the Ride Matching Service to handle the increased load. This allows us to scale the system dynamically based on demand, ensuring that no requests are dropped. We can also partition the queues based on geographic regions to further improve efficiency.

_disadvantages and challanges_

The main challenge with this approach is the complexity of managing a queueing system. We need to ensure that the queue is scalable, fault-tolerant, and highly available. We can address this by using a managed queueing service like Amazon SQS or Kafka, which provides these capabilities out of the box. This allows us to focus on the business logic of the system without worrying about the underlying infrastructure.

The other issue with this approach is that since it is a `FIFO queue you could have requests that are stuck behind a request that is taking a long time to process.` This is a common issue with FIFO queues and can be addressed by using a priority queue instead. This allows us to prioritize requests based on factors like driver proximity, driver rating, and other relevant factors. This ensures that the most important requests are processed first, leading to a better user experience.




