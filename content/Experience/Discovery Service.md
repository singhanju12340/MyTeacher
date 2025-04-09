---
Creation Time: Thursday, March 13th 2025
Modified Time: Wednesday, April 9th 2025
---
2.5 k RPM

Facets API response: 700-900 ms
Search api response: 100 - 200 ms
matches response: 110 ms
details api response: 30 -40 ms

Total inventory count: 5.5M

4 nodes 1TB each and 25 shards per node, the car inventory is horizontally partitioned into 100 shards. Each shard holds a fraction of the total inventory, determined by a hash function on the document's key. This allows Elasticsearch to efficiently distribute both data storage and query processing across the cluster, enabling scalable, high-performance search and retrieval in a large dataset.

- **Pros of 100 Shards:**
    - Each shard is relatively small (around 55,000 documents), which can lead to faster searches within a shard.
    - High parallelism if queries can target multiple shards simultaneously.
- **Cons of 100 Shards:**
    - Increased overhead for managing many shards (memory, file descriptors, coordination).
    - If every query must scan all 100 shards, the coordination overhead might negatively impact performance.


4 nodes and 25 shards per node, the car inventory is horizontally partitioned into 100 shards. Each shard holds a fraction of the total inventory, determined by a hash function on the document's key. This allows Elastic search to efficiently distribute both data storage and query processing across the cluster, enabling scalable, high-performance search and retrieval in a large dataset.



How much read traffic in VSR and VDP
What matrix we take care for elastic search DBs
How we maintain fault tolerance
### Elastic search production level things we should take care.

Define nodes based on roles:  Master, data, ingest, coordinating to distribute responsibilities effectively.

#### Tools to Monitor Metrics
**Elasticsearch's _cat APIs:**
- For quick, human-readable summaries (e.g., `_cat/health?v`, `_cat/indices?v`, `_cat/allocations?v`).
- Use kibana dashboards for real-time monitoring of cluster, node, and index performance.
- Proactive monitoring helps in early detection of issues such as unassigned shards, high GC times, or unexpected query latencies, allowing you to take corrective action before they impact production




- 


![[Screenshot 2025-04-09 at 12.46.03 PM.png]]


Custom Analyser
```
"analysis" : {
          "filter" : {
            "french_stop" : {
              "type" : "stop",
              "stopwords" : "_french_"
            },
            "fuel_type_en_stop" : {
              "ignore_case" : "true",
              "type" : "stop",
              "stopwords" : [
                "_english_",
                "fuel",
                "type",
                "fuel-type"
              ]
            },
            "drive_type_en_stop" : {
              "ignore_case" : "true",
              "type" : "stop",
              "stopwords" : [
                "_english_",
                "drive",
                "type",
                "wheel",
                "drive-type",
                "wheel-drive"
              ]
            },
            "french_vehicle_synonyms" : {
              "type" : "synonym",
              "synonyms_path" : "fr_vehicle_synonyms.txt",
              "updateable" : "true"
            },
            "french_elision" : {
              "type" : "elision",
              "articles" : [
                "l",
                "m",
                "t",
                "qu",
                "n",
                "s",
                "j",
                "d",
                "c",
                "jusqu",
                "quoiqu",
                "lorsqu",
                "puisqu"
              ],
              "articles_case" : "true"
            },
            "french_stemmer" : {
              "type" : "stemmer",
              "language" : "light_french"
            },
            "vehicle_synonyms" : {
              "type" : "synonym",
              "synonyms_path" : "en_vehicle_synonyms.txt",
              "updateable" : "true"
            }
          },
          "analyzer" : {
            "english_search_analyzer" : {
              "filter" : [
                "lowercase",
                "stop",
                "vehicle_synonyms"
              ],
              "tokenizer" : "standard"
            },
            "english_edge_ngram_analyzer" : {
              "filter" : [
                "lowercase",
                "stop"
              ],
              "tokenizer" : "edge_ngram_tokenizer"
            },
            "english_fuel_type_edge_ngram_analyzer" : {
              "filter" : [
                "lowercase",
                "fuel_type_en_stop"
              ],
              "tokenizer" : "standard"
            },
            "french_search_analyzer" : {
              "filter" : [
                "lowercase",
                "french_stop",
                "french_vehicle_synonyms"
              ],
              "tokenizer" : "standard"
            },
            "english_drive_type_edge_ngram_analyzer" : {
              "filter" : [
                "lowercase",
                "drive_type_en_stop"
              ],
              "tokenizer" : "standard"
            },
            "french_edge_ngram_analyzer" : {
              "filter" : [
                "french_elision",
                "lowercase",
                "french_stop",
                "french_stemmer"
              ],
              "tokenizer" : "edge_ngram_tokenizer"
            }
          },
          "tokenizer" : {
            "edge_ngram_tokenizer" : {
              "token_chars" : [
                "letter",
                "digit"
              ],
              "min_gram" : "3",
              "type" : "edge_ngram",
              "max_gram" : "20"
            }
          }
        }
```