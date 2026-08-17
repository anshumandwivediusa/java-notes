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


# Java Functional Programming — 20 Practice Q&A


### **1. Even Numbers**
**Q**: Find all even numbers from a list using Predicate and Stream.  
```java
List<Integer> nums = Arrays.asList(10, 15, 20, 25, 30);
Predicate<Integer> isEven = n -> n % 2 == 0;
List<Integer> evens = nums.stream().filter(isEven).toList();
System.out.println(evens); // [10, 20, 30]
```


### **2. Prime Numbers**
**Q**: Find all prime numbers using a Predicate<Integer>.  
```java
Predicate<Integer> isPrime = n -> n > 1 && IntStream.range(2, n).noneMatch(i -> n % i == 0);
List<Integer> primes = nums.stream().filter(isPrime).toList();
System.out.println(primes);
```


### **3. Greater Than 50**
**Q**: Find numbers greater than 50 using a Predicate.  
```java
Predicate<Integer> gt50 = n -> n > 50;
List<Integer> result = List.of(10, 60, 75, 30).stream().filter(gt50).toList();
System.out.println(result); // [60, 75]
```


### **4. Uppercase Names**
**Q**: Convert a list of names to uppercase using Function.  
```java
Function<String, String> toUpper = s -> s.toUpperCase();
List<String> upper = List.of("Anshuman","Raj","Amit").stream().map(toUpper).toList();
System.out.println(upper);
```


### **5. Names Starting with A**
**Q**: Print all names starting with "A".  
```java
Predicate<String> startsWithA = s -> s.startsWith("A");
List.of("Anshuman","Raj","Amit").stream().filter(startsWithA).forEach(System.out::println);
```


### **6. Sum with Reduce**
**Q**: Calculate sum of all numbers.  
```java
int sum = nums.stream().reduce(0, Integer::sum);
System.out.println(sum); // 100
```


### **7. Max & Min**
**Q**: Find maximum and minimum number.  
```java
int max = nums.stream().max(Integer::compare).get();
int min = nums.stream().min(Integer::compare).get();
System.out.println("Max=" + max + ", Min=" + min);
```


### **8. Remove Duplicates**
**Q**: Remove duplicate elements.  
```java
List<Integer> list = Arrays.asList(10,20,20,30,30,40);
List<Integer> distinct = list.stream().distinct().toList();
System.out.println(distinct); // [10,20,30,40]
```


### **9. Sort Employees**
**Q**: Sort employees by salary.  
```java
employees.stream()
         .sorted((e1,e2)->Double.compare(e1.getSalary(),e2.getSalary()))
         .forEach(System.out::println);
```


### **10. Count Employees**
**Q**: Count employees with salary > ₹10 lakh.  
```java
long count = employees.stream().filter(e->e.getSalary()>1000000).count();
System.out.println(count);
```


### **11. Square Numbers**
**Q**: Use Function to square each number.  
```java
Function<Integer,Integer> square = x->x*x;
List<Integer> squares = nums.stream().map(square).toList();
System.out.println(squares);
```


### **12. Consumer Logging**
**Q**: Use Consumer to log each element.  
```java
Consumer<String> logger = s -> System.out.println("Log: " + s);
List.of("A","B","C").forEach(logger);
```


### **13. Supplier UUID**
**Q**: Generate random UUIDs.  
```java
Supplier<String> uuidSupplier = () -> UUID.randomUUID().toString();
Stream.generate(uuidSupplier).limit(3).forEach(System.out::println);
```


### **14. Method Reference**
**Q**: Replace lambda with method reference.  
```java
List.of("A","B","C").forEach(System.out::println);
```


### **15. Grouping by Department**
**Q**: Group employees by department.  
```java
Map<String,List<Employee>> grouped = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment));
```


### **16. Average Salary**
**Q**: Find average salary.  
```java
double avg = employees.stream().mapToDouble(Employee::getSalary).average().orElse(0);
System.out.println(avg);
```


### **17. Parallel Stream**
**Q**: Use parallel stream for performance.  
```java
long count = nums.parallelStream().filter(n->n%2==0).count();
System.out.println(count);
```


### **18. Exception Handling in Lambda**
**Q**: Handle checked exception inside lambda.  
```java
List<String> files = List.of("a.txt","b.txt");
files.forEach(f -> {
    try { System.out.println(Files.readString(Path.of(f))); }
    catch(IOException e){ e.printStackTrace(); }
});
```


### **19. Reduce to Product**
**Q**: Multiply all numbers using reduce.  
```java
int product = nums.stream().reduce(1,(a,b)->a*b);
System.out.println(product);
```


### **20. Custom Functional Interface**
**Q**: Create a functional interface to calculate cube.  
```java
@FunctionalInterface
interface Cube { int calc(int x); }
Cube c = n -> n*n*n;
System.out.println(c.calc(3)); // 27
```






