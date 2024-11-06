---
Creation Time: Friday, September 20th 2024
Modified Time: Friday, September 20th 2024
---
It is an extension of Postgres Database. This extension transforms Postgres into distributed, horizontally scalable database system. It is particularly  help full for heigh performance and large scale data processing.  It helps partitioning data accross multiple nodes and allows parallel query execution and faster data handling along with postgres existing features.

### Key Features
**Horizontal Scaling**: Citus shards your data across multiple nodes. which enables it to handle large datasets by distributing them across many machines.
**Parallel Query Execution**: Queries are executed parallel across different nodes which enables analytical and transactional queries execution faster.
**PostgreSQL-Compatible:** It makes easy to migrate existing postgres based application.
**Distributed Transactions:** Support distributed transaction and guarantee ACID transaction across multi nodes.
**Real Time Analysis:** Citus is optimized for real-time analytical workloads by scaling out read-heavy operations efficiently
**Multi-Tenant SaaS Applications**: Citus can distribute tenant based data across multiple nodes which helps in load balancing and efficient scaling.

CitusDB is now part of Azure as **Azure Database for PostgreSQL Hyperscale**, providing cloud-managed distributed PostgreSQL solutions