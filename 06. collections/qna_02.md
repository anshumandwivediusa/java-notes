Here’s a **Java Collections Interview Cheat Sheet with 50 concise Q&A** — one‑liners or two‑liners, perfect for rapid revision:


## 📘 Core Concepts
1. **What is the Collection interface?** → Root interface for groups of objects.  
2. **Difference between Collection and Collections?** → Interface vs utility class.  
3. **Is Map part of Collection?** → No, Map is separate.  
4. **What is Iterable?** → Root for iteration, extended by Collection.  
5. **Fail-fast vs Fail-safe iterators?** → Fail-fast throws exception, fail-safe works on copy.  


## 📘 List
6. **ArrayList vs LinkedList?** → ArrayList = fast random access, LinkedList = fast insert/delete.  
7. **Vector vs ArrayList?** → Vector synchronized, ArrayList not.  
8. **Stack in Java?** → Legacy, extends Vector, LIFO.  
9. **CopyOnWriteArrayList?** → Thread-safe list, uses copy on write.  
10. **Difference between ListIterator and Iterator?** → ListIterator supports bidirectional traversal.  


## 📘 Set
11. **HashSet vs TreeSet?** → HashSet unordered, TreeSet sorted.  
12. **LinkedHashSet?** → Maintains insertion order.  
13. **EnumSet?** → Specialized set for enums.  
14. **CopyOnWriteArraySet?** → Thread-safe set, backed by CopyOnWriteArrayList.  
15. **Is HashSet synchronized?** → No, must wrap or use concurrent sets.  


## 📘 Map
16. **HashMap vs Hashtable?** → HashMap unsynchronized, Hashtable synchronized.  
17. **HashMap vs TreeMap?** → HashMap unordered, TreeMap sorted.  
18. **LinkedHashMap?** → Maintains insertion order.  
19. **ConcurrentHashMap?** → Thread-safe, lock-striping.  
20. **WeakHashMap?** → Keys garbage-collected when weakly referenced.  


## 📘 Queue & Deque
21. **PriorityQueue?** → Orders elements by priority.  
22. **ArrayDeque?** → Resizable array, double-ended queue.  
23. **BlockingQueue?** → Thread-safe queue for producer-consumer.  
24. **ConcurrentLinkedQueue?** → Lock-free queue for concurrency.  
25. **Deque vs Queue?** → Deque supports both ends, Queue only one.  


## 📘 Sorting & Ordering
26. **Comparable vs Comparator?** → Comparable = natural order, Comparator = custom order.  
27. **Collections.sort()?** → Sorts list using natural/comparator order.  
28. **Arrays.sort()?** → Sorts arrays.  
29. **TreeMap ordering?** → Sorted by keys.  
30. **TreeSet ordering?** → Sorted by elements.  


## 📘 Synchronization
31. **Collections.synchronizedList()?** → Wraps list with synchronized access.  
32. **Collections.synchronizedSet()?** → Wraps set with synchronized access.  
33. **Collections.synchronizedMap()?** → Wraps map with synchronized access.  
34. **ConcurrentSkipListSet?** → Thread-safe sorted set.  
35. **ConcurrentSkipListMap?** → Thread-safe sorted map.  


## 📘 Legacy
36. **Hashtable?** → Legacy synchronized map.  
37. **Vector?** → Legacy synchronized list.  
38. **Stack?** → Legacy LIFO stack.  
39. **Enumeration?** → Legacy iterator.  
40. **Properties class?** → Legacy key-value pairs for config.  


## 📘 Miscellaneous
41. **IdentityHashMap?** → Compares keys by reference, not equals().  
42. **NavigableMap?** → Extends SortedMap with navigation methods.  
43. **NavigableSet?** → Extends SortedSet with navigation methods.  
44. **Unmodifiable collections?** → Created via `Collections.unmodifiableX()`.  
45. **Singleton collections?** → Created via `Collections.singleton()`.  


## 📘 Performance & Internals
46. **HashMap load factor?** → Default 0.75, triggers rehashing.  
47. **Initial capacity of HashMap?** → Default 16.  
48. **Time complexity of HashSet add()?** → Average O(1).  
49. **Time complexity of TreeSet add()?** → O(log n).  
50. **ConcurrentHashMap concurrency level?** → Default 16 (Java 7), replaced by finer-grained locks in Java 8+.  


## 🎯 Quick Strategy
- Always mention **ordering, uniqueness, synchronization**.  
- Highlight **time complexity**.  
- Show awareness of **legacy vs modern alternatives**.  
