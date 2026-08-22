# java.util.concurrent

**The `java.util.concurrent` package provides powerful utilities for managing multithreading in Java — including thread pools, concurrent collections, synchronization tools, and atomic variables — making concurrent programming safer, faster, and easier.** It’s the backbone for building scalable applications without manually handling low-level thread management.  [docs.oracle.com](https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/util/concurrent/package-summary.html)  [docs.oracle.com](https://docs.oracle.com/javase/8/docs/technotes/guides/concurrency/overview.html)  

### Key Components of `java.util.concurrent`

### Executors
- **Executor / ExecutorService** → framework for running tasks asynchronously.  
- **ScheduledExecutorService** → supports delayed and periodic execution.  
- **ForkJoinPool** → optimized for parallelism using work-stealing.  
- **Future / Callable** → handle asynchronous results.  


### Concurrent Collections
- **ConcurrentHashMap** → thread-safe hash map with high concurrency.  
- **ConcurrentLinkedQueue / ConcurrentLinkedDeque** → non-blocking FIFO/LIFO queues.  
- **BlockingQueue** implementations:  
  - `LinkedBlockingQueue`, `ArrayBlockingQueue`, `PriorityBlockingQueue`, `DelayQueue`, `SynchronousQueue`.  
- **CopyOnWriteArrayList / CopyOnWriteArraySet** → safe for iteration during modification.  



### Synchronizers
- **CountDownLatch** → waits until a set of operations complete.  
- **CyclicBarrier** → allows threads to wait for each other at a common barrier.  
- **Semaphore** → controls access to resources.  
- **Exchanger** → threads swap data.  
- **Phaser** → advanced barrier for dynamic parties.  



### Atomic Variables
- Classes like `AtomicInteger`, `AtomicLong`, `AtomicReference`.  
- Provide lock-free thread-safe operations using CAS (compare-and-swap).  

```java
ExecutorService executor = Executors.newFixedThreadPool(3);

Future<Integer> future = executor.submit(() -> {
    // Simulate computation
    Thread.sleep(1000);
    return 42;
});

System.out.println("Result: " + future.get()); // waits for completion
executor.shutdown();
```

### Why Use `java.util.concurrent`?
- **Performance** → optimized by experts, faster than custom thread handling.  
- **Safety** → reduces risks of deadlocks, race conditions, and starvation.  
- **Maintainability** → standardized APIs make code easier to read and debug.  
- **Scalability** → supports parallelism and high-throughput designs.  [docs.oracle.com](https://docs.oracle.com/javase/8/docs/technotes/guides/concurrency/overview.html)  



### Conceptual Sense
- Think of `java.util.concurrent` as a **toolbox for multithreading**:  
  - **Executors** → manage tasks.  
  - **Collections** → safely share data.  
  - **Synchronizers** → coordinate threads.  
  - **Atomics** → perform lock-free updates.  



✅ In short:  
Use `java.util.concurrent` whenever you need **safe, efficient, and scalable multithreading** — from simple background tasks to complex parallel computations.  

## ExecutorService

The **ExecutorService** is part of the `java.util.concurrent` package and provides a **high-level framework for managing threads**. Instead of manually creating and starting threads, you submit tasks to an `ExecutorService`, which handles scheduling, execution, and lifecycle management for you.



### Key Features of ExecutorService
- **Thread Pool Management** → reuses a fixed number of threads for multiple tasks.  
- **Task Submission** → supports `Runnable` (no result) and `Callable` (returns result).  
- **Future Objects** → represent pending results of asynchronous computations.  
- **Graceful Shutdown** → allows orderly termination of threads.  
- **Scalability** → optimized for handling large numbers of short-lived tasks.  



### Common Methods

| **Method** | **Description** |
|------------|-----------------|
| **submit** | Submits a task (`Runnable` or `Callable`) for execution, returns a `Future`. |
| **invokeAll** | Executes a collection of tasks, waits for all to finish. |
| **invokeAny** | Executes tasks, returns result of one that completes first. |
| **shutdown** | Initiates an orderly shutdown (no new tasks accepted). |
| **shutdownNow** | Attempts to stop all actively executing tasks immediately. |
| **isShutdown** | Checks if shutdown has been initiated. |
| **isTerminated** | Checks if all tasks have completed after shutdown. |



```java
import java.util.concurrent.*;

public class ExecutorDemo {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        // Runnable task (no result)
        executor.submit(() -> System.out.println("Task 1 executed"));

        // Callable task (returns result)
        Future<Integer> future = executor.submit(() -> {
            Thread.sleep(1000);
            return 42;
        });

        System.out.println("Result: " + future.get()); // waits for completion

        executor.shutdown();
    }
}
```



### Conceptual Sense
- **Without ExecutorService** → you manually create threads (`new Thread(...)`).  
- **With ExecutorService** → you submit tasks, and the framework manages threads efficiently.  
- Think of it as a **task manager**: you hand over jobs, it decides which worker thread executes them.  



✅ In short:  
Use **ExecutorService** when you want to **run tasks asynchronously** without worrying about thread creation, scheduling, or cleanup. It’s the go-to tool for scalable multithreaded applications.  

## Future & Callable

**Future** and **Callable** are two key pieces of the `java.util.concurrent` framework that work hand‑in‑hand to handle asynchronous tasks and their results.



### Callable
- Similar to `Runnable`, but **returns a result** and can throw exceptions.  
- Defined as:
```java
public interface Callable<V> {
    V call() throws Exception;
}
```
- Used when you want a task to **produce a value**.  
- Example:
```java
Callable<Integer> task = () -> {
    Thread.sleep(1000);
    return 42;
};
```



### Future
- Represents the **result of an asynchronous computation**.  
- Returned when you submit a `Callable` (or `Runnable`) to an `ExecutorService`.  
- Provides methods to check status, wait for completion, and retrieve the result.  

### Common Methods:
- `get()` → waits until task completes, returns result.  
- `get(timeout, unit)` → waits up to given time.  
- `isDone()` → checks if task finished.  
- `cancel(boolean mayInterrupt)` → attempts to cancel task.  
- `isCancelled()` → checks if task was cancelled.  



Example: Callable + Future

```java
import java.util.concurrent.*;

public class FutureCallableDemo {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Callable<Integer> task = () -> {
            Thread.sleep(1000);
            return 42;
        };

        Future<Integer> future = executor.submit(task);

        System.out.println("Task submitted...");

        // Wait for result
        Integer result = future.get(); 
        System.out.println("Result: " + result);

        executor.shutdown();
    }
}
```

### Output:
```
Task submitted...
Result: 42
```



### Conceptual Sense
- **Callable** → defines the work (like a worker who promises to deliver something).  
- **Future** → represents the promise of that result (like a receipt you hold until the worker finishes).  
- Together, they let you **submit tasks asynchronously and retrieve results later** without blocking the main thread unnecessarily.  



✅ In short:  
- Use **Callable** when you need a task to **return a value**.  
- Use **Future** to **track and retrieve that value** once the task completes.  




# Concurrent vs Synchronized Collections

| Aspect | **Concurrent Collections** | **Synchronized Collections** |
| --- | --- | --- |
| **Definition** | Collections designed for multi‑threaded access with **non‑blocking / fine‑grained locking** | Legacy collections wrapped with **synchronized methods** |
| **Examples** | ConcurrentHashMap, ConcurrentLinkedQueue, CopyOnWriteArrayList | Vector, Hashtable, Collections.synchronizedList(), Collections.synchronizedMap() |
| **Performance** | High throughput, scalable under heavy concurrency | Lower throughput due to coarse‑grained locking |
| **Iteration** | Fail‑safe (works on snapshot, no ``ConcurrentModificationException``) | Fail‑fast (throws ``ConcurrentModificationException`` if modified during iteration) |
| **Locking** | Lock‑striping, CAS (Compare‑And‑Swap), copy‑on‑write | Entire collection locked for each operation |
| **Use Case** | Modern concurrent apps, scalable multi‑threaded systems | Simpler synchronization, legacy codebases |

| Collection | Type | Key Features | Best Use Case |
| --- | --- | --- | --- |
| **[ConcurrentHashMap](ca://s?q=Java_ConcurrentHashMap)** | Map | Thread‑safe hash map, high concurrency, lock‑striping | Shared caches, fast concurrent access |
| **[ConcurrentLinkedQueue](ca://s?q=Java_ConcurrentLinkedQueue)** | Queue | Non‑blocking FIFO queue, lock‑free | Producer‑consumer, task scheduling |
| **[ConcurrentLinkedDeque](ca://s?q=Java_ConcurrentLinkedDeque)** | Deque | Non‑blocking double‑ended queue | Work stealing, parallel task distribution |
| **[LinkedBlockingQueue](ca://s?q=Java_LinkedBlockingQueue)** | BlockingQueue | Linked nodes, bounded/unbounded, blocking put/take | Producer‑consumer with capacity control |
| **[ArrayBlockingQueue](ca://s?q=Java_ArrayBlockingQueue)** | BlockingQueue | Fixed‑size array, bounded, blocking | Bounded buffer, predictable capacity |
| **[PriorityBlockingQueue](ca://s?q=Java_PriorityBlockingQueue)** | BlockingQueue | Unbounded, ordered by priority | Task scheduling with priorities |
| **[DelayQueue](ca://s?q=Java_DelayQueue)** | BlockingQueue | Elements available only after delay | Scheduled tasks, time‑based caching |
| **[SynchronousQueue](ca://s?q=Java_SynchronousQueue)** | BlockingQueue | No capacity, each insert waits for remove | Direct handoff between threads |
| **[CopyOnWriteArrayList](ca://s?q=Java_CopyOnWriteArrayList)** | List | Thread‑safe, snapshot iterators, copy on write | Read‑heavy, safe concurrent iteration |
| **[CopyOnWriteArraySet](ca://s?q=Java_CopyOnWriteArraySet)** | Set | Thread‑safe, backed by CopyOnWriteArrayList | Read‑heavy sets, safe iteration |
