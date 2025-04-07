---
Creation Time: Monday, March 10th 2025
Modified Time: Wednesday, March 12th 2025
---
It's a program that traverse the internet, download the pages, find links and following links and repete , create index for search engines.
It can be used for search engine, collect data for research and Machine learning models, collect data when website modifies.
### Requirements:
1. Start crawling from given set of Seed urls.
2. Download and Extract text data from each urls pages and following pages linked present in existing given pages and store
3. give these collected data to be used to train ML models like chatGPT or OpenAI

### Scale and capacity estimation
`How many websites? (seed url)
Ex: 10B web pages

`Size of each pages?
Ex: 2MB per page avg

`Crawl for 5 days?????

`How much resources company has?
unlimited


### NFR
1. Quick search for ML 
2. Fault tolerant and should be able to start-over from where ever it failed to work.
3. Politness: follow robot.txt setting to avoid websites while crawling.
4. Efficient enough to crown 10B webpage within 5 days.

### Capacity Estimates:
will do the math later
1 server : 250 GB machine
1 Machine max network support ?


### Core Entities
Text Data
Domain meta data
Url metadata

### APIs
It do not have user facing APIs
but it is going to have interfaces for internal application uses:
Input: Seed urls
Output: Text data

### Data flows enumerations
1. Take seed urls from frontier and their IP from DNS.
Frontier:: is the set of urls that is yet to be crawls. in the begnining it will be the input list of seed urls.
1. Fetch/Download HTML for the pages.
2. Extract text from HTML pages.
3. Store that text in DB
4. Extract URLs from the text and give it to Frontier.
5. repeat these steps till frontier has empty list of urls to be crawls.


### HLD
[[WebCrawler]]


### Deep dive and issues and NFR

**_Website failure_
	Manual Kafka retry and exponential backoff with Dead later queue or using Amazon SQS with delay message processing using jitter 
	SQS supports retries with configurable exponential backoff out of the box. Initially, messages that fail to process are retried once per the visibility timeout, with the default being 30 seconds. The visibility timeout increases exponentially after each retry attempt—30 seconds, 2 minutes, 5 minutes, and up to 15 minutes. This strategy helps to manage message processing more efficiently without overwhelming the system.
	 No need to implement our own retry logic.
**_DNS failure_
If we're using a 3rd party DNS provider, we'll want to make sure they can handle the load. Most 3rd party providers have rate limits that can be increased by throwing money at them.
1. **DNS caching**: We can cache DNS lookups in our crawlers to reduce the number of DNS requests we need to make. This way all URLs to the same domain will reuse the same DNS lookup.
    
2. **Multiple DNS providers**: We can use multiple DNS providers and round-robin between them. This can help distribute the load across multiple providers and reduce the risk of hitting rate limits.
	Use multiple DNS to distribute DNS load or use caching to keep DNS data 
	
3. _Change parsing logic_
4. _How can we ensure we are fault tolerant and don't lose progress?_
	Breaking the pipeline into download and extract separate process and keep metadata about the processed urls. and queue will use offset to commit if url is processed or not.


5. _Loop detection and preventions_
6. _Non Html pages data extractions_



**. _What if a Web Crawler goes down?_
	
If a crawler goes down, the URL will be picked up by another crawler and the process will continue. The actual mechanism for accomplishing this is different per technology. I'll outline, at a very high level, how two popular technologies, Kafka and SQS, might handle this.
`Apache Kafka:
- Kafka retains messages in a log and does not remove them even after they are read. Crawlers track their progress via offsets, which are not updated in Kafka until the URL is successfully fetched and processed. If a crawler fails, the next one picks up right where the last one left off, ensuring no data is lost.
`SQS:

- With SQS, messages remain in the queue until they are explicitly deleted. A visibility timeout hides a message from other crawlers once it's fetched. If the crawler fails before confirming successful processing, the message will automatically become visible again after the timeout expires, allowing another crawler to attempt the fetch. On the other hand, once the HTML is stored in blob storage, the crawler will delete the message from the queue, ensuring it is not processed again.

**_How can we ensure politeness and adhere to robots.txt?**_
This involves ensuring that our crawling activity does not disrupt the normal function of the site by overloading its servers, respecting the website's bandwidth, and adhering to any specific restrictions or rules set by the site administrators.

