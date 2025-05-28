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
![[Screenshot 2025-04-03 at 8.09.07 PM.png]] // this example flow is different than below example
	
The **HAPPY** path through this SAGA is as follows:
- **Order Created:** Customer places an order, **Order Service** records it as `PENDING` and publishes an `OrderCreatedEvent`.
- **Payment Processed/Failed:** The **Payment Service** listens for `OrderCreatedEvent`, processes payment. If successful, it publishes `PaymentProcessedEvent`; if failed, `PaymentFailedEvent`.
- **Inventory Reserved/Released:** The **Inventory Service** listens for `PaymentProcessedEvent` (or sometimes `OrderCreatedEvent`). If it reserves stock, it publishes `InventoryReservedEvent`. If `PaymentFailedEvent` occurs, it compensates by releasing stock.
- **Order Status Updated:** The **Order Service** listens for `PaymentProcessedEvent` or `InventoryReservedEvent` to mark the order `COMPLETED`, or for `PaymentFailedEvent` to mark it `CANCELLED`.
- **Shipping Initiated:** The **Shipping Service** listens for `InventoryReservedEvent` to start the delivery process.
FAILURE Scenario (Choreography - Payment Fails):
1. **Order Service:** Creates `PENDING` order, publishes `OrderCreatedEvent`.
2. **Payment Service:** Receives `OrderCreatedEvent`, **payment fails**, publishes `PaymentFailedEvent`.
3. **Order Service:** Receives `PaymentFailedEvent`, updates order to `CANCELLED`.
4. **Inventory Service:** (If it reserved stock earlier) Receives `PaymentFailedEvent` (or `OrderCancelledEvent`), **releases previously reserved stock** (compensating transaction).

`Advantage:
Loose coupling
Simple

`Disadvantages:
Difficulty in understanding the flow
can lead to cyclic dependency


2. _**Orchestration based sagas**_
a central Saga **Orchestrator Service** class is  to tell saga participants what to do. Like Zookeeper
![[Screenshot 2025-04-03 at 8.09.59 PM.png]]


**Scenario:** User books a flight ticket.
**Services Involved:**
- **Booking Orchestrator Service:** The central brain.
- **Payment Service:** Handles charging the user.
- **Flight Reservation Service:** Reserves a seat on the flight.
- **Notification Service:** Sends confirmation emails/SMS.

- **User initiates booking.**
- **Booking Orchestrator Service (BOS):**
    - Tells _Payment Service_ to charge user.
    - **Payment Service** succeeds, replies to BOS.
    - BOS then tells **Flight Reservation Service** to reserve seat.
    - **Flight Reservation Service** succeeds, replies to BOS.
    - BOS then tells **Notification Service** to send confirmation.
    - **Notification Service** succeeds, replies to BOS.
    - BOS marks booking `COMPLETED`.

- **User initiates booking.**
- **Booking Orchestrator Service (BOS):**
    - Tells **Payment Service** to charge user.
    - **Payment Service fails**, replies to BOS.
    - BOS receives failure, triggers **compensating transactions**:
        - (If any prior steps succeeded, like a preliminary reservation, BOS would tell that service to undo it).
        - BOS marks booking `FAILED`.

Advantage
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




