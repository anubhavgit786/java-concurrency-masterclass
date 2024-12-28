# Synchronized Methods in Java

## Overview

In multi-threaded programming, synchronization is a key concept to ensure that shared resources are accessed in a thread-safe manner. In Java, the `synchronized` keyword is used to control the access of multiple threads to a particular section of code, ensuring that only one thread can execute that code at a time. This is especially important when multiple threads modify shared data to prevent data corruption or inconsistencies.

This example demonstrates the concept of **synchronized methods** for thread safety using the `Counter` class.

---

## Synchronized Methods in Action

In the provided code, the `Counter` class has three methods:

- `increment()`
- `decrement()`
- `getCount()`

Each of these methods is marked with the `synchronized` keyword.

### What does `synchronized` mean?

The `synchronized` keyword in Java is used to ensure that only **one thread at a time** can execute a particular method on the same object. When a method is marked as `synchronized`, it obtains a lock on the object it is invoked on. This lock prevents other threads from accessing any synchronized method of the same object concurrently. 

In this case, the `Counter` class methods modify the `count` variable, which is shared across multiple threads. If these methods were not synchronized, both threads could update the `count` variable simultaneously, causing a race condition where the final value of `count` may not be as expected.

### Explanation of the Concept

When two threads, `t1` and `t2`, are created in the `Main` class and they both attempt to invoke the `increment()` and `decrement()` methods on the same `Counter` object:

1. Thread `t1` will attempt to call `increment()`, and it will acquire the lock on the `Counter` object.
2. While thread `t1` holds the lock, thread `t2` will be **blocked** from calling either `increment()` or `decrement()` because those methods are synchronized and both methods are part of the same object.
3. Once thread `t1` finishes executing its synchronized method, it releases the lock, allowing thread `t2` to acquire the lock and execute its own synchronized method.
4. This continues until both threads finish their operations.

The result is that even though both threads are executing in parallel, they cannot access the synchronized methods simultaneously. Only one thread can execute a synchronized method on the `Counter` object at any given time.

---

### Visualizing the Locking Mechanism

Think of each synchronized method as a door. If thread A wants to access a method (door) and is allowed, it enters and locks that door. While thread A is inside, no other thread (like thread B) can open any of the doors. When thread A exits, the door is unlocked, allowing thread B to enter.

In other words:
- When thread A executes any synchronized method, it locks the entire object (like locking all doors of the object).
- Thread B must wait for thread A to finish and release the lock (unlock the doors) before it can access the synchronized methods.

---

## Example Walkthrough

In the `Main` class:

1. **Thread `t1`** starts and increments the `count` 1000 times.
2. **Thread `t2`** starts and decrements the `count` 1000 times.
3. Both threads modify the same shared `count` variable, but because the methods are synchronized, only one thread can execute either `increment()` or `decrement()` at any time.
4. The final value of `count` is printed after both threads have completed their tasks.

Since both threads are performing equal numbers of operations (`1000 increments` and `1000 decrements`), the final count will be `0`.

---

## Why Use `synchronized`?

The primary reason to use `synchronized` methods in Java is to avoid **race conditions**. A race condition occurs when multiple threads try to access and modify shared data concurrently, leading to unpredictable results. By using synchronization, you ensure that only one thread modifies shared data at a time, guaranteeing data consistency and thread safety.

However, it's essential to use synchronization judiciously, as it can lead to **thread contention** (where threads wait for access to the synchronized method) and **performance overhead** if overused.

---

## Summary

- The `synchronized` keyword in Java ensures that only one thread can execute a synchronized method at a time on the same object.
- When one thread enters a synchronized method, all other threads attempting to access synchronized methods on the same object will be blocked until the first thread finishes.
- This is essential for thread safety when working with shared data or resources in a multi-threaded environment.

By applying synchronization, we ensure the correct and predictable behavior of multi-threaded applications while avoiding concurrency issues like race conditions.
