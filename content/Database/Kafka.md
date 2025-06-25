

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




##### Kafka confluent vrs Kafka Kubernetes setup

**Kubernetes**:
Kubernetes offers constructs to manage a set of containers together as a stateless or stateful cluster. Kubernetes manages a set of [Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/ "Pods on Kubernetes.io"). Each Pod is a set of functionally related containers deployed together on a server called a [Node](https://kubernetes.io/docs/concepts/architecture/nodes/ "Nodes on Kubernetes.io"). To manage a stateful set of nodes like a Kafka cluster, we used Kubernetes [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/ "StatefulSets on Kubernetes.io") to control deployment and scaling of containers with an ordered and graceful deployment of changes including guarantees to prevent compromising the overall service availability. 

Used own kafka image and used own custom repository with verified application dependencies.
we extended it using [Custom Resources and Controllers](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/ "Custom Resources on Kubernetes.io"), an extension for Kubernetes API to create user-defined resources and implement actions when these resources are updated, which was not provided default.
- [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/ "Persistent Volumes on Kubernetes.io") is persistent storage for Kafka pods and guarantees that a Pod always mounts the same disk volume when it restarts.



### Can a Kafka cluster have brokers of different Kafka versions?
Issue: https://issues.apache.org/jira/browse/KAFKA-7886

**Yes, a Kafka cluster can temporarily have brokers of different Kafka versions during a rolling upgrade, and this is the recommended way to upgrade Kafka without downtime.**

how it generally works::
1. **Rolling Upgrades:** The standard Kafka upgrade procedure involves upgrading brokers one by one. This means that for a period, your cluster will contain a mix of old-version and new-version brokers.
2. **Backward/Forward Compatibility:** Kafka is designed with backward and sometimes forward compatibility in mind, primarily managed through the `inter.broker.protocol.version` configuration.
 3. set `inter.broker.protocol.version` to the **oldest version** that any broker in the cluster is running. This ensures that all brokers can communicate with each other using a mutually understood protocol.
 4. Once all brokers have been upgraded to the new binary, and the cluster is stable, you then perform _another_ rolling restart to update `inter.broker.protocol.version` to the **new version**. This allows the cluster to leverage new protocol features.
 5. Only after all brokers are running 2.x binaries and `inter.broker.protocol.version` is set to `2.x` should you consider setting `log.message.format.version` to `2.x`


## How do we ensure Kafka cluster Scalability and Relaibility?
#### Scalability:ability to handle increasing amounts of data and higher throughput

`Partitioning Strategy`: Partitions are the primary unit of parallelism in Kafka. 
_More Partitions = More Parallelism
_Choosing the Right Numbe_
Too few partitions can limit throughput. Too many can increase end-to-end latency, put pressure on ZooKeeper/KRaft (for metadata management), and lead to more open file handles on brokers
_Key-based Partitioning

`Horizontal Scaling` Adding new broker, but it needs Data rebalancing.

`Producer Scalability`: 
Batching: 
Compression
Asynchronous Sending

`Consumer Scalability`: Scale out consumption by adding more consumer instances to a consumer group, up to the number of partitions in the topic
Ensure consumer logic is efficient.

`Hardware and Infrastructure`:
 > Ensure Brokers have adequate CPU, memory (especially for page cache), fast disks (SSDs are highly recommended for log segments), and sufficient network bandwidth.
 >Optimize network settings. Use high-speed, low-latency networks between brokers and between clients and brokers
 
 `Monitoring and Tuning:`
 Continuously monitor key metrics `(broker CPU/memory/disk/network, partition distribution, consumer lag, request latency) `to identify bottlenecks.
Tune broker, topic, producer, and consumer configurations based on observed performance and workload characteristics


#### Reliability: ensuring data durability (no data loss) and high availability (the cluster remains operational even if some components fail)
`Replication Factor` : Typically 3

`Leader and Followers:` For each partition, one broker acts as the leader
`Fault Tolerance:` If a broker hosting a partition leader fails, one of the in-sync follower replicas can be automatically elected as the new leader, ensuring data remains available.`
`In-Sync Replicas`: Kafka only considers a message "committed" by the producer when it has been written to the leader and all ISRs
`Producer Acknowledgements`
**`acks=0`:** Producer doesn't wait for acknowledgement
**`acks=1`:** Producer waits for the leader to acknowledge the write
**`acks=all` (or `-1`):** Producer waits for the leader and all current in-sync replicas

`Graceful Shutdown and Leader Election`: When a leader fails, the controller (a broker responsible for cluster metadata) elects a new leader from the ISR list

`Data Durability and Persistence`: 
persists all messages to disk
`log.flush.interval.messages` and `log.flush.interval.ms` to control how frequently data is fsynced to disk, balancing performance with durability against OS crashes
`ZooKeeper / KRaft (Control Plane Reliability):`
ZooKeeper (Legacy)
KRaft (Kafka Raft Metadata mode - Recommended for new deployments)

`Monitoring and Alerting for Reliability:`
- Monitor under-replicated partitions, ISR shrink/expansion, controller health, broker availability, and disk space.
- Availability Zone Awareness: Kafka will try to distribute replicas of a partition across different racks/AZs. This protects against data loss or unavailability if an entire rack/AZ fails.
`Disaster Recovery`:
protection against data center failures, consider cross-cluster replication tools like Kafka MirrorMaker or other third-party solutions to replicate data to a standby Kafka cluster in a different geographical region.



### Kafka Vrs Message Queue

| Feature            | Kafka              | Queue              |
| ------------------ | ------------------ | ------------------ |
| Message Model      | Log Based          | Queue based        |
| Delivery semantics | At least once      | At most once       |
| Message Retention  | Time or Size based | Until consumed     |
| Replay             | Build In           | No Built in replay |
| Ordering Guarantee | Per Partition      | Per Queue          |