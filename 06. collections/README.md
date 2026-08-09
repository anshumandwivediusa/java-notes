# Collection: Java java.util Package

<p align="center">
  <img width="500" height="350" alt="image" src="https://github.com/user-attachments/assets/6c8c78dd-e5a1-4cd4-8475-e62d343dd0cc" />
</p>

## 1. Iterable

- Root interface of the collection traversal mechanism
- Provide mechanism for `for-each` loop, `Iterator` and `Spliitrator`.
- Declared in `java.lang` package.  
- **Generic interface**: `public interface Iterable<T>`  
- Introduced in **Java 5** to support the enhanced `for-each` loop.  

### Core Methods
| Method | Description |
|------|----|
| **`Iterator<T> iterator()`** | Returns an `Iterator` over elements of type `T`. |
| **`default void forEach(Consumer<? super T> action)`** | Performs the given action for each element. |
| **`default Spliterator<T> spliterator()`** | Creates a `Spliterator` for parallel iteration. |

```java
import java.util.*;

public class IterableDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

        // Using for-each loop (thanks to Iterable)
        for (String name : names) {
            System.out.println(name);
        }

        // Using iterator explicitly
        Iterator<String> it = names.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }

        // Using forEach method
        names.forEach(System.out::println);
    }
}
```

## Iterator

 - Interface in java.util.
 - Provides methods to traverse elements:
 - Used to step through elements one by one.
 - More control than for‑each loop (e.g., can remove elements during iteration).
 - Think of Iterator as the mechanism that actually performs the iteration.

   ```java
   boolean hasNext();
   T next();
   void remove(); // optional
   ```


## SplitIterator

 - Spliterator = Split + Iterator.
 - Interface in java.util.
 - Designed for parallel iteration and Streams API.
 - Can traverse elements sequentially or split them into parts for parallel processing.
 
 ```java
 import java.util.*;
 
 public class IterableDemo {
     public static void main(String[] args) {
         List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
 
         // Using Spliterator
         Spliterator<String> spliterator = names.spliterator();

         // Process elements one by one
         spliterator.forEachRemaining(System.out::println);
 
         // Or split for parallel processing
         Spliterator<String> split = spliterator.trySplit();
         if (split != null) {
             split.forEachRemaining(System.out::println);
         }
     }
 }
 ```

## Collection Methods

