---
Creation Time: Wednesday, March 26th 2025
Modified Time: Wednesday, March 26th 2025
---
  


In HTTP-based request-response pattern communication is always initiated by the client, which sends a request to the server, and the server responds with the data. This pattern works exceptionally well for retrieving static content or performing discrete operations, but it introduces challenges for real-time scenarios.

Consider these common real-time use cases:

- Authentication events (a user logging into a system)
- Content creation events (uploading a video to YouTube)
- Social media updates (posting a tweet on Twitter)

In these scenarios, information originates on the server side, often triggered by actions from other users. The fundamental challenge is: **how does a client learn about events that it has no prior knowledge of?**

With a traditional request-response model, clients must resort to **_polling_** where they repeatedly send requests to the server asking for updates:

Client: "Any new notifications?"  
Server: "No."  
[wait]  
Client: "Any new notifications?"  
Server: "No."  
[wait]  
Client: "Any new notifications?"  
Server: "Yes, user X uploaded a video."

This approach introduces several inefficiencies:

1. **Network overhead**: Each poll generates network traffic, even when no new data is available
2. **Latency issues**: Updates are only discovered when the next poll occurs, creating a trade-off between timeliness and resource consumption
3. **Connection establishment costs**: Each HTTP request typically requires establishing a new TCP connection, involving the TCP three-way handshake
4. **Server load**: The server must process numerous requests that often return no new information
5. **Header redundancy**: HTTP headers are repeatedly sent with each request, adding unnecessary overhead

The polling frequency creates a dilemma: poll too frequently, and you waste resources; poll too infrequently, and you introduce latency in receiving updates. This tradeoff makes traditional request-response inadequate for truly real-time applications.

# The Push Model

Rather than relying on clients to request data, the server proactively sends data to clients when new information becomes available.

From a theoretical computer science perspective, the push model aligns with the Observer design pattern, where subjects (servers) notify observers (clients) about state changes.

In distributed systems theory, the push model implements a form of “event-driven architecture” where components react to events rather than polling for changes. This approach is particularly valuable in scenarios with unpredictable event timing and where low latency is critical.

## Technical Implementation

At its core, the push model requires:

1. **Persistent connections**: Long-lived connections between clients and servers
2. **Bidirectional communication capability**: The ability for servers to initiate data transfer
3. **Connection state management**: Mechanisms to track connected clients and their subscriptions
4. **State synchronization**: Handling what happens when clients reconnect after being offline
5. **Message routing infrastructure**: Systems to determine which clients should receive which messages

Technically, a push system operates as follows:

1. The client establishes a connection to the server
2. This connection remains open, unlike the traditional request-response cycle where connections are typically closed after each interaction
3. The server maintains a registry of connected clients and their subscription preferences
4. When a relevant event occurs, the server identifies interested clients and pushes data through the established connections
5. The client processes the incoming data without having explicitly requested it

This model effectively creates a unidirectional stream from the server side, though the underlying protocol is typically bidirectional. While TCP can function as a transport layer for push mechanisms, higher-level protocols built on TCP are often employed to manage the complexities of push communication.

# Connection Protocols for Push

Several protocols support the push model:

1. **WebSockets**: Provides full-duplex communication channels over a single TCP connection, allowing both client-to-server and server-to-client communication
2. **Server-Sent Events (SSE)**: A standardized way for servers to initiate data transmission to clients once a client has established a connection
3. **MQTT (Message Queuing Telemetry Transport)**: A lightweight publish/subscribe protocol designed for constrained devices, widely used in IoT applications
4. **AMQP (Advanced Message Queuing Protocol)**: Used by systems like RabbitMQ to enable sophisticated message routing and delivery guarantees

Each protocol has specific characteristics that make it suitable for different push scenarios, with trade-offs in terms of browser support, header overhead, connection management, and message delivery guarantees.

# RabbitMQ’s Push Architecture

