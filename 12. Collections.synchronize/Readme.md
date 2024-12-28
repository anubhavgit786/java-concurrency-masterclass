# Understanding `Collections.synchronized*` in Java

The `Collections.synchronized*` methods in Java are utility methods provided in the `java.util.Collections` class. They are used to create thread-safe wrappers around non-thread-safe collections such as `ArrayList`, `HashMap`, etc. These wrappers ensure that all the method calls on the collection are synchronized, making them suitable for multi-threaded environments.

## Key Concepts

- **Thread Safety**: The `Collections.synchronized*` methods synchronize access to the collection's methods, ensuring that only one thread can access them at a time.
- **Synchronized Blocks**: Iterating over a synchronized collection requires explicit synchronization to avoid `ConcurrentModificationException`.

### Example Code

Below is an example demonstrating the use of `Collections.synchronizedList` to make an `ArrayList` thread-safe:

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class SynchronisedCollections 
{
    public static void main(String[] args)
    {
        List<Integer> list = Collections.synchronizedList(new ArrayList<>());
        
        Thread thread1 = new Thread(() ->
        {
            for (int i = 0; i < 1000; i++)
            {
                list.add(i);
            }
        });
        
        Thread thread2 = new Thread(() ->
        {
            for (int i = 0; i < 1000; i++)
            {
                list.add(i);
            }
        });
        
        thread1.start();
        thread2.start();
        
        try 
        {
            thread1.join();
            thread2.join();
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }
        
        System.out.println("The size : " + list.size());
    }
}
```

### Output

The output will consistently be:
```
The size : 2000
```

This demonstrates how `Collections.synchronizedList` ensures thread-safe modifications.

---

## Drawbacks of `Collections.synchronized*`

1. **Coarse-Grained Synchronization**: The synchronization happens at the method level, which can lead to contention and reduced performance in highly concurrent applications.

2. **Iteration Needs Manual Synchronization**: While the collection methods are synchronized, iterating over the collection still requires external synchronization. For example:

    ```java
    synchronized(list) {
        for (Integer i : list) {
            System.out.println(i);
        }
    }
    ```

    Without this, a `ConcurrentModificationException` can occur if another thread modifies the collection during iteration.

3. **Limited Scalability**: Since the synchronization is achieved using intrinsic locks, it can cause bottlenecks in applications with a high degree of concurrency.

4. **Not Suitable for All Use Cases**: For fine-grained concurrency or advanced use cases, concurrent collections from the `java.util.concurrent` package (e.g., `CopyOnWriteArrayList`, `ConcurrentHashMap`) are more suitable.

---

## Alternatives

For better performance and scalability in concurrent environments, consider using the collections from the `java.util.concurrent` package:

- **`CopyOnWriteArrayList`**: Suitable for collections that are read frequently but modified rarely.
- **`ConcurrentHashMap`**: Provides thread-safe access to key-value mappings with better scalability.
- **`BlockingQueue`**: Useful for producer-consumer scenarios.

---

By understanding the limitations of `Collections.synchronized*`, developers can make informed decisions on whether or not it is the right choice for their application's concurrency requirements.
