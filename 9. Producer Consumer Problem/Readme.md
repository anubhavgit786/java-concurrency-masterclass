In Java, `wait()` and `notify()` are methods provided by the `Object` class and are used for inter-thread communication. These methods are typically used in the context of synchronized blocks or methods, allowing threads to coordinate their actions effectively.

### **Key Concepts**
- **`wait()`**: This method causes the current thread to wait until another thread invokes `notify()` or `notifyAll()` on the same object. It releases the monitor lock and goes into a waiting state.
- **`notify()`**: This method wakes up a single thread that is waiting on the object's monitor. The exact thread to be woken up is determined by the JVM.
- **`notifyAll()`**: This method wakes up all threads that are waiting on the object's monitor.

### **Usage in Synchronized Block**
The `wait()` and `notify()` methods can only be called within synchronized blocks or methods because they rely on the monitor lock associated with the object.

### **Example: Producer-Consumer Problem**
Here is a simple example to demonstrate `wait()` and `notify()` for synchronizing threads in a producer-consumer scenario:

```java
class SharedResource {
    private int data;
    private boolean hasData = false;

    public synchronized void produce(int value) throws InterruptedException {
        while (hasData) {
            wait(); // Wait until the data is consumed
        }
        data = value;
        hasData = true;
        System.out.println("Produced: " + value);
        notify(); // Notify the consumer
    }

    public synchronized void consume() throws InterruptedException {
        while (!hasData) {
            wait(); // Wait until there is data to consume
        }
        System.out.println("Consumed: " + data);
        hasData = false;
        notify(); // Notify the producer
    }
}

public class WaitNotifyExample {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();

        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 5; i++) {
                    resource.produce(i);
                    Thread.sleep(500); // Simulate some delay
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        Thread consumer = new Thread(() -> {
            try {
                for (int i = 1; i <= 5; i++) {
                    resource.consume();
                    Thread.sleep(1000); // Simulate some delay
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        producer.start();
        consumer.start();
    }
}
```

### **Explanation**
1. **Producer**:
   - Checks if `hasData` is `true`. If so, it waits until the consumer processes the data.
   - Produces data, sets `hasData` to `true`, and calls `notify()` to wake up the consumer.

2. **Consumer**:
   - Checks if `hasData` is `false`. If so, it waits until the producer provides data.
   - Consumes the data, sets `hasData` to `false`, and calls `notify()` to wake up the producer.

### **Key Points**
- The `wait()` method releases the lock on the object, allowing other threads to acquire it.
- The thread that calls `notify()` does not immediately release the lock; it continues executing until it exits the synchronized block.
- Use `notifyAll()` when multiple threads might be waiting, and you want to wake all of them up.

This pattern ensures proper synchronization and avoids race conditions or deadlocks in multi-threaded applications.