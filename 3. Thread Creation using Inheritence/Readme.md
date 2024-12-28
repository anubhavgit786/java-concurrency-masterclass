# Thread Creation Using Inheritance in Java

This project demonstrates how to create and manage threads in Java by extending the `Thread` class. It includes examples of specialized threads such as `OrderProcessor` and `EmailSender`, each encapsulating specific behavior.

---

## Features

- **Inheritance-Based Thread Creation**: Each thread is a subclass of `Thread` with a custom `run()` method.
- **Custom Task Implementation**: Examples include:
  - **Order Processing**: Simulates processing an order.
  - **Email Sending**: Simulates sending an email.
- **Concurrent Execution**: Threads run concurrently to handle tasks efficiently.

---

## Classes and Responsibilities

1. **`OrderProcessor`**
   - Extends `Thread`.
   - Processes an order with a given `orderId`.
   - Simulates work using `Thread.sleep()`.

2. **`EmailSender`**
   - Extends `Thread`.
   - Sends an email to a specified recipient.
   - Simulates work using `Thread.sleep()`.

3. **`ThreadInheritanceDemo`**
   - The main application that creates and starts `OrderProcessor` and `EmailSender` threads.
   - Waits for threads to complete using `join()`.

---

## Code Example

### OrderProcessor.java
```java
class OrderProcessor extends Thread {
    private String orderId;

    public OrderProcessor(String orderId) {
        this.orderId = orderId;
    }

    @Override
    public void run() {
        System.out.println("Processing order: " + orderId);
        try {
            Thread.sleep(2000); // Simulate work
        } catch (InterruptedException e) {
            System.out.println("Order processing interrupted for: " + orderId);
        }
        System.out.println("Order processed: " + orderId);
    }
}
```

### EmailSender.java
```java
class EmailSender extends Thread {
    private String recipient;

    public EmailSender(String recipient) {
        this.recipient = recipient;
    }

    @Override
    public void run() {
        System.out.println("Sending email to: " + recipient);
        try {
            Thread.sleep(1500); // Simulate work
        } catch (InterruptedException e) {
            System.out.println("Email sending interrupted for: " + recipient);
        }
        System.out.println("Email sent to: " + recipient);
    }
}

```

### ThreadInheritanceDemo.java
```java
public class ThreadInheritanceDemo {
    public static void main(String[] args) {
        Thread orderThread = new OrderProcessor("ORD123");
        Thread emailThread = new EmailSender("user@example.com");

        orderThread.start();
        emailThread.start();

        try {
            orderThread.join();
            emailThread.join();
        } catch (InterruptedException e) {
            System.out.println("Main thread interrupted.");
        }

        System.out.println("All tasks completed.");
    }
}

```

---

## Use Case

This example is ideal for scenarios where tasks are:

- **Self-contained with distinct behaviors**: Each task encapsulates specific functionality.
- **Easily represented using inheritance**: Task logic fits naturally into a class hierarchy.
- **Concurrently executed to improve performance**: Threads run in parallel for better resource utilization.

---

## Benefits

1. **Encapsulation**: Threads encapsulate task-specific behavior in a single class.
2. **Simpler Design**: Inheritance provides a straightforward way to implement threading.

---

## Drawbacks

1. **Limited Inheritance**: Since Java supports single inheritance, extending `Thread` prevents inheriting from other classes.
2. **Runnable Alternative**: For greater flexibility and separation of concerns, consider implementing the `Runnable` interface to decouple task logic from the thread.

---