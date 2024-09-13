---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---

### Dirty Read: 
Imagine a transaction has written some data to the database, but the transaction has not yet committed or aborted. Can another transaction see that uncommitted data? If yes, that is called a dirty read

why it’s useful to prevent dirty reads:
1. If a transaction needs to update several objects, a dirty read means that another transaction may see some of the updates but not others.
	Ex: the user sees the new unread email but not the updated counter. This is a dirty read of the email.
2. If a transaction aborts, any writes it has made need to be rolled back. If the database allows dirty reads, that means a transaction may see data that is later rolled back


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

Because this is such a common problem, a variety of solutions have been developed to prevent update loss
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