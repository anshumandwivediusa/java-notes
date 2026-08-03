# Java Features and Program Execution

Java is a **programming language** and a **platform**. 

**Platform:** Any hardware or software environment in which a program runs, is known as a platform. Since Java has its own runtime environment (JRE) and Application Programming Interface (API), it is called platform.

## 1. Introduction to Java Foundations

Java is a **high-level, object-oriented programming language** known for its **portability, robustness, and scalability**. Programs written in Java run on the **Java Virtual Machine (JVM)**, making them platform-independent — the famous principle of *“Write Once, Run Anywhere.”*

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
  - In Java, we organize the software as a combination of different types of objects that incorporates both data and behavior.
  - Based on the concept of Objects, Class, Inheritance, Polymorphism, Abstraction, Encapsulation
  
- **Platform Independent:**
  - Java provides software-based platform. It has two components:
    - JRE (Runtime Environment)
    - API (Application Programming Interface)
  - Java code is compiled by the compiler and converted into bytecode. This bytecode is a platform-independent. Can run on many platforms - Windows, Linux, Mac, etc.
  
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
  - Java is compiled to bytecodes, which are interpreted by a Java run-time environment.
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

<img width="669" height="588" alt="image" src="https://github.com/user-attachments/assets/fd53a0ea-6917-497b-a517-2f181f49bf0f" />


### 1. **Java Development Kit (JDK)**
- Complete package for developers.  
- Contains **compiler (javac)**, debugger, documentation tools, and the **JRE**.  
- Used for writing, compiling, and testing Java programs.  

### 2. **Java Runtime Environment (JRE)**
- Provides the environment to run Java applications.  
- Includes **JVM** + core libraries.  
- Does not contain development tools like compiler.  
- Ideal for end-users who only need to run Java programs.  

### 3. **Java Virtual Machine (JVM)**
- Heart of Java architecture.  
- Converts **bytecode** into machine code for execution.  
- Provides **platform independence**.  
- Handles **memory management, garbage collection, and security checks**.  

### 4. **Bytecode**
- Intermediate representation of Java code.  
- Generated by the compiler (`javac`).  
- Portable across platforms — runs on any JVM.  

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


