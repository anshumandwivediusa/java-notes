# Questions and Answers

- **Definition**  
  *Q*: What is a functional interface? Why must it have exactly one abstract method?  
  *A*: A functional interface is an interface with exactly one abstract method (SAM). Lambdas need a single target method to bind to; multiple abstract methods would make binding ambiguous.

- **Annotation**  
  *Q*: What role does `@FunctionalInterface` play? Is it mandatory?  
  *A*: It enforces at compile time that the interface has only one abstract method. It’s optional, but prevents accidental misuse.

- **Lambda Binding**  
  *Q*: How does the compiler decide which functional interface a lambda implements?  
  *A*: Through **target typing** — the compiler infers the functional interface from the context where the lambda is assigned.

- **Default/Static methods**  
  *Q*: Why don’t they break the “single abstract method” rule?  
  *A*: Because they already have implementations. Only abstract methods count toward the SAM requirement.

- **Method References**  
  *Q*: How do method references relate to lambdas?  
  *A*: They are shorthand for lambdas that just call a method. Example: `String::toUpperCase` is equivalent to `(s -> s.toUpperCase())`.

- **Scope**  
  *Q*: What does “effectively final” mean in lambdas?  
  *A*: Variables captured inside a lambda must not change after initialization. This ensures predictable behavior and avoids mutable state issues.

- **Internals**  
  *Q*: How are lambdas implemented under the hood?  
  *A*: Using the JVM’s `invokedynamic` instruction. Unlike anonymous classes, lambdas don’t generate new `.class` files — they’re linked dynamically at runtime, making them lightweight.

---

## ✅ Practical Coding Questions with Answers

### 1. Predicate
*Q*: Write a lambda to filter even numbers from a list.  
*A*:
```java
List<Integer> evens = List.of(1,2,3,4,5,6).stream()
                          .filter(n -> n % 2 == 0)
                          .toList();
System.out.println(evens); // [2,4,6]
```

---

### 2. Function
*Q*: Convert a list of strings into their lengths using a lambda.  
*A*:
```java
List<Integer> lengths = List.of("Java", "Spring", "Lambda").stream()
                            .map(s -> s.length())
                            .toList();
System.out.println(lengths); // [4,6,6]
```

---

### 3. Consumer
*Q*: Print each element in a list using `forEach`.  
*A*:
```java
List.of("Anshuman", "Raj").forEach(s -> System.out.println("Hello " + s));
```

---

### 4. Supplier
*Q*: Generate 5 random integers using a Supplier.  
*A*:
```java
Supplier<Integer> randomSupplier = () -> new Random().nextInt(100);
Stream.generate(randomSupplier).limit(5).forEach(System.out::println);
```

---

### 5. Custom Functional Interface
*Q*: Implement a custom functional interface to square a number.  
*A*:
```java
@FunctionalInterface
interface Square {
    int calculate(int x);
}

Square s = x -> x * x;
System.out.println(s.calculate(5)); // 25
```

---

## 🌟 Advanced / Real-World Style Questions

- **Predicate**: How would you validate an email format using a Predicate?  
- **Supplier**: How can a Supplier help with lazy initialization in Spring Boot?  
- **Consumer**: Why is Consumer often used in logging frameworks?  
- **Function chaining**: Can you chain a Function and BiFunction using `andThen`? Show an example.  
- **Internals**: Why are lambdas more memory-efficient than anonymous inner classes?  

---

👉 This Q&A sheet covers **concepts, coding, and internals** — exactly what interviewers test.  

Would you like me to now create a **cheat sheet** with all four core functional interfaces (Predicate, Function, Consumer, Supplier) mapped to their Stream API usage, so you can memorize them quickly?