| **Category** | **Method** | **Description** |
| ---  | --- | --- |
| **CRUD (Modify)** | `boolean add(E e)` | Adds an element to the collection. |
|  | `boolean addAll(Collection<? extends E> c)` | Adds all elements from another collection. |
|  | `boolean remove(Object o)` | Removes a single instance of the specified element. |
|  | `boolean removeAll(Collection<?> c)` | Removes all elements that are also in another collection. |
|  | `boolean retainAll(Collection<?> c)` | Retains only elements present in another collection. |
|  | `void clear()` | Removes all elements. |
| **Query (Check)** | `boolean contains(Object o)` | Checks if the collection contains the given element. |
|  | `boolean containsAll(Collection<?> c)` | Checks if the collection contains all elements of another collection. |
|  | `boolean isEmpty()` | Checks if the collection is empty. |
|  | `int size()` | Returns the number of elements. |
| **Conversion** | `Object[] toArray()` | Returns an array containing all elements. |
|  | `<T> T[] toArray(T[] a)` | Returns an array containing all elements in the specified type. |
| **Iteration** | Iterator<E> iterator()` | Returns an iterator over elements. |
|  | `default void forEach(Consumer<? super E> action)` | Performs the given action for each element. |
|  | `default Spliterator<E> spliterator()` | Returns a Spliterator for parallel iteration. |
| **Identity (Object)** | `boolean equals(Object o)` | Compares this collection with another for equality. |
|  | `int hashCode()` | Returns hash code for the collection. |

_List Methods with Functional Interfaces_



## Set, Queue, List (SQL)

Key implementations:

| **Interface** | **Ordered?** | **Duplicates?** | **Null allowed?** | **Typical impl** | **Concepts / Usage Notes** |
| --- | --- | --- | --- | --- | --- |
| **[List](ca://s?q=Java_List_interface)** | Yes (index-based) | Yes | Yes | ArrayList, LinkedList | Sequential collection, preserves insertion order. Best for random access (``ArrayList``) or frequent insert/delete (``LinkedList``). |
| **[Set](ca://s?q=Java_Set_interface)** | No (HashSet), Yes (Linked/Tree) | No | 1 null (Hash/Linked), none in TreeSet | HashSet, TreeSet | Ensures uniqueness. HashSet is fastest, TreeSet keeps sorted order, LinkedHashSet preserves insertion order. |
| **[Map](ca://s?q=Java_Map_interface)** | Depends (HashMap unordered, LinkedHashMap ordered, TreeMap sorted) | Keys unique, values can repeat | 1 null key (HashMap), multiple null values | HashMap, TreeMap | Key-value pairs. HashMap for speed, TreeMap for sorted keys, LinkedHashMap for predictable iteration order. |
| **[Queue/Deque](ca://s?q=Java_Queue_interface)** | FIFO (Queue), LIFO possible with Deque | Yes | Depends on impl | ArrayDeque, PriorityQueue | Queue for task scheduling (FIFO), PriorityQueue for ordering by comparator, Deque for double-ended operations (stack/queue hybrid). |


# List Interface

The List interface in Java represents an **ordered collection of elements**, functioning like a **sequence** where each item has a specific position (index). Conceptually, you can think of a List as a flexible array: it preserves the order in which elements are inserted, allows duplicates, and provides positional access so you can retrieve, insert, or replace elements at any index. Unlike a plain array, a List grows and shrinks dynamically, offering rich operations such as searching (contains, indexOf), slicing (subList), and iteration (iterator, listIterator, forEach).

Because it extends the Collection interface, a List inherits all general collection behaviors (like add, remove, size, clear) but enhances them with index‑based control. This makes it ideal when you need to maintain order, handle duplicates, and frequently access elements by position. Common implementations include **ArrayList (backed by a dynamic array, fast random access), LinkedList (efficient insert/delete operations), and legacy classes like Vector and Stack**.

Conceptually:
- Array = fixed, rigid container.
- List = dynamic, ordered sequence with powerful operations.

### List Methods 
| **Method** | **Description** |
| --- | --- |
| ``void ``add(int ``index, ``E ``element)``; ``list.add("a");`` | Inserts element at specified position. |
| ``boolean ``addAll(int ``index, ``Collection<? ``extends ``E> ``c)`` | Inserts all elements at specified position. |
| ``E ``get(int ``index)`` | Returns element at given position. |
| ``E ``set(int ``index, ``E ``element)`` | Replaces element at given position. |
| ``E ``remove(int ``index)`` ``remove("a")`` | Removes element at given position. |
| ``int ``indexOf(Object ``o)`` | Returns first index of element, or -1. |
| ``int ``lastIndexOf(Object ``o)`` | Returns last index of element, or -1. |
| ``ListIterator<E> ``listIterator()`` | Returns list iterator (bidirectional). |
| ``ListIterator<E> ``listIterator(int ``index)`` | Returns list iterator starting at index. |
| ``List<E> ``subList(int ``fromIndex, ``int ``toIndex)`` | Returns view of portion of list. |

### Ways to create List
| **Approach** | **Code Example** | **Notes** |
| --- | --- | --- |
| **[Constructor](ca://s?q=Create_List_with_ArrayList_Constructor)** | ``List<String> ``list ``= ``new ``ArrayList<>();`` | Most common, mutable, dynamic size. |
| **[Add/Replace/Query Elements](ca://s?q=Add_Elements_to_List)** | ``list.add("a"); ``list.add("b");`` ``list.add(0, "b");`` list.set(0, "c"); ``    | Standard way to populate. |
| **[Arrays.asList](ca://s?q=Arrays_asList_in_Java)** | ``List<String> ``list ``= ``Arrays.asList("a","b","c");`` | Fixed‑size list backed by array (cannot add/remove). |
| **[Collections.singletonList](ca://s?q=Collections_singletonList_in_Java)** | ``List<String> ``list ``= ``Collections.singletonList("a");`` | Immutable list with one element. |
| **[List.of (Java 9+)](ca://s?q=List_of_in_Java)** | ``List<String> ``list ``= ``List.of("x","y","z");`` | Immutable, concise factory method. |
| **[List.copyOf (Java 10+)](ca://s?q=List_copyOf_in_Java)** | ``List<String> ``copy ``= ``List.copyOf(list);`` | Immutable copy of another list. |
| **[Stream Collectors](ca://s?q=Stream_toList_in_Java)** | ``List<String> ``list ``= ``Stream.of("a","b").collect(Collectors.toList());`` | Useful when working with streams. |
| **[Empty List](ca://s?q=Collections_emptyList_in_Java)** | ``List<String> ``list ``= ``Collections.emptyList();`` | Immutable empty list. |
| **[Pre‑Sized ArrayList](ca://s?q=Pre_sized_ArrayList_in_Java)** | ``List<String> ``list ``= ``new ``ArrayList<>(100);`` | Mutable list with initial capacity. |
| **[LinkedList](ca://s?q=LinkedList_in_Java)** | ``List<String> ``list ``= ``new ``LinkedList<>();`` | Doubly linked list, good for frequent insert/delete. |
| **[Vector/Stack](ca://s?q=Vector_and_Stack_in_Java)** | ``List<String> ``list ``= ``new ``Vector<>();`` | Legacy synchronized list; ``Stack`` extends ``Vector``. |

### Sorting, Searching and Synchronization

| **Operation** | **Method / Utility** | **Description** |
| --- | --- | --- |
| **[Sorting](ca://s?q=Sorting_List_in_Java)** | ``list.sort(Comparator)`` | Sorts elements in place using a comparator. |
|  | ``Collections.sort(list)`` | Legacy static method to sort a list. |
|  | ``Collections.reverse(list)`` | Reverses the order of elements. |
|  | ``list.replaceAll(UnaryOperator<E>)`` | Can be used to transform elements before/after sorting. |
| **[Searching](ca://s?q=Searching_in_List_in_Java)** | ``list.contains(Object ``o)`` | Checks if element exists. |
|  | ``list.indexOf(Object ``o)`` | Returns first index of element, or -1. |
|  | ``list.lastIndexOf(Object ``o)`` | Returns last index of element, or -1. |
|  | ``Collections.binarySearch(list, ``key)`` | Searches sorted list using binary search. |
| **[Synchronization](ca://s?q=Synchronized_List_in_Java)** | ``Collections.synchronizedList(list)`` | Wraps a list to make it thread‑safe. |
|  | ``CopyOnWriteArrayList`` | Thread‑safe alternative to ArrayList, good for concurrent reads. |

_Natural Order_
```java
List<String> names = new ArrayList<>(List.of("Charlie", "Alice", "Bob"));
names.sort(Comparator.naturalOrder());   // [Alice, Bob, Charlie]
```

_Reverse Order_
```java
names.sort(Comparator.reverseOrder());   // [Charlie, Bob, Alice]
```

_Custom Comparator_
```java 
class Person {
    String name;
    int age;
    Person(String name, int age) { this.name = name; this.age = age; }
    public int getAge() { return age; }
}

