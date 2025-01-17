---
Creation Time: Monday, December 2nd 2024
Modified Time: Wednesday, December 4th 2024
---



### Functional requirements:
1. Support 1:1 real-time communication
2. Show user offline/ last seen status
3. Group chat with 100s of max user
4. Can retrieve the older message
5. delete sent message 
6. Allow users to send audio, video and documents with a limit on size and count
7. Show message delivery status
8. Send push notifications to offline user for new notification

### Non Functional Requirements
1. **Scalability**: Handle Millions of concurrent users
2. **Availability**:  Ensure no downtime,  system should be resilient.
3. **Reliability**: Data should not be lost in case of system failure.
4. **No Latency**: Deliver messages in real time.


### Capacity estimates: 
1 billion registered user
500 M active user per day
50M max user connected in peak time.
500M * 10 message each user per day = 5B  message daily


`Storage for message
Average Message size: 1KB
per day : 5B * 1KB = 5TB
per year: 5 TB * 365  = 18000TB ~ 1.8 PB

`Network bandwidth estimations
assume 10 mes/ sec
data to be transferred: 10 KB /Sec 
Assume total 10 M active user
10KB * 10M = 100M = 100 GB/ Sec data to be transferred 


HLD
[[chat service HLD]]

Working:
1. One to One messaging
	1. User sends a message first time  chat server checks the connection cash to find User B chat server, if its present gets WS connection details for User B. Else create a new WS connection and save the same in the cache via write-through cache strategy.
	2. Once a connection is established users' initial Http connections are transformed to 2 way WS connection and communication starts.
	3. All messages are sent to the Message service to store them on DB for history and data persistence.
	4. Session persistence: Load balancer can use sticky session to keep connection alive from the same chat server to avoid delay. we can also use service discovery to find chat servers and establish a connection based on the algorithms like round robin or lease connection server

 Online/ Offline and Last Seen:

userId|serverId|Last Active time
	-- | -- | --
	101 | ser1 | 20-11-2024:08:08:03
	102 | ser2 | 20-11-2024:09:08:03

The last active time can be used to determine the last seen and User online status. if last seen time is older than current time, User status can be set to Offline. the client can send heart heartbeat periodically to update last active time.


	
	
	











