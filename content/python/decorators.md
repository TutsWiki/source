---
date: 2020-09-08
linktitle: Decorators
menu:
  main:
    parent: python
next: /python/overview
title: Decorators in Python
weight: 5
url: /python/decorators
description: Decorators in Python are nothing but the Gift Wrapping but for functions and classes
keywords:
  - Python
  - Decorators
  - Functions
tags: [Python]  
---
Have you ever heard about Gift wrappings? Exactly, those which we do on the presents to be gifted. Decorators in Python are nothing but the Gift Wrapping but for functions and classes. In this tutorial, we will deep dive into the implementation of decorators on functions. But before you get into the topic, you should have a proper understanding of functions in Python. Assuming that you have, let's get started.

## Definition
A decorator is nothing but a function that takes a function to be decorated as its parameter, and returns a function.
A decorator is used to extend the functionality of a function by wrapping it in another function, i.e, The decorator function, without modifying the base function.

## Syntax
```python
@decoration_function 
def test_1():
	print("Hello World")
```
The above function simply means,
```python
test_1 = decoration_function(test_1)
```
Here, `@decoration_function` denotes a decorator.
Now let's define the decorator function `decoration_function`.
```python
def decoration_function(function_to_be_decorated):
    def test_2():
	    print("Decorated your function")
	    function_to_be_decorated()
    return test_2
```
Note: The decorator definition should be above the `function_to_be_decorated`.

We are all done decorating our function `test_1()`. Now, let's try it out.
```python
test_1() //Calling the function
```
Complete Code:
```python
def decoration_function(function_to_be_decorated):
    def test_2():
    	print("Decorated your function")
    	function_to_be_decorated()
    return test_2
    
@decoration_function
def test_1():
	print("Hello World")

test_1()
```
The sequence of execution is as follows:

1. `decoration_function(test_1)`
2. `return test_2`
3. `def test_2()`
4. `print("Decorated your function")`
5. `test_1()`
6. `def test_1()`
7. `print("Hello World")`

Output:
```python
Decorated your function
Hello World
```
## Decorating functions with Parameter
Sometimes we stumble upon situations where our function takes parameters, so let us consider a function `addition()` which takes 2 integer parameters, `a` and `b`, and returns their sum.
```python
@decorate_addition
def addition(a,b):
	print(a+b)
```
Let us decorate the above function which handles situations where `a` or `b` is a negative integer.

```python
def decorate_addition(function):
	def handler(a,b):
		if(a < 0 or b < 0):
			print("Invalid values")
			return
			
		return function(a,b)
	return handler
```
Now, it's time to call our function `addition()`
```python
addition(4,6)
addition(4,-6)
```
Output:
```
10
Invalid values
```
## Using multiple decorators
We can also add multiple decorators to our function. So let’s add another decorator on the above `addition()` function.
```python
def max_range(function):
	def ranged(a,b):
		if(a > 100 or b > 100):
			print("Values out of range")
			return
			
		return function(a,b)
	return ranged
```
And modifying our decorators by adding `@max_range`,
```python
@max_range
@decorate_addition
def addition(a,b):
	print(a+b)
```
Let’s call our function again,
```python
addition(400,-6)
```
Output:
```
Values out of range
```
Note: The sequence of decorators above `addition()` matters. It is executed from top to bottom. For instance,
```python
@decorate_addition
@max_range
def addition(a,b):
	print(a+b)
```
Let's call our function again,
```python
addition(400,-6)
```
Output:
```
Invalid values
```


## Decorators with multiple arguments
Sometimes we come across situations where we have to pass multiple arguments, positional, or keyword. Python provides us with ***args** (tuple for positional arguments) and ****kwargs** (dictionary for keyword arguments).

## Keyword Arguments
```python
def multiple_args(function_to_be_decorated):
    def internal(*args,**kwargs):
        print("Positional arguments", args)
        print("Keyword arguments", kwargs)
        function_to_be_decorated(*args)
    return internal

@multiple_args
def test_fun():
    print("Keyword Args")

test_fun(name="TutsWiki", language="Python")
```
Output:
```
Positional arguments ()
Keyword arguments {'language': 'Python', 'name': 'TutsWiki'}
Keyword Args
```
## Positional Arguments
```python
def multiple_args(function_to_be_decorated):
    def internal(*args,**kwargs):
        print("Positional arguments", args)
        print("Keyword arguments", kwargs)
        function_to_be_decorated(*args)
    return internal

@multiple_args
def test_fun(a,b):
    print("Positional Args")

test_fun(1,2)
```
Output:
```
Positional arguments (1, 2)
Keyword arguments {}
Positional Args
```