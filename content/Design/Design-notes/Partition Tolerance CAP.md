---
Creation Time: Tuesday, February 11th 2025
Modified Time: Tuesday, February 11th 2025
---
Partition tolerance means that a distributed system continues to function correctly even if there is a network partition—that is, if the network splits into disjoint segments that cannot communicate with each other. In the presence of such failures, a partition-tolerant system is designed to maintain its operations, even if it must sacrifice either strict consistency or complete availability.

`**Example:**  
Imagine a distributed online shopping database replicated across multiple data centers. If a network partition occurs, one data center might become isolated from the other. The system must then decide:

- _**If it favors consistency:** It might refuse to process writes in the isolated partition until it can ensure that all replicas are in sync. This preserves data correctness but sacrifices availability.
- _**If it favors availability:** It might continue processing writes in the isolated partition, which could later result in conflicting data between partitions until reconciliation occurs. This keeps the service up but may lead to temporary inconsistency.

`Could you provide a real-world example that illustrates partition tolerance?"

**Interviewee:**  
"Sure. Consider a distributed database like Cassandra. Imagine it is deployed across multiple data centers. If a network failure causes one data center to become isolated from the others, Cassandra must decide how to handle client requests. In such a scenario, it might allow the isolated nodes to continue processing read and write operations to maintain availability, even though this could mean that the data is not perfectly consistent across all nodes until the network issue is resolved. This trade-off is a practical example of partition tolerance in action."


`how does databases sync data after network partition is resolved`
When a network partition is resolved in a distributed database, the system must reconcile the differences between the divergent replicas to bring them back into a consistent state.

	anti-entropy protocols
Merkle trees_  where each replica computes a hash tree of its data and exchanges it with others to detect discrepancies
	_Full Data Synchronization:_ where the data volume is small, replicas may simply exchange and compare entire datasets to identify and fix differences.
	
	`Conflict Resolution Strategies
Last Write Wins_: 
	_Application-Specific Logic:
	_Vector Clocks [[Vector Clock]]_
	
	`Hinted Handoff`
Storing Hints_: During a network partition, if one replica is unreachable, the coordinator node may store a “hint” indicating that a particular update was missed

`In case of conflict in Last write Wins`
tie breaker will accept on update and reject other. but we want updates from different nodes to be streamlined and updated at both the node. because I do not want to loose the data.

Using `Conflict-Free Replicated Data Types (CRDTs)` They allow distributed systems to update replicated data independently and ensure that all replicas converge to the same state eventually

