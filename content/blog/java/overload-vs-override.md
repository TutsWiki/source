---
date: 2020-10-14
linktitle: Overloading and Overriding Methods
menu:
  main:
    parent: java
title: Method Overloading and Overriding in Java
weight: 3
url: /method-overload-vs-override-java-differences
description: 
keywords:
  - java
  - override
  - overload
tags: [Java]  
---
## Method Overloading
###  Introduction
- Overloading of methods means when the class defines more than one method with the same name but with different parameters.
- The parameters being different is the basic requirement for overloading of methods.
     
### Features
- It is performed within a single class.
- The parameters must be different, no two overloaded methods with the same name should have the same type of parameters.
- Overloaded methods are allowed to have different return types.
- As method overloading is resolved at compile time it is an example of **compile-time polymorphism**.
- It enhances the readability of a program as it improves the overall structure of the code.

### Method Overloading Example
File: Test.java
```java
import java.io.*;

class Addition
{
   void add(int c, int d)
   {
       System.out.println("The first ans is: "+ (c+d));
   }
   
   void add(double c, double d)
   {
      System.out.println("The second ans is: "+(c+d));
   }
}

public class Test {
    public static void main (String[] args) {
   
      Addition obj = new Addition();
      obj.add(20,30);
      obj.add(30.2,40.4);
    }
}      
```
Output:
```console
$ javac Test.java
$ java Test

The first ans is: 50
The second ans is: 70.6
```
### Explanation
When integers are passed as parameters the first add method is called which has integer data type as parameters and when decimal values are passed the second add method is called which has double data type as parameters.

## Method Overriding

### Introduction

* Overriding of the method means when a derived class (child class) defines a method with the same name as a base class(parent class) method.
* This allows a specific implementation of the method in child class which is already implemented in the parent class.

### Features

* It is performed in two classes one is parent class and other is the child class.
* The parameters, name of the method and return type all should be same in both the methods.
* Overriding is an example of **runtime polymorphism** as overriding of methods occurs at runtime.

### Method Overriding Example
File: Test.java
```java
import java.io.*;

class Master
{
   public void fnct()
   {
      System.out.println("Master Class");
   }
}
class Servant extends Master
{
   //Overriding method
    public void fnct()
    {
      System.out.println("Servant Class");
    }
}

public class Test{
   public static void main( String args[]) {
  Servant obj = new Servant();
           obj.fnct();
   }
}
```
Output:
```console
$ javac Test.java
$ java Test

 Servant Class
```

### Explanation
`Servant` is the child class of `Master` and the overridden method is `fnct()`. `obj` refers to an object of class `Servant`, and when it is used to call the method `fnct()`, the overridden method is executed from the class `Servant`.
