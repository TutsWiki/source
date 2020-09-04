---
date: 2020-09-01
linktitle: finally
menu:
  main:
    parent: java
title: finally
weight: 8
url: /java/finally
description: finally is the block that will always get executed irrespective of the fact whether the exception is handled or not.
keywords:
  - java
  - exception
  - finally
  - resource
tags: [Java]  
---
In the previous tutorial, we learned how to handle an exception using [throw and throws](/java/exceptions/exception-handling/throw-throws/) keywords. Now we will learn about **finally** block.
## finally
`finally` is the block that will always get executed irrespective of the fact whether the exception is handled or not.

### Syntax
```java
try () { //code that might produce exception}
	catch(Exception e) { //handle exception }
		finally { //code that always gets executed }
			
```
We can also have finally block without catch block.
```java
try()
  {//code that might produce exception}
finally
  {//code that always gets executed }
```
But the problem with the second syntax is that we can not handle the exception since the catch block is missing. 
### Example
```java
class test {
	public static void main(String[] args) {
		try {
			int x = 10,
			y = 2,
			z;
			Z = x / y;
			System.out.println(z);
		}
		catch(Exception e) {
			System.out.println("welcome in catch block");
		}
		finally() {
			System.out.println("Hi, my_name_is_John");
		}
	}
}
```
Output:
```bash
5
Hi, my_name_is_John
```
The above code will not go to catch block since no exception arises. Statements of `try` and `finally` gets executed.
Now let's see an example where an exception arises. 
### Example
```java
class test {
	public static void main(String[] args) {
		try {
			int x = 10,
			y = 0,
			z;
			Z = x / y;
			System.out.println(z);
		}
		catch(Exception e) {
			System.out.println("welcome to the catch block");
		}
		finally() {
			System.out.println("Hi, my_name_is_John");
		}
	}
}
```
Output:
```bash
Welcome to the catch block.
Hi, my_name_is_John
```
Here the exception arises but then also the finally block gets executed. Now, what happens if we write the final block with try block but without catch block.
Let's see the given example
### Example
```java
class test {
	public static void main(String[] args) {
		try {
			int x = 10,
			y = 0,
			z;
			Z = x / y;
			System.out.println(z);
		}
		finally() {
			System.out.println("Hi, my_name_is_John");
		}
		System.out.println("Hello");
	}
}
```
Output:
```bash
Hi, my_name_is_John
ArithmeticException: divide by zero
```
The above code gets terminated abnormally because the exception has not been handled but then also finally block gets executed.

Unlike catch block, we can't use multiple finally block with one try block.

## When finally doesn't work?
It might happen sometime that finally block does not get executed. Some of the possible cases are
- Using the method System.exit(0)
- Using a fatal error that can cause the process to abort
- If there is exception present in finally block itself
- If the thread gets disturbed

finally block is also used for explicitly closing the resources such as connections, files, etc.

## try with resources
Unlike finally, try with resources automatically closes all the running resources which have been initiated by try and catch block.
## Syntax
```java
try (Resource declaration) {
	// code that uses the resource
} catch(ExceptionType e1) {
	// catch block which handles all the exceptions
}
```
### Example
```java
import java.io.FileReader;
import java.io.IOException;
public class test {
	public static void main(String args[]) {
		try (FileReader file = new FileReader("d://xyz.txt")) {
			int[] a = new int[10];
			file.read(a);
			for (int c: a)
			System.out.println(c);
		}
		catch(IOException e) {
			e.printStackTrace();
		}
	}
}
```
In the above code, the `FileReader` class automatically gets closed as the try and catch block gets executed.
Some important points regarding try and resource statement

- Try with resources statements can declare multiple classes and these classes are closed in the reverse order i.e. bottom to up.
- A class should be of **AutoCloseable** type.
- Resources that have been declared in the try block are automatically considered as final by the compiler.

## User defined exception
You can also create customized exception class. If you want to create a checked exception, you'll need to extend the `Exception` class, if you want to create the `unchecked` or `runtime` exception class then you'll need to extend `RuntimeException` class.

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
			throw new AgeLimit("you are not eligible for voting");
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
Above is created an `unchecked` exception class by the user which extends the `RuntimeException` and its constructor prints the exception that occurs.

## Note
With this, we have completed our tutorial of exception handling using five keywords i.e. `try`, `catch`, `finally`, `throw` and `throws`. Some important points which you need to know about the rules of the declaration of these five keywords are:

- There is no existence of a catch block without a try block
- There should be no code between try, catch, and finally block
- There is no existence of try block without either of the catch or finally block
- finally block is completely optional
