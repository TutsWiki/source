---
date: 2020-08-22
linktitle: Generics
menu:
  main:
    parent: java
next: /java/class-object
title: Generics in Java
weight: 26
url: /java/generics
description: In simple words, generic methods (or classes) are those methods that are written with single method declaration and can be called or accessed with arguments of different type.
keywords:
  - java
  - generics
  - wildcards
tags: [Java]  
---
Generics in Java is the facility which is provided to the user to make a single method or single class that can be compatible with any data type like a single method can operate on integer type or string type or even object type. If you are familiar with template in C++, then you can consider generics as a template in Java. In simple words, generic methods(or classes) are those methods that are written with single method declaration and can be called or accessed with arguments of different type.

Let's understand this with the help of an example. Suppose you have two house management system and out of which one has the facility that every new household work is done by different servants like sweeping is done by one servant, dishes are done by another servant, laundry is done by another servant, cooking is done by another servant and so on. The second option is that instead of having different servants, you can have only one servant which will do all the work like cooking, laundry, dishes, etc.

Now if you have the option to choose any one of the management systems, which one will you choose? It's quite obvious you will choose the second one because having many servants will be very inefficient and their management will be a cumbersome task. In the same way, Generics in Java works. Instead of having different methods and classes for doing the same task, we make only one method or class which will be compatible with any data type as Integer, Double, String, Employee (object type), etc.

## Syntax
To create object of generic class or methods we follow the below syntax:

`abc obj<Type1,Type2,...,Type n> = new abc<Type1,Type2,...,Type n>()`

Here `Type` can be replaced with any data type but remember that it cannot have primitive data types like int, double, etc. It should be of object type like `Integer`, `Double`, `String`, `Float`, `Employee` etc.

There is a general convention for this `Type` parameter:

- T is used for type.
- E is used for an element.
- N is used for numbers.
- K is used for the key.
- V is used for the value.

### Example
```java
public class test {
  public < E > void showArray(E[] s) {
    for (E x: s)
    System.out.println(x);
  }
  public static void main(String[] args) {
    test Obj = new test();
    String name[] = new String[] {
	  "ravi",
      "virat",
      "dhoni"
    };
    Integer roll[] = {
      10,
      23,
      44
    };
    Obj.showArray(name);
    Obj.showArray(roll);
  }
}
```

Output: 
```bash
ravi
virat
dhoni
10
23
44
```
In this example, type parameter `<E>` gets replaced by the string when called with string array and by integer when called by integer array. The type parameter is placed before the return type of the method.

We can have more than one parameter type in a method or class. Example:

```java
public class test {
  Public < E, N > void showArray(E x, N y) {
    System.out.println(x);
    System.out.println(y);
  }
  Public static void main(String[] args) {
    test obj = new test();
    obj.showArray("John Doe", 43);
  }
}
```
Output:
```bash
John Doe
43
```

## Generics in Class

Not only methods, but we can also have classes that can be of a generic type. For example-
```java
Public class test < T > {
  T i;
  Public void add(T i1) {
    i = i1;
  }
  public T get() {
    return (i);
  }
}
public class TestGen {
  public static void main(String[] args) {
    test < Integer > m1 = new test < Integer > ();
    test < String > m2 = new test < String > ();
    Integer k = new Integer(199);
    m1.add(k);
    m2.add("John Doe");
    System.out.println("m1 = " + m1.get() + "m2 = " + m2.get());
  }
}
```
OUTPUT:
```bash
m1=199 m2=John Doe
```
We used the same class test to print the two different types of data (`String` and `Integer`). Here also, we can have multiple parameter types.

## Wildcards

With wildcard, we can further enhance the usage and declaring generics functionality. We use a wildcard when we want to make a method or class that can accept any kind of data or collection or when we want to impose some restrictions or relaxation on a variable. Let’s take an example:

```java
void printCollection(Collection < Object > c) {
  for (Object e: c) {
    System.out.println(e);
  }
}
```
The above code will only accept the type object for functioning and will not accept any other type of collection. That's the point where wildcard comes to play its role. To make the method acceptable for any data collections, we will use the wildcard '?'.

```java
void printCollection(Collection<?> c) {
    for (Object e : c) {
        System.out.println(e);
    }
}
```

Now the above code will run for any type of collections like Integer, Double, Object, etc.

### Upper-bound Wildcards

