# Exception Handling
### The Idea Behind Exception
Exceptions are used to handle errors and other exceptional events in a program, allowing the program to continue running or terminate gracefully.

### Exceptions & Errors
Exceptions are problems that occur during the execution of a program, while errors are serious issues that a reasonable application should not try to catch.

### Types of Exception
Checked Exceptions: These exceptions are checked at compile-time.
Unchecked Exceptions: These are not checked at compile-time.
Errors: These are serious problems that applications should not try to catch.
### Control Flow in Exceptions
The control flow in exceptions is handled using try, catch, finally, throw, and throws.

### JVM Reaction to Exceptions
If an exception is not caught, the JVM terminates the program and prints the exception stack trace.

Use of try, catch, finally, throw, throws in Exception Handling
```java
public class ExceptionHandlingExample {
    public static void main(String[] args) {
        try {
            // Code that may throw an exception
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            // Handling the exception
            System.out.println("Caught ArithmeticException: " + e.getMessage());
        } finally {
            // Code that will always execute
            System.out.println("Finally block executed");
        }
    }
    
    // Method that throws an exception
    public void methodThatThrowsException() throws Exception {
        throw new Exception("Exception thrown");
    }
}
```

### In-built and User Defined Exceptions
```java
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

public class UserDefinedExceptionExample {
    public static void main(String[] args) {
        try {
            throw new CustomException("This is a custom exception");
        } catch (CustomException e) {
            System.out.println("Caught CustomException: " + e.getMessage());
        }
    }
}
```
### Checked and Unchecked Exceptions
Checked exceptions are checked at compile-time, while unchecked exceptions are not.

```java
public class CheckedUncheckedExceptionExample {
    // Checked exception
    public void methodWithCheckedException() throws Exception {
        throw new Exception("Checked Exception");
    }

    // Unchecked exception
    public void methodWithUncheckedException() {
        throw new RuntimeException("Unchecked Exception");
    }
}
```
Input/Output Basics
Byte Streams and Character Streams
Byte Streams
Byte streams are used to perform input and output of 8-bit bytes.

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class ByteStreamExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.txt");
             FileOutputStream fos = new FileOutputStream("output.txt")) {
            int data;
            while ((data = fis.read()) != -1) {
                fos.write(data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
### Character Streams
Character streams are used to perform input and output for 16-bit Unicode.

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CharacterStreamExample {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("input.txt");
             FileWriter fw = new FileWriter("output.txt")) {
            int data;
            while ((data = fr.read()) != -1) {
                fw.write(data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
### Reading and Writing File in Java
```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.io.IOException;
import java.util.List;

public class FileReadWriteExample {
    public static void main(String[] args) {
        String filePath = "example.txt";
        
        // Writing to a file
        try {
            Files.writeString(Paths.get(filePath), "Hello, World!\nThis is a file.");
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // Reading from a file
        try {
            List<String> lines = Files.readAllLines(Paths.get(filePath));
            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
## Multithreading
### Thread
A thread is a lightweight process. Threads allow concurrent execution of two or more parts of a program.

### Thread Life Cycle
The life cycle of a thread includes the following states: New, Runnable, Blocked, Waiting, Timed Waiting, and Terminated.

### Creating Threads
Extending Thread Class
```java
class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
        }
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();
        t1.start();
        t2.start();
    }
}
```
### Implementing Runnable Interface
```java
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
        }
    }
}

public class RunnableExample {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable());
        Thread t2 = new Thread(new MyRunnable());
        t1.start();
        t2.start();
    }
}
```
### Thread Priorities
Thread priorities can be set using setPriority() method.

```java
class PriorityThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
        }
    }
}

public class ThreadPriorityExample {
    public static void main(String[] args) {
        PriorityThread t1 = new PriorityThread();
        PriorityThread t2 = new PriorityThread();
        
        t1.setPriority(Thread.MIN_PRIORITY);
        t2.setPriority(Thread.MAX_PRIORITY);
        
        t1.start();
        t2.start();
    }
}
```
### Synchronizing Threads
Synchronization is used to control the access of multiple threads to shared resources.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class SynchronizationExample {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Count: " + counter.getCount());
    }
}
```
### Inter-thread Communication
Inter-thread communication is used for synchronization and coordination between threads.

```java
class SharedResource {
    private int value;
    private boolean valueSet = false;

    public synchronized void produce(int value) {
        while (valueSet) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.value = value;
        valueSet = true;
        System.out.println("Produced: " + value);
        notify();
    }

    public synchronized void consume() {
        while (!valueSet) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        valueSet = false;
        System.out.println("Consumed: " + value);
        notify();
    }
}

class Producer implements Runnable {
    private SharedResource resource;

    public Producer(SharedResource resource) {
        this.resource = resource;
    }

    public void run() {
        for (int i = 0; i < 5; i++) {
            resource.produce(i);
        }
    }
}

class Consumer implements Runnable {
    private SharedResource resource;

    public Consumer(SharedResource resource) {
        this.resource = resource;
    }

    public void run() {
        for (int i = 0; i < 5; i++) {
            resource.consume();
        }
    }
}

public class InterThreadCommunicationExample {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();
        Thread producerThread = new Thread(new Producer(resource));
        Thread consumerThread = new Thread(new Consumer(resource));
        
        producerThread.start();
        consumerThread.start();
    }
}
```
These examples provide a comprehensive overview of exception handling, I/O basics, and multithreading in Java. Let me know if you need further explanations or additional examples!
