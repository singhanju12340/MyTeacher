---
Creation Time: Thursday, February 27th 2025
Modified Time: Thursday, February 27th 2025
---
_Sol 1:_
Load file through bufferReader into InMemory Hash Set.

_Problems_
Large files data may not fit in 1 machine and system can result into out of memory

_Sol2:_
Use external sorting or Disk based hash table implementations, which can keep data in disk instead of RAM.
In Java, you can use libraries such as MapDB, RocksDB, or LevelDB to achieve this.

```Java
import org.mapdb.DB;
import org.mapdb.DBMaker;
import org.mapdb.Serializer;

import java.util.concurrent.ConcurrentMap;

public class DiskBasedHashTableExample {

    public static void main(String[] args) {


        // Create or open a database stored in a file named "diskMap.db".
        DB db = DBMaker
                .fileDB("diskMap.db")
                .fileMmapEnable()  // Enables memory-mapped files for fast disk access.
                .transactionEnable() // Enable transactions if needed.
                .make();

        // Create or open a disk-based hash map named "myMap".
        ConcurrentMap<String, String> diskMap = db
                .hashMap("myMap", Serializer.STRING, Serializer.STRING)
                .createOrOpen();
		
		try (BufferedReader br = new BufferedReader(new FileReader(inputFile))) {
			String line; 
			while ((line = br.readLine()) != null) { 

				diskMap.put("key1", line);

			 } 
		}

        // Retrieve and print a value.
        System.out.println("key1: " + diskMap.get("key1"));

        // Update a value.
        diskMap.put("key1", "newValue1");
        System.out.println("Updated key1: " + diskMap.get("key1"));

        // Remove a key.
        diskMap.remove("key2");
        System.out.println("After removal, key2: " + diskMap.get("key2")); // Should print null.

        // Commit changes and close the database.
        db.commit();
        db.close();
    }
}

```
_Adv:_ 
Easier to set up than a full distributed system like Flink, and works within a single application.
Provides a familiar Map interface, similar to Java’s HashMap, with on-disk storage.
Memory-mapped files can offer good performance without requiring all data in memory.

`Disadvantage: `
1. processing will be slow compared to in memory hash tables.
2. it may not scale to extremely large datasets or high-concurrency scenarios as efficiently as distributed systems. It runs in a single JVM, so you’re limited by the resources of that machine.
3. You must handle eviction, duplicate removal, and possibly manual data flushing.

**For Moderate Datasets with Limited Memory:**  
A **disk-based hash map using MapDB** is a good fit if you want to avoid memory limitations without the complexity of distributed processing. for  applications that run on a single node but need disk-based storage.

_Sol3_
Use Embedded or external datastore with unique constraints to filter duplicates.
_Adv_
Easy to use

`Dis`: 
I/O Overhead
Operational Complexity  Managing connections, schema evolution, and transactions can add complexity.
An embedded DB like SQLite works well for moderate datasets, but for larger-scale systems you might need a full-fledged external database, which can require additional infrastructure.

_Sol4_
Use cloud solutions like Apache spark or Apache flink which is build to process this kind of large files.

**For Massive, Real-Time, Distributed Data:**  
**Apache Flink** is best if you need to process streaming data or handle huge datasets across multiple nodes.