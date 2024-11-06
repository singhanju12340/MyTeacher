---
Creation Time: Monday, August 12th 2024
Modified Time: Tuesday, October 29th 2024
---
[[Rate Limiter Algorithms]]

_Rate limiting restricts the flow of requests that a server will accept in a time window. Whenever a caller exceeds this limit within this period, the rate limiter denies any excess requests, preventing them from ever reaching our server.

- the number of requests and the unit of time,
- the max size of the incoming requests,
- which endpoints to rate limit,
- how to respond when rate limiting,
- how to react on rate limiter failure,
- where to locate the rate limiter,
- where to store rate-limiting state,
- whether some endpoints require different limits,
- whether to limit per account or per IP or otherwise,
- whether to institute a global rate limit,
- whether different tiers of clients should be limited differently,
- whether to impose a strict limit or allow bursts of requests, and
- which rate-limiting algorithm to use.


### Functional Requirements

1. We should be able to add different limit values for different APIs per minutes or hours(rate limiting state)
	1. _**in-memory storage**:Not persistent across application restarts, limited scalability.,  Small-scale applications with low traffic.
	2.  _**centralized store**: add latency and create potential bottleneck in our system, and run the risk of race conditions
	3. _**distributed storage**: Increased complexity, potential for data consistency issues. we can scale out horizontally to multiple stores, hence no bottleneck in our system. But this means we will have to synchronize state across all the stores. We can use Raft consensus algorithm to achieve the same.we must update the state in all other stores. And given the delay in updating state across all instances, we introduce the possibility of inaccuracies in rate-limiting state. Depending on the use case, these inaccuracies may or may not be tolerable.
		1. Redis: highly scalable in-memory data store
		2. Memcached: - A distributed memory object caching system.
2. The user should be able to set recovery time after limit is reached for each APIs based on API uses sensitivity
3. use an algorithm like a leaky bucket for building rate limiter
4. Add user level based on user subscription
5. handle errors gracefully after the limit is reached.
	1.  _Block request with 429(too many request) or 503(Service unavailable) 
	2. _Shadowban abuser: return 200 but do not process the request
	3. _Throttle the request: return response with delays
	4. _Deprioritise:  Process request once other queued request afre processed
6. Monitor the system to define failed and success matrix patterns and users access patterns for better configuration of rate limited
7. Delete API counter key if API is not in use.
8. 
### Non Functional Requirements
1. Implementation should not be overhead on performance level in case of height API load
2. Limit mapping should be kept in the cache like redis for quick access
3. handle burst load where limit is not reached
4. It should work with a distributed system and should be scalable 


Use:
	1. **excessive use of our own resources**
		 the high load is malicious, as in a DoS attack, rate limiting serves as our first line of defense
	2. **excessive use of external resources**
		If we rely on an external service that is paid, setting a rate limit ensures that excessive usage will not lead to cost overruns
	3. **excessive use of our users' resources**


Where should we store 

