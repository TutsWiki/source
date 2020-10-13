---
date: 2020-10-13
linktitle: Modules
menu:
  main:
    parent: python
next: /python/overview
title: Modules in Python
weight: 5
url: /python/modules
description: A module is a file that consists of constants, variables, functions, and classes. In python, modules have the extension .py. It can be built-in or user-defined. import, reload, sys path.
keywords:
  - Python
  - Modules
  - import
tags: [Python]  
---
<meta property="og:image" content="https://tutswiki.com/images/cplusplus/process-address-space.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Modules in Python" />
<meta name=”twitter:description” content="A module is a file that consists of constants, variables, functions, and classes. In python, modules have the extension .py. It can be built-in or user-defined. import, reload, sys path." />
In this article, we will be learning about modules in Python. Let us begin by defining the term module.

## What is a Module?
A module is a file that consists of constants, variables, functions, and classes. In python, modules have the extension `.py`. It can be built-in or user-defined.

A module provides code reusability. We can import modules in a program (which we will be learning in the next section) and use its functions, classes, and variables so that we do not have to write them repeatedly hence reducing the length of the code.

## Creating a Module
For example, let's start by creating a module, `factorial.py`.

File: factorial.py
```python
SPEED_1 = 5
SPEED_2 = 10

def fact(number):
    if number in (0, 1):
        return 1
    else:
        return number * fact(number - 1)
```
Here, we have defined constants `SPEED_1` and `SPEED_2` with the values `5` and `10` and defined a function `fact()` that takes a number as an input and returns its factorial. 

In the next section, we will learn how to import modules and use their variables and functions in our code.

## Import modules in Python
To import a complete module, Python provides us with a keyword `import`. In the following example, we will import the `factorial.py` module to calculate the factorial of a number.

### Using the import keyword
File: test_factorial.py
```python
import factorial

num = 5
factorial_5 = factorial.fact(num)
print("Factorial of %d is %d" %(num, factorial_5))
``` 

Here, we have accessed `fact()` method of the `factorial` module using the dot (`.`) operator.

Output:
```console
Factorial of 5 is 120
```

### By using an alias name
Now let's say we want to use the `factorial` module multiple times. To make our code compact, we can give **alias** name to the imported module by writing,
```python
import factorial as f
```

Now, we can use `SPEED_1` and `fact()` method as

```python
print("Factorial of %d is %d" %(f.SPEED_1, f.fact(f.SPEED_1)))
print("Value of SPEED_2 = %d" %(f.SPEED_2))
```
Which also gives the same output

Output:
```console
Factorial of 5 is 120
Value of SPEED_2 = 10
```

**Note:** If we give alias `f` to the module, the module name `factorial` will not be recognized anymore.

### Using from...import statement
What if we only want to import the variable `SPEED_1` and the `fact()` method? For this, we will only import `SPEED_1` and `fact()` as done below.

```python
from factorial import fact, SPEED_1
```

In this case, we can directly write **variable** and **method** names to use them.

```python
print(fact(5))
print(SPEED_1)
```

And we get the following output

Output:
```console
120
5
```

### Import all using an asterisk (*)

We can also import all the functions, variables, and classes defined in the module using an asterisk (`*`).

```python
from factorial import *
```

**Note:** Let's say we create a variable, a function, or a class in our code with the same name as defined inside the module. It can lead to the definition duplicacy. Therefore, this method is not recommended.

### Importing from subdirectories

There can be cases where we would like to import from subdirectories. To achieve that, we can use the absolute path of the module.

Consider the following folder structure

```
my_project                     # root folder(level 0)
    |-- test.py
    |-- util
        |-- printer.py
    |-- source                 # folder at level 1
        |-- code               # folder at level 2
            |-- factorial.py
```

If we want to use the `fact()` method of the `factorial` module, we will have to go all the way up to folder level 2 and import `fact()` as shown.

File: test.py
```python
from source.code.factorial import fact

print(fact(5))
```

Output:
```console
120
```
But what if we have a deep or complex folder structure? In that case, it will be impractical to use absolute paths.

Here, `__init__.py` comes into the picture.

We create an `__init__.py` file inside a directory to be considered as a module. Also, inside the `__init__.py` file, we can write the following code to reduce the path complexity for the above folder structure.

New folder structure

