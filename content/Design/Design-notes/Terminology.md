---
Creation Time: Monday, September 23rd 2024
Modified Time: Monday, September 23rd 2024
---
- **Load Shedding**: Discarding excess traffic.
- **Rate Limiting**: Limiting the rate of requests by users.
- **Failed Requests Handling**: Strategies like retries or moving to dead-letter queues.
- **Backpressure**: Signal to the producer to slow down.
- **Elastic Scaling**: Dynamically adjusting resources based on demand.


#### . **Host Discovery**:

- **DNS**: The Domain Name System translates domain names to IP addresses.
- **Anycast**: Routing strategy where a single destination address has multiple routing paths to two or more endpoint destinations.


#### . **Service Discovery**:

- **Server‑side and Client-side Discovery Patterns**: Mechanisms where services find each other's network locations.
- **Service Registry**: A database containing the network locations of service instances.

#### . **Peer Discovery**:

- **Peer Discovery Options**: How nodes in a network find each other.
- **Gossip Protocol**: Nodes randomly exchange information, which gradually propagates through the system.


#### **Choosing a Network Protocol**:

- **TCP, UDP, and HTTP**: Decision depends on use case requirements like reliability, speed, and connection state.


#### . **Push and Pull Technologies**:

- **Short Polling**: Client frequently asks the server for new data.
- **Long Polling**: Client requests information and waits for the server to respond.
- **Websocket**: Protocol that allows two-way communication with a server over a single, long-lived connection.
- **Server-Sent Events**: Server pushes updates to the client over an HTTP connection.\

#### **Large-scale Push Architectures**:

- **C10K and C10M Problems**: Challenges of handling 10,000 and 10 million concurrent connections respectively.
- **Large-Scale Push Architectures**: Techniques like event-driven programming to handle large numbers of simultaneous connections.
- **Problems of Long-Lived Connections**: Resource management, detecting stale or "dead" connections, and handling dropped connections.

