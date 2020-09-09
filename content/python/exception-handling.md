---
date: 2020-09-09
linktitle: Exception Handling
menu:
  main:
    parent: python
next: /python/overview
title: Exception Handling in Python
weight: 6
url: /python/exception-handling/
description: Python provides us `try`, `raise`, `except` and `finally` blocks for handling raised exceptions properly. Built-In and User-defined Exceptions are also covered.
keywords:
  - Python
  - Exceptions
  - try
  - except
  - finally
  - raise
  - else
tags: [Python]  
---
Now that we have knowledge of [Exceptions](/python/exceptions) and its types, the question arises,

> How do we handle exceptions so that flow of our program does not stop abruptly?

Well, for that **Python Exception Handling** comes to our rescue.

## Exception Handling in Python
Python provides us `try`, `raise`, `except` and `finally` blocks for handling raised exceptions properly. Let us understand each of them using an example.

### try and except
```python
try:
    x = int(input("Enter an integer: "))
except ValueError as te:
    print("Exception Occured:",te)
```
Output:
```
Enter an integer: a
Exception Occured: invalid literal for int() with base 10: 'a'
```

### try, except and raise
```python
try:
    x = int(input("Enter an integer less than 10: "))
    if(x > 10):
        raise ValueError("Number not less than 10")
    print("Successfully entered",x)
except ValueError as ex:
    print(ex)
```
Inside the `try` block, we write code which may raise an exception. Here, we have taken an integer input which should be less than 10. If not, we have used `raise` statement to manually raise `ValueError` exception which is a built-in exception. We have also added a custom message inside the same.  

Now if we enter a number greater than 10, say x = 11, try block will throw the exception `ValueError` raised by `raise` statement which will be then caught by `except` which is used to catch any exception raised inside the `try` block. `except` will then print our custom message `Number not less than 10`. Let us see the output by entering values.

Output:
```
Enter an integer less than 10: 11
Number not less than 10
```
```
Enter an integer less than 10: 9
Successfully entered 9
```

### try, except and finally
`finally` is an optional block which will run no matter what. It is used to release all the resources which were used by the program.

```python
try:
    x = int(input("Enter an integer less than 10: "))
    if(x > 10):
        raise ValueError("Number not less than 10")
    print("Successfully entered",x)
except ValueError as ex:
    print(ex)
finally:
    print("Thank You")
```
Output:
``` 
Enter an integer less than 10: 11
Number not less than 10
Thank You
```

### try, except and else
`else` block with `try` runs only if no exception is raised inside the try block. Here, try block will look for ZeroDivisionError exception if raised and will print the message `Caught ZeroDivisionError`, otherwise the `else` block will execute. So basically it works as an if...else conditional statement.

```python
def myFun(a,b):
    try:
       a/b
    except ZeroDivisionError:
       print("Caught ZeroDivisionError")
    else:
        print("Successful")
```
If we call above function as

```python
myFun(1,2)
```

then output

```
Successful
```

and if 

```python
myFun(1,0)
```

then output

```
Caught ZeroDivisionError
```

Now that we have learned about `try`, `raise`, `except` and `finally` statements, let's use them with Built-in and User-defined exceptions.

## Built-in Exceptions
We can handle [Exceptions](/python/exceptions) using the try/except/raise/finally/else blocks we have learned above. For example,
```python
myList = [0,5,4,8,4]
for i in range(0,6):
    print(myList[i])
```
We can see that there are 5 integers in the list `myList` but our for loop is iterating 6 times. So when the loop reaches the 6th iteration, it raises the following exception,
Output:
```
0
5
4
8
4
Traceback (most recent call last):
  File "main.py", line 3, in <module>
    print(myList[i])
IndexError: list index out of range
```
Now we do not want our program to stop abruptly, so we use exception handling.

#### Example
```python
myList = [0,5,4,8,4]
try:
    for i in range(0,6):
        print(myList[i])
except:
    print("Exception occured")
finally:
    print("Bye")
```
Output:
```
0
5
4
8
4
Exception occurred
Bye
```

#### Catch Specific Exceptions
```python
myList = [0,5,4,8,4,'a']
try:
    for i in range(0,6):
        print(myList[i]**2)
except IndexError as ie:
    print(ie)
except TypeError as te:
    print(te)
except:
    print("Some Exception occured")
finally:
    print("Bye")
```
Output:
```
0
25
16
64
16
unsupported operand type(s) for ** or pow(): 'str' and 'int'
Bye
```
Let's take another example where we 

#### Catch multiple Errors with single Except block

```python
def myFun():
   try:
       1/0
   except (TypeError, ZeroDivisionError):
       print("Caught TypeError or ZeroDivisionError")
       
myFun()
```
Output:
```
caught TypeError or ZeroDivisionError
```

## User-defined exceptions
Sometimes the built-in exceptions are not good enough to handle the exceptions properly. In that case, user has an option to define a new exception by creating a custom base class which derives from the `Exception` class. For instance,

```python
class CustomClass(Exception):
    pass
```
Here `CustomClass` is a custom base class which derives from `Exception` class.
Now the user-defined exceptions derive from this base class.

```python
class CustomException1(CustomClass):
    pass
```
There can be multiple user-defined exceptions,
```python
class NotValidError(CustomClass):
    pass

class NotFoundError(CustomClass):
    pass
```
Now let's understand using an example

#### Example
Suppose we want our user to input a customer Id which starts with `CID` and is of 6 digits. For that, we define a function `myFun()` which takes a string input `x` and checks whether or not the string length is 6 and it starts with **`CID`**. 

```python
def myFun():
    try:
        x = input("Enter your Customer ID: ")
        if(len(x) != 6):
            raise NotValidError
        elif(x[0:3] != "CID"):
            raise NotFoundError
        else:
            print("Successful")
    except NotValidError:
        print("Length should be 6")
    except NotFoundError:
        print("Should start with 'CID'")
    finally:
        print("Thank You")
```
Output:
```
Enter your Customer ID: CID999
Successful
Thank You
```
```
Enter your Customer ID: CI9898
Should start with 'CID'
Thank You
```
```
Enter your Customer ID: CID9898
Length should be 6
Thank You
```

## Conclusion
Exception handling is an important concept which makes sure that our program flow does not get disrupted unexpectedly or abruptly, the exception raised is handled properly and execution continues smoothly.
