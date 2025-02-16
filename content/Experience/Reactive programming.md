---
Creation Time: Saturday, February 15th 2025
Modified Time: Sunday, February 16th 2025
---

_**Reactive programming is a programming paradigm/pattern where the focus is on developing asynchronous and non-blocking applications in an event-driven form**
Ex: - Project Reactor

# Drawbacks in the traditional way

- Once a thread is assigned to a request that thread won’t be available until that request finishes its process.
- If all the threads are occupied, the next requests that come into the server will have to wait until at least one thread frees up.
- When all the threads are busy, performance will be degraded because memory is being used by all the threads.

- **Asynchronous and non-blocking**  
    Asynchronous execution is a way of executing code without the top-down flow of the code. In a synchronous way, if there is a blocking call, like calling a database or calling a 3rd party API, the execution flow will be blocked. But in a non-blocking and asynchronous way, the execution flow will not be blocked. Rather than that, futures and callbacks will be used in asynchronous code execution.
- **Event/Message Driven stream data flow**  
    In reactive programming, data will flow like a stream and because it is reactive, there will be an event and a response message to that event. In Java, it is similar to java streams which was introduced in Java 1.8. In the traditional way, when we get data from a data source (Eg: database, API), all the data will be fetched at once. In an event-driven stream, the data will be fetched one by one and it will be fetched as an event to the consumer.
- **Functional style code**
    Similar to java 1.8 Lamda expression. In reactive programming, we mostly use this lambda expression style.
**Back Pressure**
	In reactive streams when a reactive application (Consumer) is consuming data from the Producer, the producer will publish data to the application continuously as a stream. Sometimes the application cannot process the data at the speed of the producer. In this case, the consumer can notify the producer to slow down the data publishing.
    
![[Pasted image 20250215223711.png]]


## Reactive stream specifications:

Java reactive programming provides 4 interfaces, which needs to be extended to while creative reactive stream.

1. Publisher: Single method interface, to register subscriber to producer 
	```Java
	public interface Publisher<T>{
		public void subscribe(Subscriber<? extends T> s) ;
	}
```



1. Subscriber
This is an interface that has four methods  
**_onSubscribe_** method will be called by the publisher when subscribing to the Subscribe object.  
**_onNext_** method will be called when the next data will be published to the subscriber  
**_onError_** method will be called when exceptions arise while publishing data to the subscriber  
**_onComplete_** method will be called after the successful completion of data publishing to the subscriber
```Java
	public interface Subscriber<T>{
		public void onSubscribe(Subscription s);
		public void onNext(T t);
		public void onError(Throwable t);
		public void onComplete();
	}
```
1. Subscription
```java
public interface Subscription{
	public void request(long n); // calls when subscriber wants to request data from publisher
	public void cancel(); // method called when subscriber wants to cancel the subscription
}
```
1. Processor
```java
public interface Processor<R,T> extends Subscriber<R>, Publisher<T>
```




##### Challanges and solution of reactive programming
>>**Error handling challenges:**
- _Non-Blocking Nature_: Reactive programming relies on non-blocking operations, which can lead to errors occurring at different times and potentially being handled on different threads. This makes traditional error handling mechanisms, like try-catch blocks, less effective.
- _Asynchronous Stack Traces_: Asynchronous operations can complicate stack traces, making it harder to track down the source of errors and their context.
- _Multiple Stages_: Reactive chains often consist of multiple stages with various transformations and operators. Errors could occur at any stage, making it important to handle them at the appropriate level.
>> **Strategies to handler error in reactive programming**
1. `onErrorResume` and `onErrorReturn`: These operators allow you to provide fallback values or alternative streams in case of an error.
2.  `doOnError`: This operator lets you execute specific actions when an error occurs, such as logging the error or cleaning up resources. It doesn't interfere with the error propagation itself.
3. `retry` and `retryWhen`: These operators enable you to automatically retry an operation a specified number of times or based on a certain condition.


#### Backpressure
Reactive programming provides a more interesting feature, `backpressure`.
Imagine a scenario where a fast data producer is feeding a slow data consumer. Without backpressure, the consumer could become overwhelmed, leading to _memory exhaustion_, _latency spikes_, or even application crashes.
##### Backpressure Handling Strategies
1. **BUFFER**: The most straightforward strategy. The publisher buffers emitted data until the subscriber can consume it. While this can prevent data loss, it might lead to increased memory usage.
2. **DROP**: When the subscriber signals that it can’t keep up, the publisher simply drops the excess data. This can lead to data loss but helps prevent memory overflows.
3. **LATEST**: This strategy drops the previously buffered data and only keeps the most recent data. It’s useful when older data becomes less relevant.
4. **ERROR**: The publisher throws an error when the subscriber can’t keep up. This strategy ensures that backpressure issues are surfaced explicitly but can be disruptive.
```Java
Flux<Integer> fastProducer = Flux.range(1, 1000);  
Flux<Integer> bufferedConsumer = fastProducer.onBackpressureBuffer(10);  
  
bufferedConsumer.subscribe(  
	value -> {  
	// Simulate slow processing  
	Thread.sleep(100);  
	System.out.println(value);  
	}  
);
```


### Reactive async Webclient

```Java
```