---
Creation Time: Monday, November 18th 2024
Modified Time: Monday, November 18th 2024
---
Concurrency conflicts occur when multiple processes or threads access and modify shared resources simultaneously.
To ensure data integrity and system reliability, effective conflict resolution strategies are essential.


_There are 4 ways of concurrency control_
 `Pessimistic Concurrency Control:`
	 **Shared Lock:** multiple concurrent reads are allowed
	 **Exclusive Lock for write:** Single write is allowed once lock is applied, rest other transaction are discarded.
	 _**Disavantage:** Can lead to low consistency and impact performance in case of high traffic.
 `Optmistic Concurrency Control`
	 **Read-Modify-Write:** Transactions read data, modify it, and then write it back.
	 **Conflict Detection:** Before committing a transaction, the system checks for conflicts with other transactions.
	 **Conflict Resolution:** If a conflict is detected, the transaction is aborted and retried.
	 
   _**Advantages:** Higher concurrency and better performance than pessimistic concurrency.
 **Disadvantages:** Requires careful implementation to ensure correct conflict detection and resolution.
	
 `Time Based concurrency control:`
	 **Timestamps:** Each transaction is assigned a timestamp.
	 **Conflict Detection:** Transactions are ordered based on their timestamps.
	 **Conflict Resolution:** Transactions with earlier timestamps are prioritized.
_**Advantages:** Simple and efficient.
 **Disadvantages:** Can lead to anomalies if timestamps are not strictly ordered.
 `Optimistic concurrency control with Version number`
 - **Version Numbers:** Each data item is assigned a version number.
- **Conflict Detection:** When a transaction reads a data item, it records the version number.
- **Conflict Resolution:** If the version number has changed when the transaction tries to write the data, a conflict is detected.
_**Advantages:** More flexible than timestamp-based concurrency control.
 _**Disadvantages:** Requires additional overhead for version number management.


#### ### Conflict Resolution Strategies
Right strategy depends on the `transactions frequency, data consistency requirements, system performance needs and complexity.
1. **Retry:** The transaction is simply retried until it succeeds.
2. **Rollback:** The conflicting transaction is aborted, and the user may need to retry the operation.
3. **Merge:** The changes from both conflicting transactions are merged into a single result. This requires careful consideration of the specific data and the desired outcome.
4. **Arbitration:** A third-party decides which transaction wins the conflict.
5. **User Intervention:** The user is notified of the conflict and asked to resolve it manually.

 
 
 _Concurrent transaction handling in E-Cart system

_**Optimistic Concurrency Control with Versioning:
- Assign a version number to each item in the inventory.
- When a user adds an item to their cart, record the current version number.
- During checkout, compare the current version number with the stored version.
- If the version numbers match, proceed with the checkout.
- If the version numbers don't match, it indicates that the item's inventory has been modified by another user, and the transaction is rolled back.

_**Distributed Locking:_
- Use a distributed locking mechanism (e.g., Redis, ZooKeeper) to acquire a lock on the item before processing the checkout.
- If the lock is acquired successfully, the user can proceed with the checkout.
- If the lock is already held by another user, the current user's request is queued or rejected.

_**Queue-Based System:**_
- When a user adds an item to their cart, place an order in a queue.
- A separate worker process processes orders from the queue.
- If an item is out of stock, the order is rejected, and the user is notified.
- This approach can help manage high traffic and prevent overselling.


 