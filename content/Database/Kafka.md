

**Prometheus + Grafana**, **Datadog**, or **Confluent Control Center**

## Monitoring and optimizing Kafka in production requires a **proactive approach**—
focusing on metrics, bottlenecks, and tuning. Below is a **structured strategy** used by senior engineers in high-throughput environments:

## How can we increase Kafka partition in production running ENVs? What impact it can get?
1. **Schedule During Low Traffic**    - Rebalances cause downtime; avoid peak hours.
2. **Static Membership**    - Set `group.instance.id` to reduce rebalance frequency.
3. **Existing messages stay in old partitions**; new messages use new partitions.
4. **ZooKeeper Load**    - Each partition update writes to ZooKeeper (high partition counts can slow it down).
5. **Verify Replication Factor**  - Ensure your topic has `replication.factor ≥ 2` to prevent data loss if a broker fails.
6. **Validate Offsets**  - Ensure consumers resume from the correct offsets:

 7. **Idempotent Consumers** - An idempotent consumer ensures that **processing the same message multiple times doesn’t change the system state**. Example: Deduplicating payments or database writes.
 8. **Static membership**- Default Kafka consumers trigger a **full rebalance** when a consumer joins/leaves (even temporarily). Assign a **persistent `group.instance.id`** to each consumer.  Kafka treats the consumer as a **static member**:
    - Temporary disconnects (e.g., restarts, GC pauses) **won’t trigger rebalances**.
    - Rebalance only occurs if the consumer is **explicitly shut down** (`consumer.close()`).


## How **Static Membership** helps in rebalancing 



## Topic/Partition Metrics

- **Throughput**:
    - `Messages In/Out per Second` (per topic).
- **Lag**:
    - `Consumer Lag` (via `kafka-consumer-groups.sh` or Burrow).
    - Lag spikes indicate slow consumers.
- **Partition Imbalance**:
    - Uneven leader distribution across brokers.
- `Rebalance Time` (frequent rebalances hurt performance).
- **Partition Count**:
    - Aim for `# partitions = max(consumers) × throughput per consumer`.
    - Too many partitions increase latency (avoid >10k per cluster).
- **Replication**
    - Set `replication.factor=3` for fault tolerance.
    - Use `min.insync.replicas=2` for durability/availability trade-offs.
- **Batching**:
    - Set `linger.ms=20` and `batch.size=64KB` to reduce small writes.
- **Fetch Size**:
    - Increase `fetch.max.bytes=50MB` and `max.partition.fetch.bytes=10MB`.
- **Polling**:
    - Process messages in batches (`max.poll.records=500`).

### **3. Troubleshooting Common Issues**

|**Issue**|**Diagnosis**|**Fix**|
|---|---|---|
|High Consumer Lag|Check `records-lag-max` per partition.|Scale consumers or increase `fetch.size`.|
|Broker Disk Full|`log.retention.bytes` too high.|Compact topics (`cleanup.policy=compact`) or expand storage.|
|Producer Timeouts|High `request.timeout.ms` or network.|Optimize batching or upgrade brokers.|
|Frequent Rebalances|Consumers fail `heartbeat.interval.ms`.|Tune `session.timeout.ms` or enable static membership.|


### **5. Proactive Practices**
- **Chaos Testing**:  
    Kill brokers randomly (using Chaos Monkey) to test resilience.
- **Canary Deployments**:  
    Roll out config changes to a subset of brokers first.
- **Capacity Planning**:  
    Monitor growth trends and scale before hitting limits.


## How can we be production ready ?
1. **Enable Idempotence** (Producer):
    ```Java
    props.put("enable.idempotence", "true");
```
    
2. **Configure Static Membership** (Consumer):
    ```Java
    props.put("group.instance.id", "payment-service-1"); // Unique per pod
```
    
    
3. **Handle Duplicates** (Consumer):
    ```Java
    f (isDuplicate(msg.getKey())) {
        return; // Skip processing
    }
```


Static membership (`group.instance.id`) is a powerful Kafka feature designed to reduce unnecessary consumer group rebalances. 

_Bellow are the scenarios for which this feature supports a lot.

**Long-Running Consumers (e.g., Microservices)** __ If a consumer disconnects (even briefly due to GC pauses or network blips), Kafka triggers a **full group rebalance**, redistributing partitions across all consumers.
 **Stateful Microservices** - A payment processing service holding in-memory transaction state.
 Rebalancing forces offsets to reset, causing:
- Duplicate processing (if `auto.offset.reset=earliest`).
- Lost in-memory state (e.g., aggregated metrics).
**Low-Latency Pipelines** - Rebalances add **100ms–2s latency spikes**.



#### **Kubernetes Rolling Deployments**
- **Problem**: Each replaced pod triggers a rebalance.
- **Solution**: Set `group.instance.id` to a unique but persistent value (e.g., pod name):
```Java
    props.put("group.instance.id", System.getenv("POD_NAME"));
```


 **Autoscaling Consumers*
- **Problem**: Autoscaling up/down causes constant rebalances.
- **Solution**: Static membership + **predictable scaling** (e.g., scale during low traffic).


### Kafka consumer group change
  
A **consumer group** is a logical identifier for a set of consumers that.
 **Ensures each message is processed once** (per group).

### **✅**_When should we change it?
**Starting Fresh**: You want to reprocess all messages from the beginning
**Blue-Green Deployments**- wo groups run in parallel, processing the same data independently.
**Multi-Tenant or Isolated Processing**- Different teams/departments need separate processing pipelines for the same topic.
**Resolving Offset Corruption**- If `__consumer_offsets` is corrupted, a new group starts fresh.

### **❌ `When NOT to Change**
- **Scaling consumers**: Just add/remove consumers within the **same group**.
- **Temporary consumer failures**: Kafka automatically handles rejoins.



### **Interview Answer Example**
> *"In my last role, we increased partitions for a high-throughput topic from 12 to 24 to handle Black Friday traffic. We:*
> 
> - _Ran a dry run in staging to test rebalance impact,_
> - _Used static membership to minimize consumer downtime,_
> - *Monitored broker metrics for 48 hours post-change.*  
>     _Latency spiked briefly during rebalance but stabilized with 2x throughput capacity."_
>


### **5. Interview Answer (TL;DR)**

> _"In production, existing Kafka messages are never lost during rebalancing—they stay in their original partitions. To ensure safety:_
> - _Use replication factor ≥2 and idempotent consumers._
> - _Leverage static membership (`group.instance.id`) to reduce rebalances._
> - _Redistribute data incrementally with `kafka-reassign-partitions.sh` if needed._  
>     *In my last role, we scaled a topic from 12 to 24 partitions during off-peak hours with zero data loss by pre-testing in staging and monitoring consumer lag."*
>


## **5. Interview Answer**

> _"Changing a Kafka consumer group name is needed when reprocessing data (e.g., after a bug fix) or running parallel pipelines (e.g., blue-green deployments). Existing messages aren’t lost, but the new group starts consuming from `earliest` or `latest` based on configs. At [Company X], we used this to test a new fraud-detection consumer without disrupting the legacy system."_



#### Kafka Mirrormaker
Kafka's mirroring feature makes it possible to maintain a replica of an existing Kafka cluster. The following diagram shows how to use the _MirrorMaker_ tool to mirror a source Kafka cluster into a target (mirror) Kafka cluster. The tool uses a Kafka consumer to consume messages from the source cluster, and re-publishes those messages to the local (target) cluster using an embedded Kafka producer.