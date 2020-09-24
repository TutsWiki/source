---
date: 2020-08-26
linktitle: Threading
menu:
  main:
    parent: python
next: /python/overview
title: Threading in Python
weight: 1
url: /python/threading
description: Learn what is threading, various types (kernel, user, daemon) and how to use it in Python using _thread or threading module.
keywords:
  - Python
  - threading
  - kernel
  - daemon
  - thread
tags: [Python]  
---
<meta property="og:image" content="https://tutswiki.com/images/Python/threading.png?width=30pc"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Threading in Python" />
<meta name=”twitter:description” content="Learn what is threading, various types (kernel, user, daemon) and how to use it in Python using _thread or threading module." />
In this tutorial, we will understand the concept of **threading in Python**. Let us begin by defining the term `thread`.
## What is Thread?
> A `thread` is a lightweight execution unit that can be managed independently by a scheduler consisting of its `program counter`, a `stack`, and a `register` set.

- A `register` is a temporary storage unit built inside a CPU.
- A `program counter` is a register that contains the address of the executing instruction.
-  A `Stack` is a reserved region of memory for a thread. When a function executes, it pushes the local variables to the stack, and when the function exits, it pops those variables from the stack.

Since each thread has its resource, multiple processes can be executed parallelly by increasing the number of threads in a process, as depicted below.

![Multithreading in Python](/images/Python/threading.png "Threading")

## Why use threading
- `Threads` improve the performance of the processes through `parallelism` and `concurrency`.
   - A single-threaded process can only run on one CPU, independent of the number of cores or processors available, while in a multi-threaded application, each thread is executed by different available processing cores.
   - We can run one thread that can provide a quick response to the user. And in the background, another thread is doing extensive CPU work.

- `Threads` shares `code`, `data`, and other resources like `files`:

   - Which allows multiple tasks to be executed `parallelly`.

        > For example, in `Antivirus software`, there is a thread for each of the following, which runs in parallel.
        - Scanning the computer
        - Realtime analysis
        - Updating the antivirus software.

   - Creating, managing, and context switches are much faster than performing the same tasks for processes `serially`.
   - Threads of a process shares the global variables. So any change in them will be reflected in other threads as well. A thread can also have its own set of local variables.

- In case we have limited resources and we want them to be shared, i.e, by synchronization, for better utilization of resources.
- A Complex task can be optimized by dividing them into multiple independent subtasks. Each subtask is handled by a thread. These threads will run on separate processors, thus using parallelism to reduce execution time.

## When not to use threading
1. For basic tasks, as there is overhead associated with managing threads.
2. Threading makes the program more complex which makes the debugging process more difficult.
3. Processes are CPU bound, and we only have single processing core.
> **CPU Bound Processes:** Processes that spend most of their time while executing on CPU.

Assuming that our goals align with the scenarios mentioned in the `Why use threading` section, let's now discuss its types.

There are two different kinds of threads:

1. Kernel threads
2. User threads

## Kernel Threads 
**Kernel threads** are implemented and managed by the kernel. The process context information and the threads are all managed by the kernel itself. Therefore, kernel threads are slower than user threads.

### Advantages

   - Threads from multiple processes can be scheduled simultaneously by the kernel.
   - In a process, if one thread is blocked, the Kernel can schedule another thread.

### Disadvantages

- Creating and managing kernel threads are slower and less efficient than the user threads.
- A mode switch to the Kernel is required for transfer of control from one thread to another within the same process.

## User Threads
**User threads** are implemented and managed by users, i.e. creating or destroying thread, saving or restoring thread, scheduling thread, passing message or data to the thread and, the kernel is unaware of its existence. The kernel handles these threads as if they were single-threaded processes.

### Advantages

- User threads are OS independent.
- We do not need Kernel mode privileges for thread switching.
- Scheduling can be process specific.
- It is faster to create `user threads` and to manage them.

### Disadvantages

- When a context switch happens, it blocks the process, and since a process can have multiple threads, threads are also blocked.
- For a process, if one thread executes a system call, all the other threads stop executing for that duration.

Now the big question arises,
## How to use threading in Python
Python provides us with two modules to support threading.

1. _thread
2. threading

### _thread module

`_thread` provides a method `start_new_thread()` that starts a new thread and returns its identifier.

**Syntax**
```
thread.start_new_thread(myFunction, args[, kwargs])
```
This method takes 2 arguments, a function to be executed `myFunction` and a list of tuple arguments `args`. `kwargs` argument is an optional keyword arguments (Dictionary).

Let us consider the following program,
```python
import _thread
import time
from time import ctime


def myFunction(myThread):
    counter = 5
    while counter > 0:
        print(myThread + " " + ctime(time.time()))
        counter -= 1


try:
    _thread.start_new_thread(myFunction, ("Thread 1",))
    _thread.start_new_thread(myFunction, ("Thread 2",))
except:
    print("Thread could not be started")

while True:
    pass
```

