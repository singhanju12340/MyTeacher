---
Creation Time: Tuesday, November 12th 2024
Modified Time: Tuesday, November 12th 2024
---
### . **Column Cardinality**

- This is a measure of the number of unique values in a column compared to the total number of rows.
- **High Cardinality**: Columns with many unique values, such as user IDs or email addresses. These columns are often good candidates for indexing.
- **Low Cardinality**: Columns with few unique values relative to the number of rows, such as gender or status columns (like `active/inactive`). These may not benefit from indexing in the same way as high-cardinality columns.

### . **Importance of Cardinality in Database Design**

- **Indexing**: High cardinality columns are often good candidates for indexing to speed up query performance.
- **Query Optimization**: Understanding cardinality helps optimize queries. For example, low-cardinality columns may not need indexing since it might not significantly improve query performance