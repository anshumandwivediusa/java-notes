# Java Performance Tuning

## JVM Architecture
<p align="center">
  <img width="669" height="588" alt="image" src="https://github.com/user-attachments/assets/e3a2ca37-726e-48fd-a238-e321458cb991" />
</p>


### Class Loader Subsystem
- **Bootstrap Loader** → Loads core Java classes (`java.lang.*`, etc.).  
- **Extension Loader** → Loads classes from `ext` directories.  
- **Application Loader** → Loads user-defined classes.  
- **Phases**:  
  - **Loading** → Reads `.class` files.  
  - **Linking** → Verify (bytecode check), Prepare (allocate memory), Resolve (replace symbolic references).  
  - **Initialization** → Executes static initializers.  


### Runtime Data Areas
- **Method Area** → Stores class metadata, static variables.  
- **Heap** → Objects and arrays.  
- **Stack** → Each thread has its own stack with frames (Local Variable Array, Operand Stack, Frame Data).  
- **PC Register** → Tracks current instruction for each thread.  
- **Native Method Stack** → Executes native (non-Java) code.  


### Execution Engine
- **Interpreter** → Reads and executes bytecode line by line.  
- **JIT Compiler** → Converts bytecode into native machine code for performance.  
  - Intermediate Code Generator.  
  - Code Optimizer.  
  - Target Code Generator.  
  - Profiler (monitors hotspots).  
- **Garbage Collector** → Frees unused objects from heap.  


### Native Method Interface (JNI)
- **JNI** → Allows Java to call native C/C++ libraries.  
- **Native Method Library** → Provides system-level functionality (e.g., file I/O, networking).  


### JVM vs JRE vs JDK
| **Component** | **Contains** | **Purpose** |
|---------------|--------------|-------------|
| **JVM** | Class loader, runtime areas, execution engine | Runs bytecode |
| **JRE** | JVM + core libraries | Runs Java applications |
| **JDK** | JRE + compiler + dev tools | Builds Java applications |

## Java Performance & GC Tuning Cheat Sheet

**Java performance tuning focuses on optimizing JVM settings, garbage collection, memory management, and code-level practices to ensure applications run faster, with lower latency and predictable resource usage. The most impactful areas are garbage collector choice, heap sizing, profiling, and efficient use of data structures.**

### JVM & Garbage Collection Tuning
- **Choose the right GC**:  
  - **G1GC** → Default since JDK 9, balanced throughput and pause times.  
  - **ZGC** → Ultra-low pause times (<10 ms), ideal for latency-sensitive apps.  
  - **Shenandoah** → Low pause GC with concurrent compaction.  
  - **Parallel GC** → Best for batch jobs, maximizes throughput.  

