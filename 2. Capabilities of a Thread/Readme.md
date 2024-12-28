# Capabilities of a Thread in Java

Threads in Java enable concurrent execution of tasks, improving the performance and responsiveness of applications. The provided example demonstrates various capabilities of a thread, which are explained below:

## 1. **Getting the Current Thread Name**
Every thread in Java has a name that can be retrieved using `Thread.currentThread().getName()`.

### Example:
```java
System.out.println("We are in a thread : " + Thread.currentThread().getName());
```

Output:
```
We are in a thread : main
```

## 2. **Setting the Thread Name**
A thread's name can be set using the `setName` method, which helps in identifying threads during execution.

### Example:
```java
thread.setName("Worker Thread");
```

## 3. **Setting and Getting Thread Priority**
Thread priorities determine the relative importance of threads during execution. Java provides three predefined priorities:
- `Thread.MAX_PRIORITY` (10)
- `Thread.NORM_PRIORITY` (5)
- `Thread.MIN_PRIORITY` (1)

The priority of a thread can be set using `setPriority` and retrieved using `getPriority`.

### Example:
```java
thread.setPriority(Thread.MAX_PRIORITY);
System.out.println("Current thread priority is : " + Thread.currentThread().getPriority());
```

Output:
```
Current thread priority is : 10
```

## 4. **Starting a Thread**
A thread begins execution when the `start()` method is invoked. The thread then runs concurrently with the main program.

### Example:
```java
thread.start();
```

## 5. **Pausing Execution with `sleep`**
The `Thread.sleep(milliseconds)` method pauses the current thread's execution for a specified duration. This is useful for simulating delays or managing execution flow.

### Example:
```java
Thread.sleep(1000);
```

## Program Flow Example:
Below is the output when the `ThreadCreationMain` program is executed (order may vary due to thread scheduling):
```
We are in a thread : main before starting a new thread.
We are in a thread : Worker Thread while new thread is running
Current thread priority is : 10
We are in a thread : main after starting the thread.
```

## Key Points:
- The `main` method itself runs on the "main" thread.
- Threads can have custom names and priorities to better organize and manage tasks.
- `Thread.sleep` pauses the current thread but does not release locks or resources held by the thread.

## Notes:
- Thread scheduling and execution order depend on the JVM and operating system.
- Setting thread priority does not guarantee execution order but influences it.
