# Logs

## What Are Logs?
- **Logs** are records of events happening inside your application.
- They capture information like errors, warnings, system behavior, and debugging details.

## Why Do We Need Logs?
- **Debugging** → Helps trace issues when something goes wrong.
- **Monitoring** → Keeps track of system health and performance.
- **Auditing** → Provides a trail of actions for compliance/security.
- **Troubleshooting** → Speeds up root cause analysis.


## Log Levels (Standard)
Logs are categorized by severity:

| **Level** | **Purpose** | **Example** |
|-----------|-------------|-------------|
| **TRACE** | Very detailed, step-by-step execution | Entering **method** X |
| **DEBUG** | **Developer**-focused info | Variable values, flow decisions |
| **INFO** | **General** application events | "Application started" |
| **WARN** | Potential issues | **Deprecated** API usage |
| **ERROR** | **Failures** that need attention | Database connection failed |



## Logs in Spring Boot
- Uses **SLF4J** as a logging API.
- Default implementation: **Logback**.
- Configurable via `application.properties`:
  ```properties
  logging.level.root=INFO
  logging.level.com.example=DEBUG
  logging.file.name=app.log
  ```
- Output example:
  ```
  2026-08-16 13:05:22 INFO 12345 --- [main] com.example.App : Started App in 2.345s
  ```



## Package/Class-Level Logging
Spring Boot allows you to set log levels per package or class in `application.properties`:

```properties
logging.level.root=INFO
logging.level.com.example=DEBUG
logging.level.com.example.service.EmployeeService=TRACE
logging.level.org.springframework.web=WARN
```

- **Root logger** → applies to everything unless overridden.
- **Package-level** → e.g., `com.example` set to DEBUG.
- **Class-level** → e.g., `EmployeeService` set to TRACE.

This way, you can have **INFO logs globally**, but **DEBUG/TRACE logs only for specific classes** where you need more detail.



## Method-Level Logging
Strictly speaking, you **cannot configure log levels per method** in Spring Boot properties.  
But you can achieve similar control by:

- **Using different loggers inside methods**:
  ```java
  private static final Logger auditLogger = LoggerFactory.getLogger("AUDIT");
  private static final Logger debugLogger = LoggerFactory.getLogger("DEBUG_LOG");

  public void processData() {
      debugLogger.debug("Processing data...");
      auditLogger.info("Audit entry created");
  }
  ```
  Then configure:
  ```properties
  logging.level.AUDIT=INFO
  logging.level.DEBUG_LOG=DEBUG
  ```

- **Conditional logging**:
  ```java
  if (logger.isDebugEnabled()) {
      logger.debug("Expensive debug details: {}", computeDetails());
  }
  ```

This gives you **fine-grained control at the method level** without cluttering global logs.



### Summary Table

| **Scope** | **Config Example** | **Use Case** |
|-----------|--------------------|--------------|
| **Root** | `logging.level.root=INFO` | Default for all logs |
| **Package** | `logging.level.com.example=DEBUG` | Enable debugging for your app only |
| **Class** | `logging.level.com.example.EmployeeService=TRACE` | Trace one class deeply |
| **Custom Logger** | `logging.level.AUDIT=INFO` | Separate audit/debug flows |






## Logging Basics in Spring Boot
- **SLF4J API** → Provides a unified logging interface.
- **Logback** → Default logging implementation (fast, flexible).
- **Alternatives** → You can switch to Log4j2 or Java Util Logging (JUL).

### Default Behavior
- **Console output** with timestamp, log level, logger name, and message.
- **Root logger level = INFO** (so DEBUG/TRACE messages are hidden unless configured).
- Example:
  ```
  2026-08-16 13:00:22.123 INFO 12345 --- [main] com.example.Application: Started Application in 2.345 seconds
  ```



## Configuring Logging
### Via `application.properties` or `application.yml`
- Set log levels:
  ```properties
  logging.level.root=INFO
  logging.level.com.example.service=DEBUG
  logging.level.org.springframework.web=WARN
  ```
- File logging:
  ```properties
  logging.file.name=app.log
  logging.file.path=./logs
  logging.file.max-size=10MB
  logging.file.max-history=7
  ```
- Console format:
  ```properties
  logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
  ```

### Via `logback-spring.xml`
- More advanced control: appenders, rolling policies, MDC (Mapped Diagnostic Context).
- Example snippet:
  ```xml
  <configuration>
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
      <file>logs/app.log</file>
      <encoder>
        <pattern>%d %-5level %logger{36} - %msg%n</pattern>
      </encoder>
    </appender>
    <root level="INFO">
      <appender-ref ref="FILE"/>
    </root>
  </configuration>
  ```



### Comparison of Logging Options

| **Framework** | **Default in Spring Boot** | **Strengths** | **Use Case** |
|----------------|--------------------------|----------------|----------------|
| **Logback** | Yes | Fast, flexible, supports rolling logs | General-purpose logging |
| **Log4j2** | No | Async logging, high throughput | High-performance apps |
| **JUL** | No | Built into JDK | Lightweight/simple apps |



### Best Practices
- Use **DEBUG** for development, **INFO** for production monitoring, **ERROR** for failures.
- Centralize logs with tools like **ELK stack (Elasticsearch, Logstash, Kibana)** or **Splunk**.
- Use **MDC** for tracing requests across microservices.
- Avoid excessive logging (performance hit, log noise).

# Splunk



## Steps to Integrate Spring Boot with Splunk

### 1. **Set up Splunk HEC**
- In Splunk Web → **Settings → Data Inputs → HTTP Event Collector**.
- Enable HEC globally and create a **token**.
- Configure:
  - **Source type**: `_json`
  - **Index**: e.g., `spring_logs`
  - Enable **Acknowledgement** for guaranteed delivery.

---

### 2. **Add Dependencies**
In `pom.xml`:
```xml
<dependency>
  <groupId>net.logstash.logback</groupId>
  <artifactId>logstash-logback-encoder</artifactId>
  <version>7.4</version>
</dependency>
<dependency>
  <groupId>org.apache.httpcomponents.client5</groupId>
  <artifactId>httpclient5</artifactId>
  <version>5.3.1</version>
</dependency>
```



### 3. **Configure Properties**
In `application.properties`:
```properties
splunk.hec.url=https://your-splunk:8088/services/collector
splunk.hec.token=YOUR_HEC_TOKEN
splunk.hec.index=spring_logs
splunk.hec.source=payment-service
```



### 4. **Logback Configuration**
Create `logback-spring.xml`:
```xml
<configuration>
  <appender name="SPLUNK" class="com.example.SplunkHecAppender">
    <url>${splunk.hec.url}</url>
    <token>${splunk.hec.token}</token>
    <index>${splunk.hec.index}</index>
    <source>${splunk.hec.source}</source>
    <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
  </appender>

  <root level="INFO">
    <appender-ref ref="SPLUNK"/>
  </root>
</configuration>
```


## Benefits of Splunk Integration
| **Feature** | **Advantage** |
|----------------|----------------|
| **Real-time streaming** | Immediate visibility into app behavior |
| **Centralized logs** | One place for all microservices |
| **Search & dashboards** | Powerful Splunk queries and visualizations |
| **Alerting** | Trigger alerts on error patterns |
| **Scalability** | Handles high-volume logs |



## Best Practices
- Use **structured JSON logs** with `LogstashEncoder` for better parsing.
- Separate **audit logs** from **debug logs** using different appenders.
- Avoid logging sensitive data (passwords, tokens).
- Monitor Splunk ingestion performance to prevent bottlenecks.


