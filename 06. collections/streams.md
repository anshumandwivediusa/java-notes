# Streams in Java

## 1. Definition
 - A Stream\<T\> is a sequence of elements supporting sequential and parallel aggregate operations.
 - Not a data structure → does not store elements.
 - Wraps a data source (collection, array, generator, I/O channel).
 - Enables declarative, functional‑style operations on data.

## 2. **Stream Pipeline**
A stream pipeline has **three parts**:

1. **Source** → Supplies elements (collection, array, generator, I/O channel).  
2. **Intermediate operations** → Zero or more **lazy transformations** (`filter`, `map`, `sorted`, `distinct`, `flatMap`).  
   - Always return another stream.  
   - Do not execute immediately.  
3. **Terminal operation** → Exactly one **eager operation** (`collect`, `forEach`, `reduce`, `count`, `findAny`).  
   - Triggers execution of the pipeline.  
   - Produces a result or side effect.

### **Key Properties**
- **Lazy evaluation** → Intermediate operations are not executed until a terminal operation is invoked.  
- **Single‑use** → Once a terminal operation runs, the stream is consumed and cannot be reused.  
- **Parallel support** → Streams can run sequentially or in parallel (`parallelStream()`).

### Example
```java
import java.util.*;
import java.util.stream.*;

public class StreamDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Anshuman", "Ravi", "Amit", "Anshuman");

        long count = names.stream()
                          .filter(n -> n.startsWith("A"))   // Intermediate
                          .distinct()                       // Intermediate
                          .count();                         // Terminal

        System.out.println("Count = " + count);
    }
}
```

**Output:**
```
Count = 2
```

## Foundations
### Ways to create Collections
#### List Creation
| **Method** | **Description** | **Example** |
| --- | --- | --- |
| **[Arrays.asList()](ca://s?q=Java_Arrays_asList)** | Fixed‑size list backed by array. Cannot add/remove, but can update elements. | ``List<String> ``names ``= ``Arrays.asList("Anshuman","Ravi","Amit","Anshuman");`` |
| **[new ArrayList<>(Arrays.asList())](ca://s?q=Java_ArrayList_from_Arrays_asList)** | Fully mutable list (add/remove allowed). | ``List<String> ``names ``= ``new ``ArrayList<>(Arrays.asList("Anshuman","Ravi","Amit","Anshuman"));`` |
| **[List.of()](ca://s?q=Java_List_of)** (Java 9+) | Immutable list (cannot add/remove/update). | ``List<String> ``names ``= ``List.of("Anshuman","Ravi","Amit","Anshuman");`` |
| **[Collections.unmodifiableList()](ca://s?q=Java_Collections_unmodifiableList)** | Wraps a mutable list but makes it read‑only. | ``List<String> ``names ``= ``Collections.unmodifiableList(new ``ArrayList<>(Arrays.asList("Anshuman","Ravi","Amit","Anshuman")));`` |
| **[Stream API](ca://s?q=Java_Stream_toList)** | Build list from stream operations. | ``List<String> ``names ``= ``Stream.of("Anshuman","Ravi","Amit","Anshuman").toList();`` |
| **[Manual add()](ca://s?q=Java_ArrayList_add)** | Create empty list and add elements one by one. | ``List<String> ``names ``= ``new ``ArrayList<>(); ``names.add("Anshuman"); ``names.add("Ravi"); ``...`` |

| **Method** | **Description** | **Example** |
| --- | --- | --- |
| **[new LinkedList<>()](ca://s?q=Java_new_LinkedList)** | Empty linked list, fully mutable (add/remove/update allowed). | ``List<String> ``names ``= ``new ``LinkedList<>(); ``names.add("Anshuman"); ``names.add("Ravi");`` |
| **[new LinkedList<>(Arrays.asList())](ca://s?q=Java_LinkedList_from_Arrays_asList)** | Wraps an array into a linked list, fully mutable. | ``List<String> ``names ``= ``new ``LinkedList<>(Arrays.asList("Anshuman","Ravi","Amit","Anshuman"));`` |
| **[new LinkedList<>(List.of())](ca://s?q=Java_LinkedList_from_List_of)** (Java 9+) | Creates a mutable linked list from an immutable source. | ``List<String> ``names ``= ``new ``LinkedList<>(List.of("Anshuman","Ravi","Amit","Anshuman"));`` |
| **[Collections.unmodifiableList(new LinkedList<>())](ca://s?q=Java_unmodifiable_LinkedList)** | Wraps a linked list but makes it read‑only. | ``List<String> ``names ``= ``Collections.unmodifiableList(new ``LinkedList<>(Arrays.asList("Anshuman","Ravi","Amit")));`` |
| **[Stream API → collect(Collectors.toCollection(LinkedList::new))](ca://s?q=Java_Stream_collect_LinkedList)** | Build a linked list directly from stream operations. | ``List<String> ``names ``= ``Stream.of("Anshuman","Ravi","Amit").collect(Collectors.toCollection(LinkedList::new));`` |


| **Method** | **Description** | **Example** |
| --- | --- | --- |
| **[Collection.stream()](ca://s?q=Java_Stream_from_collection)** | From a collection (List, Set, etc.) | ``list.stream()`` |
| **[Collection.parallelStream()](ca://s?q=Java_parallelStream_from_collection)** | Parallel stream from a collection | ``list.parallelStream()`` |
| **[Arrays.stream()](ca://s?q=Java_Stream_from_array)** | From an array | ``Arrays.stream(new ``int[]{1,2,3})`` |
| **[Stream.of()](ca://s?q=Java_Stream_of)** | From specified values | ``Stream.of("Anshuman","Ravi","Amit")`` |
| **[Stream.empty()](ca://s?q=Java_Stream_empty)** | Creates an empty stream | ``Stream<String> ``empty ``= ``Stream.empty();`` |
| **[Stream.generate()](ca://s?q=Java_Stream_generate)** | Infinite stream from a supplier | ``Stream.generate(Math::random)`` |
| **[Stream.iterate()](ca://s?q=Java_Stream_iterate)** | Infinite stream from seed + function | ``Stream.iterate(1, ``n ``-> ``n+1)`` |
| **[Files.lines()](ca://s?q=Java_Stream_from_file_lines)** | Stream of lines from a file | ``Files.lines(Paths.get("data.txt"))`` |
| **[Pattern.splitAsStream()](ca://s?q=Java_Stream_from_pattern)** | Stream from regex split | ``Pattern.compile(",").splitAsStream("A,B,C")`` |
| **[Stream.builder()](ca://s?q=Java_Stream_builder)** | Build stream manually | ``Stream.<String>builder().add("A").add("B").build()`` |
| **[IntStream.range()](ca://s?q=Java_IntStream_range)** | Range of ints | ``IntStream.range(1,5)`` → 1,2,3,4 |
| **[IntStream.rangeClosed()](ca://s?q=Java_IntStream_rangeClosed)** | Inclusive range of ints | ``IntStream.rangeClosed(1,5)`` → 1,2,3,4,5 |
