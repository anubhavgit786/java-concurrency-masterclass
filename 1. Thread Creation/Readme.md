# Thread Creation in Java

Java provides multiple ways to create and start threads. Below is a summary of the methods used in the provided code:

## 1. Implementing the `Runnable` Interface
- A class implements the `Runnable` interface and overrides its `run` method.
- An instance of this class is passed to the `Thread` constructor.

### Example:
```java
public class HelloWorldTask implements Runnable {
    public void run() {
        System.out.println("Hello World! - 1");
    }
}

HelloWorldTask hwt = new HelloWorldTask();
Thread thread1 = new Thread(hwt);
thread1.start();
```

## 2. Using an Anonymous Class
- An anonymous implementation of the `Runnable` interface is created and passed to the `Thread` constructor.

### Example:
```java
Thread thread2 = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello World! - 2");
    }
});
thread2.start();
```

## 3. Using a Lambda Expression
- A lambda expression is used to implement the `Runnable` interface in a concise manner.

### Example:
```java
Runnable task = () -> System.out.println("Hello World! - 3");
Thread thread3 = new Thread(task);
thread3.start();
```

## 4. Directly Passing a Lambda Expression to the `Thread` Constructor
- A lambda expression is passed directly to the `Thread` constructor.

### Example:
```java
Thread thread4 = new Thread(() -> {
    System.out.println("Hello World! - 4");
});
thread4.start();
```

## Key Points
- The `Runnable` interface is a functional interface with a single abstract method, `run()`.
- Using lambda expressions reduces boilerplate code compared to anonymous classes.
- Threads can also be created by extending the `Thread` class, but this approach is not demonstrated here.

## Execution Output
When you execute the `ThreadCreationMain` class, the following output is expected (order may vary due to thread scheduling):
```
Hello World! - 1
Hello World! - 2
Hello World! - 3
Hello World! - 4