List<Person> people = new ArrayList<>();
people.add(new Person("Alice", 25));
people.add(new Person("Bob", 20));
people.add(new Person("Charlie", 30));

people.sort(Comparator.comparingInt(Person::getAge)); // sort by age
```



```java
import java.util.*;

class Person {
    String name;
    int age;
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public int getAge() { return age; }
    public String getName() { return name; }
}

public class MultiSortDemo {
    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 25));
        people.add(new Person("Bob", 20));
        people.add(new Person("Charlie", 30));
        people.add(new Person("Alex", 25));

        // Sort by age, then by name
        people.sort(
            Comparator.comparingInt(Person::getAge)
                      .thenComparing(Person::getName)
        );

        people.forEach(p -> System.out.println(p.name + " - " + p.age));
    }
}
```

_Synchronized_

This ensures that all operations are synchronized, but iteration still requires manual synchronization:
```java
List<String> syncList = Collections.synchronizedList(new LinkedList<>());
synchronized(syncList) {
    for (String s : syncList) {
        System.out.println(s);
    }
}
```
alternates
| **Concurrent Collection** | **Description** |
| --- | --- |
| **CopyOnWriteArrayList** | Thread‑safe variant of ``ArrayList``. Best for frequent reads, infrequent writes. |
| **ConcurrentLinkedQueue** | Non‑blocking, thread‑safe queue based on linked nodes. |
| **ConcurrentLinkedDeque** | Thread‑safe double‑ended queue (similar to LinkedList but concurrent). |
| **BlockingQueue (e.g., LinkedBlockingQueue)** | Thread‑safe queue with blocking operations (``put``, ``take``). |

## ArrayList: Think of it as a resizable array: great for indexing and traversal.

| **Aspect** | **[Array](ca://s?q=Java_Array)** | **[ArrayList](ca://s?q=Java_ArrayList)** |
| --- | --- | --- |
| **Size** | Fixed at creation, cannot grow/shrink | Dynamic, resizes automatically. The resizing strategy is not 50% — it actually grows by ~1.5x (50% increase) each time. Default 10, When full, new capacity = oldCapacity + (oldCapacity >> 1) → 15 |
| **Type** | Can store primitives (``int[]``, ``char[]``) and objects | Can only store objects (``Integer``, ``String``, etc.) |
| **Flexibility** | Limited — must know size in advance | Flexible — can add/remove elements anytime |
| **Performance** | Faster for direct indexed access | Slight overhead due to resizing and wrappers |
| **Methods** | No built‑in methods (manual loops) | Rich API (``add``, ``remove``, ``contains``, ``sort``, etc.) |
| **Memory** | Compact, contiguous block | May allocate extra capacity (default 10, doubles when full) |
| **Iteration** | For loop, enhanced for loop | For loop, enhanced for loop, ``iterator()``, ``forEach()``, ``spliterator()`` |
| **Duplicates** | Allowed | Allowed |
| **Nulls** | Allowed | Allowed |
| **Synchronization** | Not synchronized (manual handling only) | Not synchronized by default, but can be wrapped: ``Collections.synchronizedList(list)`` |
| **Sorting** | Manual sorting with ``Arrays.sort(array)`` | Built‑in: ``list.sort(Comparator)``, ``Collections.sort(list)`` |
| **Use Case** | Best for fixed‑size, primitive data | Best for dynamic collections, frequent insert/delete |

_Ways to convert [] to ArrayList_
| **Approach** | **Code Example** | **Notes** |
| --- | --- | --- |
| **[Arrays.asList](ca://s?q=Arrays_asList_in_Java)** | ``String[] ``arr ``= ``{"a","b","c"}; ``List<String> ``list ``= ``Arrays.asList(arr);`` | Returns a fixed-size list backed by the array (cannot add/remove, but can update). |
| **[ArrayList Constructor](ca://s?q=ArrayList_from_Array_in_Java)** | ``List<String> ``list ``= ``new ``ArrayList<>(Arrays.asList(arr));`` | Creates a mutable ``ArrayList`` copy — you can add/remove freely. |
| **[Collections.addAll](ca://s?q=Collections_addAll_in_Java)** | ``List<String> ``list ``= ``new ``ArrayList<>(); ``Collections.addAll(list, ``arr);`` | Adds all array elements into a new ``ArrayList``. |
| **[Stream API](ca://s?q=Stream_toList_in_Java)** | ``List<String> ``list ``= ``Arrays.stream(arr).collect(Collectors.toList());`` | Flexible, works well with transformations. |
| **[List.of (Java 9+)](ca://s?q=List_of_in_Java)** | ``List<String> ``list ``= ``List.of(arr);`` | Immutable list — cannot add/remove/update. |

## LinkedList: Think of it as a chain of nodes: great for frequent insertions/deletions.
| **Aspect** | **[ArrayList](ca://s?q=ArrayList_in_Java)** | **[LinkedList](ca://s?q=LinkedList_in_Java)** |
|  |  |  |
| **Underlying Data Structure** | Dynamic array | Doubly linked list |
| **Access (get/set)** | Fast random access (``O(1)``) | Slow random access (``O(n)``) |
| **Insertion/Deletion (middle)** | Slow (``O(n)`` due to shifting) | Fast (``O(1)`` if node reference known) |
| **Insertion/Deletion (end)** | Fast (``O(1)`` amortized) | Fast (``O(1)``) |
| **Memory Usage** | Less overhead (just array) | More overhead (extra pointers per node) |
| **Iteration Performance** | Better cache locality, faster traversal | Slightly slower due to pointer chasing |
| **Sorting** | ``list.sort(Comparator)`` or ``Collections.sort(list)`` | Same methods available |
| **Searching** | ``contains``, ``indexOf``, ``binarySearch`` (on sorted list) | Same methods, but slower for index-based search |
| **Synchronization** | Not synchronized; wrap with ``Collections.synchronizedList`` | Not synchronized; wrap similarly |
| **Concurrent Alternatives** | ``CopyOnWriteArrayList`` | ``ConcurrentLinkedDeque``, ``LinkedBlockingQueue`` |
| **Best Use Case** | Frequent random access, fewer insertions/deletions in middle | Frequent insertions/deletions, especially at ends |

LinkedList can be used directly as a FIFO queue because it supports add/offer (enqueue), remove/poll (dequeue), and peek/element (examine head).

# Queue Interface

| **Category** | **Method** | **Description** |
| --- | --- | --- |
| **[Insertion](ca://s?q=Queue_offer_add_in_Java)** | ``add(E ``e)`` | Inserts element; throws exception if capacity restrictions prevent it. |
|  | ``offer(E ``e)`` | Inserts element; returns ``false`` if capacity restrictions prevent it (preferred in queues). |
| **[Removal](ca://s?q=Queue_poll_remove_in_Java)** | ``remove()`` | Removes and returns head; throws exception if empty. |
|  | ``poll()`` | Removes and returns head; returns ``null`` if empty. |
| **[Examination](ca://s?q=Queue_peek_element_in_Java)** | ``element()`` | Retrieves head without removing; throws exception if empty. |
|  | ``peek()`` | Retrieves head without removing; returns ``null`` if empty. |

The difference between add/remove/element vs offer/poll/peek is exception vs safe return:

```java
Queue<Integer> fifo = new LinkedList<>();
fifo.offer(10);
fifo.offer(20);
fifo.offer(30);

