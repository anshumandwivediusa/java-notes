# Kafka in Spring Boot

## 1. Kafka Producer in Spring Boot
- **KafkaTemplate**  
  - Core API for sending messages.  
  - Example:
    ```java
    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    public void sendMessage(String msg) {
        kafkaTemplate.send("orders", msg);
    }
    ```
- **Producer Configurations** (via `application.yml`):  
  ```yaml
  spring.kafka.producer.bootstrap-servers: localhost:9092
  spring.kafka.producer.key-serializer: org.apache.kafka.common.serialization.StringSerializer
  spring.kafka.producer.value-serializer: org.apache.kafka.common.serialization.StringSerializer
  spring.kafka.producer.acks: all
  spring.kafka.producer.retries: 3
  ```

## 2. Kafka Consumer in Spring Boot
- **@KafkaListener**  
  - Annotation-driven consumer method.  
  - Example:
    ```java
    @KafkaListener(topics = "orders", groupId = "order-service")
    public void consume(String message) {
        System.out.println("Received: " + message);
    }
    ```
- **Consumer Configurations**:  
  ```yaml
  spring.kafka.consumer.bootstrap-servers: localhost:9092
  spring.kafka.consumer.group-id: order-service
  spring.kafka.consumer.key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  spring.kafka.consumer.value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  spring.kafka.consumer.auto-offset-reset: earliest
  spring.kafka.consumer.enable-auto-commit: true
  ```

## 3. Producer–Consumer Flow in Spring Boot
1. **Producer** creates a `ProducerRecord` via `KafkaTemplate`.  
2. Message is serialized and sent to Kafka broker.  
3. **Broker** appends the message to the topic partition log.  
4. **Consumer** (via `@KafkaListener`) receives the message.  
5. Offset is tracked (auto or manual commit).  

## 4. **Consumer Groups**  
  - Multiple consumers in the same group share partitions of a topic.
  - Kafka ensures only one consumer per group reads a partition.
  -  If a consumer fails, Kafka rebalances partitions among the remaining consumers.
  ```java
    @KafkaListener(topics = "orders", groupId = "order-service")
    public void consume(String message) {
        System.out.println("Received: " + message);
    }
   ```

## 5. **Error Handling**: Dead-letter topics, retry templates.  

- **SeekToCurrentErrorHandler**  
  - Default handler in Spring Kafka.  
  - When a record fails, the consumer seeks back to the current offset and retries.  
  - Prevents skipping records but may cause repeated failures if the data is bad.

- **Dead Letter Topics (DLQ)**  
  - Failed messages are redirected to a special topic (e.g., `_orders.DLT_`).  
  - Allows you to inspect, reprocess, or manually fix problematic records.  
  - Implemented via `_DeadLetterPublishingRecoverer_`.

- **Retry Templates**  
  - Configurable retries before sending to DLQ.  
  - Useful when errors are transient (e.g., network hiccups, temporary DB outage).  
  - Can use exponential backoff strategies.

- **ErrorHandler Interface**  
  - Custom error handlers can be implemented for fine-grained control.  
  - Example: log the error, alert monitoring systems, or trigger compensating actions.

```java
@Bean
public DefaultErrorHandler errorHandler(KafkaTemplate<String, String> template) {
    
    // **DeadLetterPublishingRecoverer** is responsible for sending failed records
    // to a Dead Letter Topic (DLT). This ensures that poison messages (those
    // that cannot be processed even after retries) are not lost, but instead
    // redirected for later inspection or reprocessing.
    DeadLetterPublishingRecoverer recoverer =
        new DeadLetterPublishingRecoverer(
            template, // KafkaTemplate used to publish to the DLT
            (record, ex) -> 
                // Define the target partition for the failed record.
                // Here, we append ".DLT" to the original topic name so that
                // each topic has its own corresponding dead-letter topic.
                new TopicPartition(record.topic() + ".DLT", record.partition())
        );

    // **FixedBackOff** defines the retry policy:
    // - First parameter: delay between retries (1000 ms = 1 second).
    // - Second parameter: maximum number of retries (2 attempts).
    // This means: if a record fails, Spring Kafka will retry processing it
    // twice, waiting 1 second between each attempt.
    // If the record still fails after these retries, it will be sent to the DLT.
    FixedBackOff backOffPolicy = new FixedBackOff(1000L, 2);

    // DefaultErrorHandler ties everything together:
    // - It uses SeekToCurrent strategy (seek back to the failed record’s offset).
    // - It applies the retry policy defined above.
    // - It delegates final failure handling to the DeadLetterPublishingRecoverer.
    return new DefaultErrorHandler(recoverer, backOffPolicy);
}
```
- Retries twice with 1s delay.  
- If still failing, message is sent to `topic.DLT`.

---

## 📊 Summary Table

| **Strategy** | **Spring Boot Feature** | **Best Use Case** |
|--------------|--------------------------|-------------------|
| SeekToCurrent | Default handler | Retry same record immediately |
| Dead Letter Topic | DeadLetterPublishingRecoverer | Persist failed records for later |
| Retry Template | Retry/backoff policies | Handle transient errors gracefully |
| Custom ErrorHandler | Implement interface | Complex recovery logic |

---

## 🔑 Key Insight
Error handling in Kafka isn’t just about retries — it’s about **deciding what to do with poison messages**:
- Retry if the error is transient.  
- Redirect to DLQ if the error is permanent.  
- Monitor and alert so operators can intervene.  


## 6. **Transactions**: Exactly-once semantics with `enable.idempotence=true`.  


## 7. **Batch Consumption**: Fetch multiple records per poll for efficiency.


