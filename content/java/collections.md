---
date: 2020-10-20
linktitle: Collections
menu:
  main:
    parent: java
next: /java/class-object
title: Collections in Java
weight: 31
url: /java/collections
description: Collections in Java is not a single library, but it is a framework. It provides some data structures and algorithms that make development easier while providing better performance and speed.
keywords:
  - java
  - collections
tags: [Java]  
---
<meta property="og:image" content="https://tutswiki.com/images/DSA/Quick-Sort-Example-Array-1.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Collections in Java" />
<meta name=”twitter:description” content="Collections in Java is not a single library, but it is a framework. It provides some data structures and algorithms that makes development easier while providing better performance and speed." />

As the name suggests, a **collection is an object that encapsulates multiple elements** into a single unit and algorithms to manage them.

Collections in Java is not a single library but **it is a framework**. It provides some data structures and algorithms that make development easier while providing better performance and speed.

Collections Framework is composed of

- **Interface**: Interfaces are abstract data types which are implemented to create different collections. For example: `java.util.Map`, `java.util.List` etc.
- **Implementation**: These are the classes that implement that aforementioned Interfaces. For example: `java.util.HashMap`, `java.util.ArrayList` etc.
- **Algorithms**: These are the methods used to manage collections or perform other computations like search, sort etc. These algorithms are polymorphic, i.e, the same algorithm works for different types of Collections.<br /> 

    Example: `binarySearch()` method of `java.util.Collections` is used to search an element in a `List`.   It also works on List implementations like `ArrayList` and `LinkedList`.
 
> Java Collections Framework is very much similar to the C++ STL (Standard Template Library).

## Prerequisites for Collection

- [**Java Generics**](/java/generics/): As Collection allows the arguments of different types, prior knowledge of Generics is required.
- **Inheritance**: Since all the Data Structures in Collection extends or implements other Base Classes or Interfaces respectively.

## Types of Collections

Different ways to classify the data structures of Collection Framework:

- Modification as the ability: Modifiable and Unmodifiable.
- The ability to change: Mutable and Immutable.
- Size variability: Fixed Size and Variable Size.
- Element access: Random Access and Sequential Access.
- Declaration: `public interface List<E> extends Collection<E>`
- Library: `java.util.List`

> Collections can also be classified by interfaces that they implement.

## Lists

- `List` is an interface for a sequential collection of data.
- It maintains the order of the elements in which they are inserted.
- It provides the functionality to access, insert or delete an element from any index. This property is called **Random Access**.
- `List` is provided by `java.util` Package.

## Popular Implementations of List

### Array List

- `ArrayList` is one of the implementations of `List`.
- Unlike the common Java Arrays, `List` is Resizable.
- It allows `null` values and duplicity of elements.
- `ArrayList` is not synchronised. Its synchronised equivalent is `Vector` of the `java.util` package.
- It extends the `AbstractList` which implements the `List` interface. 
  So, all the functionalities of `List` are provided by `ArrayList`.

#### Example

```java
import java.util.ArrayList;
import java.util.List;

public class ListExample {
    public static void main(String args[]) {
        ArrayList<String> colorList = new ArrayList<String>();
        colorList.add("Red");
        colorList.add("Green");
        colorList.add("Blue");
        System.out.println("List of the Colours before adding Yellow: " + colorList);
        colorList.add("Yellow");
        System.out.println("List of the Colours after adding Yellow: " + colorList);
    }
}
```

Output:

```console
List of the Colours before adding Yellow: [Red, Green, Blue]
List of the Colours after adding Yellow: [Red, Green, Blue, Yellow]
```

### Linked List

- `LinkedList` is another implementation of `List` interface.
- It is a concrete implementation of the abstract data type **Doubly-Linked List**.
- Unlike `ArrayList`, `LinkedList` provides better performance in adding and removing the elements.
- Like `ArrayList`, `LinkedList` is also not synchronised.
- `LinkedList` can also work like a Queue (First In, First Out) by using `addLast()`, `peekFirst()` and `removeFirst()`.

## Set

- `Set` is also an interface for a sequential collection of data.
  But unlike `List`, `Set` doesn't maintain the order of the data.
- It doesn't allow duplicate values.
- Depending on the implementation, `Set` may allow zero or one `null` value.
- It may require additional logic for allowing mutable elements.
- `Set` is provided by `java.util` package.

## Popular Implementations of Set

### Hash Set

- `HashSet` is an implementation of `Set` and as the name suggests, it uses `HashTable` for storing its elements.
- `HashSet` doesn't maintain any consistent ordering for storing elements.
- It allows a single `null` value.
- It is not synchronised.
- `HashSet` doesn't allow indexing, which means we cannot use `[n]` to extract elements. 
  To Iterate over a set, we need an `Iterator` Object for `HashSet`.

