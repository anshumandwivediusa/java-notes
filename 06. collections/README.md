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
|--------|-------------|
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
| --- | --- | --- |
| **CRUD (Modify)** | ``boolean ``add(E ``e)`` | Adds an element to the collection. |
|  | ``boolean ``addAll(Collection<? ``extends ``E> ``c)`` | Adds all elements from another collection. |
|  | ``boolean ``remove(Object ``o)`` | Removes a single instance of the specified element. |
|  | ``boolean ``removeAll(Collection<?> ``c)`` | Removes all elements that are also in another collection. |
|  | ``boolean ``retainAll(Collection<?> ``c)`` | Retains only elements present in another collection. |
|  | ``void ``clear()`` | Removes all elements. |
| **Query (Check)** | ``boolean ``contains(Object ``o)`` | Checks if the collection contains the given element. |
|  | ``boolean ``containsAll(Collection<?> ``c)`` | Checks if the collection contains all elements of another collection. |
|  | ``boolean ``isEmpty()`` | Checks if the collection is empty. |
|  | ``int ``size()`` | Returns the number of elements. |
| **Conversion** | ``Object[] ``toArray()`` | Returns an array containing all elements. |
|  | ``<T> ``T[] ``toArray(T[] ``a)`` | Returns an array containing all elements in the specified type. |
| **Iteration** | ``Iterator<E> ``iterator()`` | Returns an iterator over elements. |
|  | ``default ``void ``forEach(Consumer<? ``super ``E> ``action)`` | Performs the given action for each element. |
|  | ``default ``Spliterator<E> ``spliterator()`` | Returns a Spliterator for parallel iteration. |
| **Identity (Object)** | ``boolean ``equals(Object ``o)`` | Compares this collection with another for equality. |
|  | ``int ``hashCode()`` | Returns hash code for the collection. |


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

## ArrayList

| **Aspect** | **[Array](ca://s?q=Java_Array)** | **[ArrayList](ca://s?q=Java_ArrayList)** |
| --- | --- | --- |
| **Size** | Fixed at creation, cannot grow/shrink | Dynamic, resizes automatically |
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
