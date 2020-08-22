---
date: 2020-08-22
linktitle: Classes & Objects
menu:
  main:
    parent: java
next: /java/constructors
title: Classes & Objects
weight: 26
url: /java/class-object
description: In this tutorial, you will learn about object-oriented programming in Java and you will learn about Java classes and objects with the help of examples. A class is a blueprint of objects whereas an object is an instance of a class.
keywords:
  - java
  - class
  - object
  - constructor
  - this
tags: [Java]  
---
## Objects

Java is an object-oriented language meaning it follows the object-oriented programming (OOPS) paradigm. The OOPS principle looks at any problem as an interaction between various **objects**. This, in turn, helps us to solve real-world problems easily because most of it involves objects. Objects are described by their intrinsic properties ( in coding terms some base data ) and how they can interact within the object and also with other objects ( in coding terms some functionality over the data ). So it is natural to organize objects into a composition of data and functionality.

For example: Let us look at a car and a petrol pump. We could probably limit ourselves and say that Color, Fuel, and Speed describe a car completely and Fuel and Color describe a petrol pump completely. So these become our data. Now suppose if the driver wants to fuel the car, this represents an object interaction problem between a petrol pump and the car, where the fuel attribute of the car must increase and that of the petrol pump must decrease. Hence we can introduce functionality *fill tank* in the car to interact with the petrol pump and transfer fuel from the pump to the car. From this, we can see that it would be much easier for the functionality to access the data if they were
composed together.

## Classes
Now that we have a basic understanding of why OOPS is helpful, let us see what happens if we need to manufacture a new car. Do we start everything from scratch? most of the car manufacturing companies follow a blueprint when producing a car so that they need not invest time again and again to design the properties and functionalities of the car. This is the same in OOPS as well. We use something called a Class to define our blueprint for an object so that if we want to create another object of the same kind we can use this blueprint to speed up our process.

## Classes and Objects in Java
In the software world, where we need to model real-time situations most of the time the use of OOPS principles is extremely important. In Java, we store the data of an object in a data member/field of the object and the functionality of objects are defined as methods of the object. We define a class in Java to tell the compiler about the blueprint of the object we need. The class also contains the definitions of the data members and the required methods. The compiler can use this information to allocate resources like memory when an object is created using this blueprint.

### Example
Let us model the car and petrol pump using classes and create objects using the classes.
```java
class petrolPump {
  int Fuel;
  String Color;
  public petrolPump(int Fuel) {
    this.Fuel = Fuel;
  }
}
Public class Car {
  int Fuel;
  int Speed;
  String Color;
  public Car(int Fuel) {
    this.Fuel = Fuel;
  }
  public void fillTank(int requiredFuel, petrolPump pump) {
    if (pump.Fuel >= requiredFuel) {
      pump.Fuel -= requiredFuel;
      this.Fuel += requiredFuel;
    }
  }
  public static void main(String[] args) {
    Car myCar = new Car(10);
    petrolPump pump = new petrolPump(1000);
    myCar.fillTank(50, pump);
  }
}
```
There might be several doubts in this code especially if you are new to OOPS. let us answer this one by one.

## Variables and Methods
The data members of the class also called as the **instance variables** are the variables that are declared in the class before the methods. These variables are instantiated during object creation by the constructor, which we will discuss soon. These variables have a class scope, that is they exist as long as the object of the class exists and is destroyed once the object is destroyed. They are also bound to a single object of the class and can be accessed directly or indirectly via the object, bound to access rules of the variable.

We can also have **local variables** to help us in some processing inside the methods. They have local scope to the methods and die once the method execution is finished. These are not accessible by the objects of the class and do not describe the object. The functions written within the scope of the class are called as class methods. They have direct access to the instance variables of the class and can manipulate them to change the state of the object. The functions are in general not bound to a single object of the class, but rather all objects of the class share a copy during the run time of the program. They can be accessed directly or indirectly via the object, bound to access rules of the method.

### Accessing variables and methods
We use the `.` operator in java to access methods or variables of the class via its objects. This is bound to access rules. If the access for the variable or method is public then we can use the operator to directly access the variable/method. In the given code myCar.fillTank( 50, pump ); uses the operator.

### this keyword
`this` keyword is used to specify the current object. This is especially useful when we have local variables of the same name as instance variables. To access the instance variables, we can use `this` keyword with the `.` operator. It tells the compiler that the variable being referred to is the instance variable associated with the current object
which is executing the method.

### Object Creation
The line `Car myCar = new Car(10);` creates a new Car object out of the Car class blueprint and initialises it or we can say myCar is an object of the type Car. During object creation, when we use the above syntax, it instructs the compiler to create a new object of the class Car by calling a special method which is called as the Constructor of the class. The constructor of the class is a function which has no return type, needs to have the same name as that of the class and is invoked when an object of the class is created. The main purpose of this is to instantiate the data members of the object. A constructor can be parameterized such that we can pass parameters to it to instantiate the data members with specific values rather than default ones.

```java
//parameterised
public Car(int Fuel) {
  this.Fuel = Fuel;
}
//Non-paramterised
public Car() {
  this.Fuel = 0;
}
```

The above is an example of parametrized vs non-parameterized constructor. If we do not declare this method in the class, the compiler automatically assigns a non-parameterized constructor to the class. Constructors can also be overloaded according to Java principles.