#### Example

```java
import java.util.Set;
import java.util.HashSet;

class HashSetExample {
    public static void main(String args[]) {
        Set<String> colorSet = new HashSet<>();
        colorSet.add("Red");
        colorSet.add("Blue");
        colorSet.add("Yellow");
        System.out.println("Before Adding Green: " + colorSet);
        colorSet.add("Green");
        System.out.println("After Adding Green: " + colorSet);
        colorSet.add("Red");
        System.out.println("After Re-adding Red: " + colorSet);
        colorSet.remove("Yellow");
        System.out.println("After Removing Yellow: " + colorSet);
    }
}
```

Output:

```console
Before Adding Green: [Red, Blue, Yellow]
After Adding Green: [Red, Blue, Yellow, Green]
After Re-adding Red: [Red, Blue, Yellow, Green]
After Removing Yellow: [Red, Blue, Green]
```

Explanation: When we tried to re-enter the value `Red` in the set, it is not duplicated in the set.

### Tree Set

- `TreeSet` is also an implementation of `Set`.
- It allows us to iterate over elements in ascending order.
- Each element of `TreeSet` must implement the `Comparable` Interface. All `String`, `BigInteger` and all wrapper classes implement the `Comparable` interface implicitly.
- `TreeSet` is not Synchronised.
- Because of sorted order, `TreeSet` provides additional methods like `subSet(from, to)` to get elements within a specific range, `headSet(n)` or `tailSet(n)` to get elements less than or greater than the passed value (i.e. `n` here), etc.

#### Example

```java
import java.util.TreeSet;

class TreeSetExample {
    public static void main(String args[]) {
        TreeSet<String> colorSet = new TreeSet<>();
        //First Section
        colorSet.add("Red");
        colorSet.add("Blue");
        colorSet.add("Yellow");
        colorSet.add("Green");
        System.out.println("Before Adding Orange: " + colorSet);
        colorSet.add("Orange");
        System.out.println("After Adding Orange: " + colorSet);
        colorSet.add("Red");
        
        //Second Section
        System.out.println("After Re-adding Red: " + colorSet);
        colorSet.remove("Yellow");
        System.out.println("After Removing Yellow: " + colorSet);

        //Third Section
        System.out.println("\nRange Based methods");
        System.out.println("Elements before Orange: " + colorSet.headSet("Orange"));
        System.out.println("Elements before Orange: " + colorSet.tailSet("Orange"));
        System.out.println("Elements in between Blue to Red: " + colorSet.subSet("Blue", "Red"));
    }
}
```

Output:

```console
Before Adding Orange: [Blue, Green, Red, Yellow]
After Adding Orange: [Blue, Green, Orange, Red, Yellow]
After Re-adding Red: [Blue, Green, Orange, Red, Yellow]
After Removing Yellow: [Blue, Green, Orange, Red]

Range Based methods
Elements before Orange: [Blue, Green]
Elements before Orange: [Orange, Red]
Elements in between Blue to Red: [Blue, Green, Orange]
```

Explanation:

- In the First Section, we have initialised an empty `TreeSet` and added four colour names to it. On printing the set, we can see that set is ordered (alphabetically in case of strings) irrespective of the order in which the elements are inserted.
- In the Second Section, we tried to insert `"Red"` again, but due to the property of `Set`, it is not entered again. Even after removing an element, `colourSet` maintains an alphabetical order.
- In the Third Section, methods specific to `TreeSet` are used. These methods are used to get a subset of original set based on the range passed.

## Map

- This is an interface for storing key-value pairs. Here, one `object(*value*)` is associated with another `object(*key*)` which is used to index the previous `object(*value*)`.
- `Keys` of `Map` must be unique. If `Keys` are mutable, then `Map` may require additional logic to maintain consistency.

## Popular Implementations of Map

### Hash Map

- `HashMap` is one of the implementations of `Map` interface.
- It allows `null` as keys and values.
- Same "values" can exist in a `Map` but same "keys" cannot. So, the same "value" can exist with different keys in a `Map`.
- `HashMap` provides constant average time complexity for basic operations like get or put methods.
- It is not Synchronised.

#### Example

