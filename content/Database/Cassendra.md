
Apache Cassandra is a highly scalable and distributed NoSQL database which can handle large amounts of data across multiple nodes and datacenters.

 Cassandra is a wide-column store NoSQL database, in which unlike relational databases, data is stored in a column-family format, which means each row has a unique key while column name and format can vary across rows.

![[Screenshot 2024-07-08 at 6.14.18 PM.png]]

![[Screenshot 2024-07-08 at 6.13.55 PM.png]]


Primary key of Cassendra is combination of partition key and clustering key
Create table Player{
	playerId,
	name,
	countryId
	PRIMARY_KEY((countryId), playerId)
}
here countryId is partition key and playerId is an clustering key.

*  Partitioner uses a consistent hashing algorithm to convert this key into tokens which is then used to determine where the data will reside on the ring(cluster of nodes). This is the reason why querying for data in Cassandra is super-efficient, because it knows where exactly the data is going to be *
* Clustering key is the second part of primary key used to ensure uniqueness to primary key in order to avoid collision, and to sort the data inside a partition. In the above example, we can see that data is being sorted by player_id inside each partition.
* 
![[Screenshot 2024-07-08 at 6.28.56 PM.png]]

![[Screenshot 2024-07-08 at 6.34.20 PM.png]]