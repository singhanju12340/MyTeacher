---
Creation Time: Thursday, September 12th 2024
Modified Time: Friday, September 20th 2024
---
1. Fetch all vins ingestion within 30 days
2. Fetch Vin History for 30 days
3. Fetch Detail of given txnId
4. fetch All TxnId history for 1 dayfetch all vin with failed ingestion at any step with in 30 days



Old Capacity Estimates:
Capacity estimates:
 Total vins: 10 lakh = 1M
 Total Audit event: 1M * 10 = 10M
 Audit event size : 800 Byte
 Total Size of Audit data = 800 * 10M Byte = 8000M Byte
 Audit data Size per day: 8000M Byte = 8M KB = 8 * 10^6 KB = 8000MB ~ 8 GB
 Per month audit data= 240GB 
 >>> we were working on reducing event payload size by removing duplicate data and only keeping meta data, also not audting in case of replay. 
 

New Adhoc Capacity estimates:
 total vins: 30 lakh = 3M
 total event: 30*10 = 300 lakh = 30M 

while resyncing and adding new inventory total events at audit service: 

Event : 30~32M  inventory/ day
Total Audit event per day: 320M
Audit event size : 800 Byte
1 inventory has avg 10 audit event
Total Size of data = 800 * 10 * 30M new audit data per day

Indexes defind on created time to perform delete operation
1. CreateTime
2. TransactionId
3. Vin

Problems: 
index sizes:
![[Screenshot 2024-09-12 at 10.57.42 AM.png]]Even if your reads are using indexes, even loading index to RAM will take time

Total document currently -> 811057370
storage size: 195GB
Total size: 1.02TB
Index Sie: 50GB

## Problems : 
**Q:** Actually it keeps autoscaling to M40 due to high CPU utilisation. Inserts in this collection are showing up to be consuming highest time/resources
**Ans:** If your application is inserting large volumes of data at a high rate, the CPU might be overwhelmed by constant write operations. 
1. **Batch Inserts**: Instead of inserting one document at a time, use bulk operations (e.g., `insertMany()` in MongoDB). This reduces the overhead of multiple network calls and improves overall efficiency.
2. Having too many or inefficient **indexes** on a collection can significantly slow down insert operations because MongoDB has to update the indexes on each insert.
3. **Reduce Write Concern**: If strong consistency isn't critical, try lowering the write concern (e.g., `w:1`) to reduce the burden on the cluster.
4. MongoDB’s **WiredTiger storage engine** uses compression by default, which reduces disk I/O but can increase CPU utilization, especially for write-heavy workloads

**Q:** however, on the overall cost, I’m still doing some analysis. Looks like backup costs are very weird for this cluster. I’m still doing the analysis with support. Will let you know of my findings in a week or so. But it may be that this high write in audit DB is posing a cost issue
**Ans:** 
When you delete large amounts of data from MongoDB but still notice that your backup costs remain high, several factors could be contributing to this issue.
	 1. **Backups Retain Deleted Data Until Snapshot Expiry**
	 2. **Oplog Data in Continuous Backups**
	 3. **Data Fragmentation (WiredTiger Compression)**:Even after you delete data, space is not always immediately reclaimed in the database files. While this affects database size more than backup size, **backups might include fragmented space** depending on how the data is physically stored.
	 4. **Old Backup Snapshots Still Stored**
	 5. **Inefficient Backup Strategy**
	 6. **Delayed Reflection of Data Deletions in Backups**
	 
### Explore Other database suitable for this use case

**For massive writes and bulk deletions** with minimal costs and operational overhead, **DynamoDB**, **Cassandra**, and **Google Bigtable** are excellent options if you’re looking for managed solutions. If you prefer self-hosted solutions, **ClickHouse**, **Cassandra**, and **ScyllaDB** are worth considering.

####   ClickHouse
- **Type**: Columnar Database
- **Best For**: Analytical workloads, real-time analytics, and large bulk data management.
- **Why Choose ClickHouse?**
    - **Fast Writes**: ClickHouse can handle large-scale insertions quickly, making it great for logging and event-driven workloads.
    - **Efficient Deletion via TTL**: Offers the ability to manage data retention through **TTL policies**, automatically removing old data without the need for explicit bulk deletion queries.
    - **Cost**: Being open-source, ClickHouse can be run on inexpensive infrastructure, keeping operational costs low.
- **Downside**: Primarily optimized for read-heavy workloads, but write operations are still performant. Suitable for analytical use cases but may not be ideal for transactional use cases.

Understand more about [[ClickHouse | ClickHouse DB]]





![[auditServiceHLD.png]]