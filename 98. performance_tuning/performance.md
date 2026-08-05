# Java Performance Tuning

# JVM Architecure
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


✅ So, the diagram you uploaded is **JVM architecture** — it explains the internal working of the JVM.  
The **JRE** is a package that *includes* the JVM plus libraries to run Java apps, while the **JDK** adds development tools.  