```java
import java.util.HashMap;

public class HashMapExample {
    public static void main(String[] args) {

        // First Section
        HashMap<Integer, String> shapes = new HashMap<>();
        shapes.put(3, "Triangle");
        shapes.put(4, "Sqaure");
        shapes.put(5, "Pentagon");
        System.out.println("Shapes before adding Dodecagon: " + shapes);
        shapes.put(12, "Dodecagon");
        System.out.println("Shapes after adding Dodecagon: " + shapes);
        shapes.put(4, "Rectangle");
        System.out.println("Shapes after adding Rectangle: " + shapes);

        // Second Section
        System.out.println(String.format("Shape with %d sides is %s", 12, shapes.get(12)));
        System.out.println(String.format(
            "Shape with %d sides is %s", 15, shapes.getOrDefault(15, "\"Not Found\"")));

        // Third Section
        System.out.println("Shapes with the following edges are saved in `Map`: " + shapes.keySet());
        System.out.println("Shapes with the following names are saved in `Map`: " + shapes.values());

    }
}
```

Output:

```console
Shapes before adding Dodecagon: {3=Triangle, 4=Sqaure, 5=Pentagon}
Shapes after adding Dodecagon: {3=Triangle, 4=Sqaure, 5=Pentagon, 12=Dodecagon}
Shapes after adding Rectangle: {3=Triangle, 4=Rectangle, 5=Pentagon, 12=Dodecagon}
Shape with 12 sides is Dodecagon
Shape with 15 sides is "Not Found"
Shapes with the following edges are saved in `Map`: [3, 4, 5, 12]
Shapes with the following names are saved in `Map`: [Triangle, Rectangle, Pentagon, Dodecagon]
```

Explanation:

- In the First Section, we have initialised an empty map with "keys" as the number of edges and "values" as the name of shapes. Then, we added a few shapes in the map and printed them.
- Now, we tried to insert a shape "Rectangle" with the existing key "4", so it replaced the existing value "Square" of the Map.
- In the Second Section, we used two different methods to get values from the map. The first method `get()` is used to get values from the Map by using keys as a parameter. If keys are not present, it returns `null`. Second method `getOrDefault()` is used to provide a default value in case the key doesn't already exist.
- In the third section, we used the methods to list keys and values separately.

> As discussed above, apart from data structures **Collections** framework also provides some algorithms.
  The following section will explain some of the provided algorithms in detail.

## Algorithms

### Binary Search

- `Collections` provides an implementation for the **Binary Search** algorithm.
- Binary Search algorithm finds an element in a sorted list in `O(log(n))` time complexity.
- `binarySearch()` method takes a list and a key as a parameter and finds the index on which "key" is present in the passed "list". This method returns a negative value if the provided "key" is not present in the "list".
- `binarySearch()` method also allows an optional third parameter, which can be passed for custom comparators. This comparator will be used to compare two objects.

#### Example
  
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class BinarySearchExample {
  public static void main(String[] args) {

	  List<Integer> numbers = new ArrayList<>();

	  numbers.add(12);
	  numbers.add(21);
	  numbers.add(32);
	  numbers.add(43);
	  numbers.add(54);

	  System.out.println("List: " + numbers);

	  System.out.println(String.format(
		  "Found %d in numbers list on index %d", 
		  43, 
		  Collections.binarySearch(numbers, 43)));
  }
}
```

Output:

```console
List: [12, 21, 32, 43, 54]
Found 43 in numbers list on index 3
```

Note: Check out our article on [Binary Search](/data-structures-algorithms/binary-search/) if you want to implement it from scratch.

### Sort

- `Collections` provides `sort()` method to sort a list.
- This will sort the passed list in the ascending order.
- This method takes a `List` as a parameter and sorts it. Optionally, it also takes a second parameter for Custom Comparator.
- The sorting used in the algorithm is stable, which means that two equal elements will maintain the same order as the original list.

#### Example
  
```java
import java.util.Collections;
import java.util.ArrayList;

public class SortExample {
  public static void main(String[] args) {

	  ArrayList<Integer> numbers = new ArrayList<>();

	  numbers.add(43);
	  numbers.add(12);
	  numbers.add(54);
	  numbers.add(32);
	  numbers.add(21);

	  System.out.println("Original List: " + numbers);

	  Collections.sort(numbers);
	  System.out.println("Same List in Ascending Order: " + numbers);

	  // Here, we have used "reverseOrder()" Comparator provided by Collection Framework
	  // It is used to reverse sort the list
	  Collections.sort(numbers, Collections.reverseOrder());
	  System.out.println("Same List in Descending Order: " + numbers);
  }
}
```

Output:

```console
Original List: [43, 12, 54, 32, 21]
Same List in Ascending Order: [12, 21, 32, 43, 54]
Same List in Descending Order: [54, 43, 32, 21, 12]
```

### Min and Max

- `Collections` also provides methods to find the minimum and maximum value (using `min()` and `max()` method respectively) in a List.
- The taken by these methods to produce results will be proportional to the size of the Collection passed to them.

#### Example
  
```java
import java.util.ArrayList;
import java.util.Collections;

