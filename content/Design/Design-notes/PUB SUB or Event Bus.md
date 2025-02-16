---
Creation Time: Monday, August 5th 2024
Modified Time: Sunday, February 9th 2025
---

Google pub sub
Amazon SQS
Rabit MQ
Kafka: data streamer
Azure Bus service 
Kinesis


## Kafka

_A Kafka cluster is made up of multiple **brokers**. These are just individual servers
`Each broker has a number of **partitions**. Each partition is an ordered, immutable sequence of messages that is continually appended to -- think of like a log file.
_A **topic** is just a logical grouping of partitions. Topics are the way you publish and subscribe to data in Kafka.

`**Offsets**: Kafka brokers store consumer offsets in an internal topic called `__consumer_offsets`, and this helps consumers resume from where they left off after a disconnect or restart.




### Kafka Estimates:
It is recommended to keep messages under 1MB to ensure optimal performance.
A single broker can store around 1TB of data and handle around 10,000 messages per second.


## #Why_Kafka?

### Scalability
There is no hard limit on the size of a Kafka message though It is recommended to keep messages under 1MB to ensure optimal performance.
A single broker can store around 1TB of data and handle around 10,000 messages per second.
Still if more is required from Kafka we can scale it by
1. **Horizontal Scaling With More Brokers**:  The simplest way to scale Kafka is by adding more brokers to the cluster. This helps distribute the load and offers greater fault tolerance. Each broker can handle a portion of the traffic, increasing the overall capacity of the system. It's really important that when adding brokers you ensure that your topics have sufficient partitions to take advantage of the additional brokers. More partitions allow more parallelism and better load distribution. If you are under partitioned, you won't be able to take advantage of these newly added brokers.
2. **Partitioning Strategy**: You need to decide how to partition your data across the brokers. This is done by choosing a key for your messages since the partition is determined by a consistent hashing algorithm on the key. If you choose a bad key, you can end up with hot partitions that are overwhelmed with traffic.


**How can we handle hot partitions?**
Consider an [Ad Click Aggregator](https://www.hellointerview.com/learn/system-design/answer-keys/ad-click-aggregator) where Kafka stores a stream of click events from when users click on ads. Naturally, you would start by partitioning by ad id. But when Nike launches their new Lebron James ad, you better believe that partition is going to be overwhelmed with traffic and you'll have a hot partition on your hands.

Ways to handle hot partition:
1. **Random partitioning with no key**:
2. **Random salting**:We can add a random number or timestamp to the ad ID
3. **Use Compound Key:** use a combination of ad ID and another attribute, such as geographical region or user ID segments, to form a compound key
4. **Back pressure**: The Kafka producer maintains an internal buffer (controlled by the `buffer.memory` configuration) where messages are temporarily stored before being sent to the broker. If your application produces messages faster than they can be transmitted or acknowledged, this buffer will eventually fill up. this slows down the producer if Lags too high. 

### Durability
Kafka  provide durability by keeping partitions replicas on multiple other brokers, 1 broker acting as leader and other followers. Producer acknowledgments (acks setting) play a crucial role here. Setting acks=all ensures that the message is acknowledged only when all replicas have received it, guaranteeing maximum durability.


** what happens when a consumer goes down?**
**Offset Management**:When a consumer restarts, it reads its last committed offset from Kafka and resumes processing from there, ensuring no messages are missed or duplicated.

**Rebalancing** : When part of a consumer group, if one consumer goes down, Kafka will redistribute the partitions among the remaining consumers so that all partitions are still being processed.

keeping the work of the consumer as small as possible is a good strategy -- as was the case in Web Crawler where we broke the crawler into 2 phases: downloading the HTML and then parsing it.


### ### Handling Retries and Errors
**Producer Retries:** we may fail to get a message to Kafka in the first place. Errors can occur due to network issues, broker unavailability, or transient failures. To handle these scenarios gracefully, Kafka producers support automatic retries.
**Consumer Retries:** On the consumer side, we may fail to process a message for any number of reasons. Kafka does not actually support retries for consumers out of the box (but [AWS SQS](https://aws.amazon.com/sqs/) does!). We can use other queues to push failed message and DLQ to push the messages after the retry count.


### Performance Optimizations
5. we can do is batch messages in the producer before sending them to Kafka
6. improve throughput is by compressing the messages on the producer. This can be done by setting the compression option in the producer configuration.
7. the biggest impact you can have to performance comes back to your choice of partition key
8. 