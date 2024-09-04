---
Creation Time: Thursday, August 8th 2024
Modified Time: Thursday, August 8th 2024
---

**Definition:**

- Partitioning is the process of dividing a single database table into smaller, more manageable pieces called partitions. Each partition is stored within the same database but as a separate table or file.

**Purpose:**

- It is used to improve performance and manageability of large tables by splitting them into smaller, more manageable parts.

**Implementation:**

- Data is divided based on a partition key, which determines the distribution of data across different partitions.
- Partitions can be created based on various strategies such as range, list, hash, or composite.

**Characteristics:**

- Partitions typically reside on the same database server or cluster.
- All partitions are part of the same database schema.
- Queries can be optimized to target specific partitions, reducing the amount of data scanned.

**Use Cases:**

- Databases with very large tables where querying and managing data becomes inefficient.
- Scenarios requiring efficient access to specific subsets of data, such as time-series data, regional data, or categorical data.

**Advantages:**

- Improved query performance by reducing the amount of data scanned.
- Easier management of large tables by operating on partitions independently.
- Enhanced maintenance tasks such as backup, restore, and archiving on a per-partition basis.

**Disadvantages:**

- Limited scalability compared to sharding, as partitions usually reside on the same server.
- Potential complexity in partition management and maintenance.
- Requires careful design of the partition key to ensure even data distribution.

**Type of partitioning:**

1. Horizontal
2. Vertical