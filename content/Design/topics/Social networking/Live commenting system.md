---
Creation Time: Tuesday, February 4th 2025
Modified Time: Tuesday, February 11th 2025
---
- **Functional requirements** by interviewer
    - Write: Users can write real-time comments to posts.
    - Read: Users can read real-time comments from posts.


### Functional:

User role authentication
1. The User should be able to login
2. Only authenticated users should be able to post comment
User post:
3. The user should be able to post a comment.
4. All users viewing that post/video should be able to see latest comment in real time.
Comment view:
1. User should be able to view all the comments of the post made till now
2. User should be able to see comments in real time made by other user on the post user is watching
Comment moderation
3. Admin users should be able to edit or delete comments if it's inappropriate.
4. system should be able to report user based on inappropriate comments.
5. User should have rate limit on number of comment made to avoid bad uses.
6. Comment should be validated against SQL injection.

**Below the line (out of scope):**
- Viewers can reply to comments
- Viewers can react to comments
### Non-Functional:

Low latency: comments should be viewed to other user in real time
As there will be millions of users data, data replication should be implemented to ensure no data loss.
Hot posts and comments should be cached to display comment to users


## Capacity estimation
Average comment size: 100 words, approx size: 200KB
new comment added 10K/Sec
Total memory in 1 Sec: 200 * 10000 = 2MB
Total in 1 yr = 2MB * 24 * 60 * 30 * 365 = 36TB

`Peak comment load
DB should be able to handle write with 10K/sec
Assume 1M user fetch latest comment in every 5 Sec = 1M/5
read = 200k read/sec
`Network bandwidth:
Write: 2MB/ Sec
Assume user fetches 10 Comment / sec, size 200Byte * 10 = 2000Byte
= 2000Byte * 200,000 read/sec
= 400,000,000 Byte Read/Sec
400MB/Sec
`Server requirement:
**Application server**
	1 server can handle 10K concurrent request. We will need 100 servers for 1M concurrent users.
**DB server:**
	To support 200,000 read/sec and 10K write per second we need database with multisharding and replica support. which can scale horizontally.
**Websocket connect:**
	Assume 1 web socket server can provide 50K connection, we need 20 websocket server connection.
**Caching:**
	Frequently accessed comments should be cached to improve performance.
	we can assume 20% data to be cached.

```
## API design

Endpoint: POST /api/posts/{postId}/comments
Description: add comment by user on some pos
Header: JWT | SessionToken (to authenticate user)
Sample request:
	{
		"comment" : "my comment"
	}

Sample Reponse: HTTP code 200
	{
			"status" : 200,
			"postId" : "2123",
			"timestamp" : "65789"
	}

Endpoint: GET /api/posts/{postId}/comments?cursor={last_comment_id}&pagesize=10
Descriptipn: fetch all the comment on post 
Sample Response:
	{
		"status" : "success",
		"comments" :[
			{
				"commentId" : "comment1",
				"userId" : "user1",
				"timestamp" : "2423423",
				"comment" : "my comment"
			}
		]
	}

Endpoint : DELETE /api/comments/{commentId}
Sample Response:
	{
		"status" : "SUCCESS"
	}
