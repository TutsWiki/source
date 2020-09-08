---
date: 2020-09-01
linktitle: Packages
menu:
  main:
    parent: java
title: Packages in Java
weight: 8
url: /java/packages
description: Regular expression in simple terms is a special sequence of symbols and alphanumeric characters that defines a search pattern.
keywords:
  - java
  - packages
tags: [Java]  
---
## 1. Java Packages

### 1.1 Introduction
Java Package in simple terms can be defined as a folder which may contain similar types of subfolders, with similar type of interfaces and classes.

### 1.2 Features
- Package names must be unique as no two packages can have the same names.
- We use packages in java to increase the readability , reusability and organization of our code we use packages.
- They classify interfaces and classes such that they can easily be maintained and all the things are stored in an orderly manner.
- They provide access protection as access modifiers help to provide scope for particlar class methods and data members .
- A package might contain other sub-packages also.

## 2. Importing Packages
- By importing packages into our program we will able able to use all the classes , interfaces etc. present in that package in our program.   
- It depends on the user whether to import all the classes from a particular package or just a particular
class from a package.

Example: [java.lang](https://docs.oracle.com/javase/7/docs/api/java/lang/package-summary.html) is a package with many classes, interfaces etc. some of which are [String Class](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html), [Number Class](https://docs.oracle.com/javase/7/docs/api/java/lang/Number.html), [Thread Class](https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html).

### Importing a particular Class from Package
`import java.lang.String;` This statement will import only the `String` class from the package `java.lang`.

### Importing all the Classes from Package
`import java.lang.*;` The asterisk symbol is used to import all the classes, interfaces from the package `java.lang`.

## 3. Types Of Packages

- **Built-In Packages**  
These are the packages which are already defined and are used by programmers throughout the world. Some Examples are

  - **lang** - It contains language supporting classes. This class is automatically imported in java and is default in every java program.
  - **io** - It contains classes for input and output operations. Like getting input from keyboard or geeting input from file , these operations are handled by this package.
  - **util** - It contains classes for implementing data structures . With the help of this package we can make use of data structures like Stacks , ArrayList , Priority Queue etc.
  - **net** - It contains classes which help in networking operations. With the help of this package we can implement socket programming , establish HTTP Url Connections etc.

All the classes from these packages can be imported by:

```java
import java.lang.*;
import java.io.*;
import java.util.*;
import java.net.*;
```

- **User Defined Packages**  
These are the packages which are defined by the user and are not already present in Java API.

### Simplest form of a Package

File: Hi.java
```java
package Hello ;

public class Hi
{
	public static void main(String args[]){  
    
     	System.out.println("Hello Everyone");  
   }  
}
```
Output
```
$ javac -d . Hi.java
$ java Hello.Hi
Hello Everyone
```

Here the command **javac -d . Hi.java** creates a package named `Hello` in which `Hi` Class is present.

### Importing a User Defined Package

File (Package): `Add.java`
```java
package Addition;

public class Add
{
   public int addNum(int a, int b)
   {
   	return a+b;
   }
}
```
File (program): `Test.java`
```java
import Addition.Add;

public class Test
{
   public static void main(String[]  args)
   {
  	Add obj = new Add();
  	System.out.println(obj.addNum(20,30));
   }
}
```
Output
```
$ javac -d . Add.java
$ javac Test.java
$ java  Test
50
```

### Importing a package from another package

File: `A.java`
```java
package pack; 

public class A{  
  public void hello()
  {
	System.out.println("Hello this is A");
  }  
}
```
File: `B.java`
```java
package packed;  
import pack.*;  
 
public class B{  
  public static void main(String args[]){  
   A obj = new A();  
   obj.hello();  
  }  
}
```
Output
```
$ javac -d . A.java   // This creates package pack
$ javac -d . B.java   // This creates package packed
$ javac B.java
$ java packed.B

Hello this is A
```
