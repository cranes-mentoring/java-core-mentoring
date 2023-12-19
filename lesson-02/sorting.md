Sorting collections in Java can be achieved using various approaches. Here are examples for sorting each type of collection:

1. **Sorting an ArrayList:**
   ```java
   import java.util.ArrayList;
   import java.util.Collections;
   
   public class ArrayListSortingExample {
       public static void main(String[] args) {
           ArrayList<Person> persons = new ArrayList<>();
           // Populate the ArrayList with Person objects

           // Sorting in natural order (Comparable interface)
           Collections.sort(persons);

           // Sorting with a custom comparator (Comparator interface)
           // Example: Sorting by age in descending order
           Collections.sort(persons, Collections.reverseOrder(Comparator.comparingInt(Person::getAge)));
       }
   }
   ```

2. **Sorting a LinkedList:**
   ```java
   import java.util.LinkedList;
   import java.util.Collections;
   
   public class LinkedListSortingExample {
       public static void main(String[] args) {
           LinkedList<Person> persons = new LinkedList<>();
           // Populate the LinkedList with Person objects

           // Sorting in natural order (Comparable interface)
           Collections.sort(persons);

           // Sorting with a custom comparator (Comparator interface)
           // Example: Sorting by name in ascending order
           Collections.sort(persons, Comparator.comparing(Person::getName));
       }
   }
   ```

3. **Sorting a HashSet (Convert to ArrayList for Sorting):**
   ```java
   import java.util.HashSet;
   import java.util.ArrayList;
   import java.util.Collections;
   
   public class HashSetSortingExample {
       public static void main(String[] args) {
           HashSet<Person> personSet = new HashSet<>();
           // Populate the HashSet with Person objects

           // Convert HashSet to ArrayList for sorting
           ArrayList<Person> persons = new ArrayList<>(personSet);

           // Sorting in natural order (Comparable interface)
           Collections.sort(persons);

           // Sorting with a custom comparator (Comparator interface)
           // Example: Sorting by age in ascending order
           Collections.sort(persons, Comparator.comparingInt(Person::getAge));
       }
   }
   ```

4. **Sorting a TreeMap (SortedMap):**
   ```java
   import java.util.TreeMap;
   import java.util.Map;
   import java.util.Collections;
   
   public class TreeMapSortingExample {
       public static void main(String[] args) {
           TreeMap<String, Person> personMap = new TreeMap<>();
           // Populate the TreeMap with Person objects

           // Convert TreeMap entries to ArrayList for sorting
           ArrayList<Map.Entry<String, Person>> entryList = new ArrayList<>(personMap.entrySet());

           // Sorting with a custom comparator (Comparator interface)
           // Example: Sorting by name in descending order
           Collections.sort(entryList, Collections.reverseOrder(Comparator.comparing(e -> e.getValue().getName())));

           // Now entryList contains sorted entries, and you can reconstruct the sorted TreeMap if needed
       }
   }
   ```

These examples demonstrate sorting collections using the `Collections.sort()` method along with either the natural order (implementing the `Comparable` interface in the `Person` class) or a custom comparator for more complex sorting criteria.