```

### HLD
- There are 2 ways to publish comments to users
    - Push (Fanout and write): Maintains a connection with user to directly send new comments directly.
        - Pros
            - Easy to implement
        - Cons
            - User cannot get the comments generated when he was offline.
            - For mobile platform, system resources are limited to handle lots of new comment.
    - Pull (Fanout and load): User needs to request the server to get the new comments when required.
        - Pros
            - User can get the comments generated when he was offline.
        - Cons
            - System have to handle a large amount of requests.




### Extended Requirement:

Design Facebook live video with real time comments

**Core Requirements**
1. Viewers can post comments on a Live video feed.
2. Viewers can see new comments being posted while they are watching the live video.
3. Viewers can see comments made before they joined the live feed.
**NFR:**
Millions of concurrent videos and thousands of comment per sec on each video
Availability should be priorities above consistency
System should broadcast new comment with low latency, to all the viewers with in 200ms





DB component

`posts`


DB choice for storing comments here: I would go with cassandra or dynamao DB as it provides large scalable fast write and read accessed database. We do not need complex queries to store and fetch comments so it will be best choice for storing comments.
`comments`

| id  | postId | comment | timestamp | userId |
| --- | ------ | ------- | --------- | ------ |
|     |        |         |           |        |


### Let's focus more on requirement 2, retrieving and showing list on comments on the `post on the feed` or  `live video user is watching` .

_Multiple approaches to broadcast comment to all other users like
**polling(simplest approach)
** Web Socket
** Server Sent events

**_Polling:** [[Polling]]
Client will poll to server for new comment every second using API, cursor will point to last comment Id already fetched by user
_GET /api/posts/{postId}/comments?cursor={last_comment_id}


```plantuml
ViewerClient -> Server : GET /api/posts/{postID}/comments?coursor={lastcommentId}
Server -> Database : select from from comments where commentid>lastcommentId order by timestamp
Database -> Server: return comments
Server -> ViewerClient : comments
```

If number of post and comments grows we will need to poll DB more frequently to show it in real time. it will increase DB load, as most of the time there will be no comments to fetch. 


### requirement 3 
"give me the N most recent comments that occurred before a certain timestamp".
We can use paginated to load older comments as user scrolls up to fetch older comments(infinite scrolling).

There are 2 way to do pagination `offset based` or `Cursor Pagination` [[Pagination]]

Though Cursor pagination is better way to fetch older comments it will still be difficult to scale system as there will be too many queries to be hit on DB to fetch huge list of comments.


###### How to improve real time fetch more further????
`HYBRID APPROACH ???`
Ex:
pull older comment....   can be done by cursor pagination
push new comments?    ......... lets explore

There are 2 ways to send push based comments to users feed. [[Web Socket connection]] or `SSE`

We can see writing comment has very less frequency than reading in this system, using Websocket will be less utilising Websocket capabilities as it do not need 2 way connection all the time. So here `Server Sent event will be better fit. 

[[live commenting daigrams]]

Here is our  flow:
1. User posts a comment and it is persisted to the database (as explained above)
2. In order for all viewers to see the comment, the Comment Management Service will send the comment over SSE to all connected clients that are subscribed to that live video.
3. The Commenter Client will receive the comment and add it to the comment feed for the viewer to see.


### 2) How will the system scale to support millions of concurrent viewers?

Modern servers and operating systems can handle large numbers of concurrent connections—commonly in the range of 100k to 1M. But if we want to support, Millions of user 1 server will not be enough for SSE connection. We need to add multiple server horizantly.

_Ex: 
```
 UserA is watching Live Video 1 and connected to Server 1
 UserB is watching Live Video 1 but connected to Server 2
```

`If we have distributed server for Server Sent Events
The question then becomes how do we distribute the load across multiple servers and ensure each server knows which comments to send to which viewers?

To distribute incoming traffic evenly across our multiple servers we can use a simple load balancing algorithm like [round robin](https://www.nginx.com/resources/glossary/round-robin-load-balancing/#:~:text=What%20Is%20Round%2DRobin%20Load,to%20each%20server%20in%20turn). Upon connecting to a Realtime Messaging Server through the load balancer, the client needs to send a message informing the server of which live video it is watching. The Realtime Messaging Server then updates a mapping in local memory with this information. This map would look something like this:
_We will need to maintain the connection mapping, Connection Metadata
```
{ 
	"liveVideoId1": ["sseConnection1", "sseConnection2", "sseConnection3"],
	"liveVideoId2": ["sseConnection4", "sseConnection5", "sseConnection6"],
	"liveVideoId3": ["sseConnection7", "sseConnection8", "sseConnection9"], 
}
```


`This is our key challenge: How do we ensure all viewers see new comments, regardless of which server they're connected to?`

_There are three possible ways to solve it_