System.out.println(fifo.poll()); // 10 (first in, first out)
System.out.println(fifo.poll()); // 20
System.out.println(fifo.poll()); // 30
```

## ArrayDeque

The **ArrayDeque** is one of the most versatile and efficient implementations of the **Deque (double-ended queue)** interface in Java. It’s backed by a **resizable array**, making it faster than `LinkedList` for most queue and stack operations.

### Key Features of ArrayDeque
- **Dynamic resizing** → grows automatically when full.  
- **No capacity limit** (except memory).  
- **Faster than LinkedList** for stack/queue operations due to better cache locality.  
- **Not thread-safe** → must be synchronized externally if used by multiple threads.  
- **Cannot store `null` elements** (throws `NullPointerException`).  


_As a Queue (FIFO)_
```java
Deque<String> queue = new ArrayDeque<>();
queue.offer("A");   // enqueue
queue.offer("B");
queue.offer("C");

System.out.println(queue.poll()); // A (first in, first out)
System.out.println(queue.peek()); // B (next element)
```

_As a Stack (LIFO)_
```java
Deque<String> stack = new ArrayDeque<>();
stack.push("A");   // push
stack.push("B");
stack.push("C");

System.out.println(stack.pop());  // C (last in, first out)
System.out.println(stack.peek()); // B (top element)
```

_Double-Ended Queue_
```java
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(1);  // front
deque.addLast(2);   // back
deque.addLast(3);

