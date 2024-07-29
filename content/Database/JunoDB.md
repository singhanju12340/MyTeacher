1. Distributed Key value Database open source by payPal
2. Earlier it was in single threaded c++ program similar to Redis, later written in go lang for better concurrency
3. efficient cache also with persistance
4. GoLang's go-routines  are light weight thread. It provides efficient posix thread than traditional thread which can multiplex over kernel thread.
5. Limitation of Redis: Single threaded program will always run on one core, so its waste of resources if we have 4/32 core. but it do not impact redis because redis load is memory bound not cpu bound.
6. JuneDB in other way can offer more heavy load computation as its a multithread key value storage.
7. Provide 9.9999% of availability, means only down for 31.56 second in year
8. Can handles 350 Billion request per day

Use cases:
1. Provide temporary cache with TTL few seconds to few days(provide persistance as well).
2. Idempotency: avoid duplicate process

**Latency Bridging:**
Very fast inter cluster replication

![[Screenshot 2024-07-29 at 3.52.27 PM.png]]

