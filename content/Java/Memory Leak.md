---
Creation Time: Wednesday, March 26th 2025
Modified Time: Wednesday, March 26th 2025
---
A memory leak in Java occurs when objects that are no longer needed by an application remain reachable, thus preventing the garbage collector from reclaiming the memory. Even though Java has automatic garbage collection, memory leaks can still happen when references to unused objects are inadvertently maintained

Ex: `static collections, unclosed resources, or listeners that are never removed

```JAVA
public class MemoryLeakExample {
    // A static List that holds references to objects.
    private static List<Object> cache = new ArrayList<>();

    public static void addToCache(Object obj) {
        // Each call adds an object that may not be removed later.
        cache.add(obj);
    }
}
```


`VisualVM, JProfiler, or Eclipse Memory Analyzer (MAT)` are the profiling tools used to detect memory leaks by taking heap dumps and analysing 
memory uses graphs