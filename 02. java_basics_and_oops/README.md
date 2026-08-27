# Java Fundamentals and Object Oriented Programming

## 1. Object-Oriented Programming (OOP)

**Object-Oriented Programming (OOP)** is a paradigm that structures software around **classes** and **objects**. It simplifies development and maintenance by promoting modularity and reusability.

The four main pillars of OOP in Java are often remembered as **PIEA**:

- **Polymorphism**  
  Ability of a method or object to take many forms.  
  Same class, same method name, but different parameter lists (number or type of arguments).
  Decided at compile/runtime time.
  Example: Different ways to calculate interest depending on inputs.

  <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/d9705592-43e5-4f88-974c-25e4cd251ac3" />

- **Encapsulation**  
  Wrapping data (fields) and behavior (methods) into a single unit (class).  
  Example: Using private variables with public getters/setters.

- **Inheritance**  
  Mechanism to acquire properties and behaviors of another class.  
  Example: `class SavingAccounts extends Accounts`.

- **Abstraction**  
  Hiding implementation details and exposing only essential features.  
  Example: Abstract classes and interfaces.

### Quick Example

```java
// Abstraction: Define a generic financial account
abstract class Account {
    protected double balance; // Encapsulation

    Account(double balance) {
        this.balance = balance;
    }

    abstract void calculateInterest(); // Abstraction
    abstract void accountType();
}

// Inheritance: SavingsAccount inherits from Account
class SavingsAccount extends Account {
    SavingsAccount(double balance) {
        super(balance);
    }

    @Override
    void calculateInterest() { // Polymorphism
        double interest = balance * 0.04; // 4% interest
        System.out.println("Savings Interest: " + interest);
    }

    @Override
    void accountType() {
        System.out.println("This is a Savings Account");
    }
}

// Inheritance: LoanAccount inherits from Account
class LoanAccount extends Account {
    LoanAccount(double balance) {
        super(balance);
    }

    @Override
    void calculateInterest() { // Polymorphism
        double interest = balance * 0.10; // 10% loan interest
        System.out.println("Loan Interest: " + interest);
    }

    @Override
    void accountType() {
        System.out.println("This is a Loan Account");
    }
}

// Main class
public class FinanceDemo {
    public static void main(String[] args) {
        Account acc1 = new SavingsAccount(10000);
        Account acc2 = new LoanAccount(50000);

        acc1.accountType();
        acc1.calculateInterest();

        acc2.accountType();
        acc2.calculateInterest();
    }
}
```

## 2. Class Loading Sequence
```java
// Abstract class: cannot be instantiated directly, but can define constructors
abstract class LibraryItem {
    // Static block: runs ONCE when the class is loaded into JVM
    static {
        System.out.println("Static block: LibraryItem class loaded");
    }

    String title; // Instance variable (state of the object)

    // Constructor: initializes instance state when subclass object is created
    LibraryItem(String title) {
        this.title = title;
        System.out.println("Constructor: LibraryItem initialized");
    }
}

// Concrete subclass extending abstract class
class Book extends LibraryItem {
    // Static block: runs ONCE when Book class is loaded
    static {
        System.out.println("Static block: Book class loaded");
    }

    // Constructor: calls abstract class constructor first (super)
    Book(String title) {
        super(title); // invokes LibraryItem(String) constructor
        System.out.println("Constructor: Book initialized");
    }
}

public class ObjectLifecycleDemo {
    public static void main(String[] args) {
        System.out.println("Program start");

        // Step 1: Object allocation + initialization
        // 'new Book("Effective Java")' → allocates memory on heap, runs constructors
        Book javaBook = new Book("Effective Java");

        // Step 2: Reference reassignment
        // 'refBook' now points to the same Book object as 'javaBook'
        Book refBook = javaBook;

        // Step 3: Nullify one reference
        // 'javaBook' no longer points to the object, but 'refBook' still does
        javaBook = null;

        // Step 4: Nullify all references
        // Now no variable points to the Book object → eligible for GC
        refBook = null;

        System.out.println("End of main");
    }
}
```


### Object Life‑Cycle in Your Example

#### 1. **Class Loading**
- Triggered when `new Book("Effective Java")` is first executed.  
- **Static blocks** run once per class load:
  - `LibraryItem` static block → `"Static block: LibraryItem class loaded"`.  
  - `Book` static block → `"Static block: Book class loaded"`.  
- **Static fields** (if any) would also be initialized here.  
- Managed by the **ClassLoader**, stored in the **method area**.



#### 2. **Object Allocation**
- `new Book("Effective Java")` reserves memory on the **heap**.  
- JVM often uses **TLAB (Thread Local Allocation Buffer)** for efficiency.  
- At this point, memory is reserved but not yet initialized.



#### 3. **Initialization**
- Constructor chain executes:
  - `LibraryItem(String)` constructor runs first → `"Constructor: LibraryItem initialized"`.  
  - Then `Book(String)` constructor runs → `"Constructor: Book initialized"`.  
- Abstract class constructors always execute before subclass constructors.  
- Interfaces cannot have constructors — they only define contracts.



#### 4. **Active Lifetime**
- Object is referenced by `javaBook` and `refBook`.  
- During this stage, JVM may optimize execution with **JIT compilation** and **inline caching**.  
- The object participates in program logic (though here, only constructors and print statements are used).



#### 5. **Reachability Analysis**
- `javaBook = null;` → one reference removed.  
- `refBook` still points to the object, so it remains reachable.  
- `refBook = null;` → now no references exist.  
- JVM marks the object as **unreachable**.  
- Reference types (strong, soft, weak, phantom) influence GC eligibility — here, only strong references are used.



#### 6. **Garbage Collection**
- Object is now eligible for GC.  
- GC algorithms (G1, ZGC, Shenandoah) may reclaim memory.  
- `finalize()` is deprecated; modern practice uses **cleaners** or **try‑with‑resources** for resource management.  
- GC runs **non‑deterministically** — you won’t see output unless explicitly coded.



#### 7. **Deallocation**
- Memory occupied by the `Book` object is returned to the heap pool.  
- Object ceases to exist; both references are `null`.  
- JVM can reuse the freed memory for future allocations.



#### Output Recap
```
Program start
Static block: LibraryItem class loaded
Static block: Book class loaded
Constructor: LibraryItem initialized
Constructor: Book initialized
End of main
```



#### Conceptual Takeaway
Your example demonstrates the **entire lifecycle**:
- **Class loading** → static blocks run once.  
- **Object allocation** → heap memory reserved.  
- **Initialization** → constructor chain executes.  
- **Active lifetime** → object referenced and used.  
- **Reachability analysis** → references removed.  
- **Garbage collection** → memory reclaimed automatically.  
- **Deallocation** → object ceases to exist.  



