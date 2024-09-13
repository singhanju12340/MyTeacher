---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---
## Search index reindexing challenges due to Schema changes:

Reindexing is very common  and quick process we always used to do in our ingestion systems, to accomodate quick search feature extension 
ideally reindexing do not take much time. 

total inventory count now: 3116724
POST _reindex { "source": { "index": "source_index" }, "dest": { "index": "destination_index", "op_type": "create" } }
### Use Cases for `op_type` During Reindexing

1. **`op_type: index` (Default)**:
    
    - Use this if you want the new data from the source index to overwrite any existing documents in the destination index. It ensures that the latest version of the document is in the destination index.
    - **Use Case**: When refreshing or restructuring an index, and you want to ensure the destination index contains the latest data, regardless of what's already in it.
2. **`op_type: create`**:
    
    - Use this if you want to **only create new documents** in the destination index and **skip existing documents**. If the document ID already exists in the destination index, it will be skipped, avoiding accidental overwrites.
    - **Use Case**: When migrating or merging data from multiple indices into one, and you don’t want to overwrite existing records in the target index.
### . **Ignoring the Conflict (Allow Overwriting)**

If your application logic doesn't need to worry about strict version control, you can simply ignore the conflict during reindexing. This can be done by adding the `"conflicts": "proceed"` option to the reindex request. Elasticsearch will continue reindexing without failing the whole process if a conflict occurs, but the conflicting document will not be updated.

POST _reindex { "conflicts": "proceed", // Ignore version conflicts "source": { "index": "source_index" }, "dest": { "index": "destination_index" } }

though there are alternative ways to reindex it using replay api, but that is more time taking 

## Kafka consumer lag issue due to wrong commit time

## Kafka and Mongo replay issue
