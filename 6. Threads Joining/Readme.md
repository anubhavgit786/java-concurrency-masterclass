# Understanding `thread.join()` in Java

## Purpose of `thread.join()`

In Java, the `join()` method is used to pause the execution of the current thread until the specified thread on which `join()` is called has finished executing. This is particularly useful when you need to ensure that a thread completes its task before the program continues with the remaining execution.

The key benefits of using `thread.join()` include:
1. **Synchronization**: Ensures that a particular thread completes its execution before others continue.
2. **Sequential Execution**: Helps in maintaining a logical flow where certain operations depend on the completion of another thread.
3. **Avoiding Race Conditions**: Prevents scenarios where threads might access shared resources before the required data is ready.

---

## Code Example: Demonstrating `thread.join()`

The following example illustrates the use of `thread.join()`:

```java
public class ThreadJoinExample {
    public static void main(String[] args) {
        // Creating Thread 1
        Thread thread1 = new Thread(() -> {
            System.out.println("Thread 1: Starting task...");
            try {
                Thread.sleep(2000); // Simulate some work with 2 seconds delay
            } catch (InterruptedException e) {
                System.out.println("Thread 1 interrupted");
            }
            System.out.println("Thread 1: Task completed!");
        });

        // Creating Thread 2
        Thread thread2 = new Thread(() -> {
            System.out.println("Thread 2: Starting task...");
            try {
                Thread.sleep(1000); // Simulate some work with 1 second delay
            } catch (InterruptedException e) {
                System.out.println("Thread 2 interrupted");
            }
            System.out.println("Thread 2: Task completed!");
        });

        // Start both threads
        thread1.start();
        thread2.start();

        try {
            // Main thread waits for thread1 to complete
            thread1.join();
            System.out.println("Main thread: Thread 1 has finished.");

            // Main thread waits for thread2 to complete
            thread2.join();
            System.out.println("Main thread: Thread 2 has finished.");
        } catch (InterruptedException e) {
            System.out.println("Main thread interrupted");
        }

        System.out.println("Main thread: All tasks are completed!");
    }
}
```

---

## Output

When you run the program, the output will look something like this:

```
Thread 1: Starting task...
Thread 2: Starting task...
Thread 2: Task completed!
Thread 1: Task completed!
Main thread: Thread 1 has finished.
Main thread: Thread 2 has finished.
Main thread: All tasks are completed!
```

---

## Explanation

1. The main thread starts `thread1` and `thread2`.
2. The `join()` method is called on `thread1`, causing the main thread to pause execution until `thread1` finishes.
3. After `thread1` completes, the main thread proceeds to call `join()` on `thread2`, waiting for it to finish.
4. Once both threads have completed their tasks, the main thread resumes and prints the final message.

By using `thread.join()`, the program ensures that tasks executed in separate threads are completed in a controlled and predictable manner before proceeding.

---