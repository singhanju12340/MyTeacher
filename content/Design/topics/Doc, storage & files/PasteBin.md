---
Creation Time: Tuesday, July 9th 2024
Modified Time: Wednesday, April 30th 2025
---

A [Pastebin](https://en.wikipedia.org/wiki/Pastebin) is an online text storage service where clients can store text files. The typical text files stored on Pastebin are source code snippets and configuration files. A unique paste ID is generated for each paste. The paste ID is used by clients to view the content of the paste.



### Candidate

1. What are the use cases of the system? 
2. What is the amount of Daily Active Users (**DAU**) for writes?
3. How many years should we persist a paste by default?
4. What is the anticipated read: write ratio of the system?
5. What is the typical usage pattern of a paste by the client?
6. Who will use the Pastebin service?
7. What is the reasonable length of a paste ID?

---

### [](https://systemdesign.one/system-design-pastebin/#interviewer)Interviewer
1. Store the paste and generate a unique paste ID
2. 1 million DAU
3. 5 years
4. 10: 1
5. Most of the pastes will be accessed only twice after the creation
6. General public
7. At most 10 characters to keep the paste ID readable


### Functional Requirements
- Online text storage service similar to pastebin.com or GitHub gist
- A **client** (user) enters text data into the system known as a paste
- A paste must not be greater than 1 MB in size
- The system must return a unique paste ID for each Paste
- The client visiting the paste ID must be able to view the paste
- The system must support only text-based data for a paste
- The paste ID should be readable
- The paste ID should be collision-free
- The paste ID should be non-predictable
- The client should be able to choose a custom paste ID
- The paste ID should generate an analytics report (not real-time) such as the total number of access to a paste
- The client should be able to define the expiration time of the paste
- The client should be able to delete a paste
- The client must be able to set the visibility of the paste (public, private)
- The client must be able to set an optional password for the paste
- A paste must be filtered by Pastebin to prevent questionable content

### Non-Functional Requirements
- High Availability
- Low Latency
- High Scalability
- Durability
- Fault Tolerant
- Pastebin is a read-heavy system.


## APIs
```js
endpoint pastebin/v1/pastes
Headers:
	method: POST
	authorization: Bearer <JWT>
	content-length: 1500
	content-type: application/JSON
	content-encoding: gzip
	cookie: id=xyz; status=abc
Request:
{
    title: "my_paste",
    visibility: "public",
    content: <Object>,
    expires: "datetime", (optional)
    custom: "paste-id"  (optional)

}


Response:

status code: 201 created
{
    id: "abcd1234",
    title: "my_paste"
}
```
```
```

```js
pastebin/v1/pastes/${paste-id}
method: GET
authorization: Bearer <JWT>
cookie: x=abc; y=def
user-agent: Chrome
referer: google.com
accept: application/json, text/html


Response:

status code: 200 OK
content-encoding: gzip
cache-control: private, no-cache, must-revalidate, expires=datetime
content-type: application/json

{
    title: "my_paste",
    id: "abcd1234",
    visibility: "public",
    created_at: "2030-10-10T12:11:42Z",
    expires_at: "2032-09-20T12:11:58Z",
    user: "Rick",
    content: {...}
}

The typical usage pattern of a paste is to access it at most twice after creation. The _cache-control_ HTTP response header is set to private to cache the response on the client.



```

```Js

v1/pastes/:paste-id
method: DELETE
authorization: Bearer <JWT>
cookie: id=xyz

Response:
204 no content/403 not authorised/ 200 done
```


## models
```
User
	id (partition key)
	name
	mail
	password
	last login
```

```
Permission
	userid
	paste_id
	created at
```

```
paste
	id
	content_location
	created
	expireat
	last visited 
	visibility
	userid
```


The reasons to choose a SQL store for storing the metadata are the following:

- strict schema
- relational data
- need for complex joins
- lookups by index are pretty fast


The paste content has larger storage requirements than the metadata of the paste. The content of a paste is stored in a managed [object storage](https://en.wikipedia.org/wiki/Object_storage) such as AWS [S3](https://newsletter.systemdesign.one/p/amazon-s3-durability). The paste (text data file) is identified using a unique key (paste URL) in object storage. An alternative to AWS S3 is using [MongoDB] or [Riak S2]



## Capacity planning
- 1 million requests/day = 12 requests/second
- round off the numbers for quicker calculations
- write down the units when you do conversions
The Daily Active Users (**DAU**) for writes is 1 million. The Query Per Second (**QPS**) for reads is approximately 100.


`Storage:
5 years in the data storage.

id: 20 bytes
title: 100 
content location: 1500
created at: 10
expire : 10
visibility: 2
lastvisited: 10
userId: 20 


metadata 1.5KB 
paste content size max: 1MB


Total space in 5 years: 12 * 60 * 60 * 24 * 365 * 5  * 1.2 MB = 15 PB
Replica: 3 = 50PB
Total paste in 5 years = 12 * 60 * 60 * 24 * 365 * 5  


Cache memory: 20:80 ratio
20 % from cache, 80 % from DB
2TB/Day cache needed


### Encoding:

Encoding is the process of converting data from one form to another.The paste ID is encoded to improve readability

| Encoding | Range name                       | TradeOff                            |                                                             |     |
| -------- | -------------------------------- | ----------------------------------- | ----------------------------------------------------------- | --- |
| Base 32  | A-V 0-9                          | Do not meet scalability requirement |                                                             |     |
| Base 36  | a-z 0-9                          | Do not meet scalability requirement |                                                             |     |
| Base 62  | a-z A-Z 0-9                      |                                     | The characters in base62 encoding consume 6 bits (2⁶ = 64). |     |
| Base 64  | Base 62 and + /                  |                                     |                                                             |     |
| Base 58  | Base 64 removing confusing chars |                                     | 0O I(capital I)l(small l)                                   |     |
## How encoding works? Example

base62(hello) = aGVsbG8=


62 characters ASCII values can  be represented in binary using 6 bits~ 2^6
So break binaries to 6 bits

| h        | e        | l        | l        | o        | char                                                           |        |
| -------- | -------- | -------- | -------- | -------- | -------------------------------------------------------------- | ------ |
| 01101000 | 01100101 | 01101100 | 01101100 | 01101111 | 1 char = 1 byte = 8 bit, represent char ascii value into 8 bit |        |
| 011010   | 000110   | 010101   | 101100   | 011011   | 000110                                                         | 001111 |
| 26       | 6        | 21       | 44       | 27       | 6                                                              | 15     |
| a        | G        | V        | s        | b        | G                                                              | 8      |



**Total count of paste IDs = branching factor ^ depth**
where the branching factor is the base of the encoding format and depth is the length of characters.
32^8 = 1Trilllion

The total count of paste IDs is directly proportional to the length of the encoded output. 
The base62 encoded output of 8-character length generates 217 trillion paste IDs
62^8 = 217 Trillion.

In summary, an 8-character base62 encoded output satisfies the system requirement.





https://systemdesign.one/system-design-pastebin/


## HLD

[[PasteBin]]

Steps taken in paste journey
1. Writes to Pastebin are rate limited
2. KGS creates a unique encoded paste ID
3. The object storage returns a [presigned URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html)
4. The paste URL ([http://presigned-url/paste-id](http://presigned-url/paste-id)) is created by appending the generated paste ID to the suffix of the presigned URL
5. The paste content is transferred directly from the client to the object storage using the paste URL to optimize bandwidth expenses and performance
6. The object storage persists the paste using the paste URL
7. The metadata of the paste including the paste URL is persisted in the SQL database
8. The server returns the paste ID to the client for future access




#### How to handle same content pasted by multiple clients

When multiple clients enter the same paste content (text), Pastebin can either store multiple copies of the paste content or reuse the same paste content across multiple clients. Pastebin must make the following changes to reuse the paste content:
- hash the paste content using MD5 or SHA-256 to aid in quick validation of the existence of a paste
- the hash of paste must be stored in a [bloom filter](https://systemdesign.one/bloom-filters-explained/) for faster lookups
- inverted index data store mapping between the hash value of the paste and paste ID
- data store mapping between the paste ID and the expiry time of the paste for each client
- lock (mutex) or semaphore to handle concurrency
 `reusing the paste content across multiple clients increases the complexity of the system. The optimal solution is to create multiple copies of the paste to reduce financial costs and operational complexity.
he potential solutions to create the paste ID are the following:
- Random ID Generator
- Hashing Function
- Token Range
- Custom paste ID


### KGS service which key generation technique should be used?
`Random key
The KGS must verify if the generated paste ID already exists in the database because of the randomness in the output. The random ID generation solution has the following tradeoffs:
- the probability of collisions is high due to randomness
- coordination between servers is required to prevent a collision
- frequent verification of the existence of a paste ID in the database is a bottleneck due to disk input/output (**I/O**)

`Snowflake Id generation
This include bellow params to uniquely generate ids
- Timestamp
- [Data center](https://en.wikipedia.org/wiki/Data_center) (DC) ID
- Worker node ID
- Sequence number

	Issues
- generated paste ID becomes predictable due to known bits
- probability of collision is higher due to the overlapping bits

`Hashing function for Paste id`
The KGS queries the [hashing function](https://en.wikipedia.org/wiki/Hash_function) service to create a paste ID. 
The hashing function service accepts ( `IP address of the client +  timestamp`)  as the input and executes a hash function(MD5)  to create a paste ID. The length of the MD5 hash function output is 128 bits. The hashing function service is replicated to meet the [scalability](https://newsletter.systemdesign.one/p/micro-frontends) demand of the system.
The hashing function service must be stateless to replicate the service for scaling easily. The ingress is distributed to the hashing function service using a load balancer. The potential load-balancing algorithms to route the traffic are the following:
- weighted round-robin
- least response time
- IP address hash
- 
Issues:
If same client id generate multiple request, chanches of collision increases
	Adding time stamp will solve it
If same client can request multiple at same time as well
	adding random in the input key of hash function can solve this.
In summary, do not use the hashing function solution for Pastebin.


`Token range KGS`  In summary, use the token range solution for Pastebin.

[[PasteBin]]

Token service must use atomic operator while fetching pasteid to avoid collision in concurrent request.
When the key-value store (DynamoDB) is chosen as the token range service, the [quorum](https://en.wikipedia.org/wiki/Quorum_%28distributed_computing%29#:~:text=A%20quorum%20is%20the%20minimum,operation%20in%20a%20distributed%20system.) must be set to a higher value to increase the [consistency](https://systemdesign.one/consistency-patterns/) of the token range service. The stronger consistency prevents a range collision by preventing fetching the same output range by multiple token services.

When an instance of the token service is provisioned, the fresh instance executes a request for an output range from the token range service. When the fetched output range is fully exhausted, the token service requests a fresh output range from the token range service.



### How to handle collision in custom paste id efficiently for this huge load
`Simple approach is:
- query the SQL database to check the existence of the paste ID
- use the putIfAbsent SQL procedure to check the existence of the paste ID
However, querying the database is an expensive operation because of the disk I/O.

`Bloo`
A [bloom filter](https://systemdesign.one/bloom-filters-explained/) is used to prevent expensive database lookups. When the client enters an already existing custom paste ID, the server must return an error message with HTTP response status code [409 Conflict](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/409).
However, the [bloom filter](https://systemdesign.one/bloom-filters-explained/) query might yield false positives, resulting in an unnecessary database lookup, which is acceptable


The following operations are executed by Pastebin when the client enters a custom paste ID that does not exist in the database:

1. KGS queries the [bloom filter](https://systemdesign.one/bloom-filters-explained/) to check if the custom paste ID already exists in the database
2. The token service creates a paste ID
3. KGS populates the [bloom filter](https://systemdesign.one/bloom-filters-explained/) with the generated paste ID
4. The server stores the paste content in object storage
5. The metadata of the paste is stored in the SQL database



## How to provide read with minimal latency ?

The egress (client requests to view a paste) is cached following the 80/20 rule to improve latency. The cache stores the paste and the relevant paste metadata. The paste ID is used as the cache’s key to identify the paste content. The cache handles uneven traffic and traffic spikes in egress. The server must query the cache before hitting the data store. The [cache-aside pattern](https://github.com/donnemartin/system-design-primer#cache-aside) is used to update the cache. When a cache miss occurs, the server queries the data stores and populates the cache. The tradeoff of using the cache-aside pattern is the delay in initial requests. As the data stored in the cache is memory bound, the Least Recently Used ([LRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_%28LRU%29)) policy is used to evict the cache when the cache server is full.


The cache is introduced at the following layers of the system for scalability:
Client level
CDN
reverse proxy (server)
internal cache on data store



## What should be the caching patterns?
After creation mostly paste are fetching twice.
Updating caching after every single request leads to cache thrashing.
hence
The [bloom filter](https://systemdesign.one/bloom-filters-explained/) can be updated when the paste is accessed is twice by the client. 
The cache servers are updated only when the [bloom filter](https://systemdesign.one/bloom-filters-explained/) is already set (multiple requests to the same paste ID).
The client cache prevents populating the public cache (CDN) for pastes accessed only by the owner.

A [bloom filter](https://systemdesign.one/bloom-filters-explained/) on the paste ID is introduced to prevent unnecessary queries to the data store. If the paste ID is absent in the [bloom filter](https://systemdesign.one/bloom-filters-explained/), return an HTTP status code of 404. If the paste ID is set in the [bloom filter](https://systemdesign.one/bloom-filters-explained/), delegate the client request to the cache server or the data store.


`Read coalescing` should be used to prevent thunder herd for the paste not present in cache.


The reverse proxy is used as an [API Gateway](https://aws.amazon.com/api-gateway/). The reverse proxy performs [SSL termination](https://en.wikipedia.org/wiki/TLS_termination_proxy) and compression at the expense of increased system complexity. When an extremely popular paste is accessed by hundreds of clients at the same time, the reverse proxy [collapse forwards](https://wiki.squid-cache.org/Features/CollapsedForwarding) the requests to reduce the system load.