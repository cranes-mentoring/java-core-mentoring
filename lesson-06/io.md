### Input/Output (I/O) in Java: Reading and Writing to Files

Input/Output (I/O) in Java provides mechanisms for exchanging data between a program and external sources or sinks, such as files. Working with files in Java involves reading from and writing to them.

### Reading from a File:

To read from a file, you can use the `File`, `FileReader`, and `BufferedReader` classes. Here's an example of reading from a file:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class FileReaderExample {

    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

It's important to note the use of the `try-with-resources` statement, which automatically closes resources after their use.

### Writing to a File:

To write to a file, you can use the `File`, `FileWriter`, and `BufferedWriter` classes. Here's an example of writing to a file:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterExample {

    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            writer.write("Hello, World!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### High-Performance File Writing:

For achieving high performance in file writing, you can use the `BufferedWriter` class with buffering. Buffering helps reduce the number of I/O operations, thus increasing performance. Here's an example:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class HighPerformanceFileWriter {

    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("high_performance_output.txt"))) {
            for (int i = 0; i < 1000000; i++) {
                writer.write("Line " + i);
                writer.newLine(); // Add a new line
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Here, we use a loop to write a million lines to a file. The `newLine()` method adds a new line to the file, which is a more efficient way than inserting newline characters directly.

### Importance of Closing Streams:

When working with input/output, it's crucial to close streams to free up resources. Using the `try-with-resources` block ensures automatic closure of resources after their use, simplifying code and preventing resource leaks.

```java
try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
    // Reading from the file
} catch (IOException e) {
    e.printStackTrace();
} // The stream will be automatically closed
```

These examples provide a basic understanding of how to perform reading and writing to files in Java. Performance optimization depends on the specific use case and may involve various techniques, such as multithreading or using the Java NIO (New I/O) channels.