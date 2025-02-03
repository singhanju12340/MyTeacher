---
Creation Time: Monday, July 29th 2024
Modified Time: Tuesday, January 21st 2025
---
In memory, Single threaded open source cache written in 'C'.

**Data structure:**
Hash, List, Set, sorted set, bitmap, hyperlog log, geospatial index, streams 

Application can be supported:
Gaming leaderboard
message buffer
auth session store
realtime analytics


_How much data redis single node can store: 
- **Available Physical Memory (RAM)** on the server where Redis is running.
- **Redis Internal Data Structure Overheads**, which vary depending on the data types and structures used.
-**For most 64-bit systems**, Redis can address up to **4 terabytes (TB)** of RAM, as long as the system itself supports that much physical memory.

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



## Capabilities and Uses cases of Redis
### Redis as a Cache
### Redis as a Distributed Lock
### Redis for Leaderboards
### Redis for Rate Limiting
### Redis for Proximity Search




# Redis rs Memcache

### Memcache
1. uses consistent hasing to provide dynamic partitioning support
2. Multithreaded
3. Uses LRU eviction  can be archived by linkedHashMap and Hashmap
4. Multileader/leader less
5. More flexible less convinence

### Redis
1. Single threaded help in achieving ACID properties
2. No variable  partitioning
3. write ahead log, allows us to do atomic transactions
4. Single leader application

