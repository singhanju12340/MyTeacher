---
Creation Time: Wednesday, March 19th 2025
Modified Time: Friday, March 21st 2025
---
`"How would you address performance issues in a HashMap when collisions are high?"

writing an effective `hashCode()` method that minimizes clustering

Java's built-in HashMap already uses techniques like `bit mixing.

custom solution might choose a different strategy such as `open addressing or even cuckoo hashing.

adjust the load factor and table size. By reducing the load factor threshold, the HashMap resizes more frequently, which decreases the number of elements per bucket and, therefore, the likelihood of collisions. However, this comes at the cost of increased memory usage.

`Java 8’s HashMap converts buckets with high collision counts into balanced trees, which ensures that lookup times remain O(log n) in the worst-case scenario.


`open addressing 

- In open addressing, all entries are stored directly within the hash table array.
- When a collision occurs (i.e., when two keys hash to the same slot), the algorithm searches for another free slot using a probing sequence.
- When a collision occurs (i.e., when two keys hash to the same slot), 
i.e If `hash(newkey)` leads to a full slot, try `hash(newkey) + f(1)`, then `hash(newkey) + f(2)`, then `hash(key)newkey+ f(3)`, and so on (all modulo table size), until an empty slot is found. The function `f(i)` determines the probing sequence.
- Common probing strategies include:
    - **Linear Probing:** Check the next slot (and continue sequentially) until an empty slot is found. (`H+1`, `H+2`, `H+3`, ...)
    - **Quadratic Probing:** Use a quadratic function to compute the next slot. (`H+1^2`, `H+2^2`, `H+3^2`, ...).
    - **Double Hashing:** Use a second hash function to determine the step size for probing. `f(i) = i * hash2(key)`.

**Pros:**
- **Space Efficiency:**  
    No need for additional data structures like linked lists, which can reduce memory overhead.
- **Cache-Friendly:**  
    Because all elements reside in one contiguous array, open addressing can be more cache-friendly.

**Cons:**
- Linear probing can lead to primary clustering (groups of occupied slots), increasing search times.
- - **Performance Degradation:**  
    As the load factor approaches 1, the probability of collisions increases dramatically, leading to long probe sequences.
    **Deletion Complexity:**  
Deleting an element requires careful handling (such as using "deleted" markers) to ensure the probe sequence remains intact.


`` Cuckoo hashing.

- Cuckoo hashing uses two (or more) hash functions and two (or more) hash tables (or one table with multiple potential positions) for keys. Each key can reside in one of several possible locations determined by these hash functions.

**Core Idea:**
- Each key `x` can be stored in one of two locations: `h1(x)` or `h2(x)`.
 **Lookup:** To find `x`, check `table[h1(x)]` and `table[h2(x)]`. If `x` is in the table, it must be in one of these two spots. _**This makes lookups very fast (O(1) in the common case).**_
**Insertion:**(like a cuckoo bird in another’s nest) 
1. When inserting a new key `x`, try to place it in `table[h1(x)]`.
2. If `table[h1(x)]` is empty, place `x` there. Done.
3. If `table[h1(x)]` is occupied by `y`, "kick out" `y`. Place `x` in `table[h1(x)]`.
4. Now, `y` needs to be re-homed. `y` was in `table[h1(y)]` (which is the same slot as `h1(x)`). So, try to place `y` in its _alternative_ location, `table[h2(y)]`.
5. If `table[h2(y)]` is empty, place `y` there. Done.
6. If `table[h2(y)]` is also occupied (say by `z`), kick out `z`, place `y` there, and now `z` needs to be re-homed to its alternative location.

**Cons:**
- **Insertion Complexity:**  
    Insertions can be more complex and might require multiple evictions or even table rehashing if cycles occur.
- **Space Overhead:**  
    To maintain high performance, cuckoo hashing typically requires a lower load factor (e.g., 50-90%) compared to separate chaining.
- **Implementation Complexity:**  
    - It can be trickier to implement correctly compared to chaining or simple open addressing.



**WeakHashMap vs. HashMap**
 **HashMap:**  
        Uses **strong references** for keys. This means that as long as an entry exists in the HashMap, the key will not be garbage collected—even if there are no other references to the key.
        used when you need to store data without worrying about entries disappearing due to garbage collection
**WeakHashMap:**  
        Uses **weak references** for keys. If a key is no longer referenced elsewhere (i.e., only held weakly by the map), it becomes eligible for garbage collection, and its corresponding entry is automatically removed from the map.
        Often used for caches or mappings where you don’t want the presence of the map to prevent keys from being garbage collected



`Why null is not supported in Consurrent hashmap

Allowing null keys or values in a concurrent map can create several problems, especially in a highly concurrent environment:

1. **Ambiguity in Return Values:**
    - In a standard map, if you call `get(key)` and receive a `null`, it can mean either the key is not present or the key is present with an associated `null` value.
    - In a concurrent setting, this ambiguity makes it difficult to determine whether a null is a valid cached result or simply an absence of a mapping, which can lead to erroneous decisions or retries in concurrent algorithms.
    - ConcurrentHashMap provides methods like `putIfAbsent`, `computeIfAbsent`, and `replace` that rely on checking the current value.
    - If nulls were allowed, these operations would have to distinguish between "no mapping" and "mapping to null." This would complicate the logic, as a null could mean that another thread is in the process of inserting a non-null value, leading to potential race conditions.
- **Example:**
- Imagine two threads, A and B, both calling `putIfAbsent(key, value)` concurrently:

- **With null support:**
    - Thread A checks `get(key)` and finds `null`. However, that null might mean that another thread is in the middle of updating the key with a non-null value.
    - Thread B might simultaneously perform the same check and then both try to insert their values, leading to race conditions or inconsistent behavior.