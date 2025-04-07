---
Creation Time: Wednesday, April 2nd 2025
Modified Time: Thursday, April 3rd 2025
---

Its a pattern to maintain consistency in distributed system via sequence of local messages.
A Saga is a sequence of local transactions. Each local transaction updates the local database using the familiar ACID transaction frameworks and publishes an event to trigger the next local transaction in the Saga. If a local transaction fails, then the Saga executes a series of **_compensating_** _transactions_ that undo the changes, which were completed by the preceding local transactions
	
Example Transaction systems while booking flight

Steps: 
1. deduct money
2. reserve seat
3. send confirmation

If anyone of the step fails we want all steps should be reverted. 
`succeed together or fail together

Sagas can be implemented in “two ways” primarily based on the logic that coordinates the steps of the Saga.
1. _**Choreography based sagas**_
	a local transaction publishes events that trigger other participants to execute local transactions. In an orchestrated-based saga, a centralized saga orchestrator sends command messages to saga participants telling them to execute local transactions.
	there is no central coordinator to tell saga participants what to do. Saga participants subscribe to each other’s events and respond accordingly
![[Screenshot 2025-04-03 at 8.09.07 PM.png]]
	
The **HAPPY** path through this SAGA is as follows:
2. _Order Service_ creates an Order in the `APPROVAL_PENDING` state and publishes an `OrderCreated` event.
3. _Consumer Service_ consumes the `OrderCreated` event, verifies that the consumer can place the order, and publishes a `ConsumerVerified` event.
4. _Kitchen Service_ consumes the `OrderCreated` event, validates the Order, creates a Ticket in a `CREATE_PENDING` state, and publishes the `TicketCreated` event.
5. _Accounting Service_ consumes the `OrderCreate` event and creates a `CreditCardAuthorization` in a `PENDING` state.
6. _Accounting Service_ consumes the `TicketCreated` and `ConsumerVerified` events, charge the consumer’s credit card, and publish the `CreditCardAuthorized` event.
7. _Kitchen Service_ consumes the CreditCardAuthorized event and changes the state of the Ticket to `AWAITING_ACCEPTANCE`.
8. _Order Service_ receives the `CreditCardAuthorized` events, changes the state of the Order to `APPROVED`, and publishes an `OrderApproved` event.

`Advantage:

Loose coupling
Simple

`Disadvantages:
Difficulty in understanding the flow
can lead to cyclic dependency


2. _**Orchestration based sagas**_
a central Saga orchestration class is responsible to tell saga participants what to do. Like Zookeeper
![[Screenshot 2025-04-03 at 8.09.59 PM.png]]


3. The SAGA orchestrator sends a `Verify Consumer` command to _Consumer Service_.
4. _Consumer Service_ replies with a `Consumer Verified` message.
5. The SAGA orchestrator sends a `Create Ticket` command to _Kitchen Service._
6. _Kitchen Service_ replies with a `Ticket Created` message.
7. The SAGA orchestrator sends an `Authorize Card` message to _Accounting Service._
8. _Accounting Service_ replies with a `Card Authorized` message.
9. The SAGA orchestrator sends an `Approve Ticket` command to _Kitchen Service._
10. The saga orchestrator sends an `Approve Order` command to _Order Service._

_Simpler dependencies_
_Less coupling_



## Anomalies

There are three types of anomalies found in a typical saga.
1. **Lost Updates** — One saga overwrites an update made by another saga.
2. **Dirty Reads** — One saga reads data that is in the middle of being updated by another saga.
3. **Fuzzy / Non-repeatable Reads** — Two different sets of a saga read the same data and get different results because another saga has made updates.


4. **Semantic Lock** — This is an application-level lock, in which saga’s compensable transactions set a flag (e.g. Creating an Order can have flag status such as APPROVAL_PENDING, REVISION_PENDING, etc.) in any record that it creates or updates. This flag indicates that the record is not committed and that it has the potential to change. This could be cleared by a retriable transaction or a compensating transaction.
5. **Commutative Updates** — Designing the system to have more its update operations to be commutative (updates in an orderly manner). This can basically eliminate _lost updates._
6. **Pessimistic View** — Reordering saga participants/services to minimize the effect of dirty reads.
7. **Reread Values** — This countermeasure reread values before updating it to further to re-verify the values are unchanged during the process. This will minimize _lost updates._
8. **By Value** — This strategy will select concurrency mechanisms based on the business risk. This can help to execute low-risk requests using sagas and execute high-risk requests using distributed transactions.


It can be achieved using 2 phase commit . It can be achieved by keeping `cordination service like zookeeper.
[[Two phase commit]]
[[2phase_commit]]




