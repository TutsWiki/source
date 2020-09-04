---
date: 2020-09-01
linktitle: Lambda Expressions
menu:
  main:
    parent: java
title: Lambda Expressions in Java
weight: 8
url: /java/lambda-expressions
description: finally is the block that will always get executed irrespective of the fact whether the exception is handled or not.
keywords:
  - java
  - lambda
tags: [Java]  
---
## 1. Lambda Expressions

### 1.1 Introduction
- Lambda expression is the fundamental approach to **functional programming** in Java.
- It is an anonymous function which doesn’t belong to any class nor a name.
- It provides a concise way to show a method or interface.
- Provides implementation of **functional interface**.

### 1.2 Syntax
`Parameter -> body of expression`

### 1.3 Characteristics
- **Optional type declaration** - Declaration of parameter type is not required.
- **Optional parentheses around parameters** - Required when multiple parameters are used.
- **Optional curly braces** - only to be used when the expression body contains multiple statements.
- **Optional return keyword** - automatically returns the value when the body contains one expression to return it.   

### 1.4 Parameter Types

- Zero Parameters - No parameters are passed.

```
( ) -> System.println("No Parameters")
```

- One Parameter - Only one parameter is passed.

```
(x) -> System.println("One Parameter: " + x)
```

- Multiple Parameters - Multiple parameters are passed.

```
(x,y) -> System.println("Multiple Parameters: " + x + " " + y);
```


### 1.5 Lambda Expression Example
Accepts two values x, y and returns the product of those two numbers.

```
(x, y) -> x * y
```
             
## 2. Functional Interface

### 2.1  INTRODUCTION
- It is an interface which contains only one abstract method.
- It can have any number of default or static methods.

Examples: `FileFilter`, `Comparators`, `Runnable` etc.

### Example
```java
import java.util.*;

// Functional Interface
interface Addition
{
	void answer(int x , int y);  // Abstract function
}
public class Main
{
     public static void main(String args[])
    {
       Addition obj = (int x,int y)->System.out.println(x+y);
           	obj.answer(25,25);
      }
}
```
Output:
```
50
```

## 3. Lambda Variable Capture
Lambda expressions can access variables declared outside the lambda function under certain conditions.

- Local Variables
- Instance Variables
- Static Variables
    
### Local Variable
```java
import java.io.*;


interface abst
{
	void local(String s);
}


class GFG {
    public static void main (String[] args) {
   	
   	  String text1 ="bcbcc";  // Local Variable

   	  abst obj = (String a) ->System.out.println(a);

   		obj.local(text1);
    }
}
```
Output:
```
bcbcc
```

## 4. Method reference in Lambda
Method reference is used to give reference to methods in the functional interface. It makes the code more concise and much more reading friendly.

### 4.1 Types of method references

- Static Method Reference
- Instance Method Reference
- Constructor Reference                              

### Example of Static Method Reference
```java
import java.io.*;
interface abst
{
	void local(int x,int y);        
}

public class Test {
    
	public static void adding(int x, int y)
	{
    	       System.out.println("Sum = "+(x+y));
	}
}


class Example
{
    public static void main (String[] args)
    {
   	
    	     abst ref = Test:: adding;
    	     ref.local(30,30);

           //output - Sum = 60
    }
}
```

### Example of Instance Method Reference
```java
import java.io.*;


interface abst
{
	void local(int x,int y);
}

public class Test {
    
	public void adding(int x, int y)
	{
    	      System.out.println("Sum = "+(x+y));
	}
}


class Example
{
    public static void main (String[] args)
    {
   	
    	    Test add = new Test();
    	    abst ref = add::adding;
    	    ref.local(30,30);

    }
}
```


## Miscellaneous Code Examples

#### For each loop in lambda form
```java
import java.util.*;  
public class Example{  
    
	public static void main(String[] args) {  
         	
    	List<Integer> list=new ArrayList<Integer>();  
    	list.add(1);  
    	list.add(2);  
    	list.add(3);  
    	list.add(4);  
    	list.add(5);
         	
    	list.forEach((n)->System.out.println(n));  
    	}  
}
```
Output:
```
2
3
4
5
```

#### Filtering Collection Data Using Lambda Expression
```
import java.util.*;  
import java.util.stream.Stream;


class Product{  
 	
	String name;  
	float price;  
	public Product(String name, float price) {  
    	super();  

    	this.name = name;  
    	this.price = price;  
	}  
}  
public class Example  {  
	public static void main(String[] args)
	{  
        	List<Product> list=new ArrayList<Product>();  
        	list.add(new Product("Asus",17000));  
        	list.add(new Product("Dell",65000));  
    	       list.add(new Product("Rogue",25000));  
    	       list.add(new Product("Mac Book",15000));  
         	list.add(new Product("Acer",26000));  
        	list.add(new Product("Lenovo",19000));  
     	
Stream<Product> filtered_data = list.stream().filter(p -> p.price > 18000);  
     	
filtered_data.forEach(  product -> System.out.println(product.name+": "+product.price)  
    	);  
   }  
}
```
Output:
```
Dell: 65000.0
Rogue: 25000.0
Acer: 26000.0
Lenevo: 19000.0
```
