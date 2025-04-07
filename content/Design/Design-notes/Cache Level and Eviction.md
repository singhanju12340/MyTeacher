---
Creation Time: Monday, June 24th 2024
Modified Time: Thursday, March 13th 2025
---
### Cache can be applied at 
Application type
CPU cache 
GPU cache
Etag cache
Nginx Level cache
API level cache

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
Add rate limiter on DB
add lock on key till that key is refreshed from the data store
If no value present in db for some key, store null as a value in cache to avoid db calls 


### Caching Library/Framework Integration

- **Off-The-Shelf Solutions:**  
    In production, many systems use established distributed cache solutions such as:
    - **Redis Cluster:** A distributed version of Redis that supports partitioning and replication.
    - **Hazelcast/Infinispan:** Java-based in-memory data grids that support distributed caching and provide APIs similar to Java's Map interface.
    - **Apache Ignite:** Offers distributed caching with SQL support, transactions, and more.

### How can we make local cache implementation as a distributed system
```pgsql
                     +-----------------+
                     |  Client Requests|
                     +--------+--------+
                              |
               +--------------v--------------+
               | Distributed Cache Router  |
               |  (consistent hashing ring)|
               +--------------+--------------+
                              |
          +-------------------+-------------------+
          |                   |                   |
   +------v------+      +-----v------+     +-----v------+
   | Cache Node 1|      |Cache Node 2|     |Cache Node 3|
   | (Local LRU)|      | (Local LRU)|     | (Local LRU)|
   +-------------+      +------------+     +------------+
```