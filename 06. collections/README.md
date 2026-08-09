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

## SplitIterator

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

Would you like me to also show a **comparison table of `Iterable` vs `Iterator` vs `Collection`** so you can quickly recall their differences?
