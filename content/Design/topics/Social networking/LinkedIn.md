---
Creation Time: Tuesday, December 3rd 2024
Modified Time: Tuesday, December 3rd 2024
---
Restrictions Enforcement system


### Capacity:
1 B user
100 M messages per Day


### Requirements:
- **High QPS** - The system needs to support 4-5 million queries per second. Many user actions (viewing the feed, sending messages, etc.) require checking the _restricted/blocked accounts list.
- **Low Latency** - Latency needs to be under 5 milliseconds
- This system needs to operate with 99.999% availability
- **Low Ingestion Delay** - When a user blocks/reports an account, that should be reflected in the restrictions enforcement system immediately. If they refresh their feed right after, the posts from the blocked user should be immediately hidden


### Design

_**BitSets:**
The time and space requirements with BitSets are very efficient. Checking whether users are restricted takes constant time (_since the membership lookup operations are all O(1)_)

A BitSet is an array of boolean values where each value only takes up 1 bit of space.
BitSets are _extremely_ memory efficient. If you need to store boolean values for 1 billion users (_whether a user is restricted/not restricted_), you would only need 1 billion bits (approximately 125 megabytes). assume each user has a memberID from 1 to 1 billion.
To store this, they could use an array of 64-bit integers. Each integer can store the restriction status for 64 different users (_we need 1 bit per user_) so the array would hold ~15 million integers (around 125 megabytes of space).

If LinkedIn needs to check whether user with memberID `525234320` is restricted, the steps would be:
1. Divide 523,234,320 by 64 to get the index in the integer array (which would be 8,175,536)
    
2. Take 523,234,320 modulo 64 to get which bit to check in that integer (which would be 32)
    
3. Use bitwise operations to check if that specific bit is set to 1 (restricted) or 0 (not restricted)


_**Bloom Filters**

A Bloom filter is a probabilistic data structure that lets you quickly test whether an item might be in a set. Bloom Filters are _probabilistic_ so they will tell you if an item is in a set but will occasionally give false positives (_it will mistakenly say an item is in the set when it’s not_).
The pros were that the Bloom Filters were extremely space efficient compared to traditional caching techniques (_using a set or hash table_).

_**Full Refresh-ahead Caching_

The dataset of account restrictions was quite small (_thanks to using the BitSet and Bloom Filter data structures_) so LinkedIn had each client application host store _all_ restriction data in their in-memory cache.
In order to maintain cache freshness, they implemented a polling mechanism where clients would periodically check for any new changes to member restrictions.
_disadvantage_ This increased the client-side memory footprint ended up being substantial and strained infrastructure.  the caches were stored in-memory so they weren’t persistent. Clients had to frequently build and rebuild this cache which put strain on LinkedIn’s underlying database.



