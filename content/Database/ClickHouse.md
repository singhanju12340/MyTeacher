---
Creation Time: Monday, August 19th 2024
Modified Time: Tuesday, November 12th 2024
---
ClickHouse is a true column-oriented DBMS. Whenever possible, operations are dispatched on arrays, rather than on individual values. It is called `vectorized query execution` and it helps lower the cost of actual data processing.

There are 2 ways to improve query
1. `Vectorised query execution
	1. Vectorized processing reduces the overhead of function calls and leverages the CPU's ability to work with batches of data using SIMD (Single Instruction, Multiple Data). If Data is stored column-wise (in vectors),  it is easier to perform operations like filtering or aggregation on entire columns at once
2. `runtime code generation
	1. **Efficiency**: Instead of interpreting the query (which involves parsing and executing general-purpose logic), you generate custom, optimized code specific to the query.
	2. for Query: `SELECT * FROM users WHERE age > 30;`. you would write code that interprets this query every time it runs. Instead, using runtime code generation, you create a specialized code that handles this query as efficiently as possible. For example, you can generate Java code at runtime to handle this specific query.
		1. **Generate Code**: We generate Java code to filter users whose age is greater than 30.
		2. **Compile Code**: We compile this generated code on the fly.
		3. **Execute the Code**: The compiled code is executed, and we get the filtered results.

ClickHouse uses vectorized query execution and has limited initial support for runtime code generation.

To execute queries and do side activities ClickHouse allocates threads from one of thread pools to avoid frequent thread creation and destruction. There are a few thread pools, which are selected depending on a purpose and structure of a job:

It's a heigh performance analytical database. 
- **Type**: Columnar Database
- **Best For**: Analytical workloads, real-time analytics, and large bulk data management.
- **Why Choose ClickHouse?**
    - **Fast Writes**: ClickHouse can handle large-scale insertions quickly, making it great for logging and event-driven workloads.
    - **Efficient Deletion via TTL**: Offers the ability to manage data retention through **TTL policies**, automatically removing old data without the need for explicit bulk deletion queries.
    - **Cost**: Being open-source, ClickHouse can be run on inexpensive infrastructure, keeping operational costs low.
- **Downside**: Primarily optimized for read-heavy workloads, but write operations are still performant. Suitable for analytical use cases but may not be ideal for transactional use cases.



### Indexing in ClickHouse
ClickHouse primarily uses **primary key** and **skip indexes** to optimize query performance, especially for analytical and time-series workloads
In ClickHouse, the **primary key** is not a traditional B-tree index but rather a **sorting key**. When you define a primary key on a table, the data is physically stored in **sorted order** based on that key. This helps to optimize range queries and improve performance for read-heavy workloads.

_Sparse Index:_ ClickHouse indexes are based on _Sparse Indexing_, an alternative to the B-Tree index utilized by traditional DBMSs. In B-tree, every row is indexed, which is suitable for locating and updating a single row, also known as pointy-queries common in OLTP tasks.

Database with B-Tree indexes have poor performance on high-volume insert speed and high memory and storage consumption.
On the contrary, the `sparse index` splits data into multiple _parts_, each group by a fixed portion called _granules_. ClickHouse considers an index for every granule (group of data) instead of every row. Having a query filtered on the primary keys, ClickHouse looks for those granules and loads the matched granules in parallel to the memory. That brings a notable performance on range queries common in OLAP tasks.

Ex: if we define vin as primary key instead of create time, querying all the vin data in one go will be efficiently done by clickhouse DB.
order the primary keys from **low** to **high** cardinality.


#### Example:

```
CREATE TABLE default.inventory
(

    `vin_id` UInt32,

    `name` String,

    `created_date` Date
)
ENGINE = MergeTree
ORDER BY (vin_id, created_date)
```
if you don't specify primary keys separately, ClickHouse will consider sort keys (in order by) as primary keys. Hence, in this table, `project_id` and `created_date` are the primary keys. Every time you insert data into this table, it will sort data first by `project_id` and then by `created_date`.


```
CREATE TABLE default.projects
(

    `project_id` UInt32,

    `name` String,

    `created_date` Date
)
ENGINE = MergeTree
PRIMARY KEY (created_date, project_id)
ORDER BY (created_date, project_id, name)
```

additional keys specified in the sort keys are only utilized for sorting purposes and don't play any role in indexing.`name` column is only used as the last item for sorting.



##### Partitioning Key
Unlike traditional databases, ClickHouse uses partitioning to group data logically rather than for sharding across nodes.
One of the most common practices in ClickHouse is to use a date (or time) column as the partitioning key.
```
CREATE TABLE example_table (
	event_date Date,
	user_id UInt32,
	action_type String 
) 
ENGINE = MergeTree() 
PARTITION BY toYYYYMM(event_date) -- Monthly partitions 
ORDER BY (user_id, event_date) -- Primary key, data is partitioned monthly by `event_date`.
```

**Partition key helps in** **Efficient Data Management****: When it comes to purging old data, date-based partitions allow you to quickly drop entire partitions (e.g., dropping a month at a time).



##### Skip Index
Additionally, leveraging the skip index feature intelligently helps minimize disk I/O and reduce query execution time