When a message is submitted to the RabbitMQ exchange, the following sequence occurs:

1. The exchange routes the message to appropriate queues based on binding rules
2. For each queue with active consumers, RabbitMQ identifies the connected clients
3. The broker immediately pushes the message to these consumers
4. Consumers process the message according to their implementation

This differs significantly from a polling-based message queue system like Kafka (in its default configuration), where consumers request messages at their own pace rather than receiving them automatically.

RabbitMQ’s push model is implemented through AMQP’s “basic.consume” method, which establishes a subscription. When a consumer invokes this method, it effectively tells the broker: “Send me messages from this queue as they arrive.” The broker then pushes messages to the consumer as they become available, using the existing AMQP connection.

To manage potential overload scenarios, RabbitMQ implements a flow control mechanism called “prefetch count” (or Quality of Service settings), which limits the number of unacknowledged messages that can be in flight to a consumer at any given time. This creates a form of backpressure that prevents consumers from being overwhelmed.

For example, with a prefetch count of 10, RabbitMQ will send up to 10 messages to a consumer, then wait for acknowledgments before sending more. This provides a balance between the immediacy of push and the load management of pull-based systems.

# How Push Works in Practice: Technical Deep Dive

To understand push communication at a lower level, let’s examine what happens from a networking and protocol perspective.

## Connection Establishment

Initially, a TCP connection is established between the client and server using the standard three-way handshake. This provides a reliable, ordered stream of data between the two endpoints. On top of this TCP connection, a higher-level protocol like WebSocket or AMQP is established, which defines the rules for message formatting, delivery, and flow control.

## Message Delivery Process

Once a connection is established, the server can send messages to the client at any time. From a socket programming perspective, this is simply writing data to the client’s socket. However, the higher-level protocol typically adds structure to these messages, including:

1. **Message framing**: Defining where one message ends and another begins
2. **Message headers**: Metadata about the message content
3. **Payload formatting**: Structuring the actual data (often using formats like JSON or Protocol Buffers)
4. **Sequence identifiers**: For tracking message order and processing acknowledgments

When an event occurs (like a user uploading a video), the server:

1. Identifies which connected clients should receive this notification
2. Formats the notification according to the protocol specifications
3. Writes the formatted message to each client’s socket
4. Handles any acknowledgment or flow control mechanisms required by the protocol

From the client’s perspective, it continuously reads from its socket, processes incoming messages according to the protocol, and dispatches them to appropriate handlers in the application.

## Socket-Level Implementation

At the lowest level, a push notification system might be implemented using socket programming like this (pseudocode):

Server side:

// Store all connected client sockets  
connectedClients = []

// When a client connects  
onClientConnect(clientSocket):  
    connectedClients.add(clientSocket)// When an event occurs  
onEvent(eventData):  
    message = formatMessage(eventData)  
    for each socket in connectedClients:  
        if isSubscribedToEvent(socket, eventData.type):  
            socket.write(message)

Client side:

// Establish connection  
socket = connectToServer()

// Continuously read from socket  
while socket.isConnected():  
    message = socket.read()  
    if message:  
        processMessage(message)

Real-world implementations add numerous layers of complexity for handling connection failures, message delivery guarantees, and scalability.

# The YouTube Challenge

A content creator might have over 100 million subscribers. If push notifications were enabled by default for all subscribers, each new video upload would trigger a massive notification event.

Let’s quantify the scale of this problem:

- 100 million subscribers receiving notifications
- Each notification packet might be ~1KB in size
- Total outbound data: 100 million × 1KB = 100GB of data
- If the server can send 10Gbps, it would take ~80 seconds just to transmit all notifications
- Each connection requires memory for socket buffers, typically 8–16KB per connection
- 100 million connections × 10KB average buffer = 1000GB (1TB) of memory just for connection buffers

Beyond raw bandwidth and memory requirements, maintaining 100 million simultaneous TCP connections would create enormous overhead for connection state tracking, TCP window management, and operating system resources.

