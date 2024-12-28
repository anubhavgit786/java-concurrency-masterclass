# Daemon Threads in Java

## Overview
In Java, a **daemon thread** is a background thread that does not prevent the Java Virtual Machine (JVM) from exiting when the main thread finishes execution. Daemon threads are typically used for tasks that run continuously in the background, such as garbage collection or monitoring services.

### Key Characteristics of Daemon Threads
1. **Lifecycle Dependency:**
   - Daemon threads depend on the existence of user (non-daemon) threads. When all user threads terminate, the JVM exits, stopping all daemon threads.

2. **Usage:**
   - Daemon threads are useful for low-priority background tasks that do not require immediate attention or explicit shutdown.

3. **Setting as Daemon:**
   - A thread can be marked as a daemon using `thread.setDaemon(true)` before starting it.

4. **Examples in Java:**
   - Garbage collection is a classic example of a daemon thread in the JVM.

---

## Example: Long Computation Task as a Daemon Thread
The following code demonstrates how to use a daemon thread to execute a long computation task in the background.

### Code Example
```java
import java.math.BigInteger;

public class LongComputationTask implements Runnable {
    BigInteger base;
    BigInteger power;

    public LongComputationTask(BigInteger base, BigInteger power) {
        this.base = base;
        this.power = power;
    }

    public BigInteger pow(BigInteger base, BigInteger power) {
        BigInteger res = BigInteger.ONE;

        for (BigInteger i = BigInteger.ZERO; i.compareTo(power) != 0; i = i.add(BigInteger.ONE)) {
            res = res.multiply(base);
        }

        return res;
    }

    public void run() {
        System.out.println("Base : " + base);
        System.out.println("Power : " + power);
        System.out.println("B^P : " + pow(base, power));
    }
}

public class Main {
    public static void main(String[] args) {
        BigInteger base = new BigInteger("200000");
        BigInteger power = new BigInteger("10000000");
        LongComputationTask task = new LongComputationTask(base, power);
        Thread thread = new Thread(task);
        thread.setDaemon(true); // Marking the thread as a daemon
        thread.start();
        thread.interrupt();
    }
}
```

### Explanation
1. **Daemon Thread Creation:**
   - The thread is marked as a daemon using `thread.setDaemon(true)`.

2. **Behavior:**
   - Since the thread is a daemon, it will stop execution when the main thread completes.

3. **No Output Guarantee:**
   - If the main thread finishes execution before the daemon thread completes its task, the daemon thread will terminate abruptly.

### Output
The output is not guaranteed because the daemon thread might not finish execution before the main thread terminates. For example:
```
Base : 200000
Power : 10000000
```

---

## Key Points to Remember
1. **Set Before Start:**
   - The `setDaemon(true)` method must be called before starting the thread. Otherwise, an `IllegalThreadStateException` will be thrown.

2. **Use Cases:**
   - Suitable for background services that do not require explicit termination, such as logging or monitoring.

3. **Lifecycle:**
   - Daemon threads are terminated automatically when all user threads exit.

By using daemon threads judiciously, you can ensure efficient background task management without obstructing the main application's lifecycle.
