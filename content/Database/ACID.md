---
Creation Time: Monday, August 12th 2024
Modified Time: Friday, September 20th 2024
---
### Atomicity: 
Single transaction is treated as one operation, either all the steps of a transaction are committed or none of them are applied. 

Example: Money transfer between two bank, both credit and debit must be completed, if any one fails other must roll out. 


### Consistency: 
	``In terms of database consistency is correctness. Data should never loose its correctness. 
	Database systems allows us to define rules which every transaction must adhere. 
	Some example rules :
1. Balance of an amount should never be negative.
2. No orphan mapping: there should not be any mapping of a person in DB whose entry from db is deleted.
3. No orphan comments: there should not be any comment in the database that does not belong to any existing blog.
	``these rules are defined on DB using Constraints, Cascades and triggers. ex: foreign Key, con delete casades, on update casades.
Even if thousand of concurrent request are getting executed on DB, Database Engine insures that data in the DB should always be in correct state.

### Isolation: 
	Changes done by concurent transaction in DB must not affect other transaction. A simple analogy is how we have to make our data structures and variables thread-safe in a multi-threaded (concurrent) environment. 
	 similar to how we use Mutex and Semaphores to protect variables, the database uses locks (shared and exclusive) to protect transactions from one another.
Ex: If hospital has 500 beds, max 500 patient should be allowed to book the beds
``A transaction before altering any row takes a lock (shared or exclusive) on that row, disallowing any other transaction to act on it. The other transactions might have to wait until the first one either commits or rollbacks.The granularity and the scope of locking depend on the isolation level configured.
Every database engine supports multiple Isolation levels, which determines how stringent the locking is. The 4 isolation levels are

- ##### Serializable: 
-The highest level of isolation, where transactions are executed in such a way that the result is as if the transactions were executed sequentially. 
- ##### Repeatable reads: 
-Once a transaction reads data, it will see the same data throughout the transaction. 
- ##### Read committed: 
-A transaction can only see changes that have been committed by other transactions. If a row is read twice in the same transaction, the values may change because another transaction committed a change between the two reads
- ##### Read uncommitted:
-Transactions can see changes made by other transactions before those changes are committed. Dirty read issue

### Durability


- Ensures that once a transaction is committed, The changes made by the transaction are permanently stored in the database, even in the event of a system crash or failure.
Example: After successfully placing an order in an online store, the order data is saved even if there’s a server crash immediately afterward.
##### How durability can be achieved?

``The most fundamental way to achieve durability is by using a fast transactional log. The changes to be made on the actual data are first flushed on a separate transactional log, and then the actual update is made.The write to a transaction log is made fast by keeping the file append-only and thus minimizing the disk seeks.