# Understanding `CyclicBarrier` in Java

The `CyclicBarrier` class in Java, part of the `java.util.concurrent` package, is a synchronization aid that allows a set of threads to wait for each other to reach a common barrier point. This is particularly useful in scenarios where multiple threads must start or complete a task together.

---

## Key Features

1. **Reusability**: The barrier is "cyclic," meaning it can be reused after the waiting threads are released.
2. **Custom Barrier Action**: You can specify a task (barrier action) to be executed once all threads reach the barrier.
3. **Thread Coordination**: Ensures that a group of threads waits for each other before proceeding.
4. **Count Management**: Tracks the number of threads required to trip the barrier.

---

## Example Code

Below is an example demonstrating the usage of `CyclicBarrier`:

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample 
{
    public static void main(String[] args) 
    {
        // Create a CyclicBarrier for 3 threads with a barrier action
        CyclicBarrier barrier = new CyclicBarrier(3, () -> 
        {
            System.out.println("All threads have reached the barrier. Proceeding...");
        });

        // Create 3 threads
        for (int i = 1; i <= 3; i++) 
        {
            Thread thread = new Thread(new Task(barrier, "Thread-" + i));
            thread.start();
        }
    }
}

class Task implements Runnable 
{
    private CyclicBarrier barrier;
    private String name;

    public Task(CyclicBarrier barrier, String name) 
    {
        this.barrier = barrier;
        this.name = name;
    }

    @Override
    public void run() 
    {
        try 
        {
            System.out.println(name + " is performing its task.");
            Thread.sleep((long) (Math.random() * 1000));
            System.out.println(name + " has reached the barrier.");

            barrier.await(); // Wait for other threads

            System.out.println(name + " has crossed the barrier.");
        } 
        catch (InterruptedException | BrokenBarrierException e) 
        {
            e.printStackTrace();
        }
    }
}
```

### Output

```
Thread-1 is performing its task.
Thread-2 is performing its task.
Thread-3 is performing its task.
Thread-2 has reached the barrier.
Thread-1 has reached the barrier.
Thread-3 has reached the barrier.
All threads have reached the barrier. Proceeding...
Thread-2 has crossed the barrier.
Thread-1 has crossed the barrier.
Thread-3 has crossed the barrier.
```

---

## How It Works

1. **Barrier Initialization**:
   - A `CyclicBarrier` is initialized with the number of threads and an optional barrier action.
2. **Thread Waiting**:
   - Threads call `await()` to signal they have reached the barrier.
3. **Barrier Release**:
   - Once the specified number of threads call `await()`, the barrier is tripped, executing the optional barrier action and releasing all waiting threads.

---

## Benefits

1. **Thread Synchronization**: Ensures threads wait for each other to proceed together.
2. **Reusability**: Can be reset and reused for multiple phases of tasks.
3. **Custom Actions**: Supports the execution of a custom action when the barrier is tripped.

---

## Use Cases

- **Parallel Processing**: Coordinating multiple threads in a computation.
- **Multi-Stage Tasks**: Synchronizing threads at intermediate stages of a task.
- **Simulations**: Managing threads in simulations requiring synchronized phases.

---

## Limitations

1. **Broken Barrier**: If a thread leaves the barrier prematurely (e.g., due to interruption), the barrier is considered broken.
2. **Performance Overhead**: Coordination introduces some performance overhead compared to independent thread execution.

---

## Alternatives

- **`CountDownLatch`**: For one-time synchronization.
- **Manual Synchronization**: For scenarios requiring finer control over thread behavior.

---

By using `CyclicBarrier`, developers can simplify thread synchronization in concurrent applications, particularly for scenarios requiring threads to proceed together in phases.
