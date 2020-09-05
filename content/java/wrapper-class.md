---
date: 2020-09-05
linktitle: Wrapper Class
menu:
  main:
    parent: java
title: Wrapper Class in Java
weight: 3
url: /java/wrapper-class
description: Wrapper class is used to wrap a primitive data type like int, float, char. Wrapper class provides the functionality to encapsulate (wrap) a primitive data type to enable them to use as Objects.
keywords:
  - java
  - wrapper
tags: [Java]  
---
## What is a Wrapper Class?
As the name suggests, a wrapper class is used to wrap a primitive data type like `int`, `float`, `char` etc. The wrapper class provides the functionality to encapsulate (wrap) a primitive data type to enable them to use as Objects.  
Each primitive data type has a corresponding Wrapper class.  
Wrapper classes are provided by the `java.lang` package.

| Primitive Type | Wrapper Class | Primitive Type | Wrapper Class |
|----------------|---------------|----------------|---------------|
| boolean        | Boolean       | char           | Character     |
| byte           | Byte          | short          | Short         |
| int            | Integer       | long           | Long          |
| float          | Float         | double         | Double        |

### Example
```java
void wrapperClassExample1(){
    Integer int1 = new Integer(1);  // Deprecated since, Java 9
    System.out.println(int1);
    Integer int2 = Integer.valueOf(2);
    System.out.println(int2);

    // Calculating Sum
    System.out.println(int1 + int2);

    // Example with another type
    Character another = Character.valueOf('a');
    System.out.println(another);
}
```
From Java 9, new `Integer()` format is deprecated and `Integer.valueOf()` method is preferred. Here `Integer` could be replaced by any Wrapper Class like `Boolean`, `Float` etc.

### Example
Deprecated: `Float deprecated = new Float(1.21);`  
Preferred: 	`Float preferred = Float.valueOf(1.21);`

Wrapper classes provide one more handy functionality which is to convert values from String to primitive data types. Like, if an integer is in String format like "2" we can use Parsing methods to get Integer value 2 or Float value 2.0.
### Example
```java
void wrapperClassExample2() {
    String value = "3";
    Integer intValue = Integer.parseInt(value);
    Float floatValue = Float.parseFloat(value);
    System.out.println("Actual Value  = " + value);
    System.out.println("Integer Value  = " + value);
    System.out.println("Float Value  = " + value);

    
    // Performing Arithmetic Operations
    intValue += 1;
    System.out.println("One added to Integer Value  = " + intValue);
    floatValue /= 2;
    System.out.println("Half of Float Value  = " + floatValue);
}
```
In the above example, first, the string value of 3( three ) is converted to integer and float values and then some arithmetic operations are performed on them. While being a string value, arithmetic operations are not supported.

### Use Cases
Many times we are getting input from external sources like Console Input and API parameters where we can't specify the input type, so after input we need them to be converted into specified type.

### Key Terms
The wrapping up of primitive data type into Wrapper Class objects is known as **Boxing**.  
Example: `Integer intObj = Integer.valueOf(2);`  

The unwrapping of Wrapper Class objects into primitive data types is known as **Unboxing**.  
Example: `int intValue = intObj.intValue();`  

**Factory Methods** are the methods by which instance of classes should be created instead of using [Constructors](/java/constructors/). These methods simplify Object instantiation.  
Example: `valueOf()` method of `Integer`, `Double` etc.

## Need of Wrapper Class
Now the question arises, all the operations are conveniently done by Primitive datatypes then what is the need of Wrapper Classes.  

First, Generic Classes or `java.utils` (example Java Collections) only supports Objects, and hence primitive data types are needed to be wrapped into Wrapper class.  
Example: `List<Integer> intList = new ArrayList<>();`

Second, In multithreading, the primitive data types are not used because they need a reference to lock variables. Locking variables prevents multiple threads to change the value of a variable simultaneously.

## Features of Wrapper Classes
### Autoboxing
As the name suggests Wrapper Classes supports implicit conversion of primitive data types into Wrapper Class objects. So, we can pass any primitive value in a method which requires Wrapper classes as parameters and Java will take care of it. Same is the case for unboxing. There are some use cases where we shouldn't rely on Autoboxing like in a for loop.

Example:
```java
public static void print(Integer num1){
    System.out.println(num1);
}

public static void main(String[] args) {
    int num1 = 1;
    Integer num2 = Integer.valueOf(2);
    print(num1);
    print(num2);
}
```	
### getInteger() Method
In some cases, the value of the variable is not valid and thus to make the program more robust, `getInteger()` Method allows a default value which will replace the invalid value during the execution of the program.

Example:
```java
Integer num1 = Integer.getInteger(5, 0); 		// num1 = 5
Integer num2 = Integer.getInteger(null, 0); 	// num2 = 0, since null is invalid
Integer num3 = Integer.getInteger("abc", -1); 	// num3 = -1, since "abc" can not be converted into Integer
Integer num4 = Integer.getInteger(10, 0); 		// num4 = 10
```
### Parse Methods
Every wrapper class,( except Character ) has a parse method to convert a string value to an expected primitive type. These are generally in the given format  
`public static Integer Integer.parseInt( String value )`  
Here, Integer could be replaced by any wrapper class like `Double`, `Float` etc.

Example:  
```java
Float num1 = Float.parseFloat("200");
```
### toString() Methods
Apart from the basic `toString()` method, which returns the string representation of the object. Integer and Long Wrapper classes provide another `toString()` method which converts the passed number into a specified base.

Signature: `public static Integer toString(int num, int base);`  
Parameters: `int num:` Original value, whose conversion to base is required  
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`int base:` Target base in which value is required  
Returns: `String`, representation of num value in base format

Example:
```java
public static void main(String args[]) {
    int intValue = 31;
    String binaryValue = Integer.toString(intValue, 2);
    String octalValue = Integer.toString(intValue, 8);
    String hexaValue = Integer.toString(intValue, 16);
    System.out.println("Integer Value = "+intValue);
    System.out.println("Binary Value = " + binaryValue);
    System.out.println("Octal Value = " + octalValue);
    System.out.println("Hexadecimal Value = " + hexaValue);
}
```
Output:
```
Integer Value = 31
Binary Value = 11111
Octal Value = 37
Hexadecimal Value = 1f
```
