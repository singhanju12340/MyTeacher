---
Creation Time: Wednesday, April 2nd 2025
Modified Time: Wednesday, April 2nd 2025
---
The Java Executor Framework is a high-level API in the java.util.concurrent package that abstracts away the details of thread creation and management, allowing you to focus on task submission and result handling.

**_It provides a way to separate the task execution logic from the application code, allowing developers to focus on business logic rather than thread management._**

### Core Components

Executor Interface
```
Executor executor = Runnable::run; // Simple example executing in the calling thread.
executor.execute(() -> System.out.println("Task executed"));
```


ExecutorService Interface

```
ExecutorService executorService = Executors.newFixedThreadPool(4);

Future<String> future = executorService.submit(() -> "Hello, Executor!");

executorService.shutdown();
```
	
ThreadPoolExecutor
```
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    4, 8, 60, TimeUnit.SECONDS, new LinkedBlockingQueue<>());
executor.execute(() -> System.out.println("Task running in ThreadPoolExecutor"));
executor.shutdown();
```

ScheduledExecutorService
```
schedule(Runnable command, long delay, TimeUnit unit)
```

Callable and Future
```
Callable<Integer> task = () -> {
    // perform computation
    return 42;
};

ExecutorService executor = Executors.newFixedThreadPool(2);

Future<Integer> future = executor.submit(task);

System.out.println("Result: " + future.get()); // Blocks until the result is available.

executor.shutdown();
```


### Functionality and Benefits

- **Decoupling Task Submission from Execution:**  
    The framework allows you to submit tasks without managing the threads that execute them.
    
- **Efficient Resource Management:**  
    Thread pools reuse threads, reducing the overhead of thread creation and destruction.
    
- **Simplified Concurrency Control:**  
    High-level constructs like Future, Callable, and scheduled tasks simplify handling asynchronous computations and scheduling periodic tasks.
    
- **Scalability:**  
    The framework’s configurable thread pools allow your application to scale efficiently with the workload.
    
- **Flexible Scheduling:**  
    The ScheduledExecutorService is ideal for tasks that need to run after a delay or repeatedly at fixed intervals.


Example
```Java
ExecutorService pool = Executors.newFixedThreadPool(50);

while (true) {
    Socket clientSocket = serverSocket.accept();
    pool.submit(() -> handleRequest(clientSocket)); // Decoupled!
}

void handleRequest(Socket socket) {
    // Business logic (e.g., HTTP parsing)
}
```


### Different types of thread pool provided by executor services:
`newVirtualThreadPerTaskExecutor()

- As the name suggests, this executor **creates a new virtual thread for every task submitted to it.**
- It does _not_ pool virtual threads in the traditional sense (like reusing a fixed set of threads). Each call to `execute()` or `submit()` will spawn a fresh virtual thread.
- Since virtual threads are cheap to create and manage, creating one per task is feasible and efficient for many scenarios, especially I/O-bound ones.
- The underlying work of these virtual threads is still carried out by a pool of platform threads.
- **High Concurrency:** Can handle a very large number of concurrent tasks, especially if those tasks are I/O-bound, because virtual threads don't tie up OS threads while waiting.
- **No Queueing (Typically):** Since a new virtual thread is created for each task, tasks don't usually queue up waiting for a thread from a pool to become available (unless the underlying carrier pool for platform threads becomes saturated, which is less common with virtual threads used correctly).


`newFixedThreadPool(int count)
- Creates a thread pool that reuses a fixed number of threads
- _Good for CPU-bound tasks_ where the number of threads can be tuned to the number of CPU cores to avoid excessive context switching. Also used to limit resource consumption by bounding the number of concurrent tasks.

`newCachedThreadPool()

- Creates a thread pool that creates new threads as needed if existing threads are busy, but will reuse previously constructed threads when they are available. Threads that have been idle for sixty seconds are terminated and removed from the cache.

`newSingleThreadExecutor()
- Creates an Executor that uses a single worker thread operating off an unbounded queue. Tasks are guaranteed to execute sequentially.