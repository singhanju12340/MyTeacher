---
Creation Time: Tuesday, January 21st 2025
Modified Time: Tuesday, January 21st 2025
---


# In Memory based message broker: `RabitMQ, Active MQ, Amazon SQS`
All the message are stored in memory
Once message are sent to the consumer after consumer ACK message are deleted from the memeory
All message to send to consumer are Round robin based which provides more throughput,but there is no ordering insurance

_Uses:_
if high throughput and ordering do not matter as it read from memory
ex: posting video to youtube for encoding, order does not matter.
Ex: user post tweet and delivering message to all the followers, order does not matter

# Log based message broker: `Kafka, Amazon kinesis`
message are kept in disk in sequential and read are also sequential, which provide faster read
All message are read in the order in which it was inserted
Any slow message can cause slow processing for other messages too
messages are durable, do not delete after consumer read. its deleted after given expiration time

_Uses_
Sensor matrix, where we want to take average of last 20 messages, order matter here.
Each write from data base has to be send to elastic search as an update, order of data updates needs to be preserved
