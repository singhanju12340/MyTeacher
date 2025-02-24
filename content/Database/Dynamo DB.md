---
Creation Time: Wednesday, October 9th 2024
Modified Time: Monday, February 24th 2025
---
_It is a fully-managed, highly scalable, key-value service provided by AWS. It can be majorly used for caching usecases.

DynamoDB tables are defined by a primary key, which can consist of one or two attributes:
primary key = {*Partition Key}:{*Sort Key*}

![[Screenshot 2024-10-09 at 11.09.04 PM.png]]



_**Suitable for high read and write:
ex: leader board, IOT data, product catalogue, shopping cart, session data 


### Knowing its limitations

**Cost Efficiency**: DynamoDB's pricing model is based on read and write operations plus stored data, which can get expensive with high-volume workloads.

**Complex Query Patterns**: If your system requires complex queries, such as those needing joins or multi-table transactions, DynamoDB might not cut it.

**Data Modeling Constraints**: DynamoDB demands careful data modeling to perform well, optimized for key-value and document structures. If you find yourself frequently using Global Secondary Indexes (GSIs) and Local Secondary Indexes (LSIs), a relational database like PostgreSQL might be a better fit.

**Vendor Lock-in**: Choosing DynamoDB means locking into AWS.


Example cost: 2M writes a second of ~100 bytes would cost you about 100k a day.