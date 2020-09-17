---
date: 2020-09-08
linktitle: Override/Overload Static Method in Java
menu:
  main:
    parent: java
title: How to Override/Overload Static Method in Java?
weight: 3
url: /java/override-overload-static-method
description: Can a Static Method be Overridden or Overloaded in Java? Overriding and Overloading is a way to achieve Polymorphism in OOP.
keywords:
  - java
  - override
  - overload
  - static
  - oop
tags: [Java]  
---
Overriding and Overloading is a way to achieve Polymorphism in OOP. It is one of the most common and important questions which is asked in many interview and competitive exams. Let's understand what is the meaning of these terms overriding, overloading and static method. At the end of this discussion, one will get to know whether function overriding and function overloading is feasible on a static method or not.

## Function Overloading
If more than one method of a class (be it in the same class or inherited by another class) have the same name but they differ in their method signatures, then the methods are said to be overloaded. Consider the example given below

### Example
```java
class A {
    public void read(int a) {
        System.out.println("I am in class A");
        System.out.println(a);
    }
}
class B extends A {
    public void read(int a, int b) {
        System.out.println("I am in class B");
        System.out.println(a);
        System.out.println(b);
    }
}
public class test {
    public static void main(String[] args) {
        B obj = new B();
        obj.read(10);
        obj.read(20, 30);
    }
}
```
Output
```
I am in class A
10
I am in class B
20
30
```
As we created the object of class B and passed a single argument in the function `read`, then the method which accepted only a single argument gets executed i.e. method in class `A` but when we passed two arguments in the `read` method then the method in class `B` gets executed as it accepts two arguments in its parameters.

It is decided at the compile-time only by the compiler that which method is going to be executed according to the number of arguments that are being passed to a method. Hence **overloading is used to implement compile-time polymorphism**. 

## Function Overriding
If two or more methods which have the same name and same method signature are declared in different classes to implement some specific feature then we say that those methods are overridden. **Overriding is used for implementing run-time polymorphism**. Understand this with the given example

### Example
```java
class A {
    public void read(int a) {
        System.out.println("I am in class A");
        System.out.println(a);
    }
}
class B extends A {
    public void read(int a) {
        System.out.println("I am in class B");
        System.out.println(a);
    }
}
public class test {
    public static void main(String[] args) {
        B obj = new B();
        obj.read(10);
    }
}
```
Output
```
I am in class B
10
```
Since class `B` extends class `A` and the `read` method of class `A` is available to it, still the preference is given to the `read` method that is present in class `B` and is executed. Here the method `read` in class `A` has been overridden by class `B`.

It is decided at the runtime that which function is going to be executed according to the object that is being created for calling the methods. If the object of the parent class is being used for calling, then the `read` method of the parent class is executed and if the object of child class is being used for calling, then the `read` method of child class gets executed.

## Can a Static Method be Overridden?
The answer is **NO**. We **cannot override a static method in Java**. Understand this with the help of an example given below
### Example
```java
class car {
    public static void start() {
        System.out.println("Car starts");
    }
    public void stop() {
        System.out.println("Car stops");
    }
    public void refuel() {
        System.out.println("Car refuels");
    }
}
public class honda extends car {
    public static void start() {
        System.out.println("Honda starts");
    }
}
public class test {
    public static void main(String[] args) {
        honda h = new honda();
        h.start();
        h.stop();
        h.refuel();
    }
}
```
Output:
```
Honda starts
Car stops
Car refuels
```
If you try to access the `h.start()` function, first it will give a warning because the static method will be stored at a common memory allocation in java memory and if we try to access the `h.start()` with object reference type it will give us a warning because the static method will never be stored inside the object. Hence it is preferred to call a static method with the help of class rather than calling it with an object. The above code will give the output `Honda starts` i.e. `h.start()` function of child class will be executed because we called the function with the help of the object of class `B`. If we would have called it with the help of the object of class `A` then `start` function of class A would have been executed.

Static methods are stored at a common memory allocation in Java memory and if we try to access the `h.start() `, which is basically a static function, with object reference type it will give us a warning because the static methods are never stored inside the object. Hence it is preferred to call a static method with the help of class rather than calling it with an object.

The above code will give the output `Honda starts` i.e. `h.start()` function of child class will be executed because we called the function with the help of the object of class `honda`. If we would have called it with the help of the object of class `car` then `start` function of class `car` would have been executed.

- We cannot access the `start` function of the class `car` (parent class) with the help of the class `honda` (child class). It is called **method hiding** in Java i.e. static function `start` in class car is hidden.
- A **static method cannot be overridden by a non-static method** and a **non-static method cannot be hidden by a static method**.
- Hence it depends on the type of reference variable used for calling static methods, therefore static methods are decidable at the compile time. Hence, they cannot be overridden.


## Can a Static Method be Overloaded?
Now we know that a static function cannot be overridden but what about overloading? The answer is **YES**. Static methods can be overloaded in Java without any errors. For example
### Example
```java
class A {
    public static void read(int a) {
        System.out.println("I am in class A");
        System.out.println(a);
    }
}
class B extends A {
    public static void read(int a, int b) {
        System.out.println("I am in class B");
        System.out.println(a);
        System.out.println(b);
    }
}
public class test {
    public static void main(String[] args) {
        B obj = new B();
        obj.read(10);
        obj.read(20, 30);
    }
}
```
Output:
```
I am in class A
10
I am in class B
20
30
```
The above code gets executed without any error and hence we can say that static methods can be overloaded. 

## Can a Static Method be Overloaded from Non-Static Method?
The answer is **NO**. The compiler will raise an error if we try to overload a static function which differs only in the static keyword. Consider the below example
### Example
```java
class A {
    public void read(int a) {
        System.out.println("I am in class A");
        System.out.println(a);
    }
    public static void read(int a) {
        System.out.println("I am in class B");
        System.out.println(a);
    }
    public static void main(String[] args) {
        A.read(10);
    }
}
```
Output
```
error: method read(int) is already defined in class A
error: non-static method read(int) cannot be referenced from a static context
```
The above code will show an error at the time of compilation which shows that the static method which differs only in static keyword is not overloaded.

With this, we end our topic and we hope that the concept of overriding and overloading with static methods is clear to you.