# Real-World Solution: Multi-Tier Push Architecture

YouTube (and similar platforms) solve this through a multi-tier push architecture:

1. **Service tier**: YouTube’s servers detect a new video upload
2. **Aggregation tier**: Rather than pushing directly to end-users, YouTube pushes the notification to platform notification services (Google Firebase Cloud Messaging for Android, Apple Push Notification Service for iOS)
3. **Distribution tier**: These cloud services manage the actual delivery to millions of devices
4. **Client tier**: Mobile devices receive notifications through their platform’s notification system

This architecture distributes the load across multiple systems, with each platform notification service handling the complexities of device connectivity, retry logic, and delivery optimization.

The technical advantages of this approach include:

1. **Connection pooling**: YouTube maintains only a few connections to notification services rather than millions to end-users
2. **Batched delivery**: Notifications can be batched and prioritized
3. **Specialized delivery infrastructure**: Platform notification services are specifically designed for high-scale delivery
4. **Offline handling**: Platform services handle delivery to devices that come online later

# Pros and Cons of the Push Model

## Technical Advantages

1. **Minimal latency**: In a push model, notification delivery approaches the theoretical minimum latency possible — the network transit time. There’s no additional delay waiting for the next client poll.
2. **Reduced network overhead**: For infrequent events, push significantly reduces network traffic compared to polling. Consider a scenario where events occur on average once per hour, but clients would need to poll every minute to maintain reasonable responsiveness. Push would generate 60 times less network traffic.
3. **Server-controlled timing**: The server can determine the optimal time to deliver messages, implementing strategies like batching related notifications or throttling during peak loads.
4. **Connection efficiency**: By maintaining a single persistent connection rather than establishing new connections for each poll, push models reduce the overhead of TCP handshakes and slow-start algorithms.
5. **Header reduction**: Unlike polling with HTTP, which sends full headers with each request, a persistent connection protocol like WebSocket sends headers only during connection establishment, reducing overhead for subsequent messages.

## Technical Disadvantages

1. **Connection state management**: The server must maintain state for each connected client, consuming memory and adding complexity for handling connection failures.
2. **Client availability requirement**: Clients must be online (connected) to receive push notifications. This creates challenges for mobile devices with intermittent connectivity or battery optimization features that limit background connections.
3. **Flow control complexity**: Without careful implementation of flow control mechanisms, servers can overwhelm clients by pushing messages faster than they can be processed. This is particularly problematic for:

- Resource-constrained devices like IoT sensors
- Clients with variable processing capabilities
- Situations with sudden spikes in message volume

**4. Firewall and proxy challenges**: Long-lived connections can be problematic in environments with aggressive firewalls, proxy servers, or NAT devices that might terminate seemingly idle connections.

**5. Connection recovery overhead**: When connections fail (as they inevitably do in distributed systems), reconnection logic adds complexity to both client and server implementations.

**6. Scalability limitations**: As illustrated in the YouTube example, direct push to millions of clients creates significant infrastructure challenges. Each connection consumes resources on the server, including:

- File descriptors (limited by operating system)
- Memory for connection state and buffers
- CPU cycles for connection management
- Network interface capacity

# Flow Control in Push Systems

One of the technical challenges is flow control — ensuring clients aren’t overwhelmed by pushed data. This issue doesn’t exist in polling models since clients control the pace of data retrieval.

Several approaches can address this challenge:

1. **Client-signaled backpressure**: Clients send signals to the server indicating their processing capacity or willingness to receive more messages.
2. **Acknowledgment-based flow control**: Servers limit the number of unacknowledged messages in flight (similar to RabbitMQ’s prefetch count).
3. **Rate limiting**: Servers impose global or per-client rate limits on message delivery.
4. **Priority queuing**: Messages are categorized by priority, ensuring critical messages are delivered even during high-volume periods.
5. **TCP’s built-in flow control**: At the transport layer, TCP implements window-based flow control that can prevent network buffer overflow, though this doesn’t protect application-level resources.

