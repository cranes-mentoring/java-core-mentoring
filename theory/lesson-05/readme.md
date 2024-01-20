### Exceptions in Java:

In Java, exceptions are objects representing unexpected or erroneous states in a program. Exceptions allow handling errors and recovering from them, preventing the termination of program execution. Exceptions in Java are divided into two types: **checked exceptions** and **unchecked exceptions**.

### Checked Exceptions:

Checked exceptions must be handled in the program code. They inherit from the `Exception` class. Examples:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class CheckedExceptionExample {

    public static void main(String[] args) {
        try {
            readFile("example.txt");
        } catch (IOException e) {
            System.err.println("IOException: " + e.getMessage());
        }
    }

    public static void readFile(String fileName) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(fileName));
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        br.close();
    }
}
```

In the example above, the `readFile` method throws an `IOException`, which is a checked exception. In the `main` method, this exception is handled using a `try-catch` block.

### Unchecked Exceptions:

Unchecked exceptions do not require explicit handling. They inherit from the `RuntimeException` class. Example:

```java
public class UncheckedExceptionExample {

    public static void main(String[] args) {
        divide(10, 0);
    }

    public static void divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        int result = a / b;
        System.out.println("Result: " + result);
    }
}
```

Here, the `divide` method throws an `ArithmeticException`, which is an unchecked exception.

### Creating Custom Exceptions:

You can also create your own exceptions by extending the `Exception` class or its subclasses. Example:

```java
public class CustomExceptionExample {

    public static void main(String[] args) {
        try {
            checkTemperature(120);
        } catch (TemperatureTooHighException e) {
            System.err.println("Caught custom exception: " + e.getMessage());
        }
    }

    public static void checkTemperature(int temperature) throws TemperatureTooHighException {
        if (temperature > 100) {
            throw new TemperatureTooHighException("Temperature is too high!");
        }
        System.out.println("Temperature is within the safe range.");
    }
}

class TemperatureTooHighException extends Exception {
    public TemperatureTooHighException(String message) {
        super(message);
    }
}
```

In this example, a custom exception `TemperatureTooHighException` is created as a subclass of `Exception`. The `checkTemperature` method throws this exception if the temperature is above 100.

By using exceptions in Java, you can handle errors more effectively and create more resilient programs. Remember that exception handling is a crucial part of development, and using it correctly will make your code more robust.