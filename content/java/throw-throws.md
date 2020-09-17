---
date: 2020-08-30
linktitle: throw and throws
menu:
  main:
    parent: java
title: throw and throws
weight: 7
url: /java/throw-throws
description: The keyword throw is employed to throw an exception explicitly. It is mainly used to throw custom exceptions or user-defined exceptions.
keywords:
  - java
  - exception
  - throw
  - throws
tags: [Java]  
---
In the previous tutorial, we learned how to handle an exception using [try and catch block](/java/exception-handling/#try-and-catch/). Now we will learn how exception handling is done using **throw** and **throws** keyword.

## throw keyword
The keyword `throw` is employed to throw an exception explicitly. It is mainly used to throw `custom exceptions` or `user-defined exceptions`. It is placed inside the method.

### Syntax
```java
throw new ExceptionClassName("Message");
```
`JVM` or method does not make the exception object like it used to do before. Here the user has to explicitly make an object of the exception using the `throw` keyword.

### Example
```java
class test {
  public static void DivByZero() {
    throw new ArithmeticException("Cannot divide by zero");
  }
  public static void main(String[] args) {
    DivByZero();
  }
}
```
Output: 
```bash
Java.lang.ArithmeticException: Cannot divide by zero
```
We can also have a **user-defined class** of exceptions. Check the example given below
### Example
```java
import java.util.Scanner;
class AgeLimit extends RuntimeException {
  AgeLimit(String s) {
    super(s);
  }
}
class voting {
  public static void main(String[] args) {
    int age = 16;
    if (age < 18) {
      throw new AgeLimit("you are not eligible
      for voting");
    }
    else {
      System.out.println("you can vote");
    }
  }
  System.out.println("hello");
}
```
Output:
```bash
Exception in thread "main" AgeLimit:you are not eligible for voting.
```
We note here that exception is still not handled because our program has terminated abnormally (because it did not print `hello`). To handle it, you have to use try and catch block.

### Example
```java
import java.util.Scanner;
class AgeLimit extends RuntimeException {
  AgeLimit(String s) {
    super(s);
  }
}
class voting {
  public static void main(String[] args) {
    int age = 16;
    try {
      if (age < 18) {
        throw new AgeLimit("you are not eligible
        for voting");
      }
      else {
        System.out.println("you can vote");
      }
    }
    catch(Exception e) {
      e.printStackTrace();
    }
  }
  System.out.println("hello");
}
```
Output:
```bash
Exception in thread “main” AgeLimit:you are not eligible for voting.
hello 
```
Now the above code will not terminate abnormally because we have handled it using try and catch block. Remember that you cannot write any code below the `throw` keyword statement. If you do so then you will get a compiler error.

## throws keyword
`throws` keyword is used at the time of method declaration which indicates to the caller method that a particular type of exceptions may arrive if you are using this method and accordingly the caller method has to handle that exception. `throws` keyword is used only for **checked exceptions**.
### Syntax
`Method () throws Exception1 ,Exception2……Exception n`
For better understanding, consider the example given below
### Example
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileNotFoundException; {
class files {
    void ReadFiles() throws FileNotFoundException {
        FileInputStream fi = new FileInputStream("d:/xyz.txt");
        }
    }
    class test {
        public static void main(string[] args) {
            try {
                ReadFiles();
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            }
            System.out.println("hello");
        }
    }
```
Output:
```bash
Java.io.FileNotFoundException: d:/xyz.txt
hello 
```
Now when the main function will call the `ReadFiles` method then the calling method i.e. main function will get informed that the function which you are calling will throw a `FileNotFoundException` and therefore the main function should handle it. If we would have not used the try-catch block then the compiler would have given us warning at compile time. Since we used the try-catch block, our code will get compiled easily and will not terminate abnormally (as we can confirm from `hello` message above).

## Difference between throw and throws
| throw                                             | throws                                           |
|---------------------------------------------------|--------------------------------------------------|
| It is used to create an exception object manually | It is used to declare the exception.             |
| It is mainly used for an unchecked exception      | It is mainly used for a checked exception        |
| It can throw only a single exception              | It can declare multiple exceptions               |
| "throw" keyword is used inside the method         | "throws" keyword is used with a method signature |