Output:
```console
Thread 1 Sat Sep 19 13:04:03 2020
Thread 2 Sat Sep 19 13:04:03 2020
Thread 1 Sat Sep 19 13:04:03 2020
Thread 1 Sat Sep 19 13:04:03 2020
Thread 1 Sat Sep 19 13:04:03 2020
Thread 1 Sat Sep 19 13:04:03 2020
Thread 2 Sat Sep 19 13:04:03 2020
Thread 2 Sat Sep 19 13:04:03 2020
Thread 2 Sat Sep 19 13:04:03 2020
Thread 2 Sat Sep 19 13:04:03 2020
```

#### Explanation
- `Thread 1` and `Thread 2` both read the value of the variable `counter`, which is `5`, at time `13:22:01`.
- After reading the value, both the threads enter the loop and executes the print statement at the same time `13:22:01`. 
- Both are then put to sleep for `2` and `3` seconds, respectively. 
- Now, after 2 seconds, `Thread 1` wakes up, decrements the value of `counter` by 1, and moves with the next iteration, which then executes the print statement and puts the thread to sleep again for 2 seconds.
- At the 3rd second, at `13:22:04`, `Thread 2` wakes up and decrements the value of `counter` and continues with the next iteration, which then executes the print statement and puts the thread to sleep again for 3 seconds. 
- This process stops when the value of `counter` for both `Thread 1` and `Thread 2` is `0`.

> **Note:** If we run this program multiple times, we will notice that the output sequence changes every time. It happens because the threads are not synchronized, and they run whenever they have the required resources.

### threading module
`_thread` is an effective option for low-level threading. But Python's new `threading` module is more powerful and supports high-level threading.

To create a new thread, we create an object of `Thread` class, which takes `target` as a parameter.
- `target:` Name of the function to be executed
- `args:` Optional parameter to pass argument tuple

**Syntax**
```
threading.Thread(target=myFunction, args* = ())
```

Consider the following example.
```python
import threading
import time
from time import ctime


def myThread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(2)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


for i in range(0, 3):
    print("Creating thread %d at %s" % (i, ctime(time.time())))
    thread = threading.Thread(target=myThread, args=(i,))
    print("Starting thread %d at %s" % (i, ctime(time.time())))
    thread.start()
```
#### Explanation
- To use the `threading` module, we need to import it using `import threading`
- The loop creates 3 threads by using `threading.Thread(target=myThread, args=(i,))` where we have passed `i` as an argument. Note that the target is `myThread()` function.
- `start()` method is used to start the execution of a thread.

Output:
```console
Creating thread 0 at Fri Sep 18 16:24:25 2020
Starting thread 0 at Fri Sep 18 16:24:25 2020
Thread 0: started at Fri Sep 18 16:24:25 2020
Creating thread 1 at Fri Sep 18 16:24:25 2020
Starting thread 1 at Fri Sep 18 16:24:25 2020
Thread 1: started at Fri Sep 18 16:24:25 2020
Creating thread 2 at Fri Sep 18 16:24:25 2020
Starting thread 2 at Fri Sep 18 16:24:25 2020
Thread 2: started at Fri Sep 18 16:24:25 2020
Thread 0: finished at Fri Sep 18 16:24:27 2020
Thread 2: finished at Fri Sep 18 16:24:27 2020
Thread 1: finished at Fri Sep 18 16:24:27 2020
```

Now, What if we want the create a new thread only after the previous one has been completed or stopped? In that case, we use the `join()` method as below.
```python
import threading
import time
from time import ctime


def myThread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(2)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


for i in range(0, 3):
    print("Creating thread %d at %s" % (i, ctime(time.time())))
    thread = threading.Thread(target=myThread, args=(i,))
    print("Starting thread %d at %s" % (i, ctime(time.time())))
    thread.start()
    thread.join()
```
Output:
```console
Creating thread 0 at Fri Sep 18 16:37:26 2020
Starting thread 0 at Fri Sep 18 16:37:26 2020
Thread 0: started at Fri Sep 18 16:37:26 2020
Thread 0: finished at Fri Sep 18 16:37:28 2020
Creating thread 1 at Fri Sep 18 16:37:28 2020
Starting thread 1 at Fri Sep 18 16:37:28 2020
Thread 1: started at Fri Sep 18 16:37:28 2020
Thread 1: finished at Fri Sep 18 16:37:30 2020
Creating thread 2 at Fri Sep 18 16:37:30 2020
Starting thread 2 at Fri Sep 18 16:37:30 2020
Thread 2: started at Fri Sep 18 16:37:30 2020
Thread 2: finished at Fri Sep 18 16:37:32 2020
```

`join()` pauses the main thread (`for` loop in this case) and wait for the running thread to complete.
Note that `Thread 1` is created after `Thread 0` has finished and `Thread 2` is created after `Thread 1` has finished.

