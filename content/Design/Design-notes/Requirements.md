---
Creation Time: Monday, June 24th 2024
Modified Time: Monday, September 23rd 2024
---
1. Functional
	1. 
2. NFR
 ### .**Performance**:
	 - **Latency**: Time taken to process a single operation.
	 - Number of operations processed in a given time period.
	 - 95th percentile latency = under which 95% of the request latencies fall
	 - **How to Increase**: Optimize algorithms, caching, distributed systems
### . **Availability**
	- **Time-based**: E.g., 99.9% (or "three nines") uptime.
	- **Count-based**: E.g., 1 failure allowed in 1 million requests.
	- **Design Principles**: Redundancy, failover, replication, monitoring, and regular testing.
	- **Processes**: Regular maintenance, backup procedures, disaster recovery planning.
	- **SLO vs SLA**: Service Level Objective (SLO) is the target level of service. Service Level Agreement (SLA) is the contract with the customer promising certain SLOs.

#### . **Scalability**:
	Talk about the data and no of request capacity estimates
- **Vertical Scaling**: Increasing the resources of a single node.
- **Horizontal Scaling**: Adding more nodes.
- **Elasticity vs Scalability**: Elasticity is the ability to scale automatically based on demand.

### . **Durability**:
	Ensuring data is safely stored and retrievable.
	-Talk about data backup
	- Talk about Replication
	- Talk about checksum for stored data
	- RAID: reduse redundant storage

### . **Consistency**:
	Eventual Consistency
	Linearizability: Real-time consistency.
	Monotonic Reads: If a process reads the value of a data item, any successive reads by that process will always return that value or a more recent one
	Read-Your-Writes: Guarantees that a write by a process is visible to a subsequent read by the same process
	
	
	
	


