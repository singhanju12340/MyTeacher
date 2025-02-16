---
Creation Time: Monday, June 24th 2024
Modified Time: Sunday, February 9th 2025
---
### Cache can be applied at 
Application type
CPU cache 
GPU cache
Etag cache
Nginx Level cache


## Caches solution
Redis
Varnish
Hasel cast

### Eviction Type
FIFO
LIFO
LRU
LFU
Random replacement
Most recently used



### Cache Pollution: 
 Cache pollution refers to a situation in which a caching system ends up storing data that is unlikely to be reused, thereby displacing more valuable, frequently accessed data. This degrades the cache’s effectiveness because the hit rate drops—
`Solutions`
1. divide cache into multiple segment based on the workload.
2. prefer eviction algorithm considering both recency and frequency of access. like Adaptive Replacement cache or LFU instead of LRU.
3. Implement filter and heuristic to decide if data should be cached.




### Negative Caching: 

Negative caching is a technique used to store “negative” responses—results indicating that a resource does not exist or an error occurred
Ex: DNS Resolution Error response, Web Caching when webpage not available 404 , API Responses.

`Potential Trade-offs`
Staleness


### Cache Stampede:  Thundering hurd
Cache stampede, sometimes referred to as "dog-piling," occurs when many clients simultaneously try to refresh or repopulate a cache entry that has just expired. This sudden surge of requests to the underlying data source can overwhelm the backend, leading to performance degradation or even system outages.
`Solution`
When a cache miss occurs, only one thread or process is allowed to query the backend and refresh the cache.
**TTL Jitter:**  Instead of having all cache entries expire at the same predictable moment, randomize TTLs slightly.
