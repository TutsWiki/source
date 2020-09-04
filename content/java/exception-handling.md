---
date: 2020-08-29
linktitle: Exception Handling
menu:
  main:
    parent: java
title: Exception Handling
weight: 6
url: /java/exception-handling
description: Exception handling is the facility provided by Java to handle all the unwanted event or exception that arises in our program to maintain the normal flow of our code. try,catch,throw,finally
keywords:
  - java
  - exception
  - try
  - catch
  - throw
  - finally
tags: [Java]  
---
Exception handling is the facility provided by Java to handle all the unwanted event or exception that arises in our program to maintain the normal flow of our code. Whenever there is an exception, the method in which the exception occurs will create an object and that object will store three things:

1. **Exception name**: It stores the class name which can handle the occurred exception.
2. **Description**: It describes what type of exception has occurred.
3. **Stack trace**: Stores the information about the line at which exception occurred or in which method the exception occurred.

{{<mermaid>}}
graph TD;
  Object-->JVM;
  JVM-->id1[Default Exception Handler];
  JVM-->id2[Manual Handler];
{{</mermaid>}}

Observe the above figure. As soon as the object is created, that object is passed to Java Virtual Machine (`JVM`) and now JVM checks whether the exception is handled by the user or not. If it is not handled then JVM passes the control to Default Exception Handler which will print the exception and our program is terminated abnormally.

An exception can be handled manually by the programmer using five keywords

1. try
2. catch
3. finally
4. throw
5. throws

## Try and Catch
### Syntax
```java
try {
  //code which can cause exception
}
catch(ExceptionClassName ref.var.name) {
  //handling code
}
```
After every try, there should be a catch block. Using try and catch block, we can be assured that our program will not terminate abnormally as it was terminating earlier without try and catch block.
### Example
```java
class test {
  public static void main(String[] args) {
    try {
      int x = 10,
      y = 0,
      z;
      Z = x / y;
    }
    catch(Exception e) {
      System.out.println(e);
    }

    System.out.println("hi");
  }
}
```
Output:
```bash
ArithmeticException: divide by zero
hi 
```
When you will compile the above code, it will easily get compiled without any warning and will display the exception that has occurred and it will also print hi which shows that our program has not terminated abnormally.
## Flow of try and catch

### Example
```java
class test {
  public static void main(String[] args) {
    System.out.println(“hi”);
    try {
      System.out.println("welcome");
      int x = 10,
      y = 2,
      z;
      Z = x / y;
      System.out.println("to");
    }
    catch(Exception e) {
      System.out.println("Tuts");
      System.out.println(e);
      System.out.println("Wiki");
    }
    System.out.println("hello");
  }
}
```
Output:
```bash
hi 
welcome
to 
hello
```
In the above code, the control is first at the print statement present just before try keyword which prints `hi` and then the control goes in try block. The first print statement prints `welcome` and the mathematical expression is evaluated. Since the denominator is non zero number, no exception arises and the control passes to the last print statement in try block which prints `to`. Since no exception occurred, the catch block didn’t get executed and the program terminated normally by printing `hello`.

Now observe the below code to understand the flow when an exception arises.
### Example
```java
class test {
  public static void main(String[] args) {
    System.out.println("hi");
    try {
      System.out.println("welcome");
      int x = 10,
      y = 0,
      z;
      Z = x / y;
      System.out.println("to");
    }
    catch(Exception e) {
      System.out.println("2");
      System.out.println("Tuts");
      System.out.println("Wiki");
      System.out.println(e);
    }
    System.out.println("hello");
  }
}
```
```bash
hi 
welcome
2
Tuts
Wiki
ArithmeticException: divide by zero
hello
```
Now the value of the denominator in the try block is zero which will cause an exception. The control is first at print statement before try keyword which prints `hi` and then the control goes in try block. The first print statement prints `welcome` and the mathematical expression is evaluated. Since the denominator is zero, an exception arises and at once the control is passed to catch block. All the print statement in the catch block gets executed and the program gets terminated normally by printing `hello`. Note that after the execution of the catch block, control does not pass to try block back. Hence everything written below the occurrence of exception in try block will remain unexecuted.

## Multiple catch block
We can have multiple catch blocks with a try block and the catch block which will come first will get the control first i.e. top to bottom approach.
### Example
```java
class test {
  public static void main(String[] args) {
    try {
      int x = 10,
      y = 0,
      z;
      Z = x / y;
    }
    catch(ArithmeticException e) {
      System.out.println(e);
    }
    catch(IndexOutOfBoundException e2) {
      System.out.println(e2);
    }
    System.out.println("hi");
  }
}
```
```bash
ArithmeticException: divide by zero
hi  
```
Order of catch block is very important because improper ordering may give you an error. The order of the catch block should be child class (`ArithmeticException`) to parent class (`Exception`), not the parent to the child class. For better understanding, consider below example-
### Example
```java
class test {
  public static void main(String[] args) {
    try {
      int x = 10,
      y = 0,
      z;
    }
    catch(Exception e) {
      System.out.println("exception");
    }
    catch(ArithmeticException e) {
      System.out.println("ArithmeticException");
    }
  }
}
```
```bash
Exception ArithmeticException has already been caught catch(ArithmeticException e)
```
The compiler will give an error while compiling the above code. It will tell the user that the exception which has occurred has already been caught by the first catch block and there is no use to re-write the catch block which handles the same exceptions. Therefore, the order of catch block is important and should be from the child class to the parent class, not the parent to the child class. 

### Example
```java
class test {
  public static void main(String[] args) {
    try {
      int x = 10,
      y = 0,
      z;
    }
    catch(ArithmeticException e) {
      System.out.println("Arithmeticexception");
    }
    catch(Exception e) {
      System.out.println("Exception");
    }
  }
}
```
```bash
Arithmeticexception
```
The above code will execute properly without any error because the proper ordering of the catch block has been done. The first catch block says that if any arithmetic exception arises then it can handle it and if there is any other type of exception other than arithmetic exception then the second catch block will handle it.
