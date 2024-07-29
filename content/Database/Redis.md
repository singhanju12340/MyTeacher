In memory open source cache

**Data structure:**
Hash, List, Set, sorted set, bitmap, hyperlog log, geospatial index, streams 

Application can be supported:
Gaming leaderboard
message buffer
auth session store
realtime analytics

## Advantages:
#### Single Command on Redis is atomic: 
if one command is running, no other command can be executed in between. Concurrency is not an issue for redis. and also there is no context switch as only single atomic command is executed.

#### Data is stored as in memory:
It also provide  configurable persistence storage in memory
#### Command executed is saved in log files always

#### Transition 

#### Pub/Sub

#### TTL 

#### LRU eviction


#### How Redis maintains these many connection??
I/O multiplexing

![[Screenshot 2024-07-29 at 9.05.30 PM.png]]

Redis need to perform 2 actions:
1. Read from TCP connections 
2. Perform in memory operations for the commands received over TCP connections.

Redis takes advantage of read system calls and listen to only TCP socket connection which has some data to be read. this way it is able to maintain too many connections.

Also Redis perform in memory operations too quick because redis keeps data in memory, with the single thread redis is able to perform quick action using advantage of in memeory and read only when data available feature

