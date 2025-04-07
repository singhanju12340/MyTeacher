---
Creation Time: Saturday, February 22nd 2025
Modified Time: Thursday, February 27th 2025
---
This approach is defined for `distributed systems`.
It typically refers to techniques that enforce a strict, sequential order of operations on shared resources or data.in order to prevent concurrent modifications from conflicting with one another.
but it comes at the cost of increased waiting times and potential performance bottlenecks when many threads or transactions contend for the same resource.


_Serialization in Concurrency/ sequential execution of commands to insure concurrency:_ In concurrent systems, serialization ensures that operations on a shared resource are executed one at a time rather than concurrently. via
1. Locks
2. Transactional Isolation Levels: In databases, the **Serializable** isolation level guarantees that transactions execute as if they were run sequentially.
3. Orchestrator / Order service: Distributed systems may use a centralized ordering service (or consensus algorithm) to ensure that operations are applied in a specific order.

_Contention/too much traffic_ Contention occurs when multiple threads or transactions try to access the same resource concurrently. If serialized (i.e., executed sequentially), contention can result in significant waiting times as each operation must wait for the previous one to finish.



_**How can we mitigate contention issue due to serialized data update in distributed databases?

1. Using optimistic locking patterns and resolving conflicts if occurs.
2. Using lock at very fine granular level to block only smaller section of execution
3. Partition database.
`How can Partitioning database help in reducing contention?
Ans : Partitioning can help solve contention issues during serialized database updates by dividing the workload into smaller, independent segments, allowing updates to occur concurrently on different partitions rather than serializing all updates on a single data store.
- Instead of locking a single table or dataset for every update, partitioning splits the data into segments (e.g., by range, hash, or key).
- Each partition is updated independently. Thus, updates targeting different partitions can proceed in parallel without interfering with one another. 
- With dataset partitioning locks are applied at a more granular level.
_Example:
-if you partition a table by customer ID, updates to one customer's record won't block updates to another's.
- This minimizes the overall time a lock is held and reduces waiting times.

### Example Scenario

Imagine an airline booking system that maintains a single table for seat reservations. Without partitioning, every booking update might lock the entire table (or a large portion of it) to ensure consistency. Under heavy load, this leads to significant contention.

**With Partitioning:**

- You could partition the reservations table by flight number or date.
- Updates for different flights (or even different dates) occur in separate partitions.
- As a result, while updates for the same flight might still be serialized, updates for different flights can proceed in parallel.