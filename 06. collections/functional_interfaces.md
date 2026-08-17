# Functional Interface

**A Java Functional Interface is simply an interface with exactly one abstract method (SAM – Single Abstract Method), making it the backbone of lambda expressions and method references introduced in Java 8.** They allow you to pass behavior (functions) as arguments, enabling concise, functional-style programming.  

## Core Concepts
- **Definition**: An interface with one abstract method.  
- **Annotation**: `@FunctionalInterface` (optional, but ensures only one abstract method).  
- **Return type**: Always depends on the abstract method signature.  
- **Default/Static methods**: Allowed, but they don’t count against the “one abstract method” rule.  
- **Examples in JDK**: `Runnable`, `Comparator`, and the `java.util.function` package interfaces.  [GeeksForGeeks](https://www.geeksforgeeks.org/java/java-functional-interfaces/)  [codebegun.com](https://www.codebegun.com/learn/java/java-8/functional-interfaces)  



## The “Big Four” Functional Interfaces
| **Interface** | **Abstract Method** | **Input → Output** | **Stream Usage** |
|---------------|----------------------|--------------------|------------------|
| **Predicate** | `boolean test(T t)` | T → boolean | `filter()` |
| **Function** | `R apply(T t)` | T → R | `map()` |
| **Consumer** | `void accept(T t)` | T → void | `forEach()` |
| **Supplier** | `T get()` | nothing → T | Lazy creation |



## Examples

### 1. Custom Functional Interface
```java
@FunctionalInterface
interface Square {
    int calculate(int x);
}

public class Demo {
    public static void main(String[] args) {
        Square s = (x) -> x * x;   // Lambda implements calculate()
        System.out.println(s.calculate(5)); // Output: 25
    }
}
```

### 2. Built-in Predicate
```java
Predicate<Integer> isEven = n -> n % 2 == 0;
System.out.println(isEven.test(4)); // true
```

### 3. Function
```java
Function<String, Integer> lengthFunc = s -> s.length();
System.out.println(lengthFunc.apply("Lambda")); // 6
```

### 4. Consumer
```java
Consumer<String> printer = s -> System.out.println("Hello " + s);
printer.accept("Anshuman"); // Hello Anshuman
```

### 5. Supplier
```java
Supplier<Double> randomSupplier = () -> Math.random();
System.out.println(randomSupplier.get());
```



## Why They Matter
- Enable **functional programming** in Java.  
- Reduce boilerplate (no anonymous inner classes).  
- Power the **Stream API** (`filter`, `map`, `forEach`).  
- Make code more **expressive and reusable**.  [codebegun.com](https://www.codebegun.com/learn/java/java-8/functional-interfaces)  



## Key Takeaway
- **Study functional interfaces before lambdas** — because lambdas are just shorthand implementations of them.  
- Master the **big four (Predicate, Function, Consumer, Supplier)**, and stream operations will feel natural.  


## FunctionalInterface

**The `@FunctionalInterface` annotation is a compiler-level contract: it enforces that an interface has exactly one abstract method (SAM), making it valid for use with lambdas and method references. It’s not required, but when present, the compiler will throw an error if the interface violates functional interface rules.**



## 🔎 Internals of `@FunctionalInterface`
- **Package**: `java.lang`  
- **Declaration**:
  ```java
  @Documented
  @Retention(RUNTIME)
  @Target(TYPE)
  public @interface FunctionalInterface
  ```
- **Retention**: `RUNTIME` → the annotation is available at runtime for reflection.  
- **Target**: `TYPE` → can only be applied to interfaces (not classes, enums, or annotations).  
- **Purpose**: Informative + enforcement. It signals intent to both the compiler and developers.  

---

## ✅ What the Compiler Does
1. **Validation**: If you mark an interface with `@FunctionalInterface`, the compiler checks:
   - It must be an interface (not a class, enum, or annotation).  
   - It must declare **exactly one abstract method**.  
   - Default methods and static methods don’t count (since they have implementations).  
   - Methods that override `Object` methods (`equals`, `hashCode`, `toString`) don’t count either.  

2. **Error Handling**: If you add a second abstract method, compilation fails:
   ```java
   @FunctionalInterface
   interface BadInterface {
       void m1();
       void m2(); // ❌ Compiler error: not a functional interface
   }
   ```

3. **Documentation**: Even without the annotation, any interface with one abstract method is a functional interface. The annotation just makes the intent explicit and prevents accidental misuse.  

---

## 📊 Why It Matters
| **Aspect** | **Without Annotation** | **With Annotation** |
|------------|-------------------------|----------------------|
| Compiler enforcement | No check | Ensures only one abstract method |
| Developer clarity | Ambiguous intent | Explicitly signals “this is for lambdas” |
| Errors | Silent until runtime | Compile-time error if misused |


## 🌟 Example
```java
@FunctionalInterface
interface Calculator {
    int operate(int a, int b);
}

class Main {
    public static void main(String[] args) {
        Calculator add = (x, y) -> x + y;   // Lambda
        System.out.println(add.operate(5, 3)); // Output: 8
    }
}
```

👉 Here, the annotation guarantees `Calculator` stays a valid functional interface.


## 🧠 Key Takeaway
- **Yes, it’s a hint to the compiler** — but more than that, it’s a **contract** ensuring your interface is lambda-compatible.  
- It improves **readability, safety, and intent documentation** in modern Java codebases.  