public class MinMaxExample {
  public static void main(String[] args) {
	  ArrayList<Integer> numbers = new ArrayList<>();

	  numbers.add(12);
	  numbers.add(21);
	  numbers.add(32);
	  numbers.add(43);
	  numbers.add(54);

	  System.out.println("List: " + numbers);

	  System.out.println("Minimum value of the numbers list: " + Collections.min(numbers));
	  System.out.println("Maximum value of the numbers list: " + Collections.max(numbers));
  }
}
```

Output:

```console
List: [12, 21, 32, 43, 54]
Minimum value of the numbers list: 12
Maximum value of the numbers list: 54
```

> Apart from these methods, Collections Framework provides a ton of other useful methods to perform a variety of operations like

- `swap()` to swap two elements of a list
- `copy()` to copy elements of a list to another
- `frequency()` to count the occurrence of passed value/object
- `shuffle()` to shuffle elements of a list; and many more.

## User-Defined Objects in a Collection

As discussed above, a collection can support any type of objects. The following example will show how to use user-defined objects in Data Structures and Algorithms provided by Collection Framework.

### Example

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

class Student {
    private String name;
    private Integer marks;

    public Student(String name, Integer marks) {
        this.name = name;
        this.marks = marks;
    }

    public Integer getMarks() {
        return this.marks;
    }

    public String getName() {
        return this.name;
    }

    @Override
    public String toString() {
        return String.format("{%s got %d marks}", this.name, this.marks);
    }
}

/**
Custom Comparator to compare two student objects by their marks
**/
class GradeComparator implements Comparator<Student> {
    @Override
    public int compare(Student s1, Student s2) {
        return Integer.compare(s1.getMarks(), s2.getMarks());
    }
}

public class SortStudents {
    public static void main(String[] args) {

        ArrayList<Student> studentList = new ArrayList<>();

        studentList.add(new Student("Student 1", 81));
        studentList.add(new Student("Student 2", 91));
        studentList.add(new Student("Student 3", 95));
        studentList.add(new Student("Student 4", 75));
        studentList.add(new Student("Student 5", 81));

        System.out.println("Original Student List: " + studentList);

        Collections.sort(studentList, new GradeComparator());

        System.out.println("After Sorting Student List: " + studentList);
    }
}
```

Output:

```console
Original Student List: [{Student 1 got 81 marks}, {Student 2 got 91 marks}, {Student 3 got 95 marks}, {Student 4 got 75 marks}, {Student 5 got 81 marks}]
After Sorting Student List: [{Student 4 got 75 marks}, {Student 1 got 81 marks}, {Student 5 got 81 marks}, {Student 2 got 91 marks}, {Student 3 got 95 marks}]
```

Explanation:

- `Student` class is the class whose objects we need to store in the list.
- This class has two private data members that are exposed by corresponding getters to other classes.
- This class also have a method that overrides the default `toString()` method for better readability.
- `GradeComparator` class is used to create a custom comparator for the `Student` class. It is used to compare the grades of two Students and used by the `sort` method.
- In the `main()` method, an empty list of `Student` is created which is later populated by the information of 5 students.
- Then, `studentList` is sorted by `Collection.sort()` method using a custom comparator `GradeComparator()`. As described in the *Sort* section, `sort()` method sorts the passed list using the passed comparator.
- Since `Student 1` and `Student 5` have equal marks `81`, they are placed adjacent to each other in the sorted list and maintains the order of the original list. This shows that the `sort()` method uses a stable sort.

## Benefits of Collections Framework

- **Makes programming easier**: Collections provide some of the most used data structures and algorithm. Thus, it reduces the development time and efforts and doesn't require explicit testing. User does not need to implement everything from scratch.
- **Better performance**: Collections provides better performance as compared to the custom-built algorithms.
- **Highly reusable**: All the Data structure and Algorithms are Generics and hence works for almost all type of Objects.
- **Easy to Learn**: Java has extensive [documentation for Java Collections Framework](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html) and, because of its huge popularity, there are a ton of resources to learn Collections Framework.

## Key Terms

- **Synchronised**: Synchronised Objects are the objects which are safe to use in a multi-threaded application. Synchronised method restricts simultaneous updates and maintains consistency over threads. This can be achieved by mutual exclusion that makes access to an object, exclusive for a single thread. 
- **Random Access**: Random Access means the user can access any element irrespective of its order. Like in Arrays, user can access any index anytime, but to access any index in a Linked List, the user must access its predecessor element unless it is a HEAD.
- **Constant Average Time Complexity**: It means that size of the list will not affect the time of execution of any operation.
