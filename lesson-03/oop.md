### Java Helper. Part 2. OOP and SOLID.

Object-oriented programming (OOP) is a programming methodology representing a program as a collection of objects, each being an instance of a class forming an inheritance hierarchy.

read full: [link](https://medium.com/p/136da816ecbf/edit)

In my view, four key points stand out:
1. **Encapsulation:** Hides implementation details, revealing only what is necessary for use. Example:

    ```java
    public class Connector {
        public String createConnection(String host) {
            var newConnection = openConnection();
            // todo: do some work
            return newConnection;
        }

        private String openConnection() {
            // todo: do some work
            return "ip address";
        }
    }
    ```

2. **Inheritance:** Describes a new class based on an existing one with borrowed functionality. Example:

    ```java
    public abstract class AbstractConnector {
        // ... fields ...

        public void connect() {
            // ... implementation ...
        }

        public void disconnect(String host) {
            // ... implementation ...
        }
    }

    public class MobileConnector extends AbstractConnector {
        // ... additional fields ...

        @Override
        public void connect() {
            // ... custom implementation ...
        }
    }
    ```

3. **Polymorphism:** Allows using objects with the same interface without knowledge of internal structure. Example:

    ```java
    public class Office {
        public void connectToAnotherOffice(AbstractConnector connector) {
            connector.connect(); // using abstract method
        }
    }
    ```

4. **Abstraction:** Highlights common characteristics of an object. Example:

    ```java
    public abstract class AbstractConnector {
        abstract String ping();
        abstract void drop();
    }
    ```

   Use interface if only abstract methods are needed.

SOLID Principles:
1. **Single Responsibility:** A class should have only one responsibility.

    ```java
    public class Book {
        // ... fields ...

        // ... methods ...
    }

    public class Car {
        // ... fields ...

        // ... methods ...
    }

    public class Delivery {
        public void createOrder() {
            Book book = new Book("Star wars");
            Car car = new Car("123");
            doSomeAction(book, car);
        }
    }
    ```

2. **Open/Closed:** Classes should be open for extension but closed for modification.

    ```java
    public class Office {
        // ... fields ...

        // ... methods ...
    }

    public class OfficeWithGym extends Office {
        private Gym gym;
        // ... additional fields ...
    }
    ```

3. **Liskov Substitution:** Sub-class should replace its base class.

    ```java
    public interface Connector {
        void connect();
        void disconnect();
    }

    public class InternetConnector implements Connector {
        // ... implementation ...
    }

    public class PhoneConnector implements Connector {
        // ... implementation ...
    }

    public class Router {
        Connector connector;

        public Router(Connector connector) {
            this.connector = connector;
        }
    }
    ```

4. **Interface Segregation:** Larger interfaces should be divided into smaller ones.

    ```java
    public interface Connector {
        void connect();
        void disconnect();
    }

    public interface HttpConnector extends Connector {
        void setAddress(String host);
        void setPort(int port);
    }

    public interface TelephoneConnector extends Connector {
        void setNumber(String number);
    }
    ```

5. **Dependency Inversion:** High-level and low-level modules depend on abstractions.

    ```java
    public interface Manager { }

    public class OldTownOffice {
        private final Manager manager;
        private final Connector connector;

        public OldTownOffice(Manager manager, Connector connector) {
            this.manager = manager;
            this.connector = connector;
        }
    }

    public class JoseManager extends Manager { }
    ```

That's all. Thanks and good luck! 🍀