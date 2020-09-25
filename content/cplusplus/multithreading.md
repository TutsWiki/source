---
date: 2020-08-26
linktitle: Multithreading
menu:
  main:
    parent: python
next: /python/overview
title: Multithreading in C++
weight: 1
url: /python/multithreading
description: Learn what is threading, various types (kernel, user, daemon) and how to use it in C++ using _thread or threading module.
keywords:
  - cplusplus
  - threading
  - kernel
  - daemon
  - thread
tags: [C++]  
---
<meta property="og:image" content="https://tutswiki.com/images/Python/threading.jpg?width=30pc"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Threading in Python" />
<meta name=”twitter:description” content="Learn what is threading, various types (kernel, user, daemon) and how to use it in Python using _thread or threading module." />

## What is multithreading
Multithreading enables us to do parallel computing efficiently by creating multiple threads of a single process. It is used for maximum utilization of the central processing unit by concurrently executing multiple parts of a program. These parts of the program are threads.

A process can be taken as an instance of a program that is being executed by one or more threads. When a program starts, it is loaded with an address space in the main memory from the hard disk. After being loaded on the memory it is being scheduled to an available CPU. Now when a process is scheduled on CPU, process instructions execute sequentially. There is a program counter to keep track of instructions to be executed. The program counter represents the execution context. Execution context is the state of a running process which indicates data like which instruction CPU is executing. We refer to these execution contexts as threads.

There can be multiple threads in a process. These threads can execute different sections of the program simultaneously and can be scheduled on single processor or multiprocessor and multicore systems. A process has its own address space while a thread shares the address space of its parent process along with all the threads its parent process has created. Threads are considered lightweight data structures compared to the process because they will always consume almost less memory than a new process would. Let’s visualize it a bit.

....image Process P Address Space ...

Suppose we are getting some value to variable A, and there is a possibility that each thread in process P can set the value. The programmer has no control over which thread accesses a variable A and when does it access that variable. The operating system scheduler schedules processes and threads according to some scheduling algorithms ( Scheduling algorithms - First Come First Serve, Shortest Job First, Shortest Remaining Time First, Round Robin Scheduling, Priority Based Scheduling ).

To construct multithreaded programs. We need to construct synchronization constructs. Some of them are mutexes and semaphores. These mechanisms enforce synchronization between threads. So if a thread T2 in process P is initializing A no other thread is allowed to initialize A.

## Why do we need Multithreading?
Multiprocessing is the concept of using two or more central processing units to run a program. Multiple processes are executed simultaneously. By multiprocessing, we can avoid the problem of synchronization between threads in a multithreaded process because each process has its own address space that other processes are not allowed to write as the operating system actively prevents the process from writing to another process address space. However, address spaces can be very large and we may only need to run just a portion of the program. If we were to create many processes for parallel computing we would quickly use memory on any machine and so threads are useful. It is also not economical to have multiple processing units. The advantage is bigger than the disadvantage so we use multithreading for efficient parallel computing instead of multiprocessing.

## Examples of Multithreading

1. Text Editor/ Word Processor - We can have multiple tasks like typing the text, spelling check, saving and formatting of text are all being done concurrently by multiple threads.
2. Web Browser - In a web browser, we can browse and at the same time have multiple downloads running.
3. Web Servers - Web server handles each request with a new thread. There is a pool of threads and each time a request comes in, it is assigned a thread that’s why we are able to load Facebook, Amazon, etc so easily. If it was not multithreaded and all requests were to be allotted through the queue so we would have to wait a long time for our turn to come.

## Multithreading in C++
We have the two main functions of creating a thread and waiting for the thread to finish execution.

... image ...

### We need to include a header library for threads

```c
#include<thread>
```

### Working with the threads
To start a thread we need to create a new thread object and pass it a callable as an argument to its constructor. Callable is an executable code that we want to execute when the thread is running.

We can define a callable in three ways:

- A function object
- A function pointer
- A lambda expression

### 1. Using function object as callable:
We need to define a class and in that class we overload an operator. The overloaded
function is having the code which is to be executed.
```c
#include<iostream>
#include<thread>
using namespace std;
// Define the class of function object
class func_obj {
public:
void operator()(parameters) // Overload () operator
{
// Code to be executed
}
} ;
int main(){
// Create thread object
thread thread_object(func_obj, parameters);
// wait for thread thread_object to finish
thread_object.join();
return 0;
}
```
### 2. Using function pointer as callable:
We need to define a function, then we can create a thread object with this function as callable. Parameters are passed after the function name.
```c
#include<iostream>
#include<thread>
using namespace std;
// Define a function
void func_point(parameter_1, parameter_2 {
//Code to be executed
}
int main(){
// Create thread object
thread thread_object(func_point, parameter_1,
parameter_2);
// wait for thread thread_object to finish
thread_object.join();
return 0;
}
```
Note: Parameters can be a variable, list, or vector etc.
### 3. Using Lambda Expression as Callable
We will define a lambda expression, and we will then pass it to the thread object constructor as the first argument followed by its parameters as further arguments.
```c
#include<iostream>
#include<thread>
using namespace std;
int main(){
// Defining a lambda expression
auto func_lambda =[] (parameters) {
// Code to be executed
};
// Create thread object
thread thread_object(func_lambda, parameters);
// wait for thread thread_object to finish
thread_object.join();
return 0;
}
```
Note: We can also pass lambda function directly to constructors.
```c
#include<iostream>
#include<thread>
using namespace std;
int main(){
//Create thread object
thread thread_object([], ( parameters ){
// Code to be executed
};, parameters };
// wait for thread thread_object to finish
thread_object.join();
return 0;
}
```
#### join () function:
This function is used to wait for a thread to finish before we can perform any other action.
```c
#include<iostream>
#include<thread>
using namespace std;
int main(){
//Create thread object
thread thread_object(callable)
// wait for thread thread_object to finish
thread_object.join();
// thread_objec is finished, we can do other things
now.
return 0;
}
```
#### A running C++ example using all the above callable methods:
```c
#include<iostream>
#include<thread>
using namespace std;
// function pointer
void func_point( int i )
{
for(int j=0; j<i; j++)
{
cout<<"Thread with function pointer as callable."<<endl;
}
}
// function object
class func_obj{
public:
void operator() ( int x )
{
for (int j=0; j<x; j++)
{
cout<<"Thread with function object as callable"<<endl;
}
}
};
int main(){
//This thread is created by function pointer as callable
thread T1( func_point, 5);
// This thread is created by function object as callable
thread T2(func_obj, 4);
//This thread is created by lambda expression as callable
thread T3([], ( int y ){
for(int j=0; j<y;j++)
{
cout<<"Thread with lambda expression as callable"<<endl;
}
};, 4 };
// wait for thread T1 to finish
T1.join();
//wait for thread T2 to finish
T2.join();
//wait for thread T3 to finish
T3.join();
return 0;
}
```
Output:
```console
Thread with function pointer as callable
Thread with function pointer as callable
Thread with lambda expression as callable
Thread with lambda expression as callable
Thread with function object as callable
Thread with function pointer as callable
Thread with function object as callable
Thread with lambda expression as callable
Thread with function object as callable
Thread with function pointer as callable
Thread with function object as callable
Thread with function pointer as callable
Thread with lambda expression as callable
To compile program with thread support we have to use :
g++ -std=c++11 -pthread
```
## Conclusion
Multithreading is a very important feature which helps in efficiently computing multiple tasks parallely. It provides better resource utilization, simpler program design and more responsive programs. It is easy to implement and can help a lot in providing security and efficiency to our real world problems.