# Technical Advantages of Polling

1. **Firewall friendliness**: Standard HTTP polling works reliably across various network environments, including corporate firewalls and proxies.
2. **Batch efficiency**: Clients can process multiple updates in a single batch, potentially reducing processing overhead.
3. **Reconnection simplicity**: If a connection fails, the client simply initiates a new request during the next polling interval.

# Optimized Polling Techniques

Several techniques can improve polling efficiency:

1. **Conditional polling**: Using HTTP headers like `If-Modified-Since` or ETags to avoid transferring unchanged data.
2. **Adaptive polling intervals**: Dynamically adjusting polling frequency based on update patterns or application state.
3. **Long polling**: A hybrid approach where the server holds the request open until new data is available or a timeout occurs, reducing the number of polls while maintaining client-initiated connections.
4. **Delta updates**: Returning only the changes since the last poll rather than complete data sets.

# Kafka’s Approach: Log-Based Polling

1. Messages are appended to immutable logs (topics)
2. Consumers maintain their position (offset) in these logs
3. Consumers explicitly request messages starting from their current offset
4. Consumers control how many messages they retrieve in each request

This approach gives consumers complete control over their consumption rate, preventing the overload problems that can occur in push systems during traffic spikes.

# gRPC and Modern Streaming Protocols

gRPC’s supports server-side streaming, which represents a modern implementation of push-style communication within a structured RPC framework.

# gRPC Streaming Models

gRPC, built on HTTP/2, supports four communication patterns:

1. **Unary RPC**: Traditional request-response (one request, one response)
2. **Server streaming RPC**: Client sends a request, server responds with a stream of messages (push model)
3. **Client streaming RPC**: Client sends a stream of messages, server responds with a single message
4. **Bidirectional streaming RPC**: Both sides send streams of messages

The server streaming RPC pattern implements the push model directly within the gRPC framework. After a client initiates the connection with a single request, the server can continuously push messages to the client without further client requests.

# Technical Implementation

In gRPC server streaming:

1. The client makes a single RPC call, establishing a stream
2. The server sends multiple responses over this stream as data becomes available
3. The client processes these responses as they arrive
4. The stream remains open until the server completes sending all responses or an error occurs

This approach leverages HTTP/2’s multiplexing and flow control features to provide an efficient push mechanism with built-in protections against client overload.

# Example: Notification Service in gRPC

A notification service using gRPC server streaming might define a service like this:

service NotificationService {  
  rpc Subscribe(SubscriptionRequest) returns (stream Notification);  
}

message SubscriptionRequest {  
  string user_id = 1;  
  repeated string topics = 2;  
}message Notification {  
  string id = 1;  
  string topic = 2;  
  string content = 3;  
  google.protobuf.Timestamp created_at = 4;  
}

With this definition, a client would make a single `Subscribe` call, and the server would push `Notification` messages to the client whenever relevant events occur. The client processes these notifications as they arrive through the established stream.

# Hybrid Approaches: Combining Push and Pull

## WebSockets with Heartbeats

Many WebSocket implementations combine push communication with periodic heartbeats:

1. A persistent WebSocket connection enables server-to-client pushing
2. Periodic small heartbeat messages confirm the connection remains active
3. These heartbeats prevent intermediary devices from closing seemingly idle connections
4. They also allow both sides to detect connection failures promptly

## Smart Push with Client Feedback

Advanced push systems often incorporate client feedback mechanisms:

1. Clients establish push connections to receive real-time updates
2. Clients periodically send metrics about their processing capacity and current load
3. Servers adjust pushing rates based on this feedback
4. If clients become overwhelmed, they can request temporary rate limiting

# Conclusion: Choosing the Right Model for Your Use Case