It provides the freedom to the variable by either making it a specific type or subtype of the specific type. In java, it is declared by using the `?` keyword followed by the extend keyword. Extend keyword is used to provide access to the subtypes of the specific type to the methods. Example `list<? extends Number>` can be used to extend the type `Number` along with its subtype (`Integer`, `Double` etc.) whereas `list<Numbers>` restrict user with a list of type number only. Observe the below code for better understanding.

```java
import java.util.ArrayList;
public class UpperBoundWildcard {
  private static Double add(ArrayList < ?extends Number > num) {
    double sum = 0.0;
    for (Number n: num) {
      sum = sum + n.doubleValue();
    }
    return sum;
  }
  public static void main(String[] args) {

    ArrayList < Integer > l1 = new ArrayList < Integer > ();
    l1.add(10);
    l1.add(20);
    System.out.println("displaying the sum= " + add(l1));
    ArrayList < Double > l2 = new ArrayList < Double > ();
    l2.add(30.0);
    l2.add(40.0);
    System.out.println("displaying the sum= " + add(l2));
  }
}
```
OUTPUT:
```bash
displaying the sum= 30.0
displaying the sum= 70.0
```
In the above code, a list of integer and list of double is executed without any error.

### Unbounded Wildcards

Unbounded wild cards are used when we don't know what type of list will come as input. In that case, we make our method using `list<?>` which means that list can be of any type. We use it when we want our method independent from parameter type. Example:

```java
import java.util.Arrays;
import java.util.List;
public class UnboundedWildcard {
  public static void display(List < ?>list) {
    for (Object o: list) {
      System.out.println(o);
    }
  }
  public static void main(String[] args) {
    List < Integer > l1 = Arrays.asList(1, 2, 3);
    System.out.println("displaying the Integer values");
    display(l1);
    List < String > l2 = Arrays.asList("One", "Two", "Three");
    System.out.println("displaying the String values");
    display(l2);
  }
}
```
OUTPUT:
```bash
displaying the Integer values
1
2
3
displaying the String values
One
Two
Three
```

The above code worked well for both types of lists i.e. integer and string. This is the advantage of unbounded wildcard

### Lower-bound Wildcards

We use lower bound wildcards when we want to work on a specific type or superclass of that type like integers and its superclass numbers. It is used by declaring wildcard symbol `?` followed by the `super` keyword. Consider the following example:

```java
import java.util.Arrays;
import java.util.List;
public class LowerBoundWildcard {
  public static void addNumbers(List < ?super Integer > list) {
    for (Object n: list) {
      System.out.println(n);
    }
  }
  public static void main(String[] args) {
    List < Integer > l1 = Arrays.asList(1, 2, 3);
    System.out.println("displaying the Integer values");
    addNumbers(l1);
    List < Number > l2 = Arrays.asList(1.0, 2.0, 3.0);
    System.out.println("displaying the Number values");
    addNumbers(l2);
  }
}  
```
OUTPUT:
```bash
displaying the Integer values
1
2
3
displaying the Number values
1.0
2.0
3.0
```
In the above code, we declared the list type be integers but by using the `super` keyword we made the method acceptable for superclass of integers. That's why we were able to print double values too because superclass of integers i.e. numbers contains `double` class.

## Advantage of Generics

- Reduced redundancy: It saves the user from typing the same code multiple times. Makes the code more readable and reduces the complex code into simpler code.
- Type safety: It provides the user the type safety facility i.e. a user can store only a single type of object. For example- a list can be either of integer type or string type `({1,2,3}` or `{"one","two","three"})` but list like `{1 ,2," three"}` is not acceptable. Observe the below code for better understanding.
```java
List list = new Arraylist();
list.add(50);
list.add("hello");
List < Integer > list = new Arraylist < Integr > ();
list.add(10);
list.add("10"); //compile time error because type safety not followed
```
- Compile time error detection: A user mainly prefer to detect the error at compile time rather than run time because error handling at compile time is far easier than that at run time. And this facility is provided by generics to the user.
- Free from typecasting: While accessing the member of the list, one doesn't need to typecast the object like one used to do before without generics. 

```java
List list =new ArrayList();
list.add(“hello”);
String s=(String) list.get(0);//typecasting
//After generics, we don’t need typecasting.
List<String> list=new ArrayList<String>();
List.add("hello");
String s=list.get(0);
```