System.out.println(deque.removeFirst()); // 1
System.out.println(deque.removeLast());  // 3
```


### Comparison: ArrayDeque vs LinkedList

| **Aspect** | **ArrayDeque** | **LinkedList** |
| --- | --- | --- |
| Underlying Structure | Resizable array | Doubly linked list |
| Performance | Faster (better cache locality) | Slower (pointer chasing) |
| Nulls | Not allowed | Allowed |
| Memory Overhead | Lower | Higher (extra node pointers) |
| Best Use | Queue/Stack operations | Frequent insertions/deletions in middle |


✅ In short:  
- Use **ArrayDeque** when you need a **fast queue or stack**.  
- Use **LinkedList** when you need frequent **insertions/deletions in the middle**.  

# Set

A Set is best used when you need to store a collection of elements with the guarantee that no duplicates will exist. It’s ideal for situations where uniqueness matters more than ordering or indexing.

## When to Use a Set

### Ensure Uniqueness
- When you want to avoid duplicate entries automatically.  
- Example: storing unique usernames, IDs, or email addresses.  
```java
Set<String> usernames = new HashSet<>();
usernames.add("alice");
usernames.add("bob");
usernames.add("alice"); // ignored
System.out.println(usernames); // [alice, bob]
```

### Fast Membership Checks
- `Set.contains()` is optimized (O(1) average for `HashSet`).  
- Example: checking if a word is in a dictionary.  
```java
Set<String> dictionary = new HashSet<>(List.of("cat", "dog", "bird"));
System.out.println(dictionary.contains("dog")); // true
```

### Remove Duplicates from a Collection
- Convert a list to a set to eliminate duplicates.  
```java
List<Integer> numbers = List.of(1, 2, 2, 3, 4, 4);
Set<Integer> unique = new HashSet<>(numbers);
System.out.println(unique); // [1, 2, 3, 4]
```

### Sorted or Ordered Collections
- Use `TreeSet` when you need elements sorted.  
- Use `LinkedHashSet` when you need to preserve insertion order.  
```java
Set<Integer> sorted = new TreeSet<>(List.of(5, 3, 1, 4));
System.out.println(sorted); // [1, 3, 4, 5]
```


### Remove Duplicates from a List and convert it back to List
```java
List<Integer> numbers = List.of(1, 2, 2, 3, 4, 4);