We can also pass a `timeout` time in the `join` method. It is used to manually timeout the current thread and allows the next thread to execute. We can call the `is_alive()` method (checks whether a thread is still executing and returns `boolean`) after `join()` to check whether a timeout happened. If the thread is still alive, the `join()` times out.
```python
import threading
import time
from time import ctime


def myThread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(2)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


for i in range(0, 3):
    print("Creating thread %d at %s" % (i, ctime(time.time())))
    thread = threading.Thread(target=myThread, args=(i,))
    print("Starting thread %d at %s" % (i, ctime(time.time())))
    thread.start()
    thread.join(1)
    print("Thread alive: ",thread.is_alive())
```
Output:
```console
Creating thread 0 at Fri Sep 18 16:51:42 2020
Starting thread 0 at Fri Sep 18 16:51:42 2020
Thread 0: started at Fri Sep 18 16:51:42 2020
Thread alive:  True
Creating thread 1 at Fri Sep 18 16:51:43 2020
Starting thread 1 at Fri Sep 18 16:51:43 2020
Thread 1: started at Fri Sep 18 16:51:43 2020
Thread 0: finished at Fri Sep 18 16:51:44 2020
Thread alive:  True
Creating thread 2 at Fri Sep 18 16:51:44 2020
Starting thread 2 at Fri Sep 18 16:51:44 2020
Thread 2: started at Fri Sep 18 16:51:44 2020
Thread 1: finished at Fri Sep 18 16:51:45 2020
Thread alive:  True
Thread 2: finished at Fri Sep 18 16:51:46 2020
```

- The `threading` module also provides the following additional methods -

  - `threading.active_Count()` âˆ’ It returns the number of active threads.
  - `threading.current_Thread()` âˆ’ It returns the current thread being executed.
  - `threading.enumerate()` âˆ’ It returns a list of active threads.

The below example uses these additional methods.
```python
import threading
import time
from time import ctime


def myThread(num):
    print("Current thread: ", threading.current_thread().getName())
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(2)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


for i in range(0, 2):
    thread = threading.Thread(target=myThread, args=(i,))
    thread.start()
    thread.join(1)
    print("Number of active threads: ", threading.active_count())
    print("Current thread: ", threading.current_thread().getName())
    print("Active threads: ")
    for j in threading.enumerate():
       print(j.getName(), end=";")
```
Output:
```console
Current thread:  Thread-1
Thread 0: started at Fri Sep 18 17:32:26 2020
Number of active threads:  2
Current thread:  MainThread
Active threads: 
MainThread;Thread-1;
Current thread:  Thread-2
Thread 1: started at Fri Sep 18 17:32:27 2020
Thread 0: finished at Fri Sep 18 17:32:28 2020
Number of active threads:  2
Current thread:  MainThread
Active threads: 
MainThread;Thread-2;
Thread 1: finished at Fri Sep 18 17:32:29 2020
```
### Daemon vs non-daemon threads

 > A `daemon` thread is a which runs in the background. It has low priority so that it doesn't affect execution of other threads.

`Daemons` threads can be killed when all the `non-daemon` threads have been executed successfully. For instance,
```python
import threading
import time
from time import ctime

def daemon_thread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(3)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


def non_daemon_thread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(2)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


thread1 = threading.Thread(target=daemon_thread, args=(1,), daemon=True)
thread2 = threading.Thread(target=non_daemon_thread, args=(2,))
thread1.start()
thread2.start()
```
Output:
```console
Thread 1: started at Fri Sep 18 17:51:47 2020
Thread 2: started at Fri Sep 18 17:51:47 2020
Thread 2: finished at Fri Sep 18 17:51:49 2020
```
Here `daemon_thread`, *Thread 1* is killed as soon as `non_daemon_thread` *Thread 2* has finished.

But what if we want the daemon thread to finish as well? `join()` method comes to the rescue.

```python
import threading
import time
from time import ctime

def daemon_thread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(3)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


def non_daemon_thread(num):
    print("Thread %d: started at %s" % (num, ctime(time.time())))
    time.sleep(2)
    print("Thread %d: finished at %s" % (num, ctime(time.time())))


thread1 = threading.Thread(target=daemon_thread, args=(1,), daemon=True)
thread2 = threading.Thread(target=non_daemon_thread, args=(2,))
thread1.start()
thread2.start()
thread1.join()
thread2.join()
```
Output:
```console
Thread 1: started at Fri Sep 18 18:05:53 2020
Thread 2: started at Fri Sep 18 18:05:53 2020
Thread 2: finished at Fri Sep 18 18:05:55 2020
Thread 1: finished at Fri Sep 18 18:05:56 2020
```
Unlike earlier, `Thread 1` has also finished.

## Conclusion
`Threading` is an important concept in Python.
In this tutorial, we have learned the concept of `threads` and `multithreading` in Python using two modules, `_thread`, and `threading`. We also learned about different methods provided by these modules like `start()`, `join()`, `is_alive()`, `current_Thread()`, `active_Thread`, and `enumerate`. With this article, we should also have a clear understanding of when and when not to use threading.