## Core Concepts

### 1. What is the `Collection` interface?
**Answer:** `Collection` is the **root interface** of the Java Collections Framework (excluding `Map`). It represents a group of **single objects** and provides basic CRUD operations using common methods like - `add(E e)`, addAll(Coellection c), `remove(Object o)`, `contains(Object o)`, search, and iteration.


### 2. Which package contains `Collection`?
**Answer:** It is part of the `java.util` package.


### 3. Does `Collection` extend any other interface?
**Answer:**  Yes, it extends **`Iterable`**, which means all collections can be traversed using `Iterator` or enhanced `for-each` loops.


### 4. What are the main sub interfaces of `Collection`?
**Answer:**  
- **Set** → Unique elements. No ordering, For ordering use TreeSet.
- **Queue** → FIFO/LIFO processing.  
- **List** → Ordered, allows duplicates.  

### 5. What are the key methods of `Collection`?  
**Answer:** Some important methods include:  
- `add(E e)` → Adds element.  
- `remove(Object o)` → Removes element.  
- `contains(Object o)` → Checks existence.  
- `size()` → Returns number of elements.  
- `isEmpty()` → Checks if empty.  
- `clear()` → Removes all elements.  
- `iterator()` → Returns an iterator.  
- `addAll(Collection c)` → Adds all elements from another collection.  
- `removeAll(Collection c)` → Removes all elements present in another collection.  
- `retainAll(Collection c)` → Keeps only common elements.  
- `toArray()` → Converts to array.  
- `stream()` / `parallelStream()` → Functional programming support (Java 8+).


### 6. Can `Collection` store primitive types?
**Answer:** No, it stores only **objects**. For primitives, you must use wrapper classes (e.g., `Integer`, `Double`).


### 7. Is `Collection` synchronized?
**Answer:** No, by default it is **not synchronized**. You can use `Collections.synchronizedCollection()` to make it thread-safe.


### 8. What is the difference between `Collection` and `Map`?  

or

Why does Map not inherit Collection?



**Answer:** Because the design goals are different:
 - Collection → Represents a group of single elements (objects).
 - Map → Represents key–value pairs, where each key maps to a value.
 - If Map extended Collection, it would force unnatural semantics like treating a key–value pair as a single element, which breaks the conceptual clarity of the framework.


### 9. How does `Collection` support iteration?  
**Answer:**  
Through the `iterator()` method (from `Iterable`) and enhanced `for-each` loops.

```java
for (Type element : collection) {
    // use element
}

import java.util.*;

public class ForEachDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Andrew", "Rahul", "Priya");

        // Enhanced for-each loop
        for (String name : names) {
            System.out.println("Name: " + name);
        }
    }
}
```

### 10. **What is the role of Streams in `Collection`?  
**Answer:**  
Since Java 8, `Collection` provides `stream()` and `parallelStream()` methods for functional programming (filter, map, reduce).


### 11. What is the difference between `Collection` and `Collections`?
**Answer:**
* **`Collection`** → An **interface** representing a group of objects.
* **`Collections`** → A **utility class** containing static methods such as `sort()`, `reverse()`, and `shuffle()`.

