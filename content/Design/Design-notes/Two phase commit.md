---
Creation Time: Saturday, February 22nd 2025
Modified Time: Thursday, April 3rd 2025
---
Its an algorithm used to maintain ACID property in distributed services to maintain string consistency and coordination across multiple services.
- **Prepare Phase (Voting):**  
    The coordinator asks each participant if they are ready to commit the transaction. Each participant performs local checks (e.g., locks resources, validates constraints) and replies with a "Yes" (ready) or "No" (abort).
    
- **Commit/Abort Phase (Decision):**  
    If all participants vote "Yes," the coordinator sends a commit message to all, finalizing the transaction. If any participant votes "No" (or fails to respond), the coordinator sends an abort message and every participant rolls back their local work.

EX:

```Java
public interface Participant {
    // Phase 1: Prepare – returns true if ready, false if not.
    boolean prepare();

    // Phase 2: Commit the transaction.
    void commit();

    // Phase 2: Rollback the transaction.
    void rollback();
}
```


```Java

public class ReservationParticipant implements Participant {
    private final String reservationId;

    public ReservationParticipant(String reservationId) {
        this.reservationId = reservationId;
    }

    @Override
    public boolean prepare() {
        // Simulate checking if the seat is available and reserving it
        System.out.println("Reservation " + reservationId + " is prepared.");
        return true; // return false to simulate a failure.
    }

    @Override
    public void commit() {
        System.out.println("Reservation " + reservationId + " committed.");
    }

    @Override
    public void rollback() {
        System.out.println("Reservation " + reservationId + " rolled back.");
    }
}


```



```Java
public class PaymentParticipant implements Participant {
    private final String paymentId;

    public PaymentParticipant(String paymentId) {
        this.paymentId = paymentId;
    }

    @Override
    public boolean prepare() {
        // Simulate pre-authorizing payment
        System.out.println("Payment " + paymentId + " is prepared.");
        return true; // return false to simulate a failure.
    }

    @Override
    public void commit() {
        System.out.println("Payment " + paymentId + " committed.");
    }

    @Override
    public void rollback() {
        System.out.println("Payment " + paymentId + " rolled back.");
    }
}

```


```Java
import java.util.List;

public class Coordinator {
    private final List<Participant> participants;

    public Coordinator(List<Participant> participants) {
        this.participants = participants;
    }

    public boolean commitTransaction() {
        System.out.println("Phase 1: Prepare");
        // Phase 1: Prepare – ask all participants to prepare.
        for (Participant participant : participants) {
            if (!participant.prepare()) {
                System.out.println("A participant failed to prepare. Aborting transaction.");
                rollbackAll();
                return false;
            }
        }

        System.out.println("All participants prepared successfully. Phase 2: Commit");
        // Phase 2: Commit – all participants are ready, so commit.
        for (Participant participant : participants) {
            participant.commit();
        }
        return true;
    }

    private void rollbackAll() {
        System.out.println("Rolling back transaction for all participants.");
        for (Participant participant : participants) {
            participant.rollback();
        }
    }
}

```

```Java
import java.util.Arrays;

public class TwoPhaseCommitDemo {
    public static void main(String[] args) {
        // Create participants.
        Participant reservation = new ReservationParticipant("R123");
        Participant payment = new PaymentParticipant("P456");

        // Create coordinator with the participants.
        Coordinator coordinator = new Coordinator(Arrays.asList(reservation, payment));

        // Try to commit the transaction.
        boolean success = coordinator.commitTransaction();
        System.out.println("Transaction success: " + success);
    }
}

```


### Advantage:
1. Give high consistency by ensuring no individual system in distributed transaction is in inconsistent state.
2. 

### Disadvantage:
2. make system too slow to receive consistency
3. Can create deadlock scenarios, better deadlock identification and breaking system should be enabled in place.
4. What is coordinator node goes down after the reserve is success?
	1. Coordinator will not be able to commit txn and nodes row will be in blocked state forever without updating latest data.

2PC is not fully recommended for microservices-based applications due to its **synchronous blocking**. This protocol will need to block the required object that will be changed before the transaction completes. This prevents the relevant object from being used by a different transaction (deadlock situation) until the ongoing transaction is fully completed. This is not a good situation, especially in a modern-day application.

### Example
5. Zomato 10 min delivery
	1st Phase
- Any new order will first work on reserving the required resources needed for this current order to be places.
	- Inform store service to first reserve requested food and quantity
	- Inform delivery agent service to first reserve delivery agent available for this order id.
	2nd Phase
	-if both the services will be able to reserve their resources. 
		Commit to inform delivery agent for food delivery and commit to store to heat food will be placed.
If any of the reserve method fails, commit will not be called and rollback will be called for all the process to release the reserved data for next order.

6. Distributed database update for updating txn Id in all the replicas in partitioned database.
	[[2phase_commit]]


`Its advisable to use 2PC when strong consistency is needed and less number of transcations to be commited`

#### Solution to 2 phase commit is Saga algorithm
[[Saga Patterns]]

Design system in terms of workflows using 2 patterns
1. _Orchestrator
2. _Choreographer

[[2phase_commit]]

Advantage of Choreography
	1. Loose coupling,
	2. easy extension :  adding new service is easy
	3. flexible: services are their own decision maker
	4. Robust: One service does not affect other services.
Disadvantage:
	1. We need to implement strong observability, need to implement `corelationId/ distributed tracing` across all decoupled systems.

 
 _**Orchestrator patterns can also be used where we need to ensure ordering of execution.  and synchronous processing.
 Ex: 4th flow should be executed then only when 3 flows are success
 Ex: OTP flow login, should be in sync with the help of orchestrator to ensure OTP is send to with the min delay without expiring OTP timing.
Ex: 
  