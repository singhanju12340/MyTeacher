---
Creation Time: Friday, March 21st 2025
Modified Time: Friday, March 21st 2025
---

Pre Java 8
Concurrent hash map used segment locking. and algorithms like
**Lock-Free Algorithms and CAS (Compare-And-Swap):**
- ConcurrentHashMap heavily relies on non-blocking algorithms using CAS for updating entries without locking the entire table.
- Do not throw concurrent modification exception
- provide weak consistency

JAVA 8:
_Bucket Lock_

// with hashmap
time taken by 1 thread 24
time taken by 1 thread 25
time taken by 1 thread 26
time taken by 1 thread 24
time taken by 1 thread 25
338

// with concurrent hasmap
time taken by 1 thread 27
time taken by 1 thread 28
time taken by 1 thread 28
time taken by 1 thread 28
time taken by 1 thread 28
500




