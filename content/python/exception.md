---
date: 2020-09-08
linktitle: Exceptions
menu:
  main:
    parent: python
next: /python/overview
title: Exceptions in Python
weight: 5
url: /python/exceptions/
description: Python Exception is an event that occurs at runtime which disrupts the flow of execution of the program.
keywords:
  - Python
  - Exceptions
tags: [Python]  
---
In this tutorial, we will be learning about ***Exceptions***. So without further ado, let's get started.

## What is an Exception?
The very first question that pops up in our minds.

> An Exception is an unexpected problem or issue that alters the normal flow of execution.

## What is an Exception in Python?

> Exception in Python is an event that occurs at runtime which disrupts the flow of execution of a program and terminates it abnormally if left unhandled.

Whenever an exception occurs, our program stops executing and returns an error traceback taking user to the error along with its details.

For Example, the exception `ZeroDivisionError` occurs when we divide a number by 0.
```python
def division(a,b):
    print(a/b)
    
division(2,0)
```
In output, we get an exception with traceback
```python
Traceback (most recent call last):
  File "main.py", line 4, in <module>
    division(2,0)
  File "main.py", line 2, in division
    print(a/b)
ZeroDivisionError: division by zero
```
## Types of exceptions
There are 2 types of exceptions in Python

1. Built-in
2. User-defined

### Built-in exceptions
Following are some of the Built-in exceptions provided by Python.

|Exception |Reason |
| ----------- | ----------- |
| AssertionError | Failed assert statement |
| AttributeError | Failed attribute reference or assignment |
| EOFError | Reached end-of-file (EOF) without reading any data from raw_input() or input() (Python 2.7 & Python 3.x respectively) |
| ImportError | Import statement failed to load a module or it was not found |
| IndexError | Index is not within the range (Generally while traversing in lists) |
| KeyboardInterrupt | An unexpected key press interruption occurred (like Ctrl-C) |
| MemoryError | Execution ran out of memory |
| NameError | Local or global variable name not found |
| OverflowError | Arithmetic result is too large to represent |
| ReferenceError | Object referred already removed by Garbage Collector |
| RuntimeError | When no matching category found |
| SyntaxError | Syntax error found while parsing the code |
| IndentationError | Unexpected or incorrect indentation |
| TabError | Inconsistant use of spaces or tabs for indentation |
| TypeError | Operation performed between incompatible types |
| TimeoutError | Program not executed within a timeframe |
| UnboundLocalError | Local variable referred but no value bounded to it |
| UnicodeError | Error in unicode related encoding or decoding |
| ValueError | Not a TypeError but unexpected value provided |
| ZeroDivisionError | Number divided by zero |

We handle the exceptions raised using `try`, `except` and `finally` statements.

> For `User-defined exceptions` and `Exception handling`, visit [Exception handling in Python](/python/exception-handling).
