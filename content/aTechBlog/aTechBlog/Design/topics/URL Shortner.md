

The shortened URL is nearly one-third the size of the actual URL.

URL shortening is used to optimize links across devices, track individual links to analyze audience, measure ad campaigns’ performance, or hide affiliated original URLs.


**FR:**
1. Generate short  and unique url of approx. (length original Length)/3
2. url can be names custom
3. User can specify expiration time for the url and also there should be default time, we can be changed based on user subscription model



**NFR:**
1. The system should be highly available. This is required because, if our service is down, all the URL redirections will start failing.
2. URL redirection should happen in real-time with minimal latency.
3. Shortened links should not be guessable

**Extended Requirements:**

1. Analytics; e.g., how many times a redirection happened?
2. Our service should also be accessible through REST APIs by other services.

**Capacity Estimates:**

![[Screenshot 2024-07-08 at 1.46.12 PM.png]]
Per month write request: 500M
keep record for:  5 years
Url Object size: 500bytes
total files: 500M * 5 * 12 months = 30B
Total Storage: 30B * 5 Byte  = 15TB

**Traffic estimates:** Assuming, we will have 500M new URL shortenings per month, with 100:1 read/write ratio, we can expect 50B redirections during the same period:

100 * 500M => 50B

What would be Queries Per Second (QPS) for our system? New URLs shortenings per second:

500 million / (30 days * 24 hours * 3600 seconds) = ~200 URLs/s

Considering 100:1 read/write ratio, URLs redirections per second will be:

100 * 200 URLs/s = 20K/s


**Bandwidth estimates:** For write requests, since we expect 200 new URLs every second, total incoming data for our service will be 100KB per second:

200 * 500 bytes = 100 KB/s

For read requests, since every second we expect ~20K URLs redirections, total outgoing data for our service would be 10MB per second:

20K * 500 bytes = ~10 MB/s


**Memory Estimates**
 for caching 20% of hot urls:
  20K  *3600 * 24 = 1.7 Billion request per day
  0.2 * 1.7 B * 500 Byte = 170GB per day cache will be needed
  



![[Screenshot 2024-07-08 at 2.04.44 PM.png]]


**APIs:**

1. saveUrl(String original, String custom, String expiredTime, Username, api_dev_key) return urlKey and status
2. fetchUrl()
3. deleteUrl(api_dev_key, url_key)


**DB Schema:**
Read heavy
* less data to be stored
* no relationships between the records
* Since we anticipate storing billions of rows, and we don’t need to use relationships between objects – a NoSQL store like [DynamoDB](https://en.wikipedia.org/wiki/Amazon_DynamoDB), [Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra) or [Riak](https://en.wikipedia.org/wiki/Riak)

**url_metadata**{
	urlKey, 
	currentHitCountin24Hours
}

**url_mapping**{
	String urlKey,
	String original url,
	String mapped url,
	expireTime,
}
**IMP**

how many unique char length url we would need to generate and its combination?
8 ? 10? 12?
in  5 years: 500M * 12 * 5 = 30B record

We can compute a unique hash (e.g., [MD5] or [SHA256], etc.) of the given URL.






