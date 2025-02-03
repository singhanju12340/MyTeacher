---
Creation Time: Saturday, January 18th 2025
Modified Time: Sunday, January 19th 2025
---

```java
func getId(){
	 return get_epoc_ms();
}
```

**Issue** 1: while multiple threads innovating the same function, same id will be returned to multiple thread
_Sol: append thread id in generated Id_
**Issue 2**: The distributed system will have multiple computer which will generate the same id
_Sol: Append node Id in generated ID
```java
func getId(){
	 return concat(nodeId, get_epoc_ms());
}
```


**Issue 3*: The same node will call this method twice in the same milisec
_Sol: Use static counter and append on every function call_
```java
func getId(){
static int counter=0;
	 return concat(nodeId, get_epoc_ms(), counter++);
}

```
Issue 4: If the computer reboots the counter will restart to 0
_Sol: store counter in the disk but I/O operation will be expensive and time consuming_



_**UUID_
128 bit unique non sequential id generator, but it takes too much space to store id
EX: UUID: 123e4567-e89b-12d3-a456-426655440000
- MSB (bits 0..63): `0x123e4567e89b12d3`
- LSB (bits 64..127): `0xa456426655440000`

`Should id be monotonically increasing?`
_Yes it will insure the latest id to update existing data when ever conflicts happens, but this can be issue in distributed system_

What if different nodes will have different clock cycle to generate time? Clock cycle can change because of multiple issues like hardware heating, cpu slow processing, slow network etc clocks cycle resync after next clock resync in distributed system.

Node 1:  time- 23
Node 2: time- 24
Node 3: time- 23

request came to all the nodes at single time
Id: on Node1: 231
Id: on Node2: 242
Id: on Node2: 233

Not monotonically increasing due to different clock cycle in the distributed nodes



`Different nodes handling id generation is a problem, What if we create id generation service separately?
But it also increase network load to get Id whenever needed

What if we get list of ids from ID-Server in 1 go and fetch next slot after exhausting the existing one.

### Explore Existing Solutions:

`UUID
128 Bit Hexadecimal too long
space consumed too much
inefficient indexing on ID due to its size, all indexes will not fit in memory at once. If you can not keep/ fetch all index in memory, you can not make database call fast.
chances of collision_

`MONGO DB ID generator`
12 byte  - 96 bit hexadecimal

_12 byte id pattern_

| epoc sec | __ machine id __ | __  process id __ | counter |
| -------- | ---------------- | ----------------- | ------- |
| 4 Byte   | 3 Byte           | 2 Byte            | 3 Byte  |
Mongo DB Change above implementation to bellow because generally, mongo id nodes are 1/3 so machine Id can get wasted because of same repeated Ids, and this could better utilise 3 byte space.

| epocsec | Randon id | Counter ID |
| ------- | --------- | ---------- |
| 4       | 5         | 3          |




### Id Generation case studies
1. _**Flicker**_: Database Ticket server, needed Monotonic increasing ID
	Flicker uses Mysql shared DB with master-master replica(each node generating id), so auto incremented Id had collision problem, because each system generated different auto increment id which collide any time.
	`So they had to inject ID Generator
	they did not GUID because of its big id size.
Instead of having ID generating service they use Database as database also provide id generation logic.

`Database Ticket system
Leverage DB id generation and use generated id for other tables, but this becomes very specific to database.
Example queries
```
Create table ticket{
	id bitInt,
	stub Char(1) NOT NULL DEFAULT
	PRIMARY KEY(id)
	UNIQUE KEY 'stub' ('stub')
}
```
Command to run to get new ID, for every service('a'), it will replace existing entry with new entry containing incremented counter.
```
REPLACE INTO ticket (`stub`) values (`a`); // delete existing 
			 OR
INSERT INTO tickets (`stub`) values (`a`) ON DUPLICATE KEY UPDATE ID = ID+1;		// only update more faster than replace
```

Issue with this:
One DB will have too much request overloaded
_Sol_
One ticket for even number generation
One ticket for Odd number generation

If server breaks down, they restart with after adding some buffer to last generated numbers

What if 64 bit exhausted: 1.8 * 10^19 numbers		

_**Twitter **_
Twitter can reach 64 bit number and too height write throughput
Twitter uses `SNOWFLAKE` 64 bit integers, It do not reli on central id generation server. rather app `Api server generates ID` on its own and save it in DB
It do not uses strict monotonically increasing IDs. but each api server gives unique id

| Epoc time in MS(Epoc after 2001/when company started) | machine Id(api server id)                        | per machine sequence no |
| ----------------------------------------------------- | ------------------------------------------------ | ----------------------- |
| 41 bit                                                | 10 bit(`1024 API server can run with unique id`) | 12 bit                  |
`Reason of twitter to be scalable because it was not dependent on single centralised service`


If client like Javascript do not support 64 bit integer id are given as string.
If these numbers are exhausted, per machine sequence no will auto rotate from 0.
`This approach gives twitter a  way to fetch twitter before or after a certain ID or time

_**Instagram**_
also uses `SNOWFLAKE` with some modification 

| epoc                          | shard ID                   | sequence No |
| ----------------------------- | -------------------------- | ----------- |
| 41 (starts from 1st Jan 2011) | 13 (logical shard numbers) | 10          |

1. Its requirement was Id should be sortable
2. Id index should be stored in memory(64 bit can be)

They used stored procedure to fetch latest id

```
Create table insta5.photo_table{
"id" bitint NOT NULL DEFAULT insta5.nextId() // procesure call
}

insta5 is logical shard id
insta5.nextId()  it is a procedure call
```
While storing photo it will invoke procedure call and get ID value. this procedure call will given snowflake id

_Implementation of procedure call
```
BEGIN
	Select nextInterval ('insta5.phototable_id_seq) %% 1024 as seq_id // it is database imokemented seq no which will be added in last 10 bits
	current_ms = 
	result = (current_ms - epoc )
	result = result | (shard_id << 10)
	result = result | seq_id
END
```

_**Discord**_
`SNOWFLAKE`
Epoc: 2015 because it was founded in 2015


_**Sony_
`SONYFLAKE`



