# Java Performance Tuning

## JVM Architecture
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

## 2. Java Performance
**Java performance tuning focuses on optimizing JVM settings, garbage collection, memory management, and code-level practices to ensure applications run faster, with lower latency and predictable resource usage. The most impactful areas are garbage collector choice, heap sizing, profiling, and efficient use of data structures.** 

### JVM & Garbage Collection Tuning
- **Choose the right GC**:  
  - **G1GC** → Default since JDK 9, balanced throughput and pause times.  
  - **ZGC** → Ultra-low pause times (<10 ms), ideal for latency-sensitive apps.  
  - **Shenandoah** → Low pause GC with concurrent compaction.  
  - **Parallel GC** → Best for batch jobs, maximizes throughput.  
- **Heap sizing**: Allocate **~75% of physical memory** to the heap, leaving room for OS and metaspace.  
- **Key flags**:  
  ```bash
  -Xms2g -Xmx4g
  -XX:+UseG1GC
  -XX:MaxGCPauseMillis=200
  -XX:+UseStringDeduplication
  ```

### Memory Management
- **Avoid memory leaks**: Always close resources (`try-with-resources`).  
- **String handling**: Use `StringBuilder` for concatenation in loops.  
- **Object pooling**: Avoid unnecessary object creation; reuse where possible.  
- **Compressed OOPs**: Enable `-XX:+UseCompressedOops` for reduced memory footprint.  


### Profiling & Monitoring
- **Java Flight Recorder (JFR)**: Lightweight profiling tool for production.  
- **VisualVM / JProfiler**: Detect CPU/memory hotspots.  
- **GC logs**: Enable with `-Xlog:gc*` to analyze pause times and collection frequency.  


### Code-Level Optimizations
- **Efficient collections**: Use `ArrayList` for fast iteration, `HashMap` for lookups.  
- **Concurrency**: Use `ConcurrentHashMap`, `ForkJoinPool`, and avoid excessive synchronization.  
- **I/O tuning**: Prefer NIO (`java.nio`) for scalable network/file operations.  
- **Lazy loading**: Initialize heavy objects only when needed.  


### Performance Checklist

| **Area** | **Best Practice** | **Impact** |
|----------|------------------|------------|
| **GC** | Select G1GC/ZGC based on workload | Reduces pause times |
| **Heap** | 75% of physical memory | Prevents OOM & swapping |
| **Strings** | Use `StringBuilder` | Faster concatenation |
| **Profiling** | JFR, VisualVM | Identifies bottlenecks |
| **Concurrency** | Use modern APIs | Improves throughput |
| **I/O** | Use NIO | Scales better under load |


### Risks if Ignored
- **Frequent GC pauses** → Latency spikes in APIs.  
- **Oversized heap** → Long GC pauses, OS swapping.  
- **Unoptimized strings** → High CPU usage.  
- **Memory leaks** → OutOfMemoryErrors in production.  

