---
Creation Time: Thursday, September 12th 2024
Modified Time: Wednesday, April 23rd 2025
---

==**Database isolation**== ==defines the degree to which a transaction must be isolated from the data modifications made by any other transaction(even though in reality there can be a large number of concurrently running transactions). The overarching goal is to prevent reads and writes of temporary, aborted, or otherwise incorrect data written by concurrent transactions.==

## READ Uncommitted( but do not allow write in uncommited): 
==**no transaction can update a database row if another transaction has already updated it and not committed**==.

Read Uncommitted is the lowest isolation level, If allowed, it can lead to This protects against lost updates,

## Read Committed: 
No Read or write should be allowed till other process has updated but not commited the Txn. That data entry should be locked till commited. else it leads to `dirty read`.

###### Dirty Read:  
Imagine a transaction has written some data to the database, but the transaction has not yet committed or aborted. Can another transaction see that uncommitted data? If yes, that is called a dirty read
- **Definition**: Reading uncommitted data from another transaction (which might roll back).
- **Example**:
    - Transaction 1 updates a row but hasn’t committed.
    - Transaction 2 reads the uncommitted value.
    - Transaction 1 rolls back → Transaction 2 has "dirty" data.

why it’s useful to prevent dirty reads:
1. If a transaction needs to update several objects, a dirty read means that another transaction may see some of the updates but not others.
	Ex: the user sees the new unread email but not the updated counter. This is a dirty read of the email.

[[Isolation Levels]]


### Dirty Write:
Transactions running at the read committed isolation level must prevent dirty writes, usually by delaying the second write until the first write’s transaction has committed or aborted.
By preventing dirty writes, this isolation level avoids some kinds of concurrency problems:
Example: 
A used car sales website on which two people, Alice and Bob, are simultaneously trying to buy the same car.
Buying a car requires two database writes: 
1. the listing on the website needs to be updated to reflect the buyer.
2. the sales invoice needs to be sent to the buyer
the sale is awarded to Bob (because he performs the winning update to the listings table), but the invoice is sent to Alice (because she performs the winning update to the invoices table). Read committed pre‐ vents such mishaps



The lost update problem can occur if an application reads some value from the data‐ base, modifies it, and writes back the modified value (a read-modify-write cycle). If two transactions do this concurrently, one of the modifications can be lost, because the second write does not include the first modification.

`Because this is such a common problem, a variety of solutions have been developed to prevent update loss
1. **Atomic write operations:**
UPDATE counters SET value = value + 1 WHERE key = 'foo'; 
document databases such as MongoDB provide atomic operations for making local modifications to a part of a JSON document
2. **Explicit locking**
	Another option for preventing lost updates, if the database’s built-in atomic opera‐ tions don’t provide the necessary functionality, is for the application to explicitly lock objects that are going to be updated. Then the application can perform a readmodify-write cycle, and if any other transaction tries to concurrently read the same object, it is forced to wait until the first read-modify-write cycle has completed.
**Their  are also ways to handle lost updates**
1. Automatically detecting lost updates:
	Atomic operations and locks are ways of preventing lost updates by forcing the readmodify-write cycles to happen sequentially. An alternative is to allow them to execute in parallel and, if the transaction manager detects a lost update, abort the transaction and force it to retry its read-modify-write cycle
2. Compare-and-set:
		The purpose of this operation is to avoid lost updates by allowing an update to happen only if the value has not changed since you last read it. If the current value does not match what you previously read, the update has no effect, and the read-modify-write cycle must be retried.

## Repeatable Read:
This is the most restrictive isolation level that `holds read locks on all rows it references and writes locks on all rows it inserts, updates, or deletes to avoid non reapeatable read.

[[Isolation Levels]]

## Serializable Isolation Level
This isolation level is the highest isolation level. **lock the whole table**, to prevent any other transactions from inserting or reading data from it.
serializable execution is defined to be an execution of operations in which concurrently executing transactions appears to be serially executing.



## TRANSACTION ISOLATION LEVEL SNAPSHOT:
 It allows multiple transactions to work with a consistent snapshot of the database at a specific point in time, without being affected by changes made by other transactions.
 1. When a transaction starts with SNAPSHOT isolation, SQL Server creates a virtual snapshot of the committed data as it exists at that moment.
2. The transaction reads from this snapshot throughout its duration, ensuring a consistent view of the data.
3. If another transaction modifies data that the SNAPSHOT transaction has read, the SNAPSHOT transaction continues to read from its own snapshot, avoiding conflicts or inconsistencies.
4. Once T1 goes back to commit its write, it will check for its version with current commited version if its matched it will update else rollback and retry.


- Use SNAPSHOT isolation when you need a consistent view of the data throughout a transaction, avoiding conflicts and ensuring data integrity.

![[Screenshot 2025-04-23 at 6.54.45 PM.png]]