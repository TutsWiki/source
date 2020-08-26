---
date: 2020-08-22
linktitle: Constructors
menu:
  main:
    parent: java
next: /java/constructors
title: Constructors
weight: 26
url: /java/constructors
description: A constructor initializes an object when it is created. In Java, we use the new keyword to initialize an object whose class/type is not of the primitive types.
keywords:
  - java
  - class
  - object
  - constructor
tags: [Java]  
---
## Object Initialization
We know that in Java classes are **blueprints** and objects are the **actual entities** created by following the design of the blueprint. To create an object we need to initialize it or in simpler terms assign resources like memory, initialize instance variables, load methods, etc. Without doing this Java assigns the object a null value on the declaration of the object. In Java, we use the new keyword to initialize an object whose class/type is not of the primitive types. The primitive types are a special case and are not mapped to null values on their declaration. They are assigned the following default values:

| Type    | Default Value |
|---------|---------------|
| boolean | false         |
| byte    | (byte) 0      |
| short   | (short) 0     |
| int     | 0             |
| long    | 0L            |
| char    | \u0000        |
| float   | 0.0f          |
| double  | 0.0d          |

```java
public class square {
  int perimeter;
  public square() {
    this.perimeter = 10;
  }
  public static void main(String[] args) {
    Square square_1; // null object
    square_1 = new square(); // square object
  }
}
```

In the above code, the initial declaration of square creates a null object but after using the new keyword and initializing it, it gets converted into a square object, and then we can treat it like a square object and use its member fields and functions.

### Where does object initialization start?
The whole process of starting up the initialization happens in the constructor of the class. The constructor is similar to member functions of the class but not completely. When the compiler enters the constructor all the resource allocation for that object happens in the background and its instance variables are given default values if they are of primitive type or a null value if otherwise. Also, we can use the constructor to initialize the instance variables of the class for that object explicitly. In this way, the constructor completely initializes the object.

## Constructors
As mentioned above constructors can be considered as special methods which have the following properties:

1. No return type
2. Are called implicitly during object creation
3. Cannot have private access
4. Have the same name as that of the class
5. Are not members of the class and hence cannot be inherited

There are different types of constructors. Let's see them one by one. 

### Non Parameterized Constructor
It is an explicit constructor which is declared in the class and does not have any parameters passed to it. It is used to initialize the data members with default values. The code above illustrates a non parameterized constructor.

### Parameterized Constructor
It is an explicit constructor that is declared in the class and has parameters passed to it. It is used to initialize the data members with a particular set of given values.

```java
public class square {
  int perimeter;
  public square(int val) {
    this.perimeter = val;
  }
  public static void main(String[] args) {
    Square square_1; // null object
    square_1 = new square(1); // square object
  }
}
```
The code above illustrates a non parameterized constructor.

### Copy Constructor
It is a subtype of parameterized constructors. Say we are initializing an object O1. Here in the parameters, we pass an already initialized object, say `O2` of the same class to the constructor during the initialization of `O1`. The required data members associated with `O2` are copied into the respective data members of `O1`. The copy constructor cannot exist by itself. It requires either a traditional parameterized or non-parametrized constructor to be written along with it. This is an example of **constructor overloading**.

```java
public class square{
int perimeter;
public square(int val)
 {
 this.perimeter=val;
 }
 public square(square s1)
 {
 this.perimeter=s1.perimeter;
 }
 public static void main(String []args)
 {
 Square square_1;
 Square square_2;
 square_1=new square(1);
 square_2=new square(square_1);
 }
}
```
Here when `square_2` is called, the overloaded copy constructor is called and not the parameterized constructor.

### Default Constructor
If none of the above constructors is declared by the programmer, then Java provides an inbuilt constructor that is implicit and assigns the data members with default values as seen in the table under Object Initialization. 

## Calling one constructor from another
It is possible to access another constructor of the same class from an overloaded constructor of the same class. This is achieved by using `this` keyword.
```java
public class square {
  int perimeter;
  int sides;
  public square() {
    sides = 4;
  }
  public square(this.val) {
    this();
    this.perimeter = val;
  }
  public static void main(String[] args) {
    Square square_1;
    Square square_2;
    square_1 = new square(1);
    square_2 = new square(square_1);
  }
}
```
For example, in the above code, the parameterized constructor is calling the non-parameterized one.

## Inheritance and Constructor
As constructors are not inherited, to access parent class constructors we use super keyword. Hence to access the constructor of the parent class, we have to call it explicitly by using the super keyword if the parent class has a parameterized constructor else the non-parameterized constructor or the default constructor, if it exists, of the parent, class is called by default by Java.

```java
public class shape {
  String color;
  public shape(String color) {
    this.color = color;
  }
}
public class square extends shape {
  int perimeter;
  public square(int val, String color) {
    super(color)
    this.perimeter = val;
  }
}
```
The above code illustrates this behaviour.
