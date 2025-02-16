---
Creation Time: Monday, July 8th 2024
Modified Time: Tuesday, October 8th 2024
---
_Keywords_ Keywords are non analysed fields. if complete field value has to be used for matching while query we can mark it as keyword.
_Text field_ Text fields are analysed fields. When a field is marked as analyzed, Elasticsearch uses an analyzer to
Tokenize the field's content into individual terms
### Analyzed vs. Unanalyzed Example
ex: 

For an analyzed `text` field:

- Field value: `"The Quick Brown Fox"`
- Tokens generated: `[the, quick, brown, fox]`

For an unanalyzed `keyword` field:

- Field value: `"The Quick Brown Fox"`
- Tokens generated: `[The Quick Brown Fox]` (stored as one token)

##### Search Criteria
Ex: book 
Query types and Patterns:

**Bool**: Used for complex query.  we can combine AND, OR and NOT query 
**Term**:  It is used to find exact match of terms in keyword field. It works on non analysed field & keyword.
**Match**:  Used for  search on analysed fields like text fields.

EX: 
```
{
	product_id,
	name: "smartPhone",
	des,
	category
}
```

```
query:
{
	match{
		name: "smartPhone"
	}
}
```

```
{
	"query"{
		"multimatch"{
			"query" : "smartPhone",
			"fields": ["name", "description"]
		}
	}
}
```


**Filter query:**

```
{
	"query":{
		"bool" :{
			"filter": [
				{"term":{"in-stock":"true"}},
				{"range":{"rating":{"gt:4}}}
			]
		}
	}
}
```


How does ES works:

ES works based on inverted index, each word in the document is kept as a key and values as the pointing to the document location

Elastic search is a search engine built on apache lucene. It is an open source and developed in Java. It is a real time distributed and analytic engine which helps in performing various kinds of search mechanism. It is able to achieve fast search responses because, instead of searching the text directly, it searches an index instead. Additionally, it supports full-text search which is completely based on documents instead of tables or schemas.


### Why ES:
1. Can perform search on various type of data: 
structured, unstructured, geo and matrix data type as well

2. possible to analyse Billions data in second
3. provides aggregates feature to analyse and explore trends and patterns
4. We can easily scale to 100s of nodes in an automatic and painless ways
5.  _Index size:_ Being able to manage huge indexes (in the order of hundreds of Gigabytes or Petabytes
6. _Throughput:_ Being able to manage various amount of simultaneous searches under a certain response time
7. JSON serialisation is supported by various programming languages, and has become the standard format used by the NoSQL movement
8. Auto suggestions: The completion suggester provides autocomplete/search-as-you-type functionality
9. Schema free- when an object is indexed later with a new property, it will automatically be added to the mapping definitions
10. we can use kafka connectors to populate different data sets into elastic search indexes




## ES Search Pagination:
_1. Normal pagination is not efficient for deep search as larger set of hits needs to be discarded to select the desired details.
	_GET /my_index/_search { "from": 0, "size": 10, "query": { "match": { "title": "elasticsearch" } } }

This process can consume lots of CPU and RAM if `from` size is big. And max limit is set t0 10,000. [`index.max_result_window`]. This can cause crash of nodes as well. 
There are other 2 ways of doing deep searching in ES
`1. Scroll API with open context
	_POST /my-index-000001/_search?scroll=1m  %%This scroll=1m  defines for how long the snapshot should be live for search%%
		{
		  "size": 100,
		  "query": {
		    "match": {
		      "message": "foo"
		    }
		  }
		}
		The result from the above request includes a `_scroll_id`, which should be passed to the `scroll` API in order to retrieve the next batch of results.
		POST /_search/scroll                                                               [](https://www.elastic.co/guide/en/elasticsearch/reference/current/paginate-search-results.html#CO218-1)
		{
		  "scroll" : "1m",                                                                 [](https://www.elastic.co/guide/en/elasticsearch/reference/current/paginate-search-results.html#CO218-2)
		  "scroll_id" : "DXF1ZXJ5QW5kRmV0Y2gBAAAAAAAAAD4WYm9laVYtZndUQlNsdDcwakFMNjU1QQ==" [](https://www.elastic.co/guide/en/elasticsearch/reference/current/paginate-search-results.html#CO218-3)
		}

Scrolling _is not intended for real time user requests,_ but rather for processing large amounts of data, e.g. in order to reindex the contents of one data stream or index into a new data stream or index with a different configuration.

The results that are returned from a scroll request reflect the state of the data stream or index at the time that the initial `search` request was made, _like a snapshot in time_. Subsequent changes to documents (index, update or delete) will only affect later search requests

	`  Scroll requests have optimizations that make them faster when the sort order is `_doc`. If you want to iterate over all documents regardless of the order, this is the most efficient option:`
GET /_search?scroll=1m
{
  "sort": [
    "_doc"
  ]
}

. Default context alive time= 500, context time is not the time to keep context alive for querying all the data, its only time needed to fetch one batch, after each scroll query this time is reset.
. Max number of context can open on index is = 500. i.e: search.max_open_scroll_context = 500
. There are ways to delete context single or multiple.

	`Memory challanges for Scroll query:
background merge process optimise index by merging smaller segments to create new big and deleting smaller segments. this process continue when scroll is getting executed, but scroll search prevents deleted of those unused segments because this are still in use by scroll API snapshots.  To ensure proper node functioning we need to configure node with extra free file handles.  
Ensure that your nodes have sufficient heap space if you have many open scrolls on an index that is subject to ongoing deletes or updates.

`1. Search After API 

GET twitter/_search
{
    "query": {
        "match": {
            "title": "elasticsearch"
        }
    },
    "search_after": [1463538857, "654323"],
    "sort": [
        {"date": "asc"},
        {"tie_breaker_id": "asc"}
    ]
}

If a [refresh] occurs between these requests, the order of your results may change, causing inconsistent results across pages. To prevent this, you can create a [point in time (PIT)] to preserve the current index state over your searches

_POST /my-index-000001/_pit?keep_alive=1m

its similar to scroll=1m

Uses of search_after query with PIT params is called `cursors based pagination`. Using PITs with search_after provides a consistent view of the data throughout the pagination process, even if the underlying index is being updated


### ES Distributed cluster architecture:
There are 5 types of nodes configuration in ES
1. Ingestion Node
2. Master Node
3. Data Node
4. Coordinating Node
5. Machine Learning Node

```plantuml

Client -> IngestionNode : Indexing request
IngestionNode -> IngestionNode : Process document to be saved by ingestion pipeline
IngestionNode -> DataNode1 : Forward processed document
IngestionNode -> DataNode2 : Forward processed document replica


Client -> CoordinatingNode : Send search request
CoordinatingNode -> DataNode1 : Query relevant shard
CoordinatingNode -> DataNode2 : Query relevant shard
DataNode1 -> CoordinatingNode : Return partial result
DataNode2 -> CoordinatingNode : Return partial result
CoordinatingNode -> CoordinatingNode : Merge result
CoordinatingNode -> client : return result

```
One of the most important steps in execution on a coordinating node is query planning. **Query planners** are algorithms that determine the most efficient way to execute a search query. After a query is parsed by the coordinating node, the query planner evaluates how to best retrieve the relevant documents. This involves deciding whether to use an inverted index, determining the best order to execute parts of the query, and orchestrating how results from multiple nodes should be combined.


