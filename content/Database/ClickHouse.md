---
Creation Time: Monday, August 19th 2024
Modified Time: Friday, September 20th 2024
---
ClickHouse is a true column-oriented DBMS. During the execution of arrays (vectors or chunks of columns). Whenever possible, operations are dispatched on arrays, rather than on individual values. It is called `vectorized query execution` and it helps lower the cost of actual data processing.

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