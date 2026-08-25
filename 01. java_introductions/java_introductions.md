# Java Features and Program Execution

Java is a **programming language** and a **platform**. 

1. Java as a **Programming Language**, because java Language provides set of **Instructions**:
   - Syntax, keywords, operators, and constructs to write applications.
   - Object‑oriented features (encapsulation, inheritance, polymorphism).
   - Strong typing and platform independence (via bytecode).

2. **Java as a Platform**
A platform is any environment where programs run (hardware or software). Java qualifies as a platform because it provides its own runtime:
   - JVM → Converts bytecode into machine code for the host system.
   - JRE → Runtime environment including JVM + core libraries.
   - Java API → Rich set of classes/interfaces for networking, collections, concurrency, etc.



✅ Together, **JVM + JRE + API = Java Platform**.  
✅ **Java Language → How you write instructions?**  
✅ **Java Platform → Where those instructions run?**  
✅ The JVM is not just for Java — it’s a **polyglot runtime** supporting dozens of languages. For mainstream development, stick with Java, Kotlin, Scala, Groovy, or Clojure; for specialized needs, explore JRuby, Jython, Eta, or Renjin.

<p align="center">
  <img width="400" height="275" alt="image" src="https://github.com/user-attachments/assets/daaa2e0f-19c3-497d-bca0-a3f831b2f342" />
</p>

## 1. Introduction to Java Foundations

Java is a **high-level, object-oriented programming language** known for its **portability, robustness, and scalability (PRS)**. Programs written in Java run on the **Java Virtual Machine (JVM)**, making them platform-independent — the famous principle of *“Write Once, Run Anywhere.”*

