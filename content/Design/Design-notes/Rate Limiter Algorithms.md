---
Creation Time: Wednesday, July 17th 2024
Modified Time: Sunday, January 12th 2025
---

### 1. Token Bucket
![[Screenshot 2024-07-17 at 3.40.24 PM.png]]

```public class TokenBucket {
    private final long capacity;        // Maximum number of tokens the bucket can hold
    private final double fillRate;      // Rate at which tokens are added to the bucket (tokens per second)
    private double tokens;              // Current number of tokens in the bucket
    private Instant lastRefillTimestamp; // Last time we refilled the bucket

    public TokenBucket(long capacity, double fillRate) {
        this.capacity = capacity;
        this.fillRate = fillRate;
        this.tokens = capacity;  // Start with a full bucket
        this.lastRefillTimestamp = Instant.now();
    }

    public synchronized boolean allowRequest(int tokens) {
        refill();  // First, add any new tokens based on elapsed time

        if (this.tokens < tokens) {
            return false;  // Not enough tokens, deny the request
        }

        this.tokens -= tokens;  // Consume the tokens
        return true;  // Allow the request
    }

    private void refill() {
        Instant now = Instant.now();
        // Calculate how many tokens to add based on the time elapsed
        double tokensToAdd = (now.toEpochMilli() - lastRefillTimestamp.toEpochMilli()) * fillRate / 1000.0;
        this.tokens = Math.min(capacity, this.tokens + tokensToAdd);  // Add tokens, but don't exceed capacity
        this.lastRefillTimestamp = now;
    }
}
```


### 2. Leaky Bucket
![[Screenshot 2024-10-29 at 10.14.11 AM.png]]

### _Example Scenario

- Bucket Capacity: 10 requests
- Leak Rate: 2 requests per second
- If requests come in at a rate of 5 requests per second, the bucket fills up, but it will still allow 2 requests per second to leak out and be processed.
- In the case of a sudden spike where 20 requests come in at once, the first 10 will fill the bucket, but only 2 will be processed per second. The remaining requests will be rejected until space is available in the bucket.
```java
import java.util.concurrent.locks.ReentrantLock;  
  
public class LeakyBucket {  
    private final int capacity;  
    private final int leakRate; // requests per second  
    private int currentVolume;  
    private long lastLeakTime;  
    private final ReentrantLock lock = new ReentrantLock();  
  
    public LeakyBucket(int capacity, int leakRate) {  
        this.capacity = capacity;  
        this.leakRate = leakRate;  
        this.currentVolume = 0;  
        this.lastLeakTime = System.currentTimeMillis();  
    }
        public boolean addRequest() {  
        long currentTime = System.currentTimeMillis();  
        leak(currentTime);  
  
        lock.lock();  
        try {  
            if (currentVolume < capacity) {  
                currentVolume++;  
                return true; // Request accepted  
            } else {  
                return false; // Bucket is full, request rejected  
            }  
        } finally {  
            lock.unlock();  
        }  
    }  
  
    private void leak(long currentTime) {  
        long timeElapsed = currentTime - lastLeakTime;  
        int leaks = (int) (timeElapsed / 1000) * leakRate;  
        currentVolume = Math.max(0, currentVolume - leaks); // Prevents underflow  
        lastLeakTime = currentTime;  
    }  
  
}
```
##**_Advantages**
1. _Controlled processing rate_
2. _Burst handling:_ It permits bursts of incoming requests up to the bucket's capacity. If a sudden influx of requests occurs, the system can temporarily accept them, which can enhance user experience by reducing rejections in high-traffic scenarios
3. _Consistency in service:_ - The constant processing rate helps maintain a consistent user experience by preventing spikes in response times that can occur with other rate limiting techniques, like fixed windows or token buckets.
4. 
5.  
### 3. Fixed Window Counter
_Example scenario_

- **Time Window**: 1 minute
- **Request Limit**: 100 requests per minute
- If a user makes 100 requests within a minute and then tries to make an additional request in the same minute, that request will be rejected until the next minute starts, at which point the counter resets.
_Disadvantages:
- If a user makes 100 requests just before a window ends (e.g., at 59 seconds), they can overwhelm the server, resulting in a sudden load increase when the rate limiter resets.
- - Consider a user sending 1 request per second in a 1-minute window with a limit of 60 requests. They might hit the limit if they sent the 60th request exactly when the window resets, disrupting their experience.
- - The window size must be large enough to accommodate most typical usage patterns, yet small enough to respond quickly to usage spikes.
- _potential for throtlling_ : - A user may submit two requests(101, 102) 59 seconds into the current window, which will get rejected. If they then attempt the same just a second after the window resets, request will be success full, this will create dissatisfaction in user expericence.'

_There are 2 places to add Rate limiter implementation:
1. create separate API rate limiter service and deploy a full fledges rate limiter distributed service along with load balancer.
2. Make rate limiter a library not a service, and use redis to update counter from service it self instead of calling. 

### Sliding Window Log

### 5. Sliding Window Counter

**we do reach to a common conclusion, because question raised on line items are aways miunderstood as question raised on the person working on the current line item**

this algorithm logs a timestamp for every request, whether allowed or not, which consumes significant memory. it is wasteful to perform work clearing outdated timestamps on every request.