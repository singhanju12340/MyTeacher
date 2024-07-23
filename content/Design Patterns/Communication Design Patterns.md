
## 1 . Request-Response Pattern

The request-response pattern is a fundamental building block for how the front-end and back-end of web applications chat with each other

### Benefits of the Request-Response pattern
	**Ease of Implementation and Simplicity**
	**Flexibility and Adaptability**:  You can use it for making API calls, rendering web pages on the server, fetching data from databases, and more
	
	**Scalability**:  Each request from the client is handled individually, so the server can easily manage multiple requests at once.
	
	**Reliability**: Since the server always sends back a response, the client can be sure its request is received and processed
	
	**Ease of Debugging**:  If something goes wrong, the server kindly sends an error message with a status code stating what happened

### Limitations of the Request-Response Pattern
	**Latency Problem**: Because it’s a back-and-forth conversation, there’s often a waiting periodThis amounts to idle periods and amplifies latency, especially when the request requires the server to perform time-consuming computing tasks
	
	**Data Inconsistency in Case of Failures**:  If a failure occurs after the server has processed the request but before the response is delivered to the client, data inconsistency may result
	
	**Complexity in Real-Time Communication**: like live streaming, gaming, or chat apps), this pattern can introduce delays
	
	**Inefficiency in Broadcasting**: It’s like mailing individual letters instead of sending one group message.




## 2. The Publish/Subscribe Pattern

### Benefits of the Publish/Subscribe pattern
	**Asynchronous Communication**
	**Loose Coupling of Components**
	**Highly Scalable**:  There is no limit to the number of subscribers a publisher can publish events to
	**Independent of Language and Protocol**: The Pub/Sub model can be easily integrated into any tech stack because it is language-agnostic
	**Load Balancing**: 

### Limitations of the Publish/Subscribe pattern
	**Complexity in Implementation:**
	**Message Duplication:** Depending on the configuration and network issues, messages can be duplicated. Subscribers might receive the same message more than once
	
	**Scalability Challenges:**  While Pub/Sub is highly scalable, managing extremely high volumes of messages and subscribers can become complex. You might need to consider how to distribute messages efficiently and handle massive numbers of subscribers.
	
	**Complex Error Handling:**

### When should you use it?
	
	- In building features that require real-time and low latency responsiveness for the end-users, for example live chat or gaming applications for multiple players.
	- In event notification systems
	- In building distributed systems that rely on logging and caching
	


## 3. The Short Polling Pattern

 It uses the **pull-based** communication mechanism (which is basically the client pulling data from the backend) to continuously poll the server for new updates

### Benefits of Polling

	**Simplicity:** Polling is easy to understand and implement and it’s completely stateless between the client and server. This makes it perfect for scenarios where the complexities need to be minimized.
	
	**Compatibility:** It can be used with a wide range of technologies and protocols which makes it highly compatible with a number of platforms and environments.
	
### Limitations of Polling

	**Latency:** Polling introduces latency, as clients must wait for predefined intervals before receiving updates. This can lead to delays in accessing real-time data or receiving notifications.
	
	**Inefficiency:** Constantly polling the server for updates can be inefficient and can result in unnecessary network and server overhead.
	
	**Scaling:** Handling a large number of simultaneous clients using polling can be resource-intensive for the server. It may require significant server resources to manage numerous concurrent polling requests.




## 4. The Long Polling Pattern

Long polling is like polling but uses a **push-based** communication mechanism. In long polling, instead of the client asking the server, “Any updates?” all the time, it says, “Let me know when something’s up.”

### Benefits of the Long Polling Pattern

	**Low Latency:** Long polling provides low latency when compared to traditional polling as data is immediately sent back to clients as soon as they are available.
	
	**Real-Time Updates:** With the long polling technique, applications can achieve real-time updates without the need for constantly polling the server.

### Limitations of the Long Polling Pattern

	**Resource Intensive:** Long polling requires keeping many connections open. As a result, it can be resource-consuming on both the server and client side.
	
	**Increased Latency:** Although long polling helps cut down on frequent polling, it can still introduce latency when compared to other real-time communication protocols like Web Socket. Clients may experience delays between updates since they have to wait for the server to respond.
	
	**Difficult to Scale:** When dealing with a large number of concurrent clients, long polling can strain server resources. As more clients establish long-polling connections, the server may struggle to manage and respond to all these connections efficiently.
	

## 5. The Push Pattern

	Push is a communication model that is used to deliver real-time updates to connected clients.
	
	In this model, the client opens a connection to the server and awaits messages or updates from the server. Whenever there is a new update or message, the server immediately **pushes** that update to the client without the client explicitly requesting it — as long it is connected.
	
	This model allows for bidirectional communication between the client and server. Web sockets, a popular protocol, uses the push model as its underlying data exchange method.
	
	The Push model provides the most real-time or near real-time end-user experience when compared to other closely related paradigms such as polling and long polling.

###   Benefits of Push Pattern

	**Real-Time Updates:** The push model allows clients to receive updates from the server as soon as they are available. This is key, especially in applications where real-time updates are crucial.
	
	**Reduced Latency:** Since the server pushes updates to the client as soon as they are available, it potentially reduces the latency.
	
	**Efficiency:** Because there is no need for continuous polling or frequent client requests, there is efficient use of network resources and reduced server load.
	

### Limitations of Push Pattern

	**Scalability:** It can become difficult to scale as the number of connected clients increases. At this point, it becomes resource-intensive, especially on the server side since the server needs to maintain open connections with multiple clients.
	
	**Client support:** Some clients might not be able to handle pushed messages as not all client platforms support push technologies. This may lead to compatibility issues and may need some sort of fallback mechanism for unsupported clients.