| **Method** | **Description** |
| --- | --- |
| **[sort(List)](ca://s?q=Collections_sort_method_in_Java)** | Sorts a list in natural order. |
| **[sort(List, Comparator)](ca://s?q=Collections_sort_with_comparator_in_Java)** | Sorts a list using a custom comparator. |
| **[reverse(List)](ca://s?q=Collections_reverse_method_in_Java)** | Reverses the order of elements in a list. |
| **[shuffle(List)](ca://s?q=Collections_shuffle_method_in_Java)** | Randomly permutes elements in a list. |
| **[swap(List, i, j)](ca://s?q=Collections_swap_method_in_Java)** | Swaps elements at given positions. |
| **[rotate(List, distance)](ca://s?q=Collections_rotate_method_in_Java)** | Rotates elements by given distance. |
| **[binarySearch(List, key)](ca://s?q=Collections_binarySearch_method_in_Java)** | Searches for an element in a sorted list. |
| **[max(Collection)](ca://s?q=Collections_max_method_in_Java)** | Returns the maximum element (natural order). |
| **[min(Collection)](ca://s?q=Collections_min_method_in_Java)** | Returns the minimum element (natural order). |
| **[frequency(Collection, o)](ca://s?q=Collections_frequency_method_in_Java)** | Counts occurrences of an element. |
| **[disjoint(Collection, Collection)](ca://s?q=Collections_disjoint_method_in_Java)** | Checks if two collections have no elements in common. |
| **[copy(List dest, List src)](ca://s?q=Collections_copy_method_in_Java)** | Copies elements from source to destination list. |
| **[fill(List, obj)](ca://s?q=Collections_fill_method_in_Java)** | Replaces all elements with given object. |
| **[nCopies(int, obj)](ca://s?q=Collections_nCopies_method_in_Java)** | Returns an immutable list with n copies of an object. |
| **[singleton(obj)](ca://s?q=Collections_singleton_method_in_Java)** | Returns an immutable set with one element. |
| **[unmodifiableList(List)](ca://s?q=Collections_unmodifiableList_in_Java)** | Returns an unmodifiable view of a list. |
| **[synchronizedList(List)](ca://s?q=Collections_synchronizedList_in_Java)** | Returns a synchronized (thread-safe) list. |
| **[synchronizedSet(Set)](ca://s?q=Collections_synchronizedSet_in_Java)** | Returns a synchronized set. |
| **[synchronizedMap(Map)](ca://s?q=Collections_synchronizedMap_in_Java)** | Returns a synchronized map. |
| **[emptyList()](ca://s?q=Collections_emptyList_in_Java)** | Returns an immutable empty list. |
| **[emptySet()](ca://s?q=Collections_emptySet_in_Java)** | Returns an immutable empty set. |
| **[emptyMap()](ca://s?q=Collections_emptyMap_in_Java)** | Returns an immutable empty map. |

### 12. Is `Map` part of the `Collection` interface hierarchy?
**Answer:** **No.** `Map` is part of the Java Collections Framework but does **not** extend the `Collection` interface because it stores **key-value pairs**.


### 13. What is the `Iterable` interface?
**Answer:** `Iterable` is the **root interface for iteration** in Java. It provides the `iterator()` method and allows objects to be used in an enhanced **for-each loop**.

```text
Iterable
   ↓
Collection
   ↓
List / Set / Queue
```


### 14. What is the difference between fail-fast and fail-safe iterators?

**Answer:**

* **Fail-fast** → The term Fail‑Fast is a conceptual name used in Java collections (and other systems) to describe behavior where an iterator or method immediately throws an exception if the underlying collection is structurally modified while being iterated. Throws `ConcurrentModificationException` if the collection is structurally modified during iteration.
* **Fail-safe** → Iterates over a copy or snapshot, so modifications do not cause an exception.

**Examples:**

* Fail-fast → `ArrayList`, `HashMap`
* Fail-safe / weakly consistent → `ConcurrentHashMap`, `CopyOnWriteArrayList`


### 15. What is the difference between `Collection` and `Iterator`?

**Answer:**

* **`Collection`** → Stores and manages a group of objects.
* **`Iterator`** → Traverses the elements of a collection.


### 16. What is the difference between `Set` and `List`?

**Answer:**

| `Set`                           | `List`                      |
| ------------------------------- | --------------------------- |
| Stores unique elements          | Allows duplicate elements   |
| Generally no index-based access | Supports index-based access |
| Order depends on implementation | Maintains insertion order   |


### 17. What is the difference between `Map` and `Collection`?

**Answer:**

* **`Map`** stores data as **key-value pairs**.
* **`Collection`** stores individual **objects/elements**.


### 18. What is the difference between `HashMap` and `HashSet`?

**Answer:**

* **`HashMap`** stores **key-value pairs** and allows unique keys.
* **`HashSet`** stores **unique values only**.

**Important:** Internally, `HashSet` is backed by a `HashMap`.


### 19. What is the difference between `Enumeration` and `Iterator`?

**Answer:**

* **`Enumeration`** is a **legacy interface**, mainly used with classes such as `Vector` and `Hashtable`.
* **`Iterator`** is the modern interface used across the Collections Framework.
* `Iterator` supports `remove()`, while `Enumeration` does not.
* `Iterator` can exhibit **fail-fast behavior** in standard collections.

 

### 20. **What is the difference between Iterator and ListIterator?**  
**Answer:**  
- `Iterator` → Traverses forward only.  
- `ListIterator` → Traverses both forward and backward, with extra methods like `add()`, `set()`, `previous()`.  


```java
import java.util.*;

public class ListIteratorDemo {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>(Arrays.asList("Anshuman", "Rahul", "Priya"));

        ListIterator<String> it = names.listIterator();

        System.out.println("Forward Traversal:");
        while (it.hasNext()) {
            System.out.println(it.next());
        }

        System.out.println("\nBackward Traversal:");
        while (it.hasPrevious()) {
            System.out.println(it.previous());
        }
    }
}
```

### 21. **What is the difference between Comparable and Comparator?**  
**Answer:**  
- `Comparable` → Defines **natural ordering** via `compareTo()`.  
- `Comparator` → Defines **custom ordering** via `compare()`.  


### 22. **What is the difference between HashMap and ConcurrentHashMap?**  
**Answer:**  
- `HashMap` → Not thread‑safe, allows null keys/values.  
- `ConcurrentHashMap` → Thread‑safe, better concurrency, disallows null keys/values.  


### 23. **What is the difference between HashMap and LinkedHashMap?**  
**Answer:**  
- `HashMap` → Unordered storage.  
- `LinkedHashMap` → Maintains **insertion order** of keys.  


### 24. **What is the difference between HashMap and TreeMap?**  
**Answer:**  
- `HashMap` → Unordered, O(1) average operations.  
- `TreeMap` → Sorted by keys, O(log n) operations.  


### 25. **What is the difference between HashSet and LinkedHashSet?**  
**Answer:**  
- `HashSet` → Unordered unique elements.  
- `LinkedHashSet` → Maintains **insertion order** of elements.  


### 26. **What is the difference between HashSet and TreeSet?**  
**Answer:**  
- `HashSet` → Unordered, backed by HashMap.  
- `TreeSet` → Sorted, backed by TreeMap.  


### 27. **What is the difference between ArrayList and Vector?**  
**Answer:**  
- `ArrayList` → Unsynchronized, faster in single‑threaded use.  
- `Vector` → Synchronized, legacy, slower.  


### 28. **What is the difference between ArrayList and CopyOnWriteArrayList?**  
**Answer:**  
- `ArrayList` → Not thread‑safe.  
- `CopyOnWriteArrayList` → Thread‑safe, uses copy‑on‑write strategy.  


### 29. **What is the difference between Hashtable and ConcurrentHashMap?**  
**Answer:**  
- `Hashtable` → Synchronized, legacy, slower.  
- `ConcurrentHashMap` → Modern, thread‑safe with better concurrency.  


## 📘 List
6. **ArrayList vs LinkedList? → ArrayList = fast random access, LinkedList = fast insert/delete.  
7. **Vector vs ArrayList? → Vector synchronized, ArrayList not.  
8. **Stack in Java? → Legacy, extends Vector, LIFO.  
9. **CopyOnWriteArrayList? → Thread-safe list, uses copy on write.  
10. **Difference between ListIterator and Iterator? → ListIterator supports bidirectional traversal.  


## 📘 Set
11. **HashSet vs TreeSet? → HashSet unordered, TreeSet sorted.  
12. **LinkedHashSet? → Maintains insertion order.  
13. **EnumSet? → Specialized set for enums.  
14. **CopyOnWriteArraySet? → Thread-safe set, backed by CopyOnWriteArrayList.  
15. **Is HashSet synchronized? → No, must wrap or use concurrent sets.  


## 📘 Map
16. **HashMap vs Hashtable? → HashMap unsynchronized, Hashtable synchronized.  
17. **HashMap vs TreeMap? → HashMap unordered, TreeMap sorted.  
18. **LinkedHashMap? → Maintains insertion order.  
19. **ConcurrentHashMap? → Thread-safe, lock-striping.  
20. **WeakHashMap? → Keys garbage-collected when weakly referenced.  


## 📘 Queue & Deque
21. **PriorityQueue? → Orders elements by priority.  
22. **ArrayDeque? → Resizable array, double-ended queue.  
23. **BlockingQueue? → Thread-safe queue for producer-consumer.  
24. **ConcurrentLinkedQueue? → Lock-free queue for concurrency.  
25. **Deque vs Queue? → Deque supports both ends, Queue only one.  


## 📘 Sorting & Ordering
26. **Comparable vs Comparator? → Comparable = natural order, Comparator = custom order.  
27. **Collections.sort()? → Sorts list using natural/comparator order.  
28. **Arrays.sort()? → Sorts arrays.  
29. **TreeMap ordering? → Sorted by keys.  
30. **TreeSet ordering? → Sorted by elements.  


## 📘 Synchronization
31. **Collections.synchronizedList()? → Wraps list with synchronized access.  
32. **Collections.synchronizedSet()? → Wraps set with synchronized access.  
33. **Collections.synchronizedMap()? → Wraps map with synchronized access.  
34. **ConcurrentSkipListSet? → Thread-safe sorted set.  
35. **ConcurrentSkipListMap? → Thread-safe sorted map.  


## 📘 Legacy
36. **Hashtable? → Legacy synchronized map.  
37. **Vector? → Legacy synchronized list.  
38. **Stack? → Legacy LIFO stack.  
39. **Enumeration? → Legacy iterator.  
40. **Properties class? → Legacy key-value pairs for config.  


## 📘 Miscellaneous
41. **IdentityHashMap? → Compares keys by reference, not equals().  
42. **NavigableMap? → Extends SortedMap with navigation methods.  
43. **NavigableSet? → Extends SortedSet with navigation methods.  
44. **Unmodifiable collections? → Created via `Collections.unmodifiableX()`.  
45. **Singleton collections? → Created via `Collections.singleton()`.  


## 📘 Performance & Internals
46. **HashMap load factor? → Default 0.75, triggers rehashing.  
47. **Initial capacity of HashMap? → Default 16.  
48. **Time complexity of HashSet add()? → Average O(1).  
49. **Time complexity of TreeSet add()? → O(log n).  
50. **ConcurrentHashMap concurrency level? → Default 16 (Java 7), replaced by finer-grained locks in Java 8+.  

