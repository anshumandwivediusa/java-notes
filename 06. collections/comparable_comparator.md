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

| Scenario | **Comparable Usage** | Why It Works | Example Code |
| --- | --- | --- | --- |
| **[Sorting numbers](ca://s?q=Java_Comparable_sorting_numbers)** | ``Integer``, ``Double``, ``Float`` already implement ``Comparable``. | Natural ascending order (1,2,3…). | ``List<Integer> ``nums ``= ``Arrays.asList(5,2,9); ``Collections.sort(nums); ``System.out.println(nums); ``// ``[2,5,9]`` ``Collections.sort(nums, Collections.reverseOrder());``|
| **[Sorting strings](ca://s?q=Java_Comparable_sorting_strings)** | ``String`` implements ``Comparable``. | Alphabetical order (A → Z). | ``List<String> ``names ``= ``Arrays.asList("Raj","Anshuman","Amit"); ``Collections.sort(names); ``System.out.println(names); ``// ``[Amit, ``Anshuman, ``Raj]`` |
| **[Sorting employees by salary](ca://s?q=Java_Comparable_sorting_employees_salary)** | Implement ``Comparable<Employee>`` and override ``compareTo`` to compare ``salary``. | Defines salary as the natural order for employees. | ``class ``Employee ``implements ``Comparable<Employee> ``{ ``String ``name; ``Float ``salary; ``public ``int ``compareTo(Employee ``o){ ``return ``this.salary.compareTo(o.salary);} ``} ``Collections.sort(listOfEmployees);`` |
| **[Sorting students by roll number](ca://s?q=Java_Comparable_sorting_students_roll_number)** | Implement ``Comparable<Student>`` and compare ``rollNo``. | Roll number is the natural order in academic contexts. | ``class ``Student ``implements ``Comparable<Student> ``{ ``int ``rollNo; ``public ``int ``compareTo(Student ``o){ ``return ``this.rollNo ``- ``o.rollNo;} ``} ``Collections.sort(listOfStudents);`` |
| **[Sorting products by price](ca://s?q=Java_Comparable_sorting_products_price)** | Implement ``Comparable<Product>`` and compare ``price``. | Price becomes the default order for products. | ``class ``Product ``implements ``Comparable<Product> ``{ ``double ``price; ``public ``int ``compareTo(Product ``o){ ``return ``Double.compare(this.price, ``o.price);} ``} ``Collections.sort(listOfProducts);`` |
| **[Sorting dates](ca://s?q=Java_Comparable_sorting_dates)** | ``LocalDate``, ``Date`` implement ``Comparable``. | Chronological order (earliest → latest). | ``List<LocalDate> ``dates ``= ``Arrays.asList(LocalDate.of(2026,8,18), ``LocalDate.of(2025,1,1)); ``Collections.sort(dates); ``System.out.println(dates);`` |

```java
import java.util.*;

public class SortComparison {
    public static void main(String[] args) {
        List<Integer> nums = Arrays.asList(5, 2, 9);

        Collections.sort(nums, Comparator.reverseOrder());
        System.out.println("Comparator.reverseOrder(): " + nums); // [9, 5, 2]

        Collections.sort(nums, Comparator.naturalOrder());
        System.out.println("Comparator.naturalOrder(): " + nums); // [2, 5, 9]

        Collections.sort(nums, Collections.reverseOrder());
        System.out.println("Collections.reverseOrder(): " + nums); // [9, 5, 2]
    }
}

```

Works with Collections.sort(), Arrays.sort(), Stream.sorted(), and sorted collections (TreeSet, TreeMap).

| Scenario | Code Example | Explanation |
| --- | --- | --- |
| **[Collections.sort](ca://s?q=Java_Collections_sort_with_Comparable)** | ``Collections.sort(employees);`` | Sorts the list using ``compareTo`` defined in ``Employee``. |
| **[Arrays.sort](ca://s?q=Java_Arrays_sort_with_Comparable)** | ``Employee[] ``arr ``= ``employees.toArray(new ``Employee[0]); ``Arrays.sort(arr);`` | Works for arrays; uses natural order. |
| **[Stream.sorted](ca://s?q=Java_Stream_sorted_with_Comparable)** | ``employees.stream().sorted().forEach(System.out::println);`` | Stream API automatically uses ``compareTo``. |
| **[TreeSet](ca://s?q=Java_TreeSet_with_Comparable)** | ``Set<Employee> ``set ``= ``new ``TreeSet<>(employees);`` | TreeSet stores elements in sorted order using ``compareTo``. |
| **[TreeMap](ca://s?q=Java_TreeMap_with_Comparable)** | ``Map<Employee,String> ``map ``= ``new ``TreeMap<>(); ``map.put(new ``Employee("Raj",40000f),"Dev");`` | Keys are sorted by their natural order. TreeMap keys are sorted only by the fields mentioned in compareTo.|

Reverse:
 - Natural order is what you define. If you flip the comparison, descending becomes the “natural” order for that class.
 - This means everywhere you use Collections.sort, Arrays.sort, Stream.sorted, or TreeSet/TreeMap, the objects will appear in reverse order.
 - If you want both ascending and descending options, it’s better to keep ascending in Comparable and use a Comparator.reversed() when you need descending.

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
```jaav

import java.util.*;

class Employee {
    String name;
    double salary;

    Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return name + " : " + salary;
    }
}

// Custom Comparator class
class SalaryComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee e1, Employee e2) {
        return Double.compare(e1.salary, e2.salary); // ascending order
    }
}

public class Main {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Anshuman", 50000),
            new Employee("Raj", 40000),
            new Employee("Amit", 60000)
        );

        // Use custom comparator
        Collections.sort(employees, new SalaryComparator());
        employees.forEach(System.out::println);
    }
}
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

