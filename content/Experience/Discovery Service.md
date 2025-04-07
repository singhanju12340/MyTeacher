---
Creation Time: Thursday, March 13th 2025
Modified Time: Thursday, April 3rd 2025
---
2.5 k RPM

Facets API response: 700-900 ms
Search api response: 100 - 200 ms
matches response: 110 ms
details api response: 30 -40 ms

Total inventory count: 5.5M

4 nodes 1TB each and 25 shards per node, the car inventory is horizontally partitioned into 100 shards. Each shard holds a fraction of the total inventory, determined by a hash function on the document's key. This allows Elasticsearch to efficiently distribute both data storage and query processing across the cluster, enabling scalable, high-performance search and retrieval in a large dataset.

- **Pros of 100 Shards:**
    - Each shard is relatively small (around 55,000 documents), which can lead to faster searches within a shard.
    - High parallelism if queries can target multiple shards simultaneously.
- **Cons of 100 Shards:**
    - Increased overhead for managing many shards (memory, file descriptors, coordination).
    - If every query must scan all 100 shards, the coordination overhead might negatively impact performance.


4 nodes and 25 shards per node, the car inventory is horizontally partitioned into 100 shards. Each shard holds a fraction of the total inventory, determined by a hash function on the document's key. This allows Elastic search to efficiently distribute both data storage and query processing across the cluster, enabling scalable, high-performance search and retrieval in a large dataset.



How much read traffic in VSR and VDP
What matrix we take care for elastic search DBs
How we maintain fault tolerance
### Elastic search production level things we should take care.

Define nodes based on roles:  Master, data, ingest, coordinating to distribute responsibilities effectively.

#### Tools to Monitor Metrics
**Elasticsearch's _cat APIs:**
- For quick, human-readable summaries (e.g., `_cat/health?v`, `_cat/indices?v`, `_cat/allocations?v`).
- Use kibana dashboards for real-time monitoring of cluster, node, and index performance.
- Proactive monitoring helps in early detection of issues such as unassigned shards, high GC times, or unexpected query latencies, allowing you to take corrective action before they impact production
- 