// Remove duplicates
Set<Integer> unique = new HashSet<>(numbers);
System.out.println(unique); // [1, 2, 3, 4]

// Convert back to list
List<Integer> list = new ArrayList<>(unique);
System.out.println(list); // [1, 2, 3, 4]
```

### Conceptual Sense
- **HashSet** → fastest, unordered uniqueness.  
- **LinkedHashSet** → uniqueness + insertion order.  
- **TreeSet** → uniqueness + sorted order.  

Use a **Set** when your main requirement is **uniqueness** and you don’t care about duplicates sneaking in. It’s perfect for IDs, tags, categories, dictionary words, or any collection where repetition is meaningless.  

# How Hash is Calculated

## ### Step 1: hashCode()
- Every Java object inherits a `hashCode()` method from `Object`.  
- Classes like `String`, `Integer`, etc. override it to produce a meaningful integer.  
- Example:  
  ```java
  String s = "Alice";
  int hash = s.hashCode(); // returns an int
  ```



### Step 2: HashMap Processing
- HashMap doesn’t use the raw `hashCode()` directly.  
- It applies a **hash function** to spread values more evenly across buckets.  
- In Java 8+, this is done by mixing the high and low bits of the hash:  
  ```java
  static final int hash(Object key) {
      int h;
      return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
  }
  ```
- This reduces collisions by ensuring better distribution.



### Step 3: Index Calculation
- Once the hash is computed, HashMap decides which **bucket (array index)** to use:  
  ```java
  index = (n - 1) & hash;
  ```
  where `n` = number of buckets (capacity).  
- This is a fast way of doing `hash % n` using bitwise operations.



### 📘 Example Walkthrough
```java
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 90);
```

1. `"Alice".hashCode()` → returns some integer (say 63281940).  
2. HashMap mixes bits → `(h ^ (h >>> 16))`.  
3. Index = `(capacity - 1) & hash`.  
   If capacity = 16, index = 4 → so `"Alice"` goes into bucket 4.  



### Conceptual Sense
- **hashCode()** → raw number from the object.  
- **Hash function** → improves distribution.  
- **Index calculation** → decides which bucket in the array to store the entry.  
- That’s how HashMap achieves **O(1) average lookup time**.  



✅ In short:  
Java calculates a hash by calling `hashCode()`, mixing bits for better distribution, and then mapping it to a bucket index using bitwise operations.  


# Map

The **Map** interface in Java represents a collection of **key–value pairs** where each key maps to exactly one value. Unlike `List` or `Set`, a `Map` is not a subtype of `Collection` — it’s its own hierarchy.

### Key Features of Map
- Stores data as **key → value** pairs.  
- Keys are **unique**; values can be duplicated.  
- A key can map to only one value (latest put overwrites).  
- Allows fast lookups by key.  

### Common Implementations

| **Implementation** | **Ordering** | **Performance** | **Notes** |
|---------------------|--------------|-----------------|-----------|
| **HashMap** | No order | O(1) average for get/put | Most commonly used |
| **LinkedHashMap** | Maintains insertion order | Slightly slower | Useful when order matters |
| **TreeMap** | Sorted by keys | O(log n) operations | Backed by Red‑Black tree |
| **Hashtable** | No order | Legacy, synchronized | Rarely used now |

### Common Methods of Map
- `put(K key, V value)` → adds or updates a mapping.  
- `get(Object key)` → retrieves value for a key.  
- `remove(Object key)` → removes mapping.  
- `containsKey(Object key)` → checks if key exists.  
- `containsValue(Object value)` → checks if value exists.  
- `size()` → number of mappings.  
- `isEmpty()` → checks if empty.  
- `clear()` → removes all mappings.  
- `keySet()` → returns all keys.  
- `values()` → returns all values.  
- `entrySet()` → returns all key–value pairs.  

### Sample Usage

```java
import java.util.*;

