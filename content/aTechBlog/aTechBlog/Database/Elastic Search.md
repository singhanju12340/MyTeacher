Query types and patterns:

**Bool**: Used for complex query.  we can combine AND, OR and NOT query 

**Term**:  it is used to find exact match of terms in keyword field. It works on non analysed field & keyword.

**Match** :  Text search on analysed fields

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
1. can perform search on various type of data: 
structured, unstructured, geo and matrix data type as well

2. possible to analyse Billions data in second
3. provides aggregates feature to analyse and explore trends and patterns
4. We can easily scale to 100s of nodes in an automatic and painless ways
5.  _Index size:_ Being able to manage huge indexes (in the order of hundreds of Gigabytes or Petabytes
6. _Throughput:_ Being able to manage various amount of simultaneous searches under a certain response time
7. JSON serialization is supported by various programming languages, and has become the standard format used by the NoSQL movement
8. Auto suggestions: The completion suggester provides autocomplete/search-as-you-type functionality
9. Schema free- when an object is indexed later with a new property, it will automatically be added to the mapping definitions
10. we can use kafka connectors to populate different data sets into elastic search indexes
11. 