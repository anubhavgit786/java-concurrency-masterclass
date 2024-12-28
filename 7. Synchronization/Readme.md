# Synchronized Method Locking in Java

In Java, the concept of synchronized methods allows for thread-safe access to shared resources or data. A synchronized method is one where only one thread can execute the method at any given time. If multiple threads try to access the synchronized method concurrently, one thread is allowed to execute, while the others are blocked until the executing thread finishes. This mechanism is important to avoid conflicts or data inconsistency in multithreaded programs.

## Concept of Synchronized Method Locking

A synchronized method in Java uses a mechanism called "monitor" or "lock." When a method is marked with the `synchronized` keyword, it ensures that only one thread can execute that method at a time. If thread A is executing the synchronized method, thread B is not allowed to enter any synchronized method on the same object until thread A finishes.

In simpler terms, if we imagine the synchronized methods as doors, the first thread that enters the synchronized method locks the door. All other threads attempting to enter any synchronized method on the same object are blocked, as if all the doors were locked at the same time. This prevents any concurrent access that could lead to inconsistent data or race conditions.

### Key Points:
- The `synchronized` keyword can be applied to methods to ensure thread safety.
- Only one thread can enter a synchronized method of a particular object at a time.
- The locking is object-based. If one thread locks the object, other threads trying to access any synchronized method on the same object are blocked.

## Example Code

In this example, we have a `Counter` class with synchronized methods for incrementing and decrementing a count. The `increment` and `decrement` methods are synchronized to ensure that only one thread can modify the count at any given time.

### Code:

```java
public class Counter 
{
    private int count;
    
    public Counter(int count)
    {
        this.count = count;
    }
    
    public synchronized void increment()
    {
        count++;
    }
    
    public synchronized void decrement()
    {
        count--;
    }
    
    public synchronized int getCount()
    {
        return count;
    }
}

public class Main {

    public static void main(String[] args) 
    {
        Counter counter = new Counter(0);
           
        Thread t1 = new Thread(() -> 
        {
            for (int i = 0; i < 1000; i++) 
            {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> 
        {
            for (int i = 0; i < 1000; i++) 
            {
                counter.decrement();
            }
        });

        
        t1.start();
        t2.start();

        try 
        {
            t1.join(); // Wait for t1 to finish
            t2.join(); // Wait for t2 to finish
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }
        
        System.out.println("Final count: " + counter.getCount());
    }
}
```
```markdown
# Synchronized Method Locking in Java

In Java, the concept of synchronized methods allows for thread-safe access to shared resources or data. A synchronized method is one where only one thread can execute the method at any given time. If multiple threads try to access the synchronized method concurrently, one thread is allowed to execute, while the others are blocked until the executing thread finishes. This mechanism is important to avoid conflicts or data inconsistency in multithreaded programs.

## Concept of Synchronized Method Locking

A synchronized method in Java uses a mechanism called "monitor" or "lock." When a method is marked with the `synchronized` keyword, it ensures that only one thread can execute that method at a time. If thread A is executing the synchronized method, thread B is not allowed to enter any synchronized method on the same object until thread A finishes.

In simpler terms, if we imagine the synchronized methods as doors, the first thread that enters the synchronized method locks the door. All other threads attempting to enter any synchronized method on the same object are blocked, as if all the doors were locked at the same time. This prevents any concurrent access that could lead to inconsistent data or race conditions.

### Key Points:
- The `synchronized` keyword can be applied to methods to ensure thread safety.
- Only one thread can enter a synchronized method of a particular object at a time.
- The locking is object-based. If one thread locks the object, other threads trying to access any synchronized method on the same object are blocked.

## Example Code

In this example, we have a `Counter` class with synchronized methods for incrementing and decrementing a count. The `increment` and `decrement` methods are synchronized to ensure that only one thread can modify the count at any given time.

### Code:

```java
public class Counter 
{
    private int count;
    
    public Counter(int count)
    {
        this.count = count;
    }
    
    public synchronized void increment()
    {
        count++;
    }
    
    public synchronized void decrement()
    {
        count--;
    }
    
    public synchronized int getCount()
    {
        return count;
    }
}

public class Main {

    public static void main(String[] args) 
    {
        Counter counter = new Counter(0);
           
        Thread t1 = new Thread(() -> 
        {
            for (int i = 0; i < 1000; i++) 
            {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> 
        {
            for (int i = 0; i < 1000; i++) 
            {
                counter.decrement();
            }
        });

        
        t1.start();
        t2.start();

        try 
        {
            t1.join(); // Wait for t1 to finish
            t2.join(); // Wait for t2 to finish
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }
        
        System.out.println("Final count: " + counter.getCount());
    }
}
```

### Explanation of the Code:
- **Counter Class**: This class contains a `count` variable and synchronized methods to increment, decrement, and get the count.
  - `increment()` and `decrement()` are synchronized methods, ensuring that only one thread can modify the count at any time.
  - `getCount()` is also synchronized to ensure that no other thread can read the count while it is being modified.

- **Main Class**: In the `Main` class, two threads (`t1` and `t2`) are created:
  - `t1` increments the counter 1000 times.
  - `t2` decrements the counter 1000 times.
  
  Both threads are started concurrently, but due to the synchronized methods in the `Counter` class, only one thread can increment or decrement the counter at any given time.

- The `join()` method is used to ensure that the main thread waits for both `t1` and `t2` to finish before printing the final count.

### Output:
```text
Final count: 0
```

Despite the concurrent execution of threads, the synchronized methods ensure that the final count is correct (0 in this case), without any data inconsistency.

## Conclusion

The synchronized methods provide a simple way to ensure thread safety in Java applications. By locking the object on which a synchronized method is called, it guarantees that only one thread can execute the method at any time. This is particularly important in multithreaded environments where multiple threads might attempt to modify shared resources concurrently. Using the `synchronized` keyword effectively prevents race conditions and ensures the integrity of your data.
```