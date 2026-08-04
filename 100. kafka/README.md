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
    <img width="2041" height="1243" alt="image" src="https://github.com/user-attachments/assets/b4c5ec2c-41a2-47ca-bfa0-fa272eb6a09b" />
   </p>


## 🔄 Producer–Consumer Flow
1. **Producer** sends a message → Kafka broker stores it in the topic partition log.  
2. **Consumer** fetches messages from the log → processes them in order.  
3. Kafka itself remains **stateless** about producers/consumers — only offsets are tracked.  



<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/ae9561c7-01d6-427d-9649-935831c14d65" />
