---
date: 2020-09-18
linktitle: Garbage Collection
menu:
  main:
    parent: java
title: Garbage Collection in Java
weight: 7
url: /java/garbage-collection
description: The process of releasing the heap memory occupied by objects which do not have any live references in the Java program is known as garbage collection. null, gc, finalize.
keywords:
  - java
  - garbage
  - gc
  - null
  - finalize
tags: [Java]  
---
## 1. Garbage Collection
###  1.1 Introduction
The process of releasing the heap memory occupied by objects which do not have any live references in the Java program is known as garbage collection.

###  1.2 Features
- Garbage Collection is done automatically in java by **JVM (Java Virtual Machine)**
- There is no **delete** keyword in Java as in C++ to free up the memory
- Those memory blocks which are not referenced by any pointer are known as **garbage blocks**
- The code which is responsible for garbage collection is known as **garbage collector code**
- The main advantage of garbage collection is protection from **memory leaks**
   
## 2. Process Of Garbage Collection  
Whenever a Java program is compiled and executed, **JVM** creates three threads

- **Main thread** -  this thread is responsible for running the `main method()` in the Java program

- **Thread Scheduler** - this thread controls the scheduling of the threads in the program like which thread should execute first or which thread should execute after which thread etc.

- **Garbage Collector Thread** -  this thread is responsible to release the garbage blocks present in the memory and is the main focus of this article

> Garbage Collector thread works in the background, performs its tasks in background  and comes in the category of **daemon threads** or **low priority threads**

### Garbage Collector Thread

- When the garbage collector starts running it identifies all the live objects present in the memory which have no reference in the program
- Before releasing their memory it calls a method named `finalize()`, to make sure that the objects have completed their processes or not
- Here `finalize()` method can be considered to have almost the same functionality as `destructors()` in C++
- After the `finalize()` method is executed, the garbage collector releases the memory of the objects and hence protects the program from unwanted memory leaks           

### 3. Object Dereferncing Methods
#### Nulling the reference
```java
School obj = new School();
obj = null;
```

Here the `obj` variable, first refers to an object in the heap memory but after nulling the `obj` there is no other reference to that object, hence garbage collector identifies it and releases that object’s memory.

#### Assigning the reference to another object
```java
School obj1 = new School();
School obj2 = new School();
obj1 = obj2;
```
Now the object previously referenced by obj1 in the heap memory is available for garbage collection as no live reference is available for that object now.

#### Anonymous object
```java
new School();
```
Anonymous objects don’t have any reference so garbage collector releases their memory once they serve their purpose
   
### 4. gc() Method    
- In Java, JVM implicitly calls the garbage collector but this method can be used to call the garbage collector explicitly
- This method doesn't guarantee that the garbage collector will be executed at that point
- This method is not a command, it's just a request, the final decision is taken by JVM whether to execute the garbage collector or not

### Example
File : Test.java
```java
import java.io.*;
public class Test{  

  public void finalize()
  {
    System.out.println("Memory is freed");
  }  

  public static void main(String args[]){
 
  Test obj1=new Test();  
  Test obj2=new Test();  
  obj1=null;  
  obj2=null;  
  System.gc();  
  }  
   
}
```
Output
```console
$ javac Test.java
$ java Test
Memory is freed
Memory is freed
```

- In above example `obj1` and `obj2` are dereferenced by assinging `null`, after which the objects don't have any live reference in the program and are now available for garbage collection
- The `gc()` method requests to invoke the garbage collector
- The garbage collector thread before releasing the memory calls the `finalize()` method for both objects
- After the execution of `finalize()` method it frees up the memory