## 2. Static vs Dynamic Class Loading

Static class loading in Java happens at compile-time when classes are linked directly in code, while dynamic class loading occurs at runtime using reflection or APIs like Class.forName(). Static loading is faster and simpler, but dynamic loading provides flexibility for plugins, JDBC drivers, and frameworks.
**Reference Variable: SavingsAccount sa;**
```java
// Static loading
/* A NoClassDefFoundError is 
thrown if a class is referenced with 
Java’s “new” operator (i.e. static loading) 
but the runtime system cannot find the 
referenced class. Unchecked Error
*/

SavingsAccount acc = new SavingsAccount();
acc.calculateInterest();

// Dynamic loading
/*
A ClassNotFoundException is thrown when an application tries to load in a 
class through its string name using the following methods but no definition for the 
class with the specified name could be found: Checked Exception
*/

Class<?> cls = Class.forName("com.bank.SavingsAccount");
Object obj = cls.getDeclaredConstructor().newInstance();
```

```java
Book refBook = javaBook; // Assign second reference
javaBook = null;         // Remove original reference
// But refBook still points to the Book object, so the object is not eligible for GC yet.
```

## 3. Keywords

| **Keyword** | **Meaning / Usage** |
| --- | --- |
| **[abstract](ca://s?q=Java_abstract_keyword)** | Declares a class or method as abstract (no implementation, must be overridden). |
| **[assert](ca://s?q=Java_assert_keyword)** | Used for debugging; tests assumptions at runtime. |
| **[boolean](ca://s?q=Java_boolean_keyword)** | Declares a variable of type boolean (``true``/``false``). |
| **[break](ca://s?q=Java_break_keyword)** | Exits a loop or switch immediately. |
| **[byte](ca://s?q=Java_byte_keyword)** | Declares an 8‑bit integer variable. |
| **[case](ca://s?q=Java_case_keyword)** | Defines a branch in a ``switch`` statement. |
| **[catch](ca://s?q=Java_catch_keyword)** | Handles exceptions thrown in a ``try`` block. |
| **[char](ca://s?q=Java_char_keyword)** | Declares a 16‑bit Unicode character. |
| **[class](ca://s?q=Java_class_keyword)** | Defines a class. |
| **[const](ca://s?q=Java_const_keyword)** | Reserved but unused. |
| **[continue](ca://s?q=Java_continue_keyword)** | Skips to the next iteration of a loop. |
| **[default](ca://s?q=Java_default_keyword)** | Defines default branch in ``switch``; also default methods in interfaces. |
| **[do](ca://s?q=Java_do_loop)** | Starts a do‑while loop. |
| **[double](ca://s?q=Java_double_keyword)** | Declares a 64‑bit floating‑point variable. |
| **[else](ca://s?q=Java_else_keyword)** | Defines alternative branch in ``if``. |
| **[enum](ca://s?q=Java_enum_keyword)** | Defines a set of named constants. |
| **[extends](ca://s?q=Java_extends_keyword)** | Indicates inheritance from a superclass. |
| **[final](ca://s?q=Java_final_keyword)** | Declares constants, prevents inheritance/overriding. |
| **[finally](ca://s?q=Java_finally_keyword)** | Defines block that always executes after ``try``/``catch``. |
| **[float](ca://s?q=Java_float_keyword)** | Declares a 32‑bit floating‑point variable. |
| **[for](ca://s?q=Java_for_loop)** | Loop construct. |
| **[goto](ca://s?q=Java_goto_keyword)** | Reserved but unused. |
| **[if](ca://s?q=Java_if_keyword)** | Conditional branching. |
| **[implements](ca://s?q=Java_implements_keyword)** | Declares that a class implements an interface. |
| **[import](ca://s?q=Java_import_keyword)** | Brings external classes/packages into scope. |
| **[instanceof](ca://s?q=Java_instanceof_keyword)** | Tests if an object is an instance of a class. |
| **[int](ca://s?q=Java_int_keyword)** | Declares a 32‑bit integer variable. |
| **[interface](ca://s?q=Java_interface_keyword)** | Defines a contract of methods. |
| **[long](ca://s?q=Java_long_keyword)** | Declares a 64‑bit integer variable. |
| **[native](ca://s?q=Java_native_keyword)** | Declares a method implemented in native code (JNI). |
| **[new](ca://s?q=Java_new_keyword)** | Creates new object instances. |
| **[package](ca://s?q=Java_package_keyword)** | Defines a namespace for classes. |
| **[private](ca://s?q=Java_private_modifier)** | Access modifier → visible only within class. |
| **[protected](ca://s?q=Java_protected_modifier)** | Access modifier → visible in package + subclasses. |
| **[public](ca://s?q=Java_public_modifier)** | Access modifier → visible everywhere. |
| **[return](ca://s?q=Java_return_keyword)** | Exits a method and optionally returns a value. |
| **[short](ca://s?q=Java_short_keyword)** | Declares a 16‑bit integer variable. |
| **[static](ca://s?q=Java_static_keyword)** | Declares members tied to the class, not instances. |
| **[strictfp](ca://s?q=Java_strictfp_keyword)** | Ensures floating‑point calculations are consistent across platforms. |
| **[super](ca://s?q=Java_super_keyword)** | Refers to superclass methods/constructors. |
| **[switch](ca://s?q=Java_switch_keyword)** | Multi‑branch conditional logic. |
| **[synchronized](ca://s?q=Java_synchronized_keyword)** | Ensures thread‑safe access to methods/blocks. |
| **[this](ca://s?q=Java_this_keyword)** | Refers to the current object instance. |
| **[throw](ca://s?q=Java_throw_keyword)** | Used to throw an exception. |
| **[throws](ca://s?q=Java_throws_keyword)** | Declares exceptions a method may throw. |
| **[transient](ca://s?q=Java_transient_keyword)** | Prevents a field from being serialized. |
| **[try](ca://s?q=Java_try_keyword)** | Defines a block to test for exceptions. |
| **[void](ca://s?q=Java_void_keyword)** | Declares a method with no return value. |
| **[volatile](ca://s?q=Java_volatile_keyword)** | Ensures a variable is always read/written from main memory (thread visibility). |
| **[while](ca://s?q=Java_while_loop)** | Loop construct. |

## 4. Types of Comments
- **Single‑line (`//`)** → Quick notes, ends at line break.  
- **Multi‑line (`/* ... */`)** → Block explanations or disable code.  
- **Documentation (`/** ... */`)** → Javadoc comments, generate API docs.

### Key Exam Points

- **Reserved Syntax** → Comments are ignored by the compiler, not executed.  
- **Javadoc Tags** → `@param`, `@return`, `@throws`, `@see` are exam favorites.  
- **Best Practice** → Use comments for clarity, not redundancy.  
- **Multi‑line vs Javadoc** → Both use `/* ... */`, but Javadoc starts with `/**`.  
- **Debugging Aid** → Multi‑line comments can temporarily disable code.  
- **Documentation** → Javadoc comments are essential for professional API documentation.  
- **Performance** → Comments have **no runtime cost** — purely for readability.  
- **Maintainability** → Good comments explain *why*, not just *what*.  

```java
// Single-line: explains one statement
int x = 10;

/* Multi-line:
   Explains a block of logic
   or disables code temporarily
*/

/**
 * Javadoc: describes method for API docs
 * @param a first number
 * @param b second number
 * @return sum of a and b
 */
public int add(int a, int b) {
    return a + b;
}
```


## 5. Package and Import

### Wild Import
- **Syntax** → `import java.util.*;`  
- **Meaning** → Brings in *all classes* from the `java.util` package (e.g., `List`, `ArrayList`, `HashMap`).  
- **Performance Impact** →  
  - **No runtime penalty** — imports are resolved at **compile time**, not at runtime.  
  - The compiler only loads the classes you actually use, not the entire package.  
  - So `import java.util.*;` does **not slow down performance**, but it can reduce clarity.  
- **Exam Point** → Wildcard imports are allowed, but **explicit imports are preferred** for readability and avoiding conflicts.



### Multiple Imports
- You **cannot import multiple classes in one line** separated by commas.  
- Each class must be imported separately:  
  ```java
  import java.util.List;
  import java.util.ArrayList;
  ```
- Or use wildcard:  
  ```java
  import java.util.*;
  ```
- **Exam Point** → Each `import` statement is independent; no comma‑separated imports.



### Static Import
- **Syntax** → `import static java.lang.Math.*;`  
- **Meaning** → Brings static members (methods, constants) directly into scope.  
- Example:  
  ```java
  import static java.lang.Math.*;  
  System.out.println(sqrt(16)); // no need for Math.sqrt
  ```
- **Use Cases** → Cleaner code when using many constants/methods (e.g., `PI`, `sqrt`, `max`).  
- **Exam Point** → Use sparingly; overuse can reduce readability because it hides the class origin.



### Exam‑Friendly Table

| **Feature** | **Syntax** | **Performance / Rules** | **Exam Notes** |
|-------------|------------|--------------------------|----------------|
| **Wildcard Import** | `import java.util.*;` | No runtime cost; compiler loads only used classes | Allowed, but explicit imports preferred |
| **Multiple Imports** | `import java.util.List;` <br> `import java.util.ArrayList;` | Must be separate lines; no commas | Each class imported individually |
| **Static Import** | `import static java.lang.Math.*;` | Brings static members directly into scope | Useful for constants/methods; readability trade‑off |



### Key Takeaways
- `import java.util.*;` → **No performance issue**, but readability suffers.  
- Multiple imports → **One per line**, no comma separation.  
- Static import → Brings static members into scope; use carefully.  
- Imports are **compile‑time only** — they don’t affect runtime performance.  


## 3. Data Types in Java

| **Data Type** | **Size (bits)** | **Initial Value** | **Min Value** | **Max Value** | **[Wrapper Class](ca://s?q=Java_Wrapper_Classes) Name |
| --- | --- | --- | --- | --- | --- |
| [boolean](ca://s?q=Java_boolean_data_type) | 1 | ``false`` | ``false`` | ``true`` | **Boolean** |
| [byte](ca://s?q=Java_byte_data_type) | 8 | ``0`` | -128 (-2⁷) | 127 (2⁷ – 1) | **Byte** |
| [short](ca://s?q=Java_short_data_type) (Rarely Used) | 16 | ``0`` | -32,768 (-2¹⁵) | 32,767 (2¹⁵ – 1) | **Short** |
| [char](ca://s?q=Java_char_data_type) (Only unsigned datatype) | 16 | ``'\\u0000'`` | ``'\\u0000'`` (0) | ``'\\uFFFF'`` (65,535) | **Character** |
| [int](ca://s?q=Java_int_data_type) | 32 | ``0`` | -2,147,483,648 (-2³¹) | 2,147,483,647 (2³¹ – 1) | **Integer** |
| [long](ca://s?q=Java_long_data_type) | 64 | ``0L`` | -9,223,372,036,854,775,808 (-2⁶³) | 9,223,372,036,854,775,807 (2⁶³ – 1) | **Long** |
| [float](ca://s?q=Java_float_data_type) (Single-Precision) | 32 | ``0.0F`` | 1.4E-45 | 3.4028235E38 | **Float** |
| [double](ca://s?q=Java_double_data_type) (Double-precision) | 64 | ``0.0`` | 4.9E-324 | 1.7976931348623157E308 | **Double** |

### Data Type Guidelines and Promotion in Java:

![data type promotion small](https://user-images.githubusercontent.com/2780145/34364362-403e9db4-eaab-11e7-914b-7acc9007cf41.png)

| Concept | Rule | Example |
| --- | --- | --- |
| [Numeric Types](ca://s?q=Java_numeric_data_types) | All signed except ``char`` | ``byte``, ``int``, ``long`` |
| [Object References](ca://s?q=Java_object_reference_variables) | Default = ``null`` | ``String ``s;`` |
| [Octal Literals](ca://s?q=Java_number_literals) | Begin with ``0`` | ``012`` → 10 |
| [Hex Literals](ca://s?q=Java_number_literals) | Begin with ``0x`` or ``0X`` | ``0x1A`` → 26 |
| [Char Literals](ca://s?q=Java_char_literals) | Single quotes or Unicode | ``'\\u0041'`` → 'A' |
| [Default Literals](ca://s?q=Java_number_literals_defaults) | int / double | ``10``, ``10.5`` |
| [Scientific Notation](ca://s?q=Java_scientific_notation_literals) | ``1E-5d`` valid | ``E2d`` invalid |
| [Unicode Anywhere](ca://s?q=Java_unicode_in_source_code) | Allowed in identifiers | ``ch\\u0061r`` → ``char`` |

## 4. Java Naming Conventions:

| Concept | Definition | Examples |
| --- | --- | --- |
| [Identifiers](ca://s?q=Java_identifiers) | Names for variables, methods, classes | ``age``, ``firstName``, ``Demo`` |
| [Literals](ca://s?q=Java_literals) | Fixed constant values | ``10``, ``3.14``, ``'A'``, ``"Hello"``, ``true`` |
| [Tokens](ca://s?q=Java_tokens) | Smallest unit of code | Keywords, identifiers, literals, operators, separators |

**Naming Rules**
  - Start Character → Must begin with a letter (A–Z, a–z), underscore _, or dollar sign $.
  - Subsequent Characters → Can include letters, digits (0–9), underscores, or dollar signs.
  - No Keywords → Cannot use reserved words like class, int, static, etc.
  - Case Sensitivity → Java is case-sensitive, so Name and name are different identifiers.
  - Length → No fixed limit, but should be meaningful and readable.
  - Convention → Follow camelCase for variables/methods, PascalCase for classes, UPPERCASE for constants.
    | **Feature** | **Allowed?** | **Example** |
    | --- | --- | --- |
    | **ASCII letters** | ✅ | ``int ``age;`` |
    | **Unicode letters** | ✅ | ``int ``café;`` |
    | **Unicode escapes** | ✅ | ``int ``\\u0061bc ``= ``10; ``// ``abc`` |
    | **Digits (not first char)** | ✅ | ``int ``value1;`` |
    | **Keywords (even with Unicode)** | ❌ | ``int ``\\u0069f; ``// ``"if"`` |
    | **Special chars (like @, #)** | ❌ | Not allowed |

| **Element** | **Convention** | **Examples** |
| --- | --- | --- |
| [Class Name](ca://s?q=Java_class_naming_convention) | Start with **uppercase letter**, should be a **noun** | ``String``, ``Color``, ``Button``, ``System``, ``Thread`` |
| [Interface Name](ca://s?q=Java_interface_naming_convention) | Start with **uppercase letter**, should be an **adjective** | ``Runnable``, ``Remote``, ``ActionListener`` |
| [Method Name](ca://s?q=Java_method_naming_convention) | Start with **lowercase letter**, should be a **verb** | ``actionPerformed()``, ``main()``, ``print()``, ``println()`` |
| [Variable Name](ca://s?q=Java_variable_naming_convention) | Start with **lowercase letter**, use **camelCase** | ``firstName``, ``orderNumber`` |
| [Package Name](ca://s?q=Java_package_naming_convention) | Always **lowercase letters** | ``java``, ``lang``, ``sql``, ``util`` |
| [Constant Name](ca://s?q=Java_constant_naming_convention) | Use **uppercase letters**, words separated by ``_`` | ``RED``, ``YELLOW``, ``MAX_PRIORITY`` |

## 5. Notes on Java Source File Elements

1. **Source File Structure**  
   - Fixed Order:  
     a. Package declaration  
     b. Import statements  
     c. Class/interface definitions  

2. **Importing Packages**  
   - Importing a package does **not** recursively import its sub‑packages.  

3. **Sub‑Packages**  
   - Sub‑packages are independent packages.  
   - Classes in sub‑packages cannot access enclosing package classes with default access.  

4. **Comments**  
   - Comments
      - Single-line → // comment
      - Multi-line → /* comment block */
      - Javadoc → /** documentation */
   - Can appear anywhere in source code.  
   - Cannot be nested (applies to all comment types).  

6. **Public Class Rule**  
   - At most **one public class** per file.  
   - Its name must match the file name.  
   - If multiple public classes exist, compiler accepts the one matching the file name and throws error for others.  

7. **Optional Public Class**  
   - A file may contain **no public class**.  
   - In that case, file name must differ from all class/interface names inside.  

    | **Scenario** | **File Name Rule** | **Explanation** |
    | --- | --- | --- |
    | **One public class/interface** | Must match that name | ``public ``class ``Book`` → ``Book.java`` |
    | **Multiple classes, one public** | Must match the public one | ``public ``class ``Library``, ``class ``Book`` → ``Library.java`` |
    | **Multiple classes, none public** | Can differ from all names | ``class ``Teacher``, ``class ``Course`` → file may be ``School.java`` |
    | **Empty file** | Still valid | File name arbitrary |

8. **Empty File Validity**  
   - Even an empty file is considered a valid Java source file.  

## 6. Two types of variables.

**1. Member Variables**
  - Accessible anywhere in the class.
  - Automatically initialized before invoking any constructor.
  - Static variables are initialized at class load time.
  - Can have the same name as the class.

**2. Automatic Variables(Method Local)**
  - Must be initialized explicitly. (Or, compiler will catch it.) Object references can be initialized to null to make the compiler happy. The following code won’t compile. Specify else part or initialize the local variable explicitly.

```java
[accessModifier] [specifiers] type fieldName [= initialValue];
```

The order is mandatory overall, but access modifiers and specifiers can be rearranged among themselves. The type must always come before the variable name.

## 7. Main Method
| Keyword | Purpose | Question | Answer |
| --- | --- | --- | --- |
| [public](ca://s?q=Why_is_main_method_public_in_Java) | Accessible to JVM | Why not private? | Because JVM must call ``main`` from outside the class. If it were ``private``, JVM couldn’t access it. |
| [static](ca://s?q=Why_is_main_method_static_in_Java) | No object needed | Why static? | JVM doesn’t create an object of the class before starting execution. Declaring ``main`` as ``static`` allows it to be invoked directly. |
| [void](ca://s?q=Why_is_main_method_void_in_Java) | No return value | Why void? | ``main`` doesn’t return anything to JVM. Its job is to start program execution, not provide a result. |
| [main](ca://s?q=Java_main_method_signature) | Entry point | Why fixed signature? | JVM looks specifically for ``public ``static ``void ``main(String[] ``args)`` as the entry point. Changing the signature means JVM won’t recognize it. |
| [String[] args](ca://s?q=Java_main_method_arguments) | Command-line input | How are args passed? | Arguments typed after the class name in the command line are passed as strings. Example: ``java ``Demo ``Hello ``World`` → ``args[0] ``= ``"Hello"``, ``args[1] ``= ``"World"``. |

### Observations
  - The signature is strict because JVM needs a predictable entry point.
  - You can overload main, but JVM will only call the exact public static void main(String[] args) version.
  - Command-line arguments are always strings, even if you type numbers (they must be parsed).
  - In order to be run by JVM, a class should have a main method with the following signature.
    ```java
    public static void main(String args[])
    static public void main(String[] s)
    ```
  - args array’s name is not important. args[0] is the first argument. args.length gives no. of arguments.
  - main method can be final.
  - A class with a different main signature or w/o main method will compile. But throws a runtime error.
  - A class without a main method can be run by JVM, if its ancestor class has a main method. (main is just a method and is inherited)

## 8. Array

  - Java arrays are static arrays.
  - Size has to be specified at compile time. Array.length returns array’s size.
  - Use ArrayList for Dynamic Purpose.
  - Array are full-fledged objects internally, but they have a unique JVM implementation: each array object has a header (metadata + length field) and a contiguous memory block for elements. Unlike normal objects, arrays are created directly by JVM bytecode and don’t have constructors.
    
| Rule | Explanation | Valid Example |
| --- | --- | --- |
| [Arrays are objects](ca://s?q=Java_arrays_are_objects) | Arrays are objects; creating ``String[5]`` makes 1 array + 5 element references (total 6 objects). | ``String[] ``arr ``= ``new ``String[5]; ``arr[0] ``= ``"Hello";`` |
| [Declaration](ca://s?q=Java_arrays_declaration_allocation_initialization) | Declare array reference without size. | ``int[] ``a; ``String ``b[]; ``Object[] ``c;`` |
| [Allocation](ca://s?q=Java_arrays_declaration_allocation_initialization) | Construct array with ``new``. | ``a ``= ``new ``int[10]; ``c ``= ``new ``String[5];`` |
| [Initialization](ca://s?q=Java_arrays_declaration_allocation_initialization) | Assign values to elements. | ``for ``(int ``i ``= ``0; ``i ``< ``a.length; ``i++) ``a[i] ``= ``0;`` |
| [One‑step init](ca://s?q=Java_array_initialization) | Declare, allocate, and initialize together. | ``int ``a[] ``= ``{1, ``2, ``3}; ``int ``b[] ``= ``new ``int[]{4, ``5, ``6};`` |
| [Static size](ca://s?q=Java_static_arrays) | Arrays are fixed size; length stored in object. | ``int[] ``nums ``= ``new ``int[3]; ``System.out.println(nums.length); ``// ``3`` |
| [Array size storage](ca://s?q=Java_array_length_property) | Size maintained in ``array.length``. | ``int[] ``arr ``= ``new ``int[5]; ``System.out.println(arr.length); ``// ``5`` |
| [Anonymous arrays](ca://s?q=Java_anonymous_arrays) | Created without reference, often passed directly. | ``printArray(new ``int[]{1, ``2, ``3});`` |
| [Zero‑element arrays](ca://s?q=Java_zero_element_arrays) | Arrays with 0 elements are valid. | ``String[] ``args ``= ``new ``String[0]; ``System.out.println(args.length); ``// ``0`` |
| [Initializer rules](ca://s?q=Java_array_initializer_rules) | Trailing comma allowed; certain forms invalid. | ✅ ``int ``i[] ``= ``{1, ``2, ``3, ``4,};`` <br> ✅ ``int ``i[][] ``= ``{{1,2}, ``new ``int[2]};`` <br> ❌ ``int[] ``i ``= ``new ``int[2]{5,10};`` |


```java
int[] i = new int[2] { 5, 10}; // Wrong, 
int i[5] = { 1, 2, 3, 4, 5}; // Wrong.. Correct int[] i = {1, 2, 3, 4, 5};
int[] i[] = {{}, new int[] {} }; // Correct
int i[][] = { {1,2}, new int[2] }; // Correct
int i[] = { 1, 2, 3, 4, } ; // Correct
static int a[];
static int b[] = {1,2,3};
public static void main(String s[]) {
System.out.println(a[0]); // Throws a null pointer exception
System.out.println(b[0]); // This code runs fine
System.out.println(a); // Prints ‘null’
System.out.println(b); // Prints a string which is returned by toString
}
```

## 9. Object vs Class

| **[Object](ca://s?q=Java_object_definition)** | **[Class](ca://s?q=Java_class_definition)** |
| --- | --- |
| Object is an **instance** of a class. | Class is a **blueprint/template** from which objects are created. |
| Object is a **real‑world entity** (pen, laptop, mobile, chair). | Class is a **group of similar objects**. |
| Object is a **physical entity**. | Class is a **logical entity**. |
| Object is created using ``new``** keyword**. <br> Example: ``Student ``s1 ``= ``new ``Student();`` | Class is declared using ``class``** keyword**. <br> Example: ``class ``Student ``{ ``}`` |
| Object can be created **many times** as needed. | Class is declared **once**. |
| Object **allocates memory** when created. | Class **does not allocate memory** when declared. |
| Object can be created in **many ways**: ``new``, ``newInstance()``, ``clone()``, factory method, deserialization. | Class can be defined only **one way**: using ``class`` keyword. |

### Constructors vs Methods

| **[Java Constructor](ca://s?q=Java_constructor)** | **[Java Method](ca://s?q=Java_method)** |
| --- | --- |
| Constructor is used to **initialize the state of an object**. | Method is used to **expose behaviour of an object**. |
| Constructor must **not have a return type**. | Method must have a **return type** (void or any data type). |
| Constructor is **invoked implicitly** when an object is created. | Method is **invoked explicitly** by calling it. |
| Compiler provides a **default constructor** if none is defined. | Compiler does **not provide any method** automatically. |
| Constructor name must be the **same as the class name**. | Method name may or may not be the same as the class name. |

```java
[accessModifier] [specifiers] returnType methodName([parameters]) [throws ExceptionType1, ExceptionType2, ...] {
    // method body
}
```

### finalize() – Key Points
  - **Definition** → Inherited from Object, signature:
    protected void finalize() throws Throwable { }.
  - **Purpose** → Intended for releasing non‑memory resources (files, sockets), not memory cleanup.
  - **Modern Status** → Deprecated since Java 9, removed in Java 18+.
  - Use try‑with‑resources or implement AutoCloseable instead.

### Try‑with‑Resources & AutoCloseable – Key Notes

1. **Deprecation of finalize**  
   - `finalize()` is deprecated (Java 9) and removed in Java 18+.  
   - It was unreliable, non‑deterministic, and could cause performance issues.

2. **Try‑with‑Resources**  
   - Introduced in Java 7.  
   - Ensures resources (files, sockets, DB connections) are closed automatically.  
   - Syntax:  
     ```java
     try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
         System.out.println(br.readLine());
     } // br is auto‑closed here
     ```

3. **AutoCloseable Interface**  
   - Any class implementing `AutoCloseable` can be used in try‑with‑resources.  
   - Must override `close()` method for cleanup.  
   - Example:  
     ```java
      import java.util.Arrays;

      class MyResource implements AutoCloseable {
          public void close() {
              System.out.println("Resource closed");
          }
      }
      
      class Main {
          public static void main(String[] args) {
              try (MyResource myResource = new MyResource()) {
                  Integer[] argsString = {1, 2, 3};
                  System.out.println(Arrays.toString(argsString));
              }
              // At the end of try block, close() is automatically called
          }
      }
     ```

     | Concept | **[Closeable](ca://s?q=Java_Closeable_interface)** | **[AutoCloseable](ca://s?q=Java_AutoCloseable_interface)** |
     | --- | --- | --- |
     | **Design Goal** | Specialized for I/O | General resource management |
     | **Exception Type** | Only ``IOException`` | Any ``Exception`` |
     | **Scope** | Streams, readers, writers | DB connections, sockets, custom classes |
     | **Inheritance** | Extends AutoCloseable | Root interface |
     | **Usage** | File I/O cleanup | Universal cleanup in try‑with‑resources |
     | **Example Code** | ``try ``(BufferedReader ``br ``= ``new ``BufferedReader(new ``FileReader("data.txt"))) ``{ ``System.out.println(br.readLine()); ``}`` | ``class ``MyResource ``implements ``AutoCloseable ``{ ``public ``void ``close() ``{ ``System.out.println("Closed!"); ``} ``} ``try ``(MyResource ``r ``= ``new ``MyResource()) ``{ ``System.out.println("Using ``resource"); ``}`` |
     | **Built‑in Examples** | ``FileInputStream``, ``BufferedReader``, ``OutputStream`` | ``Connection``, ``Statement``, ``ResultSet``, ``ZipInputStream`` |

4. **Advantages**  
   - Deterministic cleanup (resources closed immediately).  
   - No dependency on GC timing.  
   - Safer, cleaner, and recommended over `finalize()`.


## 10 Types of Inheritance

### Types of Inheritance (Supported through Class)

![single inheritance](https://user-images.githubusercontent.com/2780145/34364364-40b6b646-eaab-11e7-8c92-2c4cd9d0b2ca.png)

### Types of Inheritance (Supported through Interface only)

![multiple inheritance](https://user-images.githubusercontent.com/2780145/34364363-407486b8-eaab-11e7-94e2-5c1876f414d3.png)

## 11. Association vs Aggregation vs Composition

![association-aggregation-composition](https://user-images.githubusercontent.com/2780145/34364371-5db00694-eaab-11e7-8ef2-bf56d3394f15.png)

IS-A vs HAS-A vs PART RELATIONSHIP

### Aggregation vs Composition

| **[Aggregation](ca://s?q=Java_aggregation)** | **[Composition](ca://s?q=Java_composition)** |
| --- | --- |
| Aggregation is a **weak association**. | Composition is a **strong association**. |
| Class can exist **independently** without owner. | Class cannot meaningfully exist **without owner**. |
| Has its **own lifetime**. | Lifetime depends on the **owner**. |
| **A uses B.** | **A owns B.** |
| Child is **not owned by one owner**. | Child can have **only one owner**. |
| **Has‑A relationship** → A has B. | **Part‑Of relationship** → B is part of A. |
| Denoted by an **empty diamond** in UML. | Denoted by a **filled diamond** in UML. |
| We do not use ``final``** keyword** for Aggregation. | ``final``** keyword** is used to represent Composition. |
| **Examples:** <br> – Car has a Driver <br> – Human uses Clothes <br> – Company is an aggregation of People <br> – Text Editor uses a File <br> – Mobile has a SIM Card | **Examples:** <br> – Engine is part of Car <br> – Human owns the Heart <br> – Company is a composition of Accounts <br> – Text Editor owns a Buffer <br> – IMEI Number is part of Mobile |

NOTE : "final" keyword is used in Composition to make sure child variable is initialized.

```java
---ASSOCIATION EXAMPLE---
class Driver {
    String name;
    Driver(String name) {
        this.name = name;
    }
}

class Car {
    String model;
    Driver driver; // Aggregation: Car uses Driver

    Car(String model, Driver driver) {
        this.model = model;
        this.driver = driver;
    }

    void showDetails() {
        System.out.println("Car: " + model + ", Driver: " + driver.name);
    }
}

public class AggregationDemo {
    public static void main(String[] args) {
        Driver d = new Driver("John"); // Driver exists independently
        Car c = new Car("Tesla", d);   // Car uses Driver
        c.showDetails();
    }
}
```
```java
---COMPOSITION EXAMPLE---

class Engine {
    String type;
    Engine(String type) {
        this.type = type;
    }
}

class Car {
    String model;
    Engine engine; // Composition: Car owns Engine

    Car(String model) {
        this.model = model;
        this.engine = new Engine("V8"); // Engine created inside Car
    }

    void showDetails() {
        System.out.println("Car: " + model + ", Engine: " + engine.type);
    }
}

public class CompositionDemo {
    public static void main(String[] args) {
        Car c = new Car("Mustang"); // Engine cannot exist without Car
        c.showDetails();
    }
}
```

## 12. Polymorphism - Method Overloading vs Method Overriding

| **[Method Overloading](ca://s?q=Java_method_overloading)** | **[Method Overriding](ca://s?q=Java_method_overriding)** |
| --- | --- |
| Used to **increase readability** of the program. | Used to **provide specific implementation** of a method already defined in the superclass. |
| Performed **within the same class**. | Occurs **between two classes** with an IS‑A (inheritance) relationship. |
| Parameters must be **different**. | Parameters must be **same**. |
| Example of **compile‑time polymorphism**. | Example of **run‑time polymorphism**. |
| **Cannot overload by changing only the return type**. <br> Return type may be same/different, but parameters must change. | Return type must be **same or covariant** (subclass type allowed). In case Parameters is different, treated as method overriding. |





Method "Hiding"?
 - Static methods are bound at compile time.
 - The compiler decides which method to call based on the reference type or class name, not the actual object type.
 - That means the parent’s static method is still there — it isn’t replaced. The child’s static method just shadows (hides) it when you use the child’s class name.

```java
// ===============================
// Method Overloading
// Polymorphism Type: Compile-time (static binding)
// Decision Basis: Compiler chooses method based on arguments
class Calculator {
    // Overloaded methods: same name, different parameter lists
    int add(int a, int b) { 
        return a + b; 
    }

    double add(double a, double b) { 
        return a + b; 
    }
}

// ===============================
// Method Overriding
// Polymorphism Type: Runtime (dynamic binding)
// Decision Basis: JVM chooses method based on actual object type
class Animal {
    void sound() { 
        System.out.println("Animal sound"); 
    }
}

class Dog extends Animal {
    @Override
    void sound() { 
        System.out.println("Dog barks"); 
    }
}

// Usage:
// Animal a = new Dog();
// a.sound(); // Output: Dog barks (runtime dispatch)

// ===============================
// Method Hiding
// Polymorphism Type: Compile-time (static binding)
// Decision Basis: Reference type or class name
class Animal2 {
    static void info() { 
        System.out.println("Animal info"); 
    }
}

class Dog2 extends Animal2 {
    static void info() { 
        System.out.println("Dog info"); 
    }
}

// Usage:
Animal2 a2 = new Dog2();
a2.info();   // Output: Animal info (reference type = Animal2)
Dog2.info(); // Output: Dog info (class name = Dog2)
//Hides Animal.info() when accessed via Dog class
```

## 13. Abstract Class vs Interface

| **[Abstract Class](ca://s?q=Java_abstract_class)** | **[Interface](ca://s?q=Java_interface)** |
| --- | --- |
| Can have **abstract and non‑abstract methods**. | Can have **only abstract methods**. Since Java 8, can also have **default & static methods**. |
| **Does not support multiple inheritance**. | **Supports multiple inheritance**. |
| Can have **final, non‑final, static, and non‑static variables**. | Has **only static (no object creation needed) and final (immutable) variables**. |
| Can provide the **implementation of an interface**. | Cannot provide the **implementation of an abstract class**. |
| Declared using the ``abstract``** keyword**. | Declared using the ``interface``** keyword**. |
| Example: <br> ``java ``public ``abstract ``class ``Shape ``{ ``public ``abstract ``void ``draw(); ``}`` | Example: <br> ``java ``public ``interface ``Drawable ``{ ``void ``draw(); ``}`` |
| Used when classes share **common behavior** but also need **partial implementation**. | Used to define a **contract** that multiple classes can implement. |
| Can have **constructors**. | Cannot have **constructors**. |
| Can contain **instance methods with implementation**. | Cannot contain **instance methods with implementation** (except default methods since Java 8). |
| Suitable for **code reusability** with partial abstraction. | Suitable for **full abstraction** and defining APIs. |

| Concept | **[Anonymous Class](ca://s?q=Java_anonymous_class)** | **[Lambda Expression](ca://s?q=Java_Lambda_expressions)** |
| --- | --- | --- |
| **Definition** | Inline class without a name | Inline function implementing a functional interface |
| **Introduced** | Java 1.1 | Java 8 |
| **Use Case** | Multiple methods, complex logic | Single abstract method (SAM) |
| **Syntax** | Verbose | Concise |
| **Example** | ``new ``Runnable() ``{ ``public ``void ``run(){...} ``}`` | ``() ``-> ``{...}`` |

| **[Member](ca://s?q=Java_interface_members)** | **[Default Modifier](ca://s?q=Java_interface_default_modifiers)** |
| --- | --- |
| Methods | ``public ``abstract`` |
| Variables | ``public ``static ``final`` |
| Nested Types | ``public ``static`` |

### Diamond Problem
| **Concept** | **Explanation** |
| --- | --- |
| [Diamond Problem](ca://s?q=Java_diamond_problem) | Ambiguity when multiple parents provide same method. |
| [Default Methods](ca://s?q=Java_default_methods_in_interfaces) | Interfaces can have methods with implementation (Java 8+). |
| [Conflict](ca://s?q=Java_default_method_conflict) | Occurs if two interfaces define same default method. |
| [Resolution](ca://s?q=Java_default_method_resolution) | Child must override and explicitly choose implementation. |

```java
interface A {
    default void show() {
        System.out.println("A's default show");
    }
}

interface B {
    default void show() {
        System.out.println("B's default show");
    }
}

class C implements A, B {
    // Must override to resolve ambiguity
    @Override
    public void show() {
        // Choose explicitly
        A.super.show(); // or B.super.show()
        System.out.println("C's own show");
    }
}

public class DiamondProblemDemo {
    public static void main(String[] args) {
        C obj = new C();
        obj.show();
    }
}
```
### Marker Interface
In Java, **marker interfaces** are special interfaces that contain no methods or fields and serve purely as a tag to signal the compiler or JVM that a class has a particular property. They don’t add behavior but instead act as metadata, telling the runtime environment how to treat objects of the implementing class differently. For example, when a class implements the **Serializable** interface, it indicates that its objects can be converted into a byte stream for storage or transmission. Similarly, the **Cloneable** interface marks that a class’s objects can be cloned using the `clone()` method, and the **Remote** interface signals that objects can be used in remote method invocation. The JVM or frameworks check for these markers at runtime and allow or restrict certain operations accordingly.  

Although marker interfaces were widely used in earlier versions of Java, modern Java often prefers **annotations** as a replacement because they are more flexible, descriptive, and can carry additional metadata. For instance, `@FunctionalInterface` is an annotation that serves a similar purpose to a marker interface but provides compile‑time checking and clearer semantics.  

In short, marker interfaces like `Serializable` and `Cloneable` are **tags with no methods** that influence how the JVM treats objects, but in newer Java versions, annotations are the recommended approach for such tagging.


### Key Changes over Time
| **Version** | **New Interface Features** | **Example** |
| --- | --- | --- |
| **[Java 8](ca://s?q=Java_8_interface_changes)** | Default methods, Static methods, Functional interfaces |``` public interface Logger { void log(String msg); default void logInfo(String msg){ log("[INFO] " + msg);} }``` |
| **[Java 9](ca://s?q=Java_9_interface_changes)** | Private methods (helper methods inside interfaces) | ```public interface Helper { default void show(){ greet(); } private void greet(){ System.out.println("Hello"); } }``` |
| **[Java 15/16](ca://s?q=Java_16_sealed_interfaces)** | Sealed interfaces (restrict who can implement) | ```sealed interface Shape permits Circle, Square {} final class Circle implements Shape {} final class Square implements Shape {}``` |
| **[Java 21](ca://s?q=Java_21_interface_changes)** | Pattern matching with sealed interfaces, better integration with switch | ```sealed interface Shape permits Circle, Square {} static String describe(Shape s){ return switch(s){ case Circle c -> "Circle"; case Square sq -> "Square"; }; }``` |

## 14. Java Access Modifiers

| **Access Modifier** | **Within Class** | **Within Package** | **Subclass (outside package)** | **Outside Package** |
| --- | --- | --- | --- | --- |
| **[Private](ca://s?q=Private_access_modifier_in_Java)** | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **[Default](ca://s?q=Default_access_modifier_in_Java)** (no keyword) | ✅ Yes | ✅ Yes | ❌ No | ❌ No |
| **[Protected](ca://s?q=Protected_access_modifier_in_Java)** | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |
| **[Public](ca://s?q=Public_access_modifier_in_Java)** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |

## 15. Abstraction vs Encapsulation

| **Aspect** | **[Abstraction](ca://s?q=Abstraction_in_Java)** | **[Encapsulation](ca://s?q=Encapsulation_in_Java)** |
| --- | --- | --- |
| **Definition** | Hiding implementation details, showing only functionality | Wrapping data and methods together in one unit |
| **Focus** | *What* an object does | *How* data is accessed and protected |
| **Level** | Design level | Implementation level |
| **Implementation** | Interfaces, Abstract classes | Access modifiers (``private``, ``protected``, ``public``) |
| **Purpose** | Reduce complexity, expose essential behavior | Control access, ensure data safety |
| **Example** | Abstract class ``Shape`` with ``draw()`` method | Class ``Student`` with private fields and getters/setters |



## 16. Methods of Object Class
The Object class is the parent class of all the classes in java by default.

| **Method** | **Description (Latest Java)** |
| --- | --- |
| **[public final Class getClass()](ca://s?q=getClass_method_in_Java)** | Returns the runtime ``Class`` object of this instance. Still widely used for reflection and metadata. |
| **[public int hashCode()](ca://s?q=hashCode_method_in_Java)** | Returns a hash code value for the object. Must be consistent with ``equals()``. |
| **[public boolean equals(Object obj)](ca://s?q=equals_method_in_Java)** | Compares this object with another for equality. Default is reference equality; can be overridden. |
| **[protected Object clone()](ca://s?q=clone_method_in_Java)** | Creates and returns a shallow copy of the object. Requires implementing ``Cloneable``. |
| **[public String toString()](ca://s?q=toString_method_in_Java)** | Returns a string representation of the object. Default: ``ClassName@hashcode``. Often overridden. |
| **[public final void notify()](ca://s?q=notify_method_in_Java)** | Wakes up a single thread waiting on this object’s monitor. Used in synchronization. |
| **[public final void notifyAll()](ca://s?q=notifyAll_method_in_Java)** | Wakes up all threads waiting on this object’s monitor. |
| **[public final void wait(long timeout)](ca://s?q=wait_method_in_Java)** | Causes the current thread to wait until notified or timeout expires. |
| **[public final void wait(long timeout, int nanos)](ca://s?q=wait_method_in_Java)** | Same as above, with nanosecond precision. |
| **[public final void wait()](ca://s?q=wait_method_in_Java)** | Causes the current thread to wait indefinitely until notified. |
| **[protected void finalize()](ca://s?q=finalize_method_in_Java)** | **Deprecated since Java 9, removed in Java 18.** It was invoked by GC before object collection, but now replaced by ``Cleaner`` and ``try-with-resources``. |

### Important Updates
  - finalize() → Removed in Java 18. Use java.lang.ref.Cleaner or AutoCloseable instead.

  - Concurrency methods (wait, notify, notifyAll) → Still present, but modern concurrency prefers java.util.concurrent APIs (Executors, Locks, Semaphores, Virtual Threads in JDK 21).

  - Reflection (getClass) → Still core, but newer APIs like sealed classes and pattern matching reduce the need for manual reflection.

## 17. `==`, equals and hashCode()
### `==` Operator
- **Definition**: Compares **references** (memory addresses) for objects, not their content.  
- For **primitives**, it compares actual values.  
- For **objects**, it checks if both references point to the same object in memory.  

```java
public class StringComparison {
    public static void main(String[] args) {
        // Using string literals (preferred)
        String s1 = "Java";
        String s2 = "Java";

        // Using 'new' forces a new object
        String s3 = new String("Java");

        // Reference equality
        System.out.println("s1 == s2: " + (s1 == s2));   // true (same pool object)
        System.out.println("s1 == s3: " + (s1 == s3));   // false (different objects)

        // Value equality
        System.out.println("s1.equals(s2): " + s1.equals(s2)); // true
        System.out.println("s1.equals(s3): " + s1.equals(s3)); // true

        // Interning example
        System.out.println("s3.intern() == s1: " + (s3.intern() == s1)); // true
    }
}
```

### `equals()` Method
- **Definition**: Compares **content/values** of objects.  
- Default implementation in `Object` class → behaves like `==` (reference equality).  
- Should be **overridden** in custom classes to compare meaningful fields.  

```java
class Student {
    String name;
    Student(String name) { this.name = name; }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Student)) return false;
        Student s = (Student) obj;
        return this.name.equals(s.name);
    }
}

Student s1 = new Student("Anshuman");
Student s2 = new Student("Anshuman");

System.out.println(s1.equals(s2)); // true (same content)
```

### `hashCode()` Method
- **Definition**: Returns an integer hash value for the object.  
- Used in **hash-based collections** (`HashMap`, `HashSet`).  
- Must be **consistent with equals()**:  
  - If `equals()` returns true → both objects must have the same `hashCode()`.  
  - If `equals()` returns false → hash codes may differ.  

```java
class Student {
    String name;
    Student(String name) { this.name = name; }

    @Override
    public boolean equals(Object obj) {
        return (obj instanceof Student) && this.name.equals(((Student)obj).name);
    }

    @Override
    public int hashCode() {
        return name.hashCode(); // consistent with equals Objects.hash(name)
    }
}

HashSet<Student> set = new HashSet<>();
set.add(new Student("Anshuman"));
set.add(new Student("Anshuman"));

System.out.println(set.size()); // 1 (because equals + hashCode are consistent)
```

### Comparison Table

| **Aspect** | **==** | **equals()** | **hashCode()** |
|------------|---------------------------------------|-----------------------------------------------|-----------------------------------------------|
| **Type** | Operator | Method | Method |
| **Default Behavior** | Compares references | Compares references (unless overridden) | Generates integer hash |
| **Use Case** | Check if two references point to same object | Check if two objects have same content | Used in hash-based collections |
| **Override?** | No | Yes (for meaningful equality) | Yes (must align with equals) |

### Easy Way to Remember
- **`==`** → Same memory reference?  
- **`equals()`** → Same content?  
- **`hashCode()`** → Same bucket in hash collections?  

### The Contract Between equals() and hashCode()
**Rule 1:** If two objects are equal according to equals(), they must return the same hashCode().

**Rule 2:** If two objects are not equal, they may still return the same hashCode() (called a collision).

**Rule 3:** hashCode() is used for bucket placement in hash-based collections (HashMap, HashSet).
