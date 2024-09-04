---
Creation Time: Monday, July 29th 2024
Modified Time: Monday, August 12th 2024
---
User table 

| id  | name | age | bio     | total_blogs |
| --- | ---- | --- | ------- | ----------- |
| 4B  | 60 B | 4B  | 128B    | 4B          |
| 1   | anju | 32  | nasdjns | aadjasnjk   |
1 row 200B
let 100 rows, total size = 200 * 100 B 

### How read happens:
one disk read = one block read of sample size 600B
1 GB hard disk is split into 600B size blocks
1 read will load complete block which contains that data.


![[Screenshot 2024-07-29 at 5.38.37 PM.png]]

Each record is 200B, Block size= 600B, total 100 records
Each block can hold 3 record. 
100/3 = 33.33 = 34 blocks will be needed to store 100 records.

assume 1 sec to read one block
so total 34 sec will be needed to read all the records i.e 34 blocks

### Example query without index:
> Select User where age>23
> **steps involved:**
> 	iterate table row by row i.e block by block
> 	read block in memory
> 	check if age>23 in each record of that block
> 	if yes, add record to output buffer
> 	if no, discard that row
> 	return the output buffer

Time taken: Time taken to read all the blocks 34 Sec

### With Index:
user_age_index

| age(4B) | rowId(4B) |
| ------- | --------- |
| 21      | 2         |
| 22      | 3         |
| 22      | 5         |
| 23      | 1         |
| 23      | 4         |
| 24      | 6         |

Total index size: 4+4 = 8 * 100 = 800B
600B for 1 block
2 blocks needed for complete index

**Steps involved:**
> Iterate index (worst case complete index block by block)
> Check if entry has age>23
> If yes add id in buffer
> If no discard
> For all id from the buffer
> 	read all id from disk
> 	add it to buffer
> 	return buffer
> 	


for this data, we only need to read 4(2 for index, 2 for data) block instead of 34 blocks. so it will take 4 sec.

![[Screenshot 2024-07-29 at 7.30.12 PM.png]]



# Think about big size data



# Indexing on distributed database

Example:
**blog database**

| id  | author | category | title  | body  |
| --- | ------ | -------- | ------ | ----- |
| 1   | u1     | mysql    | title1 | body1 |
| 9   | u2     | go       | title  | body  |
| 8   | u3     | ngnix    | title  | body  |
| 7   | u3     | mysql    | title  | body  |

index on author

data is stored in 2 shards, and sharded based on authorId
![[Screenshot 2024-07-29 at 8.10.31 PM.png]]

if we have to fetch all books with category  mysql
	select * from blogs where category = "mysql" 
it will need to fanout query on all the shards and scan complete DB.
	if any of the shard is slow, it will wait for all the shards to respond.
	
To solve this issue GSI Global Secondary Indexes comes into picture.
Create Global index for these kinds of data patterns.
Data will be partitioned based on the category attribute of the blog.
Some DB like dynamo already provide this feature

![[Screenshot 2024-07-29 at 8.15.37 PM.png]]

There should be limit on GSI, else write operation will become too heavy



# If your query pattern is designed to fetch all data belonging to given category for given Author.

Data is shared based on authorId. here partition key is present in query where clause.

We can create LSI local secondary index, this index data will be stored on that particular shard which will store data on the node keeping data of that author. 

instead of fanning out query on all the shard it will go to particular shard, fetch row detail from LSI and then fetch actual record.