Notes here: [link](https://lookingforere.medium.com/java-helper-part-5-notes-about-threads-c4b5ebd38893)

### Multithreading in Java: A Comprehensive Overview

### Threads and Processes:

In Java, a **thread** represents the smallest unit of execution within a process. A **process** is an independent program that runs in its own memory space. A process can have multiple threads, each executing its own set of instructions.

### Executors:

**Executors** in Java provide a higher-level replacement for managing threads. They simplify the process of managing thread creation, pooling, and execution.

#### Executors Example:
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorExample {

    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5);

        for (int i = 0; i < 10; i++) {
            Runnable worker = new WorkerThread("Task " + i);
            executor.execute(worker);
        }

        executor.shutdown();
        while (!executor.isTerminated()) {
            // Wait for all tasks to finish
        }

        System.out.println("All tasks completed");
    }
}
```

### Locks, Semaphores, and Features:

- **Locks:** Provide exclusive access to a shared resource. Example: `ReentrantLock`.

#### Locks Example:
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockExample {
    private static final Lock lock = new ReentrantLock();

    public static void main(String[] args) {
        new Thread(LockExample::doSomething).start();
        new Thread(LockExample::doSomething).start();
    }

    private static void doSomething() {
        lock.lock();
        try {
            // Critical section
            System.out.println("Thread " + Thread.currentThread().getId() + " is in the critical section");
        } finally {
            lock.unlock();
        }
    }
}
```

- **Semaphores:** Control the number of threads accessing a resource. Example: `Semaphore`.

#### Semaphore Example:
```java
import java.util.concurrent.Semaphore;

public class SemaphoreExample {
    private static final Semaphore semaphore = new Semaphore(2);

    public static void main(String[] args) {
        new Thread(SemaphoreExample::doSomething).start();
        new Thread(SemaphoreExample::doSomething).start();
        new Thread(SemaphoreExample::doSomething).start();
    }

    private static void doSomething() {
        try {
            semaphore.acquire();
            // Critical section
            System.out.println("Thread " + Thread.currentThread().getId() + " is in the critical section");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            semaphore.release();
        }
    }
}
```

- **Features:** Represent the result of an asynchronous computation. Example: `Future`.

#### Features Example:
```java
import java.util.concurrent.*;

public class FeaturesExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(1);

        Callable<String> task = () -> {
            TimeUnit.SECONDS.sleep(2);
            return "Result of the asynchronous computation";
        };

        Future<String> future = executor.submit(task);

        try {
            String result = future.get(3, TimeUnit.SECONDS); // Wait for at most 3 seconds
            System.out.println("Result: " + result);
        } catch (InterruptedException | ExecutionException | TimeoutException e) {
            e.printStackTrace();
        } finally {
            executor.shutdown();
        }
    }
}
```

### Atomic Operations:

**Atomic operations** in Java ensure that a variable is accessed atomically, without interference from other threads. They are implemented using the `java.util.concurrent.atomic` package.

#### Atomic Example:
```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicExample {
    private static AtomicInteger counter = new AtomicInteger(0);

    public static void main(String[] args) {
        new Thread(AtomicExample::increment).start();
        new Thread(AtomicExample::increment).start();
    }

    private static void increment() {
        System.out.println("Thread " + Thread.currentThread().getId() + " incremented counter: " + counter.incrementAndGet());
    }
}
```

**Difference Between Atomic and Lock:**
- **Atomic:** Provides low-level atomic operations for variables. Simple and efficient but may not be suitable for complex synchronization scenarios.

- **Lock:** Offers more flexibility in managing critical sections and allows for more complex synchronization strategies.

**Performance Considerations:**
- **Atomic Operations:** Generally have lower overhead and better performance for simple use cases.

- **Locks:** Can be more efficient in scenarios requiring complex synchronization, but may have higher overhead for simple use cases.

### Conclusion:

Multithreading in Java involves managing threads, processes, and synchronization mechanisms like locks, semaphores, and features. Executors simplify thread management, while atomic operations provide efficient and thread-safe variable access. Understanding these concepts helps in designing robust and efficient concurrent applications.

