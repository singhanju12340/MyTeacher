---
Creation Time: Monday, August 12th 2024
Modified Time: Tuesday, August 13th 2024
---
Rate limiting restricts the flow of requests that a server will accept in a time window. Whenever a caller exceeds this limit within this period, the rate limiter denies any excess requests, preventing them from ever reaching our server.

Use:
	1. **excessive use of our own resources**
		 the high load is malicious, as in a DoS attack, rate limiting serves as our first line of defense
	2. **excessive use of external resources**
		If we rely on an external service that is paid, setting a rate limit ensures that excessive usage will not lead to cost overruns
	3. **excessive use of our users' resources**


Requirements to be considered before building Rate Limiter
1. What should be the max number of request and within how much time.
2. Do we need limit on size of the request.
3. Which API endpoint to be rate limited.
4. What should be the response when limit reached.
5. What should be the response when rate limiter failed.


fun(ip first, ip second, country )

IP: process octat processing

graph ql
why compound index

