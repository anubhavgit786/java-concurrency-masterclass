# Introduction
In multi-threaded programming, synchronization is used to control the access of multiple threads to shared resources. Block synchronization is one of the techniques in Java that ensures that only one thread can access a block of code at a time, thereby preventing race conditions and ensuring the consistency of shared resources.

In this README, we'll explain the concept of block synchronization in Java and demonstrate it with an example. The example will involve incrementing and decrementing a counter variable in multiple threads while ensuring that the counter is updated safely using synchronization.

## What is Block Synchronization?
Block synchronization is used to make sure that only one thread can execute a block of code at a time. It is done using the `synchronized` keyword, which can be applied to a method or a block of code. When a thread enters a synchronized block, it acquires the lock for the object that the block is synchronized on. Other threads will be blocked until the lock is released, ensuring that no two threads can execute the synchronized block simultaneously.

### Key Points:
- Synchronization can be applied to a method or a specific block of code.
- The synchronized keyword ensures that only one thread can access the block at a time.
- Block synchronization helps prevent race conditions and maintains data consistency when multiple threads access shared resources.

## Example Code: Counter with Block Synchronization

The following example demonstrates how block synchronization is used to safely update a shared counter variable using two threads. One thread increments the counter, while the other decrements it. Synchronization is applied to the methods that modify the counter to prevent concurrent access, ensuring the correct final value.

### Code Explanation

```java
public class Counter 
{
    private int count;
    
    // Constructor to initialize the counter with a given value
    public Counter(int count)
    {
        this.count = count;
    }
    
    // Method to increment the counter in a synchronized block
    public void increment()
    {
        synchronized (this) 
        {
            count++;
        }
    }
    
    // Method to decrement the counter in a synchronized block
    public void decrement()
    {
        synchronized (this) 
        {
            count--;
        }
    }
    
    // Method to get the current value of the counter
    public int getCount()
    {
        return count;
    }
}

public class Main {

    public static void main(String[] args) 
    {
        Counter counter = new Counter(0);
        
        // Thread t1: increments the counter 1000 times
        Thread t1 = new Thread(() -> 
        {
            for (int i = 0; i < 1000; i++) 
            {
                counter.increment();
            }
        });

        // Thread t2: decrements the counter 1000 times
        Thread t2 = new Thread(() -> 
        {
            for (int i = 0; i < 1000; i++) 
            {
                counter.decrement();
            }
        });

        // Start both threads
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
        
        // Print the final count value
        System.out.println("Final count: " + counter.getCount());
    }
}
```

### Code Walkthrough:
1. **Counter Class**:
   - The `Counter` class contains an integer variable `count` and methods to increment and decrement the value of `count`.
   - The methods `increment()` and `decrement()` are synchronized using `synchronized(this)`. This ensures that only one thread can access the code inside the synchronized block at a time.
   - The `getCount()` method returns the current value of the counter.

2. **Main Class**:
   - In the `main` method, two threads `t1` and `t2` are created. Thread `t1` increments the counter 1000 times, while thread `t2` decrements the counter 1000 times.
   - Both threads are started using the `start()` method, and the `join()` method is used to ensure that the main thread waits for both threads to complete before printing the final value of the counter.

3. **Synchronization**:
   - The `synchronized(this)` block ensures that when one thread is updating the `count`, the other thread is blocked until the first thread completes. This prevents race conditions, where both threads might try to modify the counter simultaneously, leading to incorrect results.

### Expected Output:
The final value of the counter should be `0`, as the total number of increments and decrements is equal. However, due to synchronization, even if both threads run simultaneously, the operations will be performed in a safe and consistent manner.

```
Final count: 0
```

## Conclusion

Block synchronization in Java ensures that only one thread can execute a block of code at a time, preventing race conditions and ensuring thread safety. In the example provided, the `synchronized` keyword is used to protect the critical sections of code that modify the shared counter variable, ensuring that the final result is consistent and correct.

When working with multi-threaded applications, block synchronization is a crucial technique to maintain data integrity and avoid issues such as race conditions.