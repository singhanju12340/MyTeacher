---
Creation Time: Friday, March 21st 2025
Modified Time: Friday, March 21st 2025
---
achieves thread safety and high concurrency by `moving away from a single lock for the entire map and instead using more granular locking mechanisms`. This allows multiple threads to access different parts of the map simultaneously.


##### Pre Java 8
The map was internally divided into a fixed number of **segments**. Each segment was essentially an independent `HashMap` (or a similar hash table structure) with its own dedicated lock.
when you perform put or remove. The thread would then acquire the lock **only for that specific segment**.

**Read Operations (`get()`):** Read operations were generally more optimized. They often didn't require acquiring the segment lock if the value was already present and visible (due to `volatile` reads of segment entries).

###### Post Java 8
`ConcurrentHashMap` moved to an even **finer-grained locking mechanism**. typically at the **node/bucket level**
Using Lock-Free Algorithms and `CAS (Compare-And-Swap):
> locks are usually applied to the **first node of a bucket's linked list or Red-Black tree** when a write operation is needed for that bucket.

**`synchronized` blocks:** For write operations (like `put`, `remove` if the bucket is not empty)
only threads trying to modify the _same bucket_ will contend for that bucket's lock.
Example:
To add a new node to an empty bucket, `ConcurrentHashMap` might use CAS to try and set the new node as the head of that bucket. If the CAS succeeds (meaning no other thread modified it in the meantime), the operation is done without acquiring a lock. If it fails (another thread beat it), it might then fall back to using a lock or retry the CAS.

**Volatile Reads/Writes:** Variables pointing to nodes and their values are often `volatile` to ensure visibility of changes across threads. `get()` operations are generally lock-free and rely on these volatile reads.

Example:
**Thread A wants to `put("apple", 1)`:**
- `hashCode("apple")` maps to bucket index, say **B5**.
- If B5 is empty: Thread A tries to use CAS to place a new Node("apple", 1) as the head of B5.
    - If CAS is successful: Done. No explicit lock was held for a long duration.
    - If CAS fails (another thread just initialized B5): Thread A might retry or proceed to lock.
- If B5 is not empty (it has a head node `nodeX`): Thread A will `synchronize` on `nodeX`.
    - Once the lock on `nodeX` is acquired, Thread A traverses the list/tree in B5 to find/update "apple" or add it.
    - Lock on `nodeX` is released.



## The primary reason `ConcurrentHashMap` does not allow `null` keys or `null` values is to **avoid ambiguity, especially in its `get()`**
Example
```Java
ConcurrentHashMap<String, String> map = new ConcurrentHashMap<>(); map.put("activeUser", "Alice");
map.put("inactiveUser", null); // Let's pretend this is allowed for a moment

- **`map.get("activeUser")`** would return `"Alice"`. (Clear)
- **`map.get("nonExistentUser")`** would return `null`.
- **`map.get("inactiveUser")`** (if `null` values were allowed) would _also_ return `null`.


**If `map.get(key)` returns `null`, you cannot tell if:** a. The key is not present in the map.

>>>>>>>>>>>>>>
>>>>>>>>>>>>>>
To resolve this confusion map needs to make 2 steps process similar to below
// Hypothetical scenario IF null values were allowed in ConcurrentHashMap 
String key = "myKey"; 
if (map.containsKey(key)) { 
	String value = map.get(key);  // If this returns null, you assume it's an explicit null value 
	if (value == null) { 
			System.out.println(key + " is present with a null value."); } 
	else { 
		System.out.println(key + " = " + value);
	} 
} else {
	System.out.println(key + " is not in the map."); 
}


This 2 step verification will not be an atomic operation and cause issue in maintaning concurrency. It can lead to race condition.


```

Example of race condition(if `null` values were allowed):

```Java
thread 1
 map.put("myKey", "actualValue"); 
- **Thread 1:** Executes `if (map.containsKey("myKey"))`. This is `true`.

- **Context Switch:** The operating system switches execution from Thread 1 to Thread 2.

Thread 2
 Executes `map.remove("myKey");
- **Context Switch:** Execution returns to Thread 1.
    
- **Thread 1:** Now executes `String value = map.get("myKey");`. Since "myKey" was just removed by Thread 2, this `get()` call will return `null`.
    
- **Thread 1 (Incorrect Conclusion):** Enters the `if (value == null)` block and incorrectly prints `"myKey is present with a null value."`, even though the key was actually removed entirely.
```


**Simplified Design:** Not having to handle special `null` cases internally can simplify the implementation of the `ConcurrentHashMap` itself.


In new Java implementations.
## Node Linked-list is converted to `Red black tree` after certain threshold. when does this happens and why?

- **Linked List to  RED Black Tree:** If a bucket's node count reaches `TREEIFY_THRESHOLD` (8) AND the map's total capacity is at least `MIN_TREEIFY_CAPACITY` (64).
`Reason:
With a linked list, operations like `get()`, `put()`, and `remove()` in such a bucket would take O(n)
By converting a long linked list into a balanced Red-Black tree, the time complexity for these operations within that specific bucket improves to **O(log n)**

`This process also gets reversed when linklist size is reduced to certain threshod.
**Tree back to Linked List:** If a treeified bucket's node count drops to `UNTREEIFY_THRESHOLD` (6) or below.
_Reason_
the overhead of tree operations and the memory footprint of tree nodes might be greater than or not significantly better than a simple linked list due to nodes parent/child/color pointers)

### What is map load factor and how does it impact resizing map
`Low Load Factor (e.g., 0.5):` The map will resize sooner. Fewer hash collisions. More wasted space
`High Load Factor (e.g., 0.9)`: The map will resize slow. High hash collision, which will cause slower lookups and insertion, as buckets become more crowded, leading to longer linked lists or larger trees. 

`When it happens:
_**HashMap_ when called `put()`, it determines if resize is necessary ( `capacity * loadFactor`), the resizing process happens **synchronously within that same `put()` call**.
`ConcurrentHashMap` is designed for this. When it resizes:
_**ConcurrentHashMap**_
The resizing process is done in a thread-safe and often incremental manner.
- **New `put`, `remove`, and `get` requests can generally proceed concurrently** even during a resize.
- Threads performing `put` operations might even help with the resizing process by moving some elements from the old table to the new table.
- `get` operations might need to check both the old and new tables if a resize is in progress for the relevant segment/portion of the map.


#### Example run to modify concurrent hashmap by multiple threads simultenously.
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


**What are some of the atomic operations provided by `ConcurrentHashMap` (e.g., `putIfAbsent`, `compute`, `merge`)? Why are they useful?**

- They allow complex operations to be performed as a single atomic unit, preventing race conditions.






[[HashMap Implementations]]