For high-volume scenarios or applications with resource-constrained clients, a polling approach or a hybrid model might be more appropriate. Modern technologies like gRPC provide the flexibility to implement either approach or combinations of both to meet specific application requirements.

[

Design Patterns

](https://medium.com/tag/design-patterns?source=post_page-----15788789b74a---------------------------------------)

[

Networking

](https://medium.com/tag/networking?source=post_page-----15788789b74a---------------------------------------)

[

Rabbitmq

](https://medium.com/tag/rabbitmq?source=post_page-----15788789b74a---------------------------------------)

[

Programming

](https://medium.com/tag/programming?source=post_page-----15788789b74a---------------------------------------)

[

Coding

](https://medium.com/tag/coding?source=post_page-----15788789b74a---------------------------------------)

3

[

![Towards Dev](https://miro.medium.com/v2/resize:fill:96:96/1*c2OaLMtxURd1SJZOGHALWA.png)



](https://towardsdev.com/?source=post_page---post_publication_info--15788789b74a---------------------------------------)

[

## Published in Towards Dev

](https://towardsdev.com/?source=post_page---post_publication_info--15788789b74a---------------------------------------)

[6.8K Followers](https://towardsdev.com/followers?source=post_page---post_publication_info--15788789b74a---------------------------------------)

·[Last published 1 day ago](https://towardsdev.com/how-i-stopped-kubernetes-clusters-breaches-best-practices-for-2025-bef7fbfc0170?source=post_page---post_publication_info--15788789b74a---------------------------------------)

A publication for sharing projects, ideas, codes, and new theories.

Follow

[

![Mohamed-Ali](https://miro.medium.com/v2/resize:fill:96:96/1*GMF7Z-gIVY2Ac9vCH-lrwQ.png)



](https://medium.com/@moali314?source=post_page---post_author_info--15788789b74a---------------------------------------)

[

## Written by Mohamed-Ali

](https://medium.com/@moali314?source=post_page---post_author_info--15788789b74a---------------------------------------)

[2 Followers](https://medium.com/@moali314/followers?source=post_page---post_author_info--15788789b74a---------------------------------------)

·[1 Following](https://medium.com/@moali314/following?source=post_page---post_author_info--15788789b74a---------------------------------------)

[https://www.linkedin.com/in/mo-ali314/](https://www.linkedin.com/in/mo-ali314/)

Follow

## No responses yet

[](https://policy.medium.com/medium-rules-30e5502c4eb4?source=post_page---post_responses--15788789b74a---------------------------------------)

![ANJU SINGH](https://miro.medium.com/v2/resize:fill:64:64/2*juzrUrJletvEZGhezybJmg.jpeg)

ANJU SINGH

What are your thoughts?﻿

Cancel

Respond

Also publish to my profile

## More from Mohamed-Ali and Towards Dev

![Understanding Short Polling Model in Modern Web Architecture](https://miro.medium.com/v2/resize:fit:1358/1*yuVzMhCJyDENbyhwAsrkwA.png)

[

![Mohamed-Ali](https://miro.medium.com/v2/resize:fill:40:40/1*GMF7Z-gIVY2Ac9vCH-lrwQ.png)



](https://medium.com/@moali314?source=post_page---author_recirc--15788789b74a----0---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

Mohamed-Ali

](https://medium.com/@moali314?source=post_page---author_recirc--15788789b74a----0---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

## Understanding Short Polling Model in Modern Web Architecture

### In today’s web applications, handling time-consuming operations efficiently is crucial for a smooth user experience. This article explores…



](https://medium.com/@moali314/understanding-short-polling-model-in-modern-web-architecture-3cc65170249c?source=post_page---author_recirc--15788789b74a----0---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

Mar 11

[](https://medium.com/@moali314/understanding-short-polling-model-in-modern-web-architecture-3cc65170249c?source=post_page---author_recirc--15788789b74a----0---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

![This Is How I Scrape 99% of Websites (Step-by-Step Guide)](https://miro.medium.com/v2/resize:fit:1358/0*cPM8RhTgl9Lcv1r2)

[

![Towards Dev](https://miro.medium.com/v2/resize:fill:40:40/1*c2OaLMtxURd1SJZOGHALWA.png)



](https://towardsdev.com/?source=post_page---author_recirc--15788789b74a----1---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

In

[

Towards Dev

](https://towardsdev.com/?source=post_page---author_recirc--15788789b74a----1---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

by

[

Kevin Meneses González

](https://medium.com/@kevinmenesesgonzalez?source=post_page---author_recirc--15788789b74a----1---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

## This Is How I Scrape 99% of Websites (Step-by-Step Guide)

### “Without data, you’re just another person with an opinion.” — W. Edwards Deming



](https://towardsdev.com/this-is-how-i-scrape-99-of-websites-step-by-step-guide-40356d673d73?source=post_page---author_recirc--15788789b74a----1---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

Jan 15

[

588

8





](https://towardsdev.com/this-is-how-i-scrape-99-of-websites-step-by-step-guide-40356d673d73?source=post_page---author_recirc--15788789b74a----1---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

![Multi-Tenant Architecture using SpringBoot and PostgreSQL](https://miro.medium.com/v2/resize:fit:1358/1*LIaBuD3MeIeK6PiyCwHzzQ.png)

[

![Towards Dev](https://miro.medium.com/v2/resize:fill:40:40/1*c2OaLMtxURd1SJZOGHALWA.png)



](https://towardsdev.com/?source=post_page---author_recirc--15788789b74a----2---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

In

[

Towards Dev

](https://towardsdev.com/?source=post_page---author_recirc--15788789b74a----2---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

by

[

Prasad Patare

](https://medium.com/@prasadsp2001?source=post_page---author_recirc--15788789b74a----2---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

## Multi-Tenant Architecture using SpringBoot and PostgreSQL

### Multi-Tenancy in Spring Boot Using PostgreSQL (Schema-per-Tenant)



](https://towardsdev.com/multi-tenant-architecture-using-springboot-and-postgresql-d3d800e44ab0?source=post_page---author_recirc--15788789b74a----2---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

Mar 5

[

285

1





](https://towardsdev.com/multi-tenant-architecture-using-springboot-and-postgresql-d3d800e44ab0?source=post_page---author_recirc--15788789b74a----2---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

![Server-Sent Events: A Comprehensive Guide](https://miro.medium.com/v2/resize:fit:1358/1*yuVzMhCJyDENbyhwAsrkwA.png)

[

![Mohamed-Ali](https://miro.medium.com/v2/resize:fill:40:40/1*GMF7Z-gIVY2Ac9vCH-lrwQ.png)



](https://medium.com/@moali314?source=post_page---author_recirc--15788789b74a----3---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

Mohamed-Ali

](https://medium.com/@moali314?source=post_page---author_recirc--15788789b74a----3---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

## Server-Sent Events: A Comprehensive Guide

### The genius lies in how SSE leverages standard HTTP to create a streaming model: one request, but an infinitely long response. This approach…



](https://medium.com/@moali314/server-sent-events-a-comprehensive-guide-e4b15d147576?source=post_page---author_recirc--15788789b74a----3---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

Mar 12

[](https://medium.com/@moali314/server-sent-events-a-comprehensive-guide-e4b15d147576?source=post_page---author_recirc--15788789b74a----3---------------------9996ba72_0584_417f_b985_c8847f04536c--------------)

[

See all from Mohamed-Ali

](https://medium.com/@moali314?source=post_page---author_recirc--15788789b74a---------------------------------------)

[

See all from Towards Dev

](https://towardsdev.com/?source=post_page---author_recirc--15788789b74a---------------------------------------)

## Recommended from Medium

![Using RabbitMQ with .NET Core Web API and Worker Services](https://miro.medium.com/v2/resize:fit:1358/1*BTu50R8elucR39fQfqCq_A.png)

[

![DevOps.dev](https://miro.medium.com/v2/resize:fill:40:40/1*3SZtM9yhQyfh-dfWk5gGFw.jpeg)



](https://blog.devops.dev/?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

In

[

DevOps.dev

](https://blog.devops.dev/?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

by

[

Kevin Patrick Boylan | .NET Software Engineer

](https://medium.com/@kevinpatrickboylan?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

## Using RabbitMQ with .NET Core Web API and Worker Services

### I won’t dive into the Microservice vs. Monolithic question because it’s obviously a big and complex one and way beyond the scope of what…



](https://blog.devops.dev/using-rabbitmq-with-net-core-web-api-and-worker-services-15330c53cfb0?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

Feb 20

[

64





](https://blog.devops.dev/using-rabbitmq-with-net-core-web-api-and-worker-services-15330c53cfb0?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

![This new IDE from Google is an absolute game changer](https://miro.medium.com/v2/resize:fit:1358/1*f-1HQQng85tbA7kwgECqoQ.png)

[

![Coding Beauty](https://miro.medium.com/v2/resize:fill:40:40/1*ViyWUoh4zqx294no1eENxw.png)



](https://medium.com/coding-beauty?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

In

[

Coding Beauty

](https://medium.com/coding-beauty?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

by

[

Tari Ibaba

](https://medium.com/@tariibaba?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

## This new IDE from Google is an absolute game changer

### This new IDE from Google is seriously revolutionary.



](https://medium.com/coding-beauty/new-google-project-idx-fae1fdd079c7?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

Mar 12

[

2.4K

149





](https://medium.com/coding-beauty/new-google-project-idx-fae1fdd079c7?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

![How I Scaled a Go Backend to Handle 1 Million Requests per Second](https://miro.medium.com/v2/resize:fit:1358/0*7EolJQSw6lEqNsNh)

[

![Level Up Coding](https://miro.medium.com/v2/resize:fill:40:40/1*5D9oYBd58pyjMkV_5-zXXQ.jpeg)



](https://levelup.gitconnected.com/?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

In

[

Level Up Coding

](https://levelup.gitconnected.com/?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

by

[

Renaldi Purwanto

](https://renaldid.medium.com/?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

## How I Scaled a Go Backend to Handle 1 Million Requests per Second

### From 100 Requests to 1 Million: My Journey in Scaling a Go Backend



](https://levelup.gitconnected.com/how-i-scaled-a-go-backend-to-handle-1-million-requests-per-second-16156da33f45?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

Mar 19

[

262

8





](https://levelup.gitconnected.com/how-i-scaled-a-go-backend-to-handle-1-million-requests-per-second-16156da33f45?source=post_page---read_next_recirc--15788789b74a----0---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

![Java’s Funeral Has Been Announced….☠️💻](https://miro.medium.com/v2/resize:fit:1358/1*eLKPByQ0xSg_gjrbYziG6A.png)

[

![Javarevisited](https://miro.medium.com/v2/resize:fill:40:40/1*ceHirGi683U9Xn6tAlr0jQ.jpeg)



](https://medium.com/javarevisited?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

In

[

Javarevisited

](https://medium.com/javarevisited?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

by

[

Rasathurai Karan

](https://rasathuraikaran26.medium.com/?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

## Java’s Funeral Has Been Announced….☠️💻

### Oh, Java is outdated! Java is too verbose! No one uses Java anymore!



](https://medium.com/javarevisited/javas-funeral-has-been-announced-but-nobody-showed-up-bcfe0995a77b?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

Mar 7

[

2K

118





](https://medium.com/javarevisited/javas-funeral-has-been-announced-but-nobody-showed-up-bcfe0995a77b?source=post_page---read_next_recirc--15788789b74a----1---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

![5 Microservices Design Patterns You Must Know in 2025](https://miro.medium.com/v2/resize:fit:1358/1*yuVzMhCJyDENbyhwAsrkwA.png)

[

![JavaGuides](https://miro.medium.com/v2/resize:fill:40:40/1*9h3m7W2RvZi5zkydCxCgpg.png)



](https://medium.com/javaguides?source=post_page---read_next_recirc--15788789b74a----2---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

In

[

JavaGuides

](https://medium.com/javaguides?source=post_page---read_next_recirc--15788789b74a----2---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

by

[

Ramesh Fadatare

](https://rameshfadatare.medium.com/?source=post_page---read_next_recirc--15788789b74a----2---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

## 5 Microservices Design Patterns You Must Know in 2025

### Here are five important microservices design patterns you should know in 2025, explained in simple terms with examples. Microservices…



](https://medium.com/javaguides/5-microservices-design-patterns-you-must-know-in-2025-51d416540c4e?source=post_page---read_next_recirc--15788789b74a----2---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

Jan 24

[

473

10





](https://medium.com/javaguides/5-microservices-design-patterns-you-must-know-in-2025-51d416540c4e?source=post_page---read_next_recirc--15788789b74a----2---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

![Why the Industry is Moving Back to JDBC from JPA — This one will hurt a lot of developers.](https://miro.medium.com/v2/resize:fit:1358/0*A9RKXIAaCPIJRQgU)

[

![Full Stack Developer](https://miro.medium.com/v2/resize:fill:40:40/1*ANzDaum1wnT55zhf1sbLSg@2x.jpeg)



](https://medium.com/@ByteCodeBlogger?source=post_page---read_next_recirc--15788789b74a----3---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

Full Stack Developer

](https://medium.com/@ByteCodeBlogger?source=post_page---read_next_recirc--15788789b74a----3---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

## Why the Industry is Moving Back to JDBC from JPA — This one will hurt a lot of developers.

### I recently attended a Spring event where experts who work directly with Oliver Drotbohm — a key figure in the Spring ecosystem — discussed…



](https://medium.com/@ByteCodeBlogger/why-the-industry-is-moving-back-to-jdbc-from-jpa-this-one-will-hurt-a-lot-of-developers-6556589b7cc5?source=post_page---read_next_recirc--15788789b74a----3---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

Mar 14

[

581

49





](https://medium.com/@ByteCodeBlogger/why-the-industry-is-moving-back-to-jdbc-from-jpa-this-one-will-hurt-a-lot-of-developers-6556589b7cc5?source=post_page---read_next_recirc--15788789b74a----3---------------------3da66595_df51_40e6_b42d_984dfe431b6e--------------)

[

See more recommendations

](https://medium.com/?source=post_page---read_next_recirc--15788789b74a---------------------------------------)

[

Help

](https://help.medium.com/hc/en-us?source=post_page-----15788789b74a---------------------------------------)

[

Status

](https://medium.statuspage.io/?source=post_page-----15788789b74a---------------------------------------)

[

About

](https://medium.com/about?autoplay=1&source=post_page-----15788789b74a---------------------------------------)

[

Careers

](https://medium.com/jobs-at-medium/work-at-medium-959d1a85284e?source=post_page-----15788789b74a---------------------------------------)

[

Press

](mailto:pressinquiries@medium.com)

[

Blog

](https://blog.medium.com/?source=post_page-----15788789b74a---------------------------------------)

[

Privacy

](https://policy.medium.com/medium-privacy-policy-f03bf92035c9?source=post_page-----15788789b74a---------------------------------------)

[

Terms

](https://policy.medium.com/medium-terms-of-service-9db0094a1e0f?source=post_page-----15788789b74a---------------------------------------)

[

Text to speech

](https://speechify.com/medium?source=post_page-----15788789b74a---------------------------------------)

[

Teams

](https://medium.com/business?source=post_page-----15788789b74a---------------------------------------)