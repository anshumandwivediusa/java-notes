# Comparable vs Comparator 

**Comparable defines a class’s *natural ordering* (one fixed way to sort), while Comparator allows *custom, flexible ordering* outside the class. For exams, remember: Comparable = “default order inside the class,” Comparator = “external order, multiple strategies.”**

## Comparable
- **Package:** `java.lang`  
- **Method:** `int compareTo(T other)`  
- **Purpose:** Defines *natural ordering* (default sort order baked into the class).  
- **Limitation:** Only one ordering per class.  

### Example
```java
class Employee implements Comparable<Employee> {
    String name;
    Float salary;

    @Override
    public int compareTo(Employee other) {
        return this.salary.compareTo(other.salary); // natural order by salary
    }
}
```
👉 Now `Collections.sort(list)` or `list.stream().sorted()` will sort employees by salary automatically.


## Comparator
- **Package:** `java.util`  
- **Method:** `int compare(T o1, T o2)`  
- **Purpose:** Defines *custom ordering* externally.  
- **Advantage:** Multiple strategies possible (by name, by salary, by department).  

### Example
```java
Comparator<Employee> byName = (e1, e2) -> e1.name.compareTo(e2.name);
Comparator<Employee> bySalaryDesc = (e1, e2) -> e2.salary.compareTo(e1.salary);

employees.stream().sorted(byName).forEach(System.out::println);
employees.stream().sorted(bySalaryDesc).forEach(System.out::println);
```


## Comparison Table

| Feature | **Comparable** | **Comparator** |
|---------|----------------|----------------|
| Package | `java.lang` | `java.util` |
| Method | `compareTo(T o)` | `compare(T o1, T o2)` |
| Ordering | Natural (default) | Custom (flexible) |
| Implementation | Inside the class | Outside the class |
| Use Case | One fixed sort order | Multiple sort orders |
| Example | `String`, `Integer` | Custom sort by salary, name |


## Key Exam Notes
- **Comparable = “I decide my own order.”**  
- **Comparator = “Others decide how to order me.”**  
- Comparable is used when there’s a *single natural order* (like roll number, salary).  
- Comparator is used when you need *different sorting strategies*.  
- Both can be used with **Collections.sort()**, **Arrays.sort()**, and **Streams.sorted()**.  


## Common Pitfalls (Exam Traps)
- Don’t cast subtraction results to `int` for floating-point comparison — use `Double.compare` or `Float.compare`.  
- Remember: `compareTo` returns **negative, zero, positive** — not strictly -1, 0, 1.  
- Comparator is a **functional interface** → can use lambdas and method references.  

