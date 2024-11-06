---
Creation Time: Monday, August 5th 2024
Modified Time: Tuesday, October 29th 2024
---

#### Read Through
the cache acts as an intermediary between the application and the database.
it first looks in the cache. If data is available (**cache hit**), it’s returned to the application. If the data is not available (**cache miss**), the cache itself is responsible for fetching the data from the database, storing it, and returning it to the application.

For **cache hits**, Read Through provides **low-latency** data access.

But for **cache misses**, there is a potential **delay** while the cache queries the database and stores the data. This can result in higher latency during initial reads.

_** **Read Through caching****
is best suited for **read-heavy applications** where data is accessed frequently but updated less often, such as content delivery systems (CDNs), social media feeds, or user profiles.


#### Cache Aside
Lazy loading: The data is loaded into the cache only when needed.
Uses:
**Cache Aside** is perfect for systems where the **read-to-write** ratio is **high**, and data updates are infrequent. For example, in an e-commerce website, product data (like prices, descriptions, or stock status) is often read much more frequently than it's updated.

#### Write Through
both the cache and the database are updated as part of the same operation, ensuring that there is no delay in data propagation.

 **Write Through is** ideal for **consistency-critical systems**, such as financial applications or online transaction processing systems, where the cache and database must always have the latest data.


#### Write Around
caching strategy where data is written directly to the database, bypassing the cache.
This approach ensures that only **frequently accessed data** resides in the cache, preventing it from being polluted by data that may not be accessed again soon.

Uses:
 best used in **write-heavy systems** where data is frequently written or updated, but **not immediately or frequently read** such as logging systems.
