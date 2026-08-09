# Advanced & Internal Mechanics

### **Q1: How does HashMap store key–value pairs?**
**A:** HashMap uses **hashing** to place entries into buckets. Each key’s `hashCode()` decides the bucket, and inside the bucket, `equals()` ensures uniqueness.


### **Q2: How are collisions handled in HashMap?**
**A:** In Java 7, collisions were managed with **linked lists**. In Java 8+, if too many entries collide in one bucket, it switches to a **red‑black tree** for faster lookups.


### **Q3: What is a load factor in HashMap?**
**A:** The load factor (default 0.75) determines when HashMap resizes. Once 75% of buckets are filled, it expands to reduce collisions.


### **Q4: What is rehashing in HashMap?**
**A:** Rehashing happens when capacity doubles. All existing entries are redistributed into new buckets based on their hash values.


### **Q5: Why must equals() and hashCode() be implemented correctly for keys?**
**A:** Because `hashCode()` decides the bucket, and `equals()` checks for exact matches. Incorrect implementation can cause duplicate keys or lost entries.


### **Q6: What is ConcurrentModificationException?**
**A:** It’s thrown when a collection is modified while iterating with a **fail‑fast iterator** (e.g., ArrayList, HashMap). Fail‑safe iterators (e.g., ConcurrentHashMap) avoid this by iterating over a copy.


### **Q7: Difference between fail‑fast and fail‑safe iterators?**
**A:**  
- **Fail‑fast** → throws exception immediately if structure changes.  
- **Fail‑safe** → works on a snapshot copy, so no exception is thrown.


### **Q8: What is IdentityHashMap?**
**A:** It compares keys using `==` instead of `equals()`. Useful when object identity matters more than logical equality (e.g., reference tracking).


### **Q9: What is WeakHashMap?**
**A:** Keys are weakly referenced. If no strong reference exists, GC removes the entry. Commonly used for caches.


### **Q10: How does LinkedHashMap maintain order?**
**A:** By default, it maintains **insertion order**. It can also be configured for **access order**, making it ideal for LRU cache implementations.


### **Q11: How does TreeMap work internally?**
**A:** TreeMap is backed by a **red‑black tree**, ensuring O(log n) performance for `get`, `put`, and `remove`. It also supports navigable methods like `higherKey`, `floorEntry`, and `subMap`.


### **Q12: What is CopyOnWriteArrayList?**
**A:** It creates a new copy of the array on every modification. Iterators never throw `ConcurrentModificationException`. Best for read‑heavy, write‑rare scenarios.


are a **rapid‑fire quiz version (short questions only, no answers)** so you can self‑test before interviews?