| **Reason** | **Benefit** | **Example Use Case** |
| --- | --- | --- |
| [Platform Independence](ca://s?q=Java_platform_independence) | Runs on any JVM | Cross-platform apps |
| [Enterprise Reliability](ca://s?q=Java_enterprise_reliability) | Secure & scalable | Banking systems |
| [Community Support](ca://s?q=Java_community_support) | Huge ecosystem | Open-source frameworks |
| [Security & Stability](ca://s?q=Java_security_and_stability) | Safe & future-proof | Government apps |
| [Standard Library](ca://s?q=Java_standard_library) | Rich APIs | Networking, collections |
| [Cloud & Big Data](ca://s?q=Java_in_cloud_and_big_data) | Cloud-native & data-heavy apps | Hadoop, Spark |
| [Ease of Hiring](ca://s?q=Java_learning_resources) | Large talent pool | Enterprise staffing |

## 2. Features of Java :

![java-features](https://user-images.githubusercontent.com/2780145/34343690-2fd47db0-e9ff-11e7-9630-75423dda7eaa.png)

- **Simple:**
  - User friendly syntax based on C++
  - It has Automatic Garbage Collection
  - It has Rich set of APIs 
  - Removed confusing features - explicit pointers, operator overloading, multiple inheritance, etc
 
- **Object-Oriented:**
  - Java’s Object‑Oriented design is not just about syntax — it’s about **thinking in terms of entities and relationships.**
  - By combining **Objects, Classes, Polymorphism, Inheritance, Encapsulation, and Abstraction (PEI-A)** Java enables developers to build systems that are modular, extensible, and resilient.
  - In Java, we organize the software as a combination of different types of objects that incorporates both **data (properties) and behavior (methods)**.
  
- **Platform Independent:**
   - Java Compiler translates human‑readable .java source code into bytecode (.class files).
   - This bytecode is not tied to any single operating system or hardware — it’s platform‑independent.
   - The JVM (Java Virtual Machine) interprets or compiles this bytecode into native machine instructions for the host system.
   - As a result, the same .class file can run seamlessly on Windows, Linux, macOS, or Android, without modification.

  👉 Formula:  Java Source → Compiler → Bytecode → JVM → Native Execution

- **Secured:**
  - **No explicit pointer**
  - **JVM -** java Programs run inside virtual machine sandbox 
  - **Classloader -** adds security by separating the package for the classes of the local file system from those that are imported from network sources.
  - **Bytecode Verifier -** checks the code fragments for illegal code that can violate access right to objects.
  - **Security Manager -** determines what resources a class can access such as reading and writing to the local disk.
  - **More -** developers can add extra security through SSL, JAAS, Cryptography etc.
  
- **Robust:**
  - **Good memory management -** automatic garbage collection.
  - **No pointers -** increases security. 
  - **Exception handling -** increases robustness against errors.
  - **Strongly typed -** every variable must be declared with a data type.
  - **Statically typed -** type checking of variables is performed at compile time.
  
- **Architecture-Neutral:**
  - There is no implementation dependent features. e.g. size of primitive types is fixed.
  
- **Portable:**
  - Write Once and Run Anywhere.
   
- **Interpreted:**
  - Java is compiled into bytecode (by Javac Compiler), and the JVM within the JRE interprets that bytecode to make it platform‑independent.
  - The interpreter reads bytecode stream then execute the instructions.
  
- **High-Performance:**
  - **Uses ByteCode -** Java is faster than traditional interpreted languages since byte code is "close" to native code. 
  - **Just-In-Time (JIT) -** it is designed to support JIT compilers, which dynamically compile bytecodes to machine code. 
  - **Garbage collector -** collect the unused memory space and improve the performance of the application.
  - NOTE: Java is still slower than a compiled language like C/C++.
  
- **Distributed:**
  - We can create distributed applications in java. RMI and EJB are used for creating distributed applications.
  - We may access files by calling the methods from any machine on the internet.
  
- **Multi-threaded:**
  - A thread is like a separate program, executing concurrently. We can write Java programs that deal with many tasks at once by defining multiple threads.
  - The main advantage of multi-threading is that it doesn't occupy memory for each thread. It shares a common memory area.
  - Threads are important for multi-media, Web applications etc.
  
- **Dynamic:**
  - **Dynamic Compilation (JIT) -** Implementations to gain performance during program execution. The machine code emitted by a dynamic compiler is constructed and optimized at program runtime, the use of dynamic compilation enables optimizations for efficiency.
  - **Load on Demand -** Loads in classes as they are needed, even from across the network.
  - **Dynamic memory allocation -** All Java objects are dynamically allocated. 
  - **Dynamic Polymorphism -** Compiler doesn’t know which method to be called in advance. JVM decides which method to called at run time.


## 3. Java Architecture Notes

<p align="center">
  <img width="669" height="588" alt="image" src="https://github.com/user-attachments/assets/fd53a0ea-6917-497b-a517-2f181f49bf0f" />
<br><a>Java Virtual Machine</a>
</p>

### 1. **Java Virtual Machine (JVM)**
- Heart of Java architecture.  
- Converts **bytecode** into machine code for execution.  
- Provides **platform independence**.  
- **Bytecode** → Platform‑independent instructions generated by the Java compiler.
- **Specification vs Implementation** → The JVM is a specification; vendors can build their own implementations as long as they follow it.
- **JVM Implementations** → HotSpot (default), OpenJ9, Zing, JRockit, and others — all conforming to the JVM specification.
- **Polyglot Runtime** → JVM runs not only Java but also Kotlin, Groovy, Scala, JRuby, Clojure, and more.
- **Class Loading** → Dynamically loads classes at runtime.
- **JIT Compilation** → Just‑In‑Time compiler translates bytecode into native machine code for performance.
- **Garbage Collection** → Automatic memory management with multiple GC algorithms (G1, ZGC, Shenandoah).
- **Security** → Bytecode verification and sandboxing protect against unsafe code.
- **Performance Tuning** → JVM parameters (-Xmx, -Xms, GC tuning) allow fine‑grained optimization.Handles **memory management, garbage collection, and security checks**.  

### 2. **Java Runtime Environment (JRE)**
- Provides the environment to run Java applications.  
- Includes **JVM** + core libraries (like java.util.*, java.io.*) and supporting files.
- Does not contain development tools like compiler.  
- Ideal for end-users who only need to run Java programs.  

### 3. **Java Development Kit (JDK)**
- Complete package for developers.  
- Contains **compiler (javac)**, debugger, documentation tools, and the **JRE**.  
- Used for writing, compiling, and testing Java programs.  

### 4. **Bytecode**
- Intermediate representation of Java code.
- Generated by the compiler (`javac`).
- Portable across platforms — runs on any JVM.
   ```java
   public class AddExample {
       public static void main(String[] args) {
           int a = 5;
           int b = 10;
           int sum = a + b;
           System.out.println(sum);
       }
   }
   
   Compiled from "AddExample.java"
   public class AddExample {
     public AddExample();
       Code:
          0: aload_0
          1: invokespecial #1   // calls Object.<init>
          4: return
   
     public static void main(java.lang.String[]);
       Code:
          0: iconst_5           // push constant 5
          1: istore_1           // store into local variable a
          2: bipush 10          // push constant 10
          4: istore_2           // store into local variable b
          5: iload_1            // load a
          6: iload_2            // load b
          7: iadd               // add integers
          8: istore_3           // store result in sum
          9: getstatic #2       // get System.out
         12: iload_3            // load sum
         13: invokevirtual #3   // call println(int)
         16: return
   }
   ```

### 5. **Class Loader**
- Part of JVM responsible for loading classes into memory.  
- Types:  
  - **Bootstrap Class Loader** → loads core Java classes. (java.lang, java.util).
  - **Extension Class Loader** → loads extension libraries. (JAVA_HOME/lib/ext - Plugins) 
  - **Application Class Loader** → loads user-defined classes. (Loads user-defined classes from the classpath.)


<img width="262" height="217" alt="image" src="https://github.com/user-attachments/assets/03d97f8b-baf0-4d32-be95-04ad91589e17" />

### 6. **Execution Engine**
- Executes bytecode instructions.
- Components:
  - **Interpreter** → executes line by line.
  - **JIT Compiler (Just-In-Time)** → improves performance by compiling bytecode into native machine code.
- **Simple Flow of JVM Execution**
  - **Startup → Interpreter runs immediately**
    - When you launch a Java program, the JVM doesn’t wait.
    - The interpreter reads bytecode line by line and executes it right away.
    - The Interpreter inside the JVM reads bytecode instructions one by one and directly performs the equivalent CPU operations using the JVM’s internal routines.
      - Think of it like a translator who reads a sentence in one language and immediately speaks it in another — no permanent translation stored.
    - This gives you fast startup.
  - **As program runs → JVM watches usage**
    - The JVM keeps track of which methods are used often.
    - This is called profiling — it’s like the JVM is observing which parts of your code are “hot” (used a lot).
  - **Hot methods → JIT compiler steps in**
    - For frequently used methods, the **JIT compiler translates bytecode into native machine code**.
    - **Native code runs much faster than interpreted bytecode**.
    - This makes your program high‑performance after it warms up.
  - **Result → Best of both worlds**
    - Interpreter → quick start.
    - JIT compiler → long‑term speed.

Together, they balance fast startup with optimized performance.

### 7. **Java API & Libraries**
- Rich set of pre-built classes and interfaces.  
- Includes **Collections, Networking, I/O, Concurrency, JDBC**.  
- Saves development time by providing reusable components.  

### Summary Table

| Component | Role | Example |
|-----------|------|---------|
| JDK | Development tools | Compiler, debugger |
| JRE | Runtime environment | JVM + libraries |
| JVM | Executes bytecode | Garbage collection |
| Bytecode | Portable code format | `.class` files |
| Class Loader | Loads classes | Bootstrap loader |
| Execution Engine | Runs instructions | JIT compiler |
| Java API | Pre-built libraries | Collections, JDBC |
```
 Source Code (.java)
        |
        v
   [ JDK = JRE + Compiler, Debugger...]
        |
        v
   Bytecode (.class)
        |
        v
   [ JRE = JVM + Libraries (rt.jar, i18.jar ]
        |
        v
   JVM Execution Engine
        |
        v
   Machine Code (Platform-specific)
 ```

## 3. Java LTS versions
| **Version** | **Release Date** | **Key Features** | **Support Timeline** |
| --- | --- | --- | --- |
| **[Java 8](ca://s?q=Java_8_features)** | March 2014 | **Lambdas**, **Functional**, **Streams API**, Date/Time API | Oracle Premier Support ended 2022; extended support via vendors |
| **[Java 11](ca://s?q=Java_11_features)** | September 2018 | **HTTP Client API**, Flight Recorder, Local-Variable Syntax for Lambda | Oracle Premier Support until 2023; extended support until 2026+ |
| **[Java 17](ca://s?q=Java_17_features)** | September 2021 | **Sealed Classes**, Pattern Matching (switch preview), Strong encapsulation of JDK internals | Oracle Premier Support until 2026; extended support until 2029 |
| **[Java 21](ca://s?q=Java_21_features)** | September 2023 | **Virtual Threads** (Project Loom), Sequenced Collections, Record Patterns, Pattern Matching for switch | Oracle Premier Support until 2028; extended support until 2031 |
| **[Java 25](ca://s?q=Java_25_features)** | September 2025 | Compact source files, **Scoped Values**, Primitive type patterns, Stream Gatherers | Oracle Premier Support until 2030; extended support until 2033 |

Lambda Client Seals Thread Scope.

## 4. Evolution of JDK & JRE

- **Pre‑Java 9**  
  - JDK (Java Development Kit) = tools + compiler + JRE.  
  - JRE (Java Runtime Environment) = JVM + core libraries, used only to run applications.  
  - Developers installed JDK, end‑users often installed JRE.

- **Java 9**  
  - Introduced the **Java Platform Module System (JPMS)** → JDK reorganized into **94 modules**.  
  - Added **jlink** → command‑line tool to generate **custom runtime images** containing only the modules your application needs.  
  - Result → No strict separation between JDK and JRE; everything is modular.

- **Java 11**  
  - Oracle stopped offering separate JRE downloads.  
  - Only the JDK is distributed, but you can still create a runtime image with **jlink** if you want a lightweight runtime.  
  - This simplified distribution and pushed developers toward modular runtimes.

### Why This Matters
- **Smaller footprint** → Applications can ship with only the modules they need.  
- **Better security** → Unused modules aren’t included, reducing attack surface.  
- **Consistency** → No more confusion between JDK vs JRE downloads.  
- **Flexibility** → Developers can build custom runtimes for embedded systems, containers, or microservices.  

### Conceptual Flow
```text
Before Java 9 → JDK vs JRE (separate downloads)
Java 9 → Modules + jlink (custom runtimes)
Java 11 → JDK only (no separate JRE)
```

### Takeaway
- **Before Java 9** → JDK and JRE were distinct.  
- **From Java 9 onward** → The JDK became modular, and **jlink** allowed custom runtimes.  
- **From Java 11 onward** → Oracle stopped shipping a separate JRE; the JDK is the single distribution, and developers use **jlink** to create runtime images if needed.  

## 5. Ahead‑of‑Time (AOT)

**Ahead‑of‑Time (AOT) compilation improves startup speed and predictability by compiling Java bytecode into native machine code before execution, but it cannot fully replace Just‑In‑Time (JIT) compilation because JIT adapts dynamically to runtime workloads and optimizes “hot spots” better.**  



### Advantages of AOT Compilation
- **Faster Startup** → Native code is ready before execution, reducing warm‑up time.  
- **Predictable Performance** → No runtime compilation overhead, so performance is more consistent.  
- **Reduced Memory Usage** → Less memory needed since JIT compiler structures aren’t loaded.  
- **Better for Short‑lived Apps** → Ideal for serverless, microservices, and CLI tools where startup latency matters.  
- **Security & Deployment** → Produces self‑contained native images, easier to deploy in restricted environments.  



### Why Not Use AOT Entirely?
- **Dynamic Optimization** → JIT adapts to actual runtime behavior, optimizing hot methods more aggressively than static AOT.  
- **Cross‑Platform Portability** → AOT generates native code tied to a specific CPU architecture; JIT keeps bytecode portable.  
- **Code Size** → Native images can be larger than bytecode + JIT combo.  
- **Limited Coverage** → Some dynamic features (reflection, dynamic proxies) are harder to handle with pure AOT.  
- **Best Balance** → Modern JVMs combine AOT (for startup) with JIT (for runtime optimization) to achieve both responsiveness and speed.  



### Comparison Table

| **Aspect** | **AOT Compilation** | **JIT Compilation** |
|------------|----------------------|----------------------|
| **Startup Speed** | Very fast (native code ready) | Slower (needs profiling + compilation) |
| **Performance** | Predictable, but less adaptive | Highly optimized for runtime workload |
| **Memory Usage** | Lower (no JIT structures) | Higher (JIT compiler + profiling data) |
| **Portability** | Tied to CPU architecture | Platform‑independent bytecode |
| **Best Use Case** | Short‑lived apps, microservices, serverless | Long‑running apps, enterprise workloads |


### ✅ Takeaway
- **AOT alone** → Great for fast startup, predictable performance, and lightweight runtimes.  
- **JIT alone** → Best for adaptive optimization in long‑running applications.  
- **Modern JVMs (Project Leyden, GraalVM)** → Use a **hybrid approach**: AOT for startup + JIT for runtime, giving the best of both worlds.   [OpenJDK](https://openjdk.org/jeps/8335368)  [Java Code Geeks](https://www.javacodegeeks.com/understanding-ahead-of-time-aot-caching-in-java.html)  [javaspring.net](https://www.javaspring.net/blog/aot-compilation-java/)  


### Using AOT in Java

- **JDK 9 AOT Introduction**  
  - Java 9 introduced experimental AOT support via the tool **`jaotc`** (Java Ahead‑Of‑Time Compiler).  
  - It compiles Java bytecode into native code before runtime.  

- **jaotc Tool**  
  - Command‑line utility included in JDK 9+.  
  - Generates native shared libraries (`.so` on Linux, `.dll` on Windows).  
  - These libraries can be loaded by the JVM at startup.  

#### Example Usage

1. **Compile Java source into bytecode**  
   ```bash
   javac HelloWorld.java
   ```

2. **Generate AOT compiled library with jaotc**  
   ```bash
   jaotc --output libHelloWorld.so HelloWorld.class
   ```
   - `--output` specifies the native library file.  
   - Input is the `.class` file (bytecode).  

3. **Run with AOT library**  
   ```bash
   java -XX:AOTLibrary=./libHelloWorld.so HelloWorld
   ```
   - JVM loads the AOT‑compiled code at startup.  
   - Startup is faster since native code is already available.  

### When to Use AOT
- **Short‑lived apps** → CLI tools, microservices, serverless functions.  
- **Fast startup needs** → Applications where latency matters.  
- **Embedded systems** → Limited resources, predictable performance.  

### Takeaway
- AOT is available via **`jaotc`** starting from Java 9.  
- It’s best used for **fast startup** and **predictable performance**.  
- Not a full replacement for JIT — modern JVMs combine **AOT + JIT** for optimal balance.  



## JDK Versions 
| **JDK** | **Company** | **Base** | **Licensing Model** | **Docker Support** | **Highlights** |
| --- | --- | --- | --- | --- | --- |
| **[OpenJDK](ca://s?q=OpenJDK)** | OpenJDK Community (backed by Oracle, Red Hat, others) | Open source | GPL v2 with Classpath Exception | Yes (official OpenJDK Docker images) | Reference implementation, widely adopted |
| **[Oracle JDK](ca://s?q=Oracle_JDK)** | Oracle | OpenJDK | Oracle Binary Code License (commercial, free for dev/test, paid for production support) | Yes (Oracle provides Docker images) | Enterprise support, LTS, security patches |
| **[GraalVM](ca://s?q=GraalVM)** | Oracle Labs | OpenJDK | GPL v2 CE (Community Edition), Oracle commercial EE | Yes (GraalVM CE Docker images available) | Polyglot, AOT native images, high performance |
| **[Amazon Corretto](ca://s?q=Amazon_Corretto)** | Amazon Web Services (AWS) | OpenJDK | Free, long‑term support | Yes (AWS provides Docker images) | Production‑ready, security updates, AWS integration |
| **[Zulu JDK](ca://s?q=Zulu_JDK)** | Azul Systems | OpenJDK | GPL v2 (free builds), commercial support available | Yes (Azul provides Docker images) | Certified builds, wide OS support |
| **[Liberica JDK](ca://s?q=Liberica_JDK)** | BellSoft | OpenJDK | GPL v2 (free builds), commercial support available | Yes (BellSoft provides Docker images) | Includes JavaFX, IoT focus |
| **[OpenJ9](ca://s?q=Eclipse_OpenJ9)** | Eclipse Foundation (originally IBM) | Alternative JVM | EPL (Eclipse Public License) | Yes (OpenJ9 Docker images available) | Low memory footprint, fast startup |
