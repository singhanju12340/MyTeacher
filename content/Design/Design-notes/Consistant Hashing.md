---
Creation Time: Monday, June 24th 2024
Modified Time: Wednesday, January 15th 2025
prefix sum:
---
Its an algorithm/function/logic to identify the _data node / service which will handle this request_  based on the request key using Hash functions.
This logic has to be added in load balancer or request manager .

_Ex uses:
**content delivery networks (CDNs)**, consistent hashing is used to distribute content across multiple servers. This ensures that requests for the same content are served by the same server, reducing latency and improving performance (In a content delivery network, consistent hashing is used to map the content to the closest server to reduce the latency).

_In **databases**, consistent hashing is used to shard data across multiple nodes. This allows for horizontal scaling and improves performance by reducing the load on individual nodes.

_In **caching systems**, consistent hashing is used to distribute cache entries across multiple servers. In a distributed caching system, consistent hashing is used to map the cached data to the closest cache server on the ring. This improves cache hit rates and reduces the load on individual servers

### Implementation: 
1.  key-> hashFun(key1) -> value0
each entry contains{ Starting range of hash value, server address}
{ {0, server1}, {10, server2}, {20, server3}, {30, server4} }  
linked list: size will be the number of servers.
i.e liner search

_**How does consistence hashing handle adding new node/server or removal of node/service if server or node is Statefull?

Current Keys: K1, K2, K3, K4, K5, K6
Total Nodes = N0, N1, N2

Hash (K1.key) = K1 % 3 = 2 
Hash (K2.key) = K1 % 3 = 0
Hash (K3.key) = K1 % 3 = 1
Hash (K4.key) = K1 % 3 = 2
Hash (K5.key) = K1 % 3 = 1
Hash (K6.key) = K1 % 3 = 0

_What if Node 2 goes down._ then there is only 2 nodes.
Hash (K1.key) = K1 % 2 = 0 
Hash (K2.key) = K1 % 2 = 1
Hash (K3.key) = K1 % 2 = 1
Hash (K4.key) = K1 % 2 = 0
Hash (K5.key) = K1 % 2 = 1
Hash (K6.key) = K1 % 2 = 0

_All the data from Node 2 needs to be moved to 0 and 1, and every things needs to be rebalanced.
Normal Hash based routing will be overhead in case of node changes. `Consistence hashing will give a function which will help in minimising minimal data movement in case of node changes.

HashFunction(SHA 128): range of node position can be  0 to 2^128

_Consistence hashing:_ Every node occupy a slot in the ring(Simple array places in order)
[ {Slot3-> Node 0}, {Slot 8-> Node2}, {Slot12->Node1}] are array of hash fun result values

HashFun( K1- IP of node 0) = 3 -> Node 0 is put on slot 3
HashFun( K2- IP of node 2) = 8 -> Node 2 is put on slot 8


_Example:
HashFun( K1) = slot 0 -> Node immediate right of 0 will keep data for K1.

HashFun( K2) = slot 10 -> Node immediate right of Slot 10, i.e slot 12(Node 1) will keep data for K2 .

_Ex: Add Node 3
HashFun( K3- IP of node 3) = 1 -> Node 3 is put on slot 1_
 All the keys After slot 12 used to go to Slot 3, i.e Node 0
 But now  all will go to Slot 0, i.e node 3
 [{Slot0->Node 3 }, {Slot3-> Node 0}, {Slot 8-> Node2}, {Slot12->Node1}]
So only node between slot 12 to slot 0 will get affected


_Ex: Delete Node 0
 All the keys After slot 0 to slot 3 used to go to Slot 3, i.e Node 0
 But now  all will go to Slot 8, i.e node 2
 [{Slot0->Node 3 }, {Slot 8-> Node2}, {Slot12->Node1}]
So only node between slot 0 to slot 3 will get affected and moved to Node 2




## Issues
is it expensive?
1. Memory: No
2. Time: No


_Check machine coding for the same_
To implement consistent hashing in Java, we’ll use the `TreeMap` class to represent the hash ring. The `TreeMap` class is a sorted map that maps the hash values to server names. We'll use the `MD5` hash function to generate the hash values, which is a widely-used hash function that produces a 128-bit hash value. To add a server to the ring, we'll generate multiple virtual nodes for that server and add them to the ring. We’ll also use a hash function to generate the name of the virtual node by appending a unique identifier to the server name.