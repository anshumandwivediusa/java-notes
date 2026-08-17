# Questions and Answers

  *Q*: What is Functional Programming?  
  *A*: Functional programming is a **declarative programming paradigm** where you focus on *what to do* rather than *how to do it*.  
  
  - It treats **functions as first-class citizens**:  
    - You can **pass them as arguments**  
    - You can **return them from other functions**  
    - You can **store them in variables**  
  
  - In Java, functional programming was introduced with **Java 8** through **lambda expressions** and the **Stream API**, enabling concise, predictable, and testable code.

  *Q*: What is the difference between Declarative and Imperative programming?  
  *A*:   
   - **Imperative**: Explicitly tells the computer step-by-step how to do something (loops, mutable state).  
   - **Declarative**: Focuses on describing the result you want, leaving the “how” to the language/runtime.  

     **Imperative (Before Java 8):**
     ```java
     for (int n : numbers) {
         if (n % 2 == 0) {
             System.out.println(n);
         }
     }
     ```
     
     **Declarative (Java 8+ with Streams):**
     ```java
     numbers.stream()
            .filter(n -> n % 2 == 0)
            .forEach(System.out::println);
     ```

  *Q*: What is a functional interface? Why must it have exactly one abstract method?  
  *A*: A functional interface is an interface with exactly one abstract method (SAM). Lambdas need a single target method to bind to; multiple abstract methods would make binding ambiguous.

  *Q*: What role does `@FunctionalInterface` play? Is it mandatory?  
  *A*: It enforces at compile time that the interface has only one abstract method. It’s optional, but prevents accidental misuse.

  *Q*: How does the compiler decide which functional interface a lambda implements?  
  *A*: Through **target typing** — the compiler infers the functional interface from the context where the lambda is assigned.

  *Q*: Why don’t they break the “single abstract method” rule?  
  *A*: Because they already have implementations. Only abstract methods count toward the SAM requirement.

  *Q*: How do method references relate to lambdas?  
  *A*: They are shorthand for lambdas that just call a method. Example: `String::toUpperCase` is equivalent to `(s -> s.toUpperCase())`.

  *Q*: What does “effectively final” mean in lambdas?  
  *A*: Variables captured inside a lambda must not change after initialization. This ensures predictable behavior and avoids mutable state issues.

  *Q*: How are lambdas implemented under the hood?  
  *A*: Using the JVM’s `invokedynamic` instruction. Unlike anonymous classes, lambdas don’t generate new `.class` files — they’re linked dynamically at runtime, making them lightweight.


  *Q*: How are lambdas different from anonymous inner classes?  
  *A*: Lambdas don’t create a separate `.class` file; they’re implemented using `invokedynamic`. Anonymous classes are heavier and generate extra bytecode.


  *Q*: What are the core functional interfaces in `java.util.function`?  
  *A*: `Predicate<T>`, `Function<T,R>`, `Consumer<T>`, `Supplier<T>`, `BiFunction<T,U,R>`, `UnaryOperator<T>`, `BinaryOperator<T>`.


  *Q*: How do lambdas integrate with the Stream API?  
  *A*: They are passed as behavior into operations like `map`, `filter`, `reduce`, `forEach`.


  *Q*: Can lambdas throw checked exceptions?  
  *A*: Only if the functional interface’s abstract method declares them. Otherwise, you must handle them inside the lambda.


  *Q*: Are lambdas serializable?  
  *A*: Only if the target functional interface extends `Serializable`. By default, they are not.


  *Q*: How do lambdas capture variables?  
  *A*: They can capture variables from the enclosing scope, but only if they are *effectively final*.


  *Q*: Are lambdas always faster than loops?  
  *A*: Not necessarily. For small datasets, traditional loops may be faster. Lambdas shine with readability, parallelism, and large data sets.


  *Q*: Can a functional interface extend another interface?  
  *A*: Yes, but the resulting interface must still have only one abstract method.
