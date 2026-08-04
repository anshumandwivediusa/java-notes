# Kafka
## 1. What is Kafka?
**Apache Kafka is an open-source distributed event streaming platform used to build real-time data pipelines and streaming applications. It allows you to publish, subscribe, store, and process streams of events reliably and at scale.** It’s widely adopted across industries, including finance, telecom, logistics, and healthcare, for mission-critical applications.  [Apache Kafka](https://kafka.apache.org/)  [Apache Kafka](https://kafka.apache.org/intro/)  

## 2. Why Kafka is Fast

 - **TCP Protocol**
Kafka uses TCP sockets for communication between producers, brokers, and consumers. TCP provides reliable, ordered delivery of data streams, which is essential for event streaming.

 - **Persistent Connections**  
Instead of opening and closing connections for each message, Kafka maintains long-lived TCP connections. This reduces overhead from repeated handshakes and improves efficiency.

 - **Efficient Network I/O**  
Kafka leverages NIO (Non-blocking I/O) in Java, allowing it to handle thousands of client connections concurrently without blocking threads.

 - **Zero Copy with TCP**  
Kafka integrates with the OS kernel’s sendfile system call, which streams data directly from the file system cache to the TCP socket. This avoids multiple copies between user space and kernel space.

- **Batching Data**  
  Kafka groups messages into batches before sending them over the network or writing them to disk. This reduces the number of network round-trips and disk I/O operations, improving throughput and minimizing latency. It’s especially effective when handling millions of small messages.

- **Sequential Disk Access**  
  Instead of random reads/writes, Kafka appends data sequentially to log files. Modern disks (even HDDs) are optimized for sequential access, and with SSDs this becomes even faster. Kafka’s design ensures disk performance is close to memory speed.

- **Horizontal Scalability**  
  Kafka topics can be partitioned across multiple brokers. Each partition can be replicated and consumed independently, allowing Kafka to scale linearly with the number of brokers and partitions. This means you can handle trillions of events per day by simply adding more machines.

- **Compression**
  Kafka supports message compression (e.g., GZIP, Snappy, LZ4), reducing payload size and speeding up transfer.  

- **Replication**
  While replication ensures durability, Kafka optimizes leader–follower communication to minimize overhead.  

- **Page Cache Usage**
  Kafka relies heavily on the OS page cache rather than implementing its own caching layer, reducing complexity and maximizing performance.  

## 3. Kafka vs RabitMQ
- **Kafka** is ideal for **event streaming, analytics, and replayable pipelines** where throughput and durability matter.  
- **RabbitMQ** excels at **low-latency messaging, complex routing, and microservices communication**.  
- Compared to other systems like **ActiveMQ** or **Amazon SQS**, Kafka stands out for **scalability and replay**, while RabbitMQ is valued for **protocol flexibility and ease of integration**.  

## 4. Kafka architecture

<img width="790" height="441" alt="image" src="https://github.com/user-attachments/assets/c41dd9fd-c802-437b-bbe7-b6097e3d1476" />

- **Broker**  
  A broker is a Kafka server that acts as an intermediary between producers and consumers. It receives messages from producers, stores them, and serves them to consumers. Multiple brokers form a **cluster** for scalability and fault tolerance.

- **Log**  
  A log is the physical file on disk where Kafka appends incoming records sequentially.  
  - Append-only, ordered by time.  
  - Configured via `log.dir`.  
  - Provides durability and replayability.

```
//Each topic gets its own folder, and partitions are represented as numbered suffixes.
//Inside each partition directory, Kafka maintains segment files (append-only logs) where records are stored sequentially. These files are rolled over periodically based on size or time.
Example:
/logs/topicA_0   → topicA with 1 partition
/logs/topicB_0   → partition 0 of topicB
/logs/topicB_1   → partition 1 of topicB
/logs/topicB_2   → partition 2 of topicB
```

- **Topic**  
  A topic is a logical category or stream of records (like a database table or folder). Topics are logs that are seperated by topic name. Thinks topics as labeled logs. 
  - Examples: `orders`, `customers`, `payments`.  
  - Topics are broken into **partitions** for scalability.

- **Partition**  
  A partition is a **logical subdivision of a topic**.  
  - Each partition is stored as a log file on disk.  
  - Enables parallelism: different consumers can read different partitions.  
  - Provides redundancy: partitions can be replicated across brokers.  
  - Messages with the same **key** always go to the same partition, preserving order.
  - Why This Matters
    - **Scalability**: More partitions → more directories → distributed across brokers.
    - **Parallelism**: Consumers in a group can read different partitions simultaneously.
    - **Durability**: Logs are persisted on disk, enabling replay and recovery.

   <p align="center">
    <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/a2ac9831-d4e0-4a5d-8d60-8356ecb93086" />
   </p>
  
  - Partitioning Formula
    When a producer sends a message with a key:
    ```
    \[
    \text{target\_partition} = \text{HashCode(key)} \% \text{number\_of\_partitions}
    \]
    ```
    - Ensures all records with the same key go to the same partition.  
    - Guarantees ordering within that partition.

- Producers
  - **Producer** is any application or service that publishes (writes) messages into Kafka topics.  
  - Producers decide:
    - **Topic**: where the message goes.  
    - **Partition**: either explicitly or via Kafka’s partitioner (hashing the key).  
    - **Key**: ensures ordering by sending related messages to the same partition.  
  - **Batching**: Producers send messages in batches to reduce network overhead.  
  - **Acknowledgments**: Producers can configure how many broker acknowledgments they wait for (e.g., `acks=0`, `acks=1`, `acks=all`) to balance speed vs durability.

- Consumers
  - **Consumer** is any application or service that subscribes (reads) messages from Kafka topics.  
  - Consumers track their position in a partition using **offsets**.  
  - They can:
    - Read messages sequentially from a partition.  
    - Commit offsets to Kafka (or external storage) for fault tolerance.  
  - **Consumer Groups**:  
    - Multiple consumers can join a group to share the load.  
    - Kafka ensures each partition is consumed by only one consumer in the group.  
    - Different groups can independently consume the same topic.

   <p align="center">
    <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/72d6b40a-c4b8-426e-a748-daef8a826edb" />
    <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/1aac28c7-b5e5-4048-baac-168690fb61f9" />
   </p>


## 3. Flow of Sending a Message in Kafka

1. **Create ProducerRecord**  
   - Must include: **topic** and **value**.  
   - Optional: **key** (for partitioning/order) and **partition** (explicit assignment).

2. **Serialization**  
   - Key and value objects are converted into **ByteArrays** using serializers (e.g., StringSerializer, AvroSerializer).  
   - Ensures data can be transmitted over the network.

3. **Partitioner**  
   - If a partition is explicitly specified → use it.  
   - If not → partitioner chooses one based on:  
     ``` \[
     \text{target\_partition} = \text{HashCode(key)} \% \text{number\_of\_partitions}
     \]  ```
   - Guarantees ordering for messages with the same key.

4. **Batching**  
   - Producer groups records destined for the same topic + partition into a batch.  
   - Reduces network overhead and improves throughput.

5. **Send to Broker**  
   - Batch is sent over a persistent TCP connection to the broker.  
   - Broker appends records sequentially to the partition log file.

6. **Broker Response**  
   - If successful → returns **RecordMetadata** containing `<topic, partition, offset>`.  
   - If failed → returns an error. Producer may retry (based on retry settings).

<p align="center">
 <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/985f6d5d-b527-47a2-adbd-02d3df2e90b4" />
</p>

## 4. Kafka Log Cleanup Policies

### 1. **Delete Policy**
- Old log segments are **deleted** once they exceed a configured retention threshold.  
- Controlled by settings like:
  - `log.retention.hours` (default: 168 hours = 7 days)  
  - `log.retention.bytes` (maximum size before deletion)  
- Use case: transient data streams (e.g., clickstream, logs) where old data isn’t needed.

### 2. **Compact Policy**
- Instead of deleting, Kafka **compacts logs** by retaining only the latest record for each key.  
- Older records with the same key are removed, ensuring the log contains a complete snapshot of the latest state.  
- Controlled by:
  - `cleanup.policy=compact`  
  - `min.cleanable.dirty.ratio` (threshold for compaction)  
- Use case: stateful data (e.g., user profiles, account balances) where you only need the most recent value per key.

### How Compaction Works
1. Producer sends multiple updates for the same key (e.g., `user123 → address`).  
2. Kafka appends all updates to the log.  
3. During compaction, Kafka scans the log and removes older entries for `user123`, keeping only the latest.  
4. Consumers replay the log and reconstruct the current state.


### Delete vs Compact — Quick Comparison

| **Policy** | **Behavior** | **Best Use Case** |
|------------|--------------|-------------------|
| **Delete** | Removes old records after time/size threshold | Event streams, logs, transient data |
| **Compact** | Keeps only the latest record per key | Stateful data, snapshots, configuration tables |


## 5. ACK
acks: Controls how many partition replicas must receive the record before the producer can consider write successful.

 - **acks = 0**: the producer will not wait for a reply from the broker before assuming the message was sent successfully. The message may be lost but it can send messages as fast as the network will support, so this setting can be used to achieve very high throughput

 - **acks=1**: With a setting of 1, the producer will consider the write successful when the leader receives the record. The leader replica will know to immediately respond the moment it receives the record and not wait any longer.

 - **acks=all**: the producer will consider the write successful when all of the in-sync replicas receive the record. This is achieved by the leader broker being smart as to when it responds to the request — it’ll send back a response once all the in-sync replicas receive the record themselves.

 - **Acks**=all must be used in conjunction with min.insync.replicas

 - **In-sync replicas**: An in-sync replica (ISR) is a replica that has the latest data for a given partition. A leader is always an in-sync replica. A follower is an in-sync replica only if it has fully caught up to the partition it’s following.

 - **Minimum In-Sync Replica**: min.insync.replicas is a config on the broker that denotes the minimum number of in-sync replicas required to exist for a broker to allow acks=all requests. That means if you use replication.factor=3, min.insync=2, acks=all, you can only tolerate 1 broker going down, otherwise the producer will receive an exception on send.

 - **max.in.flight.request.per.connection**: setting while controls how many produce requests can be made in parallel. Set it to 1 if you need to ensure ordering(may impact throughput) 

 -  **min.insync.replicas=X allows acks=all** requests to continue to work when at least x replicas of the partition are in sync
if we go below that value of in-sync replicas, the producer will start receiving exceptions. 




<p align="center">
  <img width="737" height="837" alt="image" src="https://github.com/user-attachments/assets/dbf6f8c5-36e9-47d0-92c6-8915f8e34fea" />
</p>


## 6. Idempotence

### Kafka < 0.11 (Pre‑Idempotence Era)
- **acks=all**  
  Producer waits until all in‑sync replicas (ISR) acknowledge the message → ensures durability.  
- **min.insync.replicas**  
  Broker/topic setting requiring at least 2 replicas to confirm before acknowledging.  
- **retries=MAX_INT**  
  Producer retries indefinitely on transient errors.  
- **max.in.flight.requests.per.connection=1**  
  Only one request at a time → prevents reordering during retries, but reduces throughput.

### Kafka ≥ 0.11 (Idempotent Producers)
- **enable.idempotence=true**  
  Automatically implies:  
  - `acks=all`  
  - `retries=MAX_INT`  
  - `max.in.flight.requests.per.connection=1` (or 5 in Kafka ≥ 1.0)  
- Guarantees **exactly‑once semantics** for producers (no duplicates, no reordering).  
- Still requires `min.insync.replicas=2` for durability.  
- Improves performance while keeping ordering guarantees.

### Trade‑offs
- Running a **safe producer** (idempotence + strong durability) may reduce throughput and increase latency.  
- For critical systems (payments, financial transactions), this is essential.  
- For high‑volume, less critical streams (logs, metrics), weaker guarantees may be acceptable for speed.





 

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/ae9561c7-01d6-427d-9649-935831c14d65" />
