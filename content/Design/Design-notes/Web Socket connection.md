---
Creation Time: Wednesday, February 5th 2025
Modified Time: Wednesday, February 5th 2025
---
Web Socket is a 2 way messaging protocol between server and client. where client opens a collection to a server and keeps it open it timeout.

Server sends the new data on the same connect established before instead of creating new connection every time.

When a new comment arrives, the Comment Management Server distributes it to all clients, enabling them to update the comment feed. This is much more efficient as it eliminates polling and enables the server to immediately push new comments to the client upon creation.

###### Challenges

Web sockets are a good solution, and for real-time chat applications that have a more balanced read/write ratio, they are optimal. However, for our use case, the read/write ratio is not balanced. Comment creation is a relatively infrequent event, so while most viewers will never post a comment, they will be viewing/reading all comments.

Because of this imbalance, it doesn't make sense to open a two-way communication channel for each viewer, given that the overhead of maintaining the connection is high.

```plantuml
Client -> Server : Estabilish websocket connect
Server --> Client : Connection estabilished
Note over Server : Keep Connection Open

group 
	loop [ New message posted]
		Server --> Client : Send new messages
	end	

Note over Client : Process new data immediatly
```