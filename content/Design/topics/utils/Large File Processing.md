---
Creation Time: Tuesday, April 29th 2025
Modified Time: Tuesday, April 29th 2025
---
https://www.reddit.com/r/SystemDesignConcepts/comments/1f3ujcc/handling_file_operations_in_system_design/


### Requirements:
1.  Reading from multiple files, aggregating data, and writing it to a database.
2.  Exporting a database table to files efficiently.
3. Aggregating Data from Multiple CSVs

### NFR
.  Designing a file-sharing application where files have a max size of 4MB, an average size of 4KB, and the system needs to handle 200 million requests per second.



##### Issues:

1. File Sharing Application: Initially, I focused on splitting files into chunks for reading, but I realized that given the small file size, processing them in one request is more efficient. The real challenge lies in handling the high number of read requests per second, not the file size itself.
    
2. Exporting from a Database: I considered parallel exporting by having multiple threads, each reading and writing 1000 rows to separate files. However, I wasn’t sure how database engines handle concurrent reads and whether merging the files should be done in memory or on disk for optimal performance.
    
3. Aggregating Data from Multiple CSVs: I processed the CSVs line by line, streaming the data to a message queue for aggregation. However, I realized that to aggregate the data correctly, you need to read all files first, as a record might appear in multiple files with the same ID.



4. Aggregating Data from Multiple CSVs

Problem Focus: Ensuring data consistency across multiple files for aggregation.

Considerations:

Data Deduplication: Handle duplicate IDs across files by maintaining a hash map or set of processed IDs.

File Indexing: Create an index or metadata file with offsets to quickly locate records.

Batch Processing: Read files in manageable batches to limit memory usage.

Message Queue: Use a message queue (e.g., Kafka) to buffer and sequence data for aggregation.

Challenges:

Data Ordering: If aggregation requires order (e.g., timestamps), ensure all files are sorted or merge-sort them during processing.

Latency: Streaming data to a queue introduces slight delays, but it improves fault tolerance.

Fault Tolerance: Ensure the process can resume from failures without reprocessing completed records.

Options:

Use distributed frameworks like Apache Spark or Hadoop for large-scale CSV aggregation.

Preprocess files into a unified format or a database for efficient querying.