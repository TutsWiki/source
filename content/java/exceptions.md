---
date: 2020-08-22
linktitle: Exceptions
menu:
  main:
    parent: java
title: Exceptions in Java
weight: 26
url: /java/exceptions
description: An exception in Java is defined as an unwanted or unexpected event. Object, Throwable, Exception, Error.
keywords:
  - java
  - exception
  - throwable
  - error
tags: [Java]  
---
## Exceptions
An exception in java is defined as an unwanted or unexpected event, which arises at the time of execution of a program i.e. at run time which disturbs the normal flow or working of our program.

For better understanding, consider the following scenario where you have planned to watch a movie in a nearby theater and you got ready and departed from your house but in the mid-way, your vehicle got punctured and for reaching your destination you took a taxi. What happened here is that your normal work or normal flow got disturbed due to an unexpected event (here puncture). This unexpected event may occur in a program too and there it is called exceptions. To handle these unwanted events, we use the concept of exception handling i.e. finding an alternate way to maintain the normal flow like taking a taxi in the above scenario.

Observe the below code. The code will execute flawlessly until the value of i is less than zero. As soon as the value of i becomes zero then in the print statement, the value of n/i will be 6/0 which is mathematically not correct and is an exception. Hence the compiler will give an error and all the code written after the point of exception will not get executed.

#### Example
```java
class test {
  public static void main(String[] args) {
    int n = 6;
    for (int i = 3; i >= 0; i--) {
      System.out.println(n / i);
    }
  }
}
```
Output:
```bash
2
3
6
Arithmetic Exception (divide by zero)
```

## Exception Hierarchy
{{<mermaid>}}
graph TD;
  java.lang-->Object;
  Object-->Throwable;
  Throwable-->Exceptions;
  Throwable-->Errors;
  Exceptions-->Runtime;
  Exceptions-->Other;
{{</mermaid>}}

From the above figure, we can say that throwable class is the parent class of error, and exception class and object is the parent class of the throwable class.

### Error
- It occurs because of a lack of system resources e.g. the hard drive is full etc.
- Errors are not recoverable by programmers
- Errors are only of one type i.e. runtime exceptions or unchecked exceptions
- Error has the sub class as `StackOverflowError`, `VirtualMachineError`, `OutOfMemoryError`

### Exception
- Programs are the root cause of exceptions
- Exceptions can be recovered or fixed by a programmer using techniques provided by Java
- Exceptions are of two types
  - Compile-time exceptions or Checked exceptions 
  - Runtime exceptions or Unchecked exceptions.

There are many inbuilt types of exceptions class provided by Java. Some of them are as follows:

- **IOException**: When the user is operating on input-output commands and if any exception arises at that time then IOException is thrown.
- **RuntimeException**: If any unwanted event arises at the time of execution of the code or at runtime then RuntimeException is thrown. It has further sub classes as
  - **ArithmeticException**
  - **NullPointerException**
  - **NumberFormatException**
  - **IndexOutOfBoundException**

## Checked Exception
This type of exception is also known as compile-time exception. These exceptions can be checked at compile time. The compiler will show a warning at compile time when a program containing such exceptions are compiled. It includes classes like **IOException**, **SQLExcpetion** etc.

### Example
```java
import java.io.FileInputStream;
class test {
  public static void main(String[] args) {
    FileInputStream fis = new FileInputStream("d:/a.txt");
}
}
```
Output: `Error: unreported exception FileNotFoundException. Must be caught or declared to be thrown`

The above code will give you a warning at compile time that file not found, exception may arrive in future and you must handle that exception.

## Unchecked Exception
These types of exceptions are also acknowledged as runtime exceptions. These exceptions can't be checked and ignored by the compiler at compile time. Examples include **ArithmetcException**, **ArrayIndexOutOfBoundException** etc.
### Example
```java
class test {
  public static void main(String[] args) {
    int x = 10,
    y = 0,
    z;
    Z = x / y;
    System.out.println(c);
  }
}
```
Output: `ArithmeticException: divide by zero`

The compiler will not give any warning at the compilation time and the program will get compiled successfully. But at the runtime, it will give us an error. Hence these exceptions are ignored by the compiler and they remain unchecked. 

Now we have learned about exceptions and its type, in the next section we will learn about how to handle these exceptions.
