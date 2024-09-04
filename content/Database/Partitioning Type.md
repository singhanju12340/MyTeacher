---
Creation Time: Thursday, August 8th 2024
Modified Time: Thursday, August 8th 2024
---
[[Screenshot 2024-08-08 at 2.02.56 PM.png | Picture representation of partitioning types]]

## 1. Partition of Key Value Data
	Allocating data randomly to avoid hostspot is randomly distributing data on nodes, but the challange with this approach is to find on which node the data was saved.To to query we need to all the nodes in parrel to find the data.
	To solve this issue we can store key-value data store which will keep keys in sorted order and value will be detail of the node on which data is saved and key will be primary key.

## 2. Partitioning by Key Range:
Example: Store words and decide partitions based on starting alphabets
A-B:  anju, apple, ant
B-C: 
C-D: 
.
.
.
T-Z

we know T to Z will have less word counts, so we can add all in single partition.

## 3. Partitioning by Hash of Key
To avoid the risk of hotspot and skew data partitioning we can use Hash function to decide the node on which data needs to be saved. 
if we take 32 bit hash function every time it will generate random values between  0 to 2^32 -1. 
String cryptography function needs to be used.
	Example: 
	Cassandra and MongoDB use MD5. 
	Valdemort uses Fowler-Noll-Vo function.
Partitioning based on hash key looses the advantage of efficient range query of Key-Range partitioning.
	
1. **In MongoDB,** if you have enabled hash-based sharding mode, any range query has to be sent to all partitions.
2. Range queries on the primary key are not sup‐ ported by **Riak**, **Couchbase**, or **Voldemort** databases.
3. Cassandra though achieve some level of efficient range query by having two partition strategies. Tables in Cassandra has compound primary key made by combining several columns. First part of the key determines parititons and other part can be used to do range based query within the partition.
		{ Partition Key | Composite Key }
		ex: {userId, user action timestamp}. can query all user activities for given usserId by filtering baased on time range.
	
		



	