```
User-agent: * 
Disallow: /private/
Crawl-delay: 10
```

The User-agent line specifies which crawler the rules apply to. In this case, * means all crawlers.
The Disallow line specifies which pages the crawler is not allowed to crawl.
In this case, the crawler is not allowed to crawl any pages in the /private/ directory.
The Crawl-delay line specifies how many seconds the crawler should wait between requests. In this case, 10 seconds.

_To ensure politeness and adhere to robots.txt
**Respect robots.txt**_
To handle the crawl delay, we need to introduce some additional state. We can add a Domain table to our Metadata DB that stores the last time we crawled each domain. This way, we can check the Crawl-delay directive and wait the specified number of seconds before crawling the next page. If we pull a url off the queue for a domain that we have already crawled within the Crawl-delay time, we'll just put it back on the queue with the appropriate delay so that we can come back to it later.

**_Rate limiting**:_ We will want to limit the number of requests we make to any single domain. The industry standard is to limit the number of requests to 1 request per second.

With multiple crawlers, all N crawlers could be hitting a single domain at the same time.

We can implement a global, domain-specific rate limiting mechanism using a centralized data store (like Redis) to track request counts per domain per second. Each crawler, before making a request, checks this store to ensure the rate limit has not been exceeded. We'll use a sliding window algorithm to track the number of requests per domain per second. If the rate limit has been exceeded, the crawler will wait until the next second to make the request.


**_How to scale to 10B pages and efficiently crawl them in under 5 days?_

Our one lonely machine won't be able to do this alone, so we need to parallelize the crawling process. But, how many crawler machines will we need?
we should recognize that this is an I/O intensive task. If we take our average page size of 2MB which we gathered during our non-functional requirements, we can estimate where our bandwidth will be capped.

In the AWS ecosystem, a network optimized instance can handle about 400 Gbps. This means that a single instance, from a network perspective, can handle `about 400 Gbps / 8 bits/byte / 2MB/page = 25,000 pages/second. That's a ton, but it's likely not actually possible.



There will be practical limitations on the number of requests we can make per second dictated by factors like server response latency, DNS resolution, rate limiting, politeness, retries, etc.
let's say that we can utilize 30% of the available bandwidth. This would give us `25,000 pages/second * 30% = 7,500 pages/second.


**_How can we scale parser**_

It just need to download the HTML from blob storage, extract the text data, and store it back in blob storage. We need to make sure this keeps pace with our crawlers. Rather than estimating how many we need, we can just scale this up and down dynamically based on the number of pages in the Further Processing Queue. `This could be via Lambda functions, ECS tasks, or any other serverless technology.


**_No repeated processing of same page**_
	To be as efficient as possible, we will want to ensure we don't waste our time crawling pages that have already been crawled. we can keep url metadata and its status if it is crawled.

_Loop detection and preventions_
But what about when different URLs point to the same page? This is a common occurrence on the web. For example, http://example.com and http://www.example.com might point to the same page.
It's also common for totally different domains to have exactly the same content (a maybe depressing fact about the internet). For these cases, we can't just compare the URLs, instead, we'll want to compare a hash of the content. Let discuss a couple options:

Great Solution: Hash and Store in Metadata DB w/ Index

Great Solution: Bloom Filter

**Last thing! Watch out for crawler traps**

Crawler traps are pages that are designed to keep crawlers on the site indefinitely. They can be created by having a page that links to itself many times or by having a page that links to many other pages on the site. If we're not careful, we could end up crawling the same site over and over again and never finish.
we can implement a maximum depth for our crawlers. We can add a depth field to our URL table in the Metadata DB and increment this field each time we follow a link. If the depth exceeds a certain threshold, we can stop crawling the page. This will prevent us from getting stuck in a crawler trap.




1. **How to handle continual updates**: While our requirements are for a one-time crawl, we may want to consider how we would handle continual updates to the data. This could be that we plan to re-train the model every month or that our crawler is for a search engine that needs to be updated regularly. I'd suggest adding a new component "URL Scheduler" that is responsible for scheduling URLs to be crawled. So rather than putting URLs on the queue directly from the parser workers, the parser workers would put URLs in the Metadata DB and the URL Scheduler would be responsible for scheduling URLs to be crawled by using some logic base on last crawl time, popularity, etc.