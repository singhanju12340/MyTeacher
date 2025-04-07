---
Creation Time: Monday, December 2nd 2024
Modified Time: Thursday, March 27th 2025
---



### Functional requirements:
1. Group chat with 100/10s of max user
2. Send message 
3. Receive message in real time
4. User should be able to receive old message when he comes back online (upto 30 days) or login to other device
5. Allow users to send audio, video and documents with a limit on size and count
6. Send push notifications to offline user for new notification, if user is connected
7. Access message after I am offline


**Additional Functional**
 Delete sent message 
 Show message delivery status
 Show user offline/ last seen status
Audio/ Video call

### Non Functional Requirements
1. **Scalability**: Handle Millions of concurrent users
2. **Availability**:  Ensure no downtime,  system should be resilient.
3. **Reliability**: Data should not be lost in case of system failure.
4. **No Latency**: Deliver messages in real time. <500ms
5.  Messages should be stored on centralized servers no longer than necessary.
6. The system should be resilient against failures of individual components.
7. Message sent should not be lost
8. Handle Security issues, detect spam, block spam



### Entity and Preferred storage and keys
```
User (can be in mysql)
	id (partition key)
	name
	username
	password(hash)
	email
	profilePicUrl

1B users, data is hugewe need to partition. user userId. less read and less write



Chats/groups
	id,
	name
	metadata


ChatParticipants
	id, 
	userId, (index and partition by user id)
	chatId
	joinedDate

(max 100/10 users for 1 chatId, user can be participate in multiple chats)
This data should be handled via single leader, to avoid conflict in join/remove partitcipents status if not synced properly in multile leader databse. preferred is Mysql.
Partition by userID will ensure all my chat list will be on 1 single database, so quering will be fast to render list of chats I am currently into.

If there are too much write(frequent joining in new chats by user), we can use leaderless write, but there can be  consistency issue with leader less db, we need some extra handling ex: when both join and remove has occured ignore join operation.



Message
	senderId
	chatId(partition based on chatid)
	content
	timestamp(sort based on time, store server side timestamp).
	metadata (for analytics)

(for book keeping and delivering message for offline users later).
 pariting over chatId and sorting over timestamp column
  order message on all the users device should be in same order, better to use server side timestamp

more read or more write? 
> More write
> 	Cassandra
> More Read
> 	LSM tree index database, colulmn oriented database like HBase. Hbase uses single leader write.

Inbox
	messageId
	clientIds 
(all the devices for a userid/participents belonging to the chat)
( create entry for all the devices for a userid/participents belonging to the chat for retrying and sending message to offline users later, entry will be deleted once message is delivered)
	




	

UserDevices(max 5 for 1 userId)
	userid
	client: // phone, laptop

Group
 list<users>



```


### Connection patterns for real time updates
`What connection protocols to use??
	Http request and response? `No` because server will not be able to send message to client received from other client.

[[Communication Design Patterns]]


##### Commands Sent
createChat
sendMessage
createAttachment
modifyParticipants

##### Commands Received
newMessage
chatUpdate


HLD
[[chat service HLD]]

One user can have multiple client Id, as we need to store user to client mapping and keep tracking in inbox and message table w.r.t to client id, instead of userId.


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

`Show user in the group chat online??
Query table which keeps connection data.  when user connects we need to keep details of userId and status. whenever it disconnects it will set status to offline.
	
### Capacity estimates do it later/end: 
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


	











