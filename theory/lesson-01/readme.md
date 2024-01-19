Let's start with a simple Java program. We'll create a program that greets the user. Here's an example code:

```java
public class HelloWorld {
    public static void main(String[] args) {
        // Print a greeting to the console
        System.out.println("Hello, World!");
    }
}
```

Now, let's break down what's happening:

- `public class HelloWorld`: This defines a class named "HelloWorld." In Java, every program consists of at least one class.

- `public static void main(String[] args)`: This is the `main` method, which serves as the entry point for the program. All Java programs start execution from this method.

- `System.out.println("Hello, World!");`: This line of code prints the text "Hello, World!" to the console. The `println` method is responsible for printing the string and adds a newline at the end.

Now, let's see how to run this program:

1. Write this code in a text editor such as Notepad, Visual Studio Code, Eclipse, or IntelliJ IDEA.

2. Save the file with the ".java" extension, for example, "HelloWorld.java."

3. Open the command prompt (or terminal) and navigate to the directory where your file is located.

4. Compile the program by entering `javac HelloWorld.java` in the command prompt.

5. Run the compiled program by entering `java HelloWorld`.

You should see the output "Hello, World!" on the screen.

This is a simple program that serves as a great starting point for beginners. Gradually, you can explore more advanced concepts such as variables, conditional statements, loops, functions, and object-oriented programming (OOP).

---

Agenda:

1. Variables and types
2. Switch and case. Try, catch and finally

Let’s start with the general and important things that you really should have learned earlier. Java, like many languages, has two types of variables:

Primitives:
[alt text](img/1*sY7Bhksyl9qRUpg0npCWog.webp)  

Non-Primitive: String, Arrays and Classes.

Usually we need to compare variables, but what methods should we use? “equals” or “== “ ? Let’s look at all the comparison methods available to us:
```dtd
==      equal to
!=      not equal to
>       greater than
>=      greater than or equal to
<       less than
<=      less than or equal to
```

This tricky question was asked in the interview. We can use it with classes, but:
```dtd
// Integer != int
int a = 1, b = 1;
System.out.println(a == b); // true

int c = 1, j = c;
System.out.println(c == j); // true

Integer ai = Integer.valueOf(1), bi = Integer.valueOf(1);
System.out.println(ai == bi); // true 

Integer ci = Integer.valueOf(128), ji = Integer.valueOf(128);
System.out.println(ci == ji); // false
/* You can always count on the fact that 
*  for values between -128 and 127, 
*  you get the identical Integer objects after autoboxing, 
*  and on some implementations you might get identical objects
*  even for higher values.
*/
```

It’s important to learn that the Integer class is not a primitive value, because it is a regular class. Class Object is the root of the class hierarchy. Every class has Object as a superclass. All objects, including arrays, implement the methods of this class.
The “==” operator compares the values of two object references to see if they refer to the same thing. For an object we should use “equals”:

```dtd
// Indicates whether some other object is "equal to" this one.
public boolean equals(Object obj)
```

Or Comparable interface for natural ordering (if you need to sort you collection with own type of object):
```dtd
public class HugePoint implements Comparable<HugePoint> {

    // same as before

    @Override
    public int compareTo(HugePoint otherHugePoint) {
        return Integer.compare(getSize(), otherHugePoint.getSize());
    }

}
```

---- 
**Switch and case. Try, catch and finally**


When we have too many conditions, and they are all tied to the same type of variable in “if-then-else”, it is worth using switch-case.
```dtd
// case 
int expression = 9;
switch(expression) {
    case 2:
    System.out.println("Small Size");
    break;

    case 3:
    System.out.println("Large Size");
    break;

    // default case
    default:
    System.out.println("Unknown Size");
}
```
Also, up Java to version 19 or more and we can use expression:
```dtd
Day day = Day.WEDNESDAY;
var resultDay = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
    default -> throw new IllegalStateException("Invalid day: " + day);
} 

System.out.println(resultDay);    
```
