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

| **Operation** | **Purpose** | **Example** |
| --- | --- | --- |
| **[filter(Predicate)](ca://s?q=Java_Stream_filter)** | Select elements that match a condition | ``list.stream().filter(x ``-> ``x ``> ``10)`` |
| **[map(Function)](ca://s?q=Java_Stream_map)** | Transform each element into another form | ``list.stream().map(String::toUpperCase)`` |
| **[flatMap(Function)](ca://s?q=Java_Stream_flatMap)** | Flatten nested streams into a single stream | ``list.stream().flatMap(List::stream)`` |
| **[sorted()](ca://s?q=Java_Stream_sorted)** | Sort elements in natural or custom order | ``list.stream().sorted()`` |
| **[distinct()](ca://s?q=Java_Stream_distinct)** | Remove duplicate elements | ``list.stream().distinct()`` |
| **[peek()](ca://s?q=Java_Stream_peek)** | Debug/logging without modifying elements | ``list.stream().peek(System.out::println)`` |

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