| **[GC Type](ca://s?q=Java_GC_types)** | **[JVM Option](ca://s?q=JVM_GC_configuration_flags)** | **[Best Use Case](ca://s?q=Java_GC_use_cases)** |
| --- | --- | --- |
| **[G1GC](ca://s?q=Configure_G1GC_in_Java)** | ``-XX:+UseG1GC`` (default since JDK 9) | Balanced throughput and pause times; general-purpose server apps |
| **[ZGC](ca://s?q=Configure_ZGC_in_Java)** | ``-XX:+UseZGC`` | Ultra-low pause times (<10 ms); latency-sensitive apps, trading systems |
| **[Shenandoah](ca://s?q=Configure_Shenandoah_in_Java)** | ``-XX:+UseShenandoahGC`` | Low pause GC with concurrent compaction; interactive apps |
| **[Parallel GC](ca://s?q=Configure_ParallelGC_in_Java)** | ``-XX:+UseParallelGC`` | Maximizes throughput; batch jobs, background processing |

- **Heap sizing**: Allocate ~75% of physical memory to JVM heap, leaving room for OS and metaspace.  
- **Key flags**:  
  ```bash
  -Xms2g -Xmx4g
  -XX:+UseG1GC
  -XX:MaxGCPauseMillis=200
  -XX:+UseStringDeduplication
  -XX:NewRatio=2
  -XX:SurvivorRatio=8
  ```

### Heap Generations
- **Young Generation** → Where new objects are created.
  Most newly instantiated objects begin their life here. Because a high percentage of objects are short-lived (die young), this area experiences frequent, fast cleanups called Minor Garbage Collections.  
  - **Eden Space** → All **brand new** objects start here. Minor GC. 
  - **Survivor Spaces (S0, S1)** → Objects surviving Minor GC move here.  
  - **Minor GC** → Frequent, fast collections.  
- **Old Generation (Tenured)** → Long-lived objects promoted here after surviving multiple GCs.  
  - **Major GC / Full GC** → Less frequent, more expensive.  
- **Metaspace** → Stores class metadata (replaced PermGen in Java 8).  
<img width="591" height="414" alt="image" src="https://github.com/user-attachments/assets/9a087abc-e6cc-4428-b810-741252dfb422" />

### Heap Memory vs. Stack Memory in Java
| **[Feature](ca://s?q=Java_memory_features)** | **[Heap Memory](ca://s?q=Java_heap_memory)** | **[Stack Memory](ca://s?q=Java_stack_memory)** |
| --- | --- | --- |
| **[Primary Content](ca://s?q=Heap_vs_Stack_primary_content)** | Stores actual object instances and array elements | Stores primitive local variables and object reference pointers |
| **[Thread Visibility](ca://s?q=Heap_vs_Stack_thread_visibility)** | Shared globally across all threads | Private to the specific thread that owns it |
| **[Size Limitation](ca://s?q=Heap_vs_Stack_size_limitation)** | Much larger; depends on system configuration | Smaller; fixed size per thread |
| **[Life Cycle](ca://s?q=Heap_vs_Stack_life_cycle)** | Lifespan depends on references; managed by Garbage Collection | Lifespan matches method execution; wiped when method finishes |
| **[Performance Impact](ca://s?q=Heap_vs_Stack_performance)** | Slower due to GC overhead | Faster due to direct memory access |
| **[Error Types](ca://s?q=Heap_vs_Stack_errors)** | OutOfMemoryError when heap is exhausted | StackOverflowError when stack depth exceeds limit |
| **[Use Cases](ca://s?q=Heap_vs_Stack_use_cases)** | Long-lived objects, collections, large data structures | Method calls, recursion, temporary variables |

### Memory Management
- **Avoid leaks**: Always close resources (`try-with-resources`).  
  - **Java:** Always close resources (try-with-resources) for streams, sockets, DB connections.
  - **Spring Boot:** Use Spring Data JPA with proper transaction boundaries (@Transactional) so connections are released. Configure HikariCP (default pool) with sensible timeouts to avoid connection leaks.
- **String handling**: Use `StringBuilder` for concatenation in loops.  
  - **Java:** Use StringBuilder or StringBuffer for concatenation in loops.
  - **Spring Boot:** For JSON/XML serialization, rely on Jackson or Spring converters instead of manual string concatenation.
- **Object pooling**: Reuse objects where possible, avoid unnecessary creation.  
  - **Java:** Reuse objects where possible, avoid unnecessary creation.
  - **Spring Boot:** Use connection pools (HikariCP, Redis Lettuce pool) and thread pools (@Async, TaskExecutor) to manage resources efficiently.
- **Compressed OOPs**: Enable `-XX:+UseCompressedOops` for reduced memory footprint.  
  - **Java:** Enable -XX:+UseCompressedOops to reduce memory footprint.
  - **Spring Boot:** Combine with Actuator metrics to monitor heap usage and GC behavior in production.
- **Spring Methods**
  - **Caching:** Use @Cacheable with providers like Redis or Caffeine to reduce repeated DB calls.
  - **Lazy Initialization:** Enable spring.main.lazy-initialization=true to avoid loading unused beans at startup.
  - **Batch Processing:** Use chunk-oriented processing in Spring Batch to avoid loading huge datasets into memory.
  - **Streaming APIs:** For large responses, use ResponseBodyEmitter or Flux (WebFlux) to stream data instead of holding everything in memory.
  - **Monitoring:** Expose /actuator/metrics/jvm.memory.used and integrate with Prometheus/Grafana for real-time tracking.

### Profiling & Monitoring
- **Java Flight Recorder (JFR)** → Lightweight production profiler.  
- **VisualVM / JProfiler** → Detect CPU/memory hotspots.  
- **GC logs** → Enable with `-Xlog:gc*` (Java 9+) or `-verbose:gc` (Java 8).  

### Code-Level Optimizations
- **Efficient collections**: Use `ArrayList` for fast iteration, `HashMap` for lookups.  
- **Concurrency**: Use `ConcurrentHashMap`, `ForkJoinPool`, avoid excessive synchronization.  
- **I/O tuning**: Prefer NIO (`java.nio`) for scalable network/file operations.  
- **Lazy loading**: Initialize heavy objects only when needed.  

### Performance Checklist

| **Area** | **Best Practice** | **Impact** |
|----------|------------------|------------|
| **GC** | Select G1GC/ZGC based on workload | Reduces pause times |
| **Heap** | 75% of physical memory | Prevents OOM & swapping |
| **Young Gen** | Tune Eden/Survivor ratios | Faster Minor GC |
| **Old Gen** | Monitor promotion rates | Avoid costly Full GC |
| **Strings** | Use `StringBuilder` | Faster concatenation |
| **Profiling** | JFR, VisualVM | Identifies bottlenecks |
| **Concurrency** | Use modern APIs | Improves throughput |
| **I/O** | Use NIO | Scales better under load |

### Risks if Ignored
- **Frequent GC pauses** → Latency spikes in APIs.  
- **Oversized heap** → Long GC pauses, OS swapping.  
- **Unoptimized strings** → High CPU usage.  
- **Memory leaks** → OutOfMemoryErrors in production.  
- **Poor Survivor/Eden tuning** → Excessive promotions → Old Gen fills quickly → Full GC storms.  
