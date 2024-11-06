---
Creation Time: Tuesday, October 8th 2024
Modified Time: Tuesday, October 8th 2024
---
**Change Data Capture (CDC)** is a technique used to track and capture changes in data as they happen in a database or data store. CDC allows applications to detect and react to changes (inserts, updates, and deletes) in real-time without needing to frequently query the entire dataset. This technique is essential for use cases like data replication, syncing databases, auditing, or driving real-time analytics.

### Why Use Change Data Capture?

- **Real-time data integration**: CDC enables real-time syncing between databases, ensuring up-to-date information in multiple systems.
- **Analytics**: Helps push data changes to analytics systems, enabling real-time dashboards and reports.
- **Data Migration**: CDC is often used in data migration processes where data is moved incrementally between systems without downtime.
- **Audit and Logging**: Capturing every change to maintain a log for auditing and compliance purposes.


How Change Data Capture Works
1. **Database Logs** (Log-based CDC):
	- **Transaction Logs**: Most databases maintain transaction logs (WAL in PostgreSQL, binlog in MySQL) where changes are recorded. CDC reads these logs to capture changes.
	- **High Efficiency**: This method is efficient because it doesn’t require querying the database for changes; it simply reads the logs where all changes are already recorded.
	- **Example Tools**: Debezium (open-source CDC tool), AWS DMS, Oracle GoldenGate.
-2 . **Triggers** (Trigger-based CDC):
	**Database Triggers**: Special triggers are placed on the tables to track changes. When data is updated, the trigger captures this change and writes it to a special CDC table or logs.
	**Moderate Efficiency**: Triggers may introduce overhead on the database, especially for high-throughput systems, as they fire on every write operation.
3. **Polling** (Table Diff-based CDC):
	1. - **Polling for Changes**: The system periodically queries the database to check for changed rows based on timestamps or versioning columns.
		- **Less Efficient**: Polling the database constantly to detect changes can be resource-intensive, especially on large datasets.