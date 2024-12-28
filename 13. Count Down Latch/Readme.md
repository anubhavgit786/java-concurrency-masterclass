# Understanding `CountDownLatch` in Java

The `CountDownLatch` class, part of the `java.util.concurrent` package, is a synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads is completed.

## Key Concepts

- **Initialization**: A `CountDownLatch` is initialized with a count (the number of times `countDown()` must be invoked before threads can proceed).
- **Decrementing the Count**: Each time a thread calls `countDown()`, the count decreases by 1.
- **Awaiting Completion**: Threads can call `await()` to block until the count reaches zero.

This mechanism is useful in scenarios where a thread must wait for other threads to complete their tasks before it can proceed.

---

## Example Code

Here is an example demonstrating the use of `CountDownLatch`:

```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchExample 
{
    public static void main(String[] args) throws InterruptedException 
    {
        int numberOfWorkers = 3;
        CountDownLatch latch = new CountDownLatch(numberOfWorkers);

        // Worker threads
        for (int i = 1; i <= numberOfWorkers; i++) 
        {
            new Thread(new Worker(i, latch)).start();
        }

        // Main thread waits for workers to complete
        System.out.println("Main thread waiting for workers to complete...");
        latch.await();

        System.out.println("All workers completed. Main thread proceeding.");
    }
}

class Worker implements Runnable 
{
    private final int workerId;
    private final CountDownLatch latch;

    public Worker(int workerId, CountDownLatch latch) 
    {
        this.workerId = workerId;
        this.latch = latch;
    }

    @Override
    public void run() 
    {
        System.out.println("Worker " + workerId + " is working...");
        try 
        {
            Thread.sleep(1000); // Simulate work
        } 
        catch (InterruptedException e) 
        {
            Thread.currentThread().interrupt();
        }
        System.out.println("Worker " + workerId + " finished.");
        latch.countDown(); // Decrement the latch count
    }
}
```

### Output

```
Worker 1 is working...
Worker 2 is working...
Worker 3 is working...
Main thread waiting for workers to complete...
Worker 1 finished.
Worker 2 finished.
Worker 3 finished.
All workers completed. Main thread proceeding.
```

---

## How It Works

1. The main thread creates a `CountDownLatch` with a count of 3 (the number of workers).
2. Each worker thread performs its task and calls `countDown()` upon completion.
3. The main thread calls `await()` and blocks until the latch count reaches zero.
4. When all worker threads complete, the latch count reaches zero, and the main thread resumes execution.

---

## Benefits

- **Thread Coordination**: Ensures that threads are synchronized based on task completion.
- **Simple API**: Provides a straightforward way to implement thread synchronization.

---

## Limitations

1. **One-Time Use**: A `CountDownLatch` cannot be reset. Once the count reaches zero, it cannot be reused.
2. **Manual Count Management**: Developers need to manage the count explicitly, which can lead to errors if not handled properly.

---

## Alternatives

- **`CyclicBarrier`**: Suitable for scenarios where threads need to wait for each other to reach a common point.
- **`Semaphore`**: Useful for controlling access to a shared resource.

---

By understanding and using `CountDownLatch` effectively, developers can handle complex thread synchronization requirements with ease.
