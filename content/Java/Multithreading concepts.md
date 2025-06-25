

### Understanding `synchronized` in Java

The `synchronized` keyword in Java is a core mechanism for achieving thread safety. It ensures two things:

1. **Mutual Exclusion (Atomicity):** Only one thread can execute a `synchronized` block or method on a given object at any time. This prevents race conditions for shared mutable data.
2. **Visibility:** When a thread exits a `synchronized` block/method, all writes performed by that thread are guaranteed to be flushed to main memory. When a thread enters a `synchronized` block/method, it's guaranteed to read the latest values from main memory. This establishes a "happens-before" relationship.


**Synchronised Block vrs Method**


### Differentiate between `wait()`, `notify()`, and `notifyAll()` and their proper usage.?

These methods are fundamental inter-thread communication and are used to implement the **Producer-Consumer pattern** where threads need to pause and resume based on a certain condition.

Example:
LLD-collection git hub. same problem can be solved by blocking queue.



### is volatile sufficient for thread safety in multi threaded system?
No
Let's imagine two threads, T1 and T2, call `increment()` concurrently:
- **Scenario (False `volatile` assumption):**
    1. `counter` is `0`.
    2. T1 reads `counter` (`0`).
    3. T2 reads `counter` (`0`). (Visibility works, both see `0`).
    4. T1 increments its local copy (`0 + 1 = 1`).
    5. T2 increments its local copy (`0 + 1 = 1`).
    6. T1 writes `1` back to `counter` (visible to T2).
    7. T2 writes `1` back to `counter` (visible to T1, but overwrites T1's increment).
- **Result:** `counter` is `1`, but it should be `2` (since it was incremented twice). This is a classic **race condition**.
Even though `volatile` ensures that T1's write of `1` is eventually visible to T2, and T2's write of `1` is visible to T1, it doesn't prevent both threads from reading the _same initial value_ before either of them has finished the complete read-modify-write cycle.

### When `volatile` _is_ Sufficient:?
`volatile` is sufficient for thread safety only when:
1. **The variable is accessed by only one thread for writes, and multiple threads for reads**
2. The variable's new value does not depend on its old value


###  **What is a `ThreadLocal` variable?** Provide a use case and explain how it prevents thread interference.?
A `ThreadLocal` variable in Java provides a way to store data that will be accessible only by the current thread.
Example: - store current user's ID. A unique transaction ID for logging. A database connection specific to that request.




### **What is the difference between `Callable` and `Runnable`?** How do you retrieve results from an asynchronous task? (`Future`, `FutureTask`)?
Both `Runnable` and `Callable` interfaces define tasks that can be executed by a thread. The core difference lies in their capabilities regarding return values and exceptions.

`Callable:
Ideal for tasks that perform a computation and need to return a result to the caller, or tasks that might encounter exceptions that need to be handled.
Returns a generic value of type `V`.
Can throw checked exceptions (e.g., `IOException`, `SQLException`).


 ### **Describe common concurrency problems:**
    - **Race Conditions:** Give an example and how to prevent it.
    - **Deadlock:** Explain the four necessary conditions. How can you detect and prevent deadlocks?
    - **Livelock:** How is it different from a deadlock? Provide an example.
    - **Thread Starvation:** How does it occur?

### When would you choose `Lock` over `synchronized`? Discuss features like `tryLock()`?

`synchronized` is simpler for basic mutual exclusion, `Lock` (from `java.util.concurrent.locks` package) provides a richer set of capabilities.

If a thread is blocked waiting for a `synchronized` lock, it **cannot be interrupted**. It will remain blocked until the lock is released.

`ReentrantLock` allows you to specify a **fairness policy** in its constructor (`new ReentrantLock(true)`). A fair lock tries to grant access to the longest-waiting thread. Synchronisation do not provide that.




### Other problems?

**Design & Problem Solving:**

11. **You have N tasks that need to run concurrently. Design a system to manage their execution and collect their results.** (Guide them towards ExecutorService + Callable/Future).
12. **Implement a simple thread-safe counter.** Discuss different approaches (e.g., `synchronized`, `AtomicInteger`). What are the performance implications?
13. **Design a simple caching mechanism for a multi-threaded application.** How would you ensure its thread safety and handle cache invalidation concurrently?
14. **Given a scenario where a thread needs to wait for _multiple_ other threads to complete before proceeding, which concurrency utility would you use and why?** (e.g., `CountDownLatch`, `CyclicBarrier`). Explain the difference between them.



### **For Principal Software Engineer (PSE)**



1. **Explain `ReentrantReadWriteLock`.** When is it a good choice, and what are its performance benefits and potential drawbacks compared to a simple `ReentrantLock`?

2. **Beyond the basic Producer-Consumer, discuss more complex patterns:**
    - **Work Stealing:** How does `ForkJoinPool` implement this?
    - **Batching/Throttling:** How would you design a concurrent system to handle high throughput with rate limiting or batch processing?
3. **Explain the concept of "false sharing" in multi-threaded programming and how to mitigate it.**

4. **What is a "memory barrier" (or "memory fence") and how does it relate to Java's concurrency constructs?**
5. **Discuss the use of `StampedLock` introduced in Java 8.** What advantages does it offer over `ReentrantReadWriteLock`, particularly regarding optimistic reads? What are its complexities?

**Architecture, Debugging & Performance:**

11. **You are designing a critical microservice that has high concurrency requirements for data access. What architectural patterns and Java concurrency utilities would you consider, and why?** (e.g., event-driven, actor model, specific concurrent data structures).
12. **How do you debug complex deadlocks or livelocks in a production environment?** What tools and techniques would you use (e.g., `jstack`, thread dumps, profilers)?
13. **What are the common performance bottlenecks in concurrent Java applications?** How do you identify and mitigate them? (e.g., lock contention, false sharing, cache misses, context switching overhead).
14. **How do you ensure thread safety in a Spring Boot application, particularly with shared service instances or injected components?** Discuss `@Scope("prototype")`, `@Transactional`, and other Spring-specific considerations.
15. **Discuss the trade-offs between parallelism and concurrency.** When does adding more threads actually hurt performance? (Amdahl's Law, context switching).
16. **Design a highly concurrent logging mechanism.** What are the challenges, and how would you make it non-blocking or low-contention?
17. **What is the importance of immutability in concurrent programming?** Provide examples of how designing with immutable objects can simplify thread safety.
18. **Discuss common pitfalls in Java concurrency that you've encountered in large-scale systems.** How did you diagnose and resolve them? (This is a great behavioral/experience question).