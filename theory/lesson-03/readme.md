### **Creating Functions and Methods:**

#### Example:

```java
public class Calculator {
    // Addition method
    public static int add(int a, int b) {
        return a + b;
    }

    // Subtraction method
    public static int subtract(int a, int b) {
        return a - b;
    }

    // Multiplication method
    public static int multiply(int a, int b) {
        return a * b;
    }

    // Division method
    public static double divide(int a, int b) {
        if (b != 0) {
            return (double) a / b; // Casting to double for precise division
        } else {
            System.out.println("Error: division by zero.");
            return Double.NaN; // NaN (Not a Number) in case of an error
        }
    }

    // Entry point
    public static void main(String[] args) {
        int num1 = 10;
        int num2 = 2;

        // Calling methods and outputting results
        System.out.println("Sum: " + add(num1, num2));
        System.out.println("Difference: " + subtract(num1, num2));
        System.out.println("Product: " + multiply(num1, num2));
        System.out.println("Quotient: " + divide(num1, num2));
    }
}
```

- In this example, we have a `Calculator` class containing four methods for basic mathematical operations.
- The methods take two parameters, perform the corresponding operation, and return the result.

### 2. **Passing Parameters to Functions:**

#### Example:

```java
public class ParameterExample {
    // Method that takes parameters
    public static void printNumbers(int num1, int[] num2Array) {
        System.out.println("First number: " + num1);
        System.out.println("Second number from the array: " + num2Array[0]);
    }

    // Entry point
    public static void main(String[] args) {
        int x = 10;
        int[] numbers = { 20, 30, 40 };

        // Calling the method and passing parameters
        printNumbers(x, numbers);
    }
}
```

- The `printNumbers` method takes two parameters: `num1` (primitive type) and `num2Array` (array).
- In the `main` method, a variable `x` and an array `numbers` are created.
- When calling the `printNumbers` method, the values of variable `x` and a reference to the `numbers` array are passed.

### 3. **Returning Values:**

#### Example:

```java
public class ReturnValueExample {
    // Method returning the sum
    public static int add(int a, int b) {
        return a + b;
    }

    // Method returning a message
    public static String greet(String name) {
        return "Hello, " + name + "!";
    }

    // Entry point
    public static void main(String[] args) {
        // Calling the method and storing the result
        int sumResult = add(5, 3);
        System.out.println("Sum: " + sumResult);

        // Calling the method and outputting the result
        String greeting = greet("John");
        System.out.println(greeting);
    }
}
```

- The `add` method returns the sum of two integers.
- The `greet` method returns a string greeting with the provided name.

Using methods helps structure the code, makes it more readable, and reduces code duplication. Learning how to create and use methods is a crucial step in mastering Java programming.