public class MapDemo {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();

        // Add mappings
        scores.put("Alice", 90);
        scores.put("Bob", 85);
        scores.put("Charlie", 92);

        // Retrieve
        System.out.println(scores.get("Alice")); // 90

        // Check existence
        System.out.println(scores.containsKey("Bob")); // true

        // Iterate
        for (Map.Entry<String, Integer> entry : scores.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```

### Conceptual Sense
- **Map** → dictionary or phonebook analogy: key = name, value = number.  
- **HashMap** → fastest, unordered.  
- **LinkedHashMap** → preserves insertion order.  
- **TreeMap** → keeps keys sorted.  

✅ In short:  
Use a **Map** when you need **fast lookups by key** and want to associate values with unique identifiers.  

## Map.Entry

Exactly — you’ve captured the essence. Let’s expand the **conceptual sense** of `Map.Entry` so it’s crystal clear:

### Conceptual Sense of `Map.Entry`

- **Unit of a Map** → A `Map.Entry<K,V>` is the smallest building block of a `Map`. Each entry is a **single object** that bundles together a **key** and its **associated value**.  
- **Set of Entries** → When you call `map.entrySet()`, you get a `Set<Map.Entry<K,V>>`. Each element of that set is one entry object, not just a raw key or value.  
- **Uniqueness** → The `Set` ensures no duplicate entries. Since keys in a map are unique, each `Map.Entry` is unique by its key.  
- **Two-in-One** → Conceptually, think of it as a **pair container**:  
  - `getKey()` → the identifier.  
  - `getValue()` → the data associated with that identifier.  
- **Mutability** → You can update the value directly via `entry.setValue(newValue)`, which changes the mapping inside the map itself.  

### Example in Action
```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
scores.put("Bob", 85);

for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
}
```

### Output:
```
Key: Alice, Value: 90
Key: Bob, Value: 85
```


### Analogy
Think of a **Map.Entry** like a **row in a table**:
- **Key** → the primary column (unique identifier).  
- **Value** → the data stored in that row.  
- The `entrySet()` is like the entire table, but represented as a `Set` of rows.  



✅ In short:  
`Map.Entry` is the **conceptual representation of one mapping** inside a map. It’s the way Java lets you work with both the key and the value together, rather than separately.  

## TreeMap

The **TreeMap** is a `Map` implementation in Java that stores key–value pairs in **sorted order**. It’s backed by a **Red‑Black tree**, which guarantees log‑time performance for basic operations.



### Key Features of TreeMap
- **Sorted by keys** (natural order or custom `Comparator`).  
- **Unique keys** (like all maps).  
- **NavigableMap** implementation → supports navigation methods like `higherKey`, `lowerKey`, `ceilingEntry`, etc.  
- **Logarithmic performance** → O(log n) for `put`, `get`, `remove`.  
- **Null keys not allowed** (throws `NullPointerException`), but null values are permitted.  



### Common Methods

| **Method** | **Description** |
|------------|-----------------|
| **put** | Adds or updates a key–value pair. |
| **get** | Retrieves value by key. |
| **remove** | Removes mapping for a key. |
| **firstKey** | Returns the lowest key. |
| **lastKey** | Returns the highest key. |
| **higherKey** | Returns the least key strictly greater than given key. |
| **lowerKey** | Returns the greatest key strictly less than given key. |
| **ceilingEntry** | Returns entry ≥ given key. |
| **floorEntry** | Returns entry ≤ given key. |



### Example Usage

```java
import java.util.*;

public class TreeMapDemo {
    public static void main(String[] args) {
        TreeMap<String, Integer> scores = new TreeMap<>();

        scores.put("Charlie", 92);
        scores.put("Alice", 90);
        scores.put("Bob", 85);

        System.out.println(scores); 
        // {Alice=90, Bob=85, Charlie=92} (sorted by keys)

        System.out.println(scores.firstKey());   // Alice
        System.out.println(scores.lastKey());    // Charlie
        System.out.println(scores.higherKey("Alice")); // Bob
    }
}
```



### Conceptual Sense
- Think of **TreeMap** as a **dictionary that is always sorted by keys**.  
- Perfect when you need **range queries**, ordered traversal, or nearest‑key lookups.  
- Unlike `HashMap` (fast but unordered), `TreeMap` trades a bit of speed for **ordering guarantees**.  



✅ In short:  
Use a **TreeMap** when you need a **sorted map** or want to perform **range queries** efficiently.  

HashMap to TreeMap
```java
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("Charlie", 92);
hashMap.put("Alice", 90);
hashMap.put("Bob", 85);

// Sort keys in reverse order
Map<String, Integer> treeMap = new TreeMap<>(Comparator.reverseOrder());
treeMap.putAll(hashMap);

System.out.println(treeMap); 
// {Charlie=92, Bob=85, Alice=90}
```