```
my_project                      # root folder(level 0)         
    |-- test.py
    |-- util
        |-- printer.py
    |-- source                  # folder at level 1
        |-- __init__.py        
        |-- code                # folder at level 2
            |-- factorial.py
```
Add the following code inside the `__init__.py` file.

File: \_\_init\_\_.py
```python 
from .code.factorial import fact
```

Now, in our test.py file, we only have to write
```python
from source import fact

print(fact(5))
```

Output:
```python
120
```

## Module Search Path
Until now, we have learned about modules and how to import them.  But how does the python interpreter find the modules we import?

First, the python interpreter looks for a **built-in** module. If the required module is not found, it looks into the directories mentioned in `sys.path`.

The following code will print a list of values inside `sys.path`.

```python
from sys import path

print(path)
```

**Output**
```
[
'C:\\Users\\user\\PycharmProjects\\my_project',
'C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python37-32\\python37.zip', 'C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python37-32\\DLLs', 'C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python37-32\\lib', 'C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python37-32','C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python37-32\\lib\\site-packages'
]

Process finished with exit code 0
```

The search order is,

- The current working directory, **my_project**.
- `PYTHONPATH`, which is an environment variable that contains a list of directories
- Default directory based on the installation process.

## Reloading a module
A module in a python script is imported only once by the interpreter for efficiency. 

Consider the following module, `printer.py`
```python
number = 5
print(number)
```

Now if we import this module in our script file as
```python
>>> import printer
5
```
The value of `number` is printed.

Now let's modify the `printer.py` module by changing the value of `number` to `6`. But if we import the module again, we don't see any output.

```python
>>> import printer
5
>>> import printer
```

**Why is it so?**

If we import a module and make changes to it, those changes will not be reflected in the current script. For this, we need to either restart the interpreter or reload the module by using the `reload()` function.

The `reload()` function is defined inside the `importlib` module which we need to import.

```python
>>> import printer
5
>>> import printer
>>> from importlib import reload
>>> reload(printer)
6
```

## dir() function

Python provides us with a function `dir()` which can be used to find the names that are defined in a module. It returns a sorted list with the names of the functions, variables, and classes of the module. For example

```python
import factorial

print(dir(factorial))
```
We get,

Output:
```
[
'__builtins__', 
'__cached__', 
'__doc__', 
'__file__', 
'__loader__', 
'__name__', 
'__package__', 
'__spec__', 
'fact', 
'SPEED_1', 
'SPEED_2'
]
```

Here, apart from the names defined inside the module `factorial`, we get names with underscores. These are the default built-in python attributes provided to the module.

## Executing a module as a script file

We already know that a module in Python is simply a Python script file with extension `.py`. So, like any script file, the module can also be executed. 

We can execute a module (`printer.py` in our case) using `cmd` on Windows or `terminal` on Linux/Mac and run the following command.

```terminal
python printer.py
```
And we get the output,

```
6
```

You may have noticed that we got the same output when we imported the `printer.py` module in the `Reloading a module` section. So, how do we differentiate between when we run the module as a script and when the file is imported as a module?

> If the module is run as a script file, the python interpreter sets the special variable `__name__` to the value `__main__`. If this file is imported as a module, `__name__` will be set to the name of the module.

So, we add the following code where we check if the value of `__name__` is equal to `__main__`. If true, it will mean that the module is run as a script. Otherwise, it is imported as a module.

File: printer.py
```python
SPEED_1 = 5
SPEED_2 = 10


def fact(number):
    if number in (0,1):
        return 1
    else:
        return number * fact(number - 1)


if __name__ == '__main__':
    print(fact(5))
```

**As imported**
```python
>> import printer
>> printer.fact(5)
120
```
**As a script file**
```python
C:\Users\user\PycharmProjects\my_project> printer.py 5
120
```

## Conclusion
In this section, we learned

- What is a Module, and how it is created and used?
- How a module is searched by the python interpreter and listing its content
- Reloading a module and executing the module as a script file

## Also see
- [What is if __name__ == "__main__" in Python?](/if-name-main-in-python/)
- [How to run a Python module as script?](/run-module-as-script-python)
- [How to convert a Python script to module?](/convert-python-script-to-module/)
- [PEP 338 -- Executing modules as scripts](https://www.python.org/dev/peps/pep-0338/)
- [What is the difference between a module and a script in Python?](https://stackoverflow.com/questions/2996110/what-is-the-difference-between-a-module-and-a-script-in-python)
