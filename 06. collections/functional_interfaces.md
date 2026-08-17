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