_**Horizontal scaling with with load balancer and Pub/Sub
Pub/Sub partitioning into channels per live video
![[live commenting daigrams]]




<font color=DarkGreen>Advanced candidates may point out the tradeoffs in different pub/sub systems. For example, Kafka is a popular pub/sub system that is highly scalable and fault-tolerant, but it has a hard time adapting to scenarios where dynamic subscription and unsubscription based on user interactions, such as scrolling through a live feed or switching live videos, is required. This is due to its pull-based model leading to degraded latency, limited scalability as consumers need to subscribe to all topics, and operational complexity. Redis, on the other hand, is a more suitable option for scenarios requiring dynamic subscription and unsubscription thanks to its efficient in-memory storage and support for both blocking and non-blocking consumption methods, leading to improved latency.However, the main concern with Redis is its potential for data loss due to periodic disk writes and the challenges of memory limitation, which could be a bottleneck for scalability. Additionally, while Redis offers high availability configurations like Redis Sentinel or Redis Active-Active, these add to the operational complexity of managing a Redis-based system. The pub/sub solution is the "correct academic answer" and should clearly pass the interview, but the reality is choosing the right pub/sub system is a complex decision that requires a deep understanding of the system's requirements and tradeoffs.
</font>

`Best Solution
_**Partition Pub/Sub with layer 7 load balancer_

The older approach  explained in Diag 4: RTMS server allocation to client connection via round robin can lead to many connection on partition, and if partition  contains video comments for other Videos ex video2 as well, RTMS will be reading extra comments even tough its not broadcasting comments for Video 2.

**Option 1: Intelligent Routing via Scripts or Configuration**
We can use layer 7 load balancer or API gateway can leverage consistent hashing based on the liveVideoId. By inspecting the request—such as a header or path parameter that includes the liveVideoId—the load balancer can apply a hashing function that always routes viewers of the same video to the same server. Tools like NGINX or Envoy can be scripted with Lua or configuration directives to achieve this content-based routing, reducing overhead and keeping related viewers together. Diag 5

**Option 2: Dynamic Lookup via a Coordination Service (e.g., Zookeeper)**
Instead of relying on hashing alone, we can store a dynamic mapping of liveVideoId to a specific server in a coordination service like Zookeeper. The load balancer or custom proxy layer queries Zookeeper to determine the correct server for each liveVideoId. As traffic patterns change, new servers can register themselves or existing mappings can be updated in Zookeeper, and the load balancer will route future requests accordingly. This approach offers more flexibility but introduces operational complexity and requires caching and careful failure handling.


_**Dispatcher approach instead of PUB/SUB_

Diag:2 
This approach can be acheived by
1. **Maintaining Dynamic Mappings:** The Dispatcher Service keeps a dynamic map of which Realtime Messaging Server is responsible for which liveVideoId. This can be stored in memory and periodically refreshed from a consistent store (like Zookeeper or etcd), or updated via heartbeats and registration protocols as servers join or leave.
2.  **Registration & Discovery:** When a Realtime Messaging Server comes online, it registers with the Dispatcher Service (or the coordination store it consults). The Dispatcher updates its internal mapping to reflect which server is now serving particular live videos. Likewise, if a server scales down or fails, the Dispatcher updates the mapping accordingly.
3.  **Direct Routing:** Upon receiving a new comment, the comment management service calls the Dispatcher Service to determine the correct server. The Dispatcher uses its current mapping and forwards the comment directly to the responsible Realtime Messaging Server, which then sends it to connected viewers.
4.  **Scalability & Redundancy:** In high-demand scenarios, multiple Dispatcher instances can run in parallel behind a load balancer. They all consult the same coordination data, ensuring consistency. This replication adds redundancy, making the system more resilient to failures.

### DataBase 
High right throughput. little less consistency
**primary Key**: video ID,  shard based on video ID in `Cassandra` to scale horizantaly with CAP's A and P 
**Sort Key**: created AT
1M(video ) * 100(comment per video) * 48 (30 min video size, in 1 day 48 live video)  = 