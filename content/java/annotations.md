---
date: 2020-09-08
linktitle: Annotations
menu:
  main:
    parent: java
title: Annotations in Java
weight: 10
url: /java/annotations
description: Java annotations are a special type of comments which embed instruction for the Java compiler. These also embed metadata which are read at runtime.
keywords:
  - java
  - annotations
tags: [Java]  
---
## 1. Java Annotation
### 1.1 Introduction
- Java annotations are a special type of comments which embed instruction for Java compiler.
- These embed instructions for code processing tools.
- These also embed **metadata** which are read at runtime by Java compiler.

### 1.2 Built-In Annotations
- **@Override**: This annotation makes sure to tell the compiler that the method in subclass should **override** the method of the superclass. If it does not override that method then it gives a **compile-time error**.
- **@SuppressWarnings**: This annotation is used to **ignore** the warnings in certain parts of the program. These are used to remove the unnecessary warnings which don't affect our program.
- **@Deprecated**: This annotation is used to tell the compiler that these methods are deprecated methods, which means that they **may not** be present in the program in future. When we use such methods the compiler prints warning.

#### @Override Example
```java
import java.util.*;
class Parent {
    void childOf() {
        System.out.println("Child of Parent");
    }
}

class Child extends Parent {
    @Override
    void childof() {
        System.out.println("Parent");

    }
}

public class Annotation {

    public static void main(String args[]) {
        Parent a = new Child();
        a.childOf();
    }

}
```
Output:
```
/*prog.java:14: error: method does not override or implement a method from a supertype
@Override  
^
*/
```
### 1.3 Built-in Annotations used in other Annotations
- **@Target**: This allows the user to specify the type of the declaration to which the annotation can be applied like whether the annotation can be used for class, methods, interface, enumeration etc.
- **@Retention**: This allows the user to specify, to what level the annotation will be available or till when the annotation will be retained in the program.

  1. **RetentionPolicy.SOURCE**: will not be available in compiled class and will only be retained till source level.
   
  2. **RetentionPolicy.CLASS**: these are retained till compile-time and will be ignored by JVM.
   
  3. **RetentionPolicy.RUNTIME**: available during runtime and is available to JVM  also.

- **@Documented**: It is used for documenting custom annotation by signalling the JavaDoc tool which compiles it and   adds it to the generated document.
- **@Inherited**: This allows the subclasses to inherit the marked annotations by this annotation from the superclass.   

#### @Target Example
```java
@Target(ElementType.METHOD)
@interface Annotation {
    int val();
    String val2();
}
// this means that this annotation can be only applied to methods.
```
## 2. Types of Annotations
- **Marker Annotation**: Annotations which don't have any data or methods in them. Their only purpose is to mark a declaration.  
Example: `@interface Annotation{ }`
- **Single Value Annotation**: Annotations which have only one method in them. In this, we can either declare the variable then give value or we can also directly give the value it's up to us.  
Example - `@interface Annotation { int value() default 0; }`
- **Multi Value Annotations**: Annotations which have more than one method in them. Here we have to declare members and cannot directly give them values.  
Example: `@interface Annotation{ int value1(); String value2() }`

## 3. Creating a Custom Annotation

### 3.1 Rules

- To define an annotation in Java add `@` before the interface keyword.
- [Throw clauses](/java/throw-throws) should not be present in our method.
- The parameter list inside the method should be empty.
- A default value can be assigned to the method based on our requirement.
- The method should return one of these - primitive data type, String, Class etc.
    
### Example
File: Test.java
```java
import java.util.*;
import java.lang.annotation.*;
import java.lang.reflect.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface Annotation {
    int val();
}


class Example {
    @Annotation(val = 25)
    public void sayHello() {
        System.out.println("Hello annotation");
    }
}


public class Test {
    public static void main(String args[]) throws Exception {

        Example example = new Example();
        Method method = example.getClass().getMethod("sayHello");

        Annotation obj = method.getAnnotation(Annotation.class);
        System.out.println("Value: " + obj.val());
    }

}
```
Output
```
$ javac Test.java
$ java Test
Value: 25
```


