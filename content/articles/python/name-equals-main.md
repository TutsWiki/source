---
date: 2017-05-24
linktitle: What is main function
title: What is if __name__ == "__main__" in Python?
weight: 10
url: /if-name-main-in-python
description: If you are new to Python then you may have noticed if __name__ == "__main__" line in some python codes.
keywords:
  - python
  - name
  - main
  - built in
  - module
  - import
categories: [development, publishing]
tags: [hugo,content,static site generator]
---

If you are new to Python then you may have noticed ```if __name__ == "__main__"``` line in some python codes. 

You may be wondering:
 
 - What does that mean? 
 - What purpose does it serve?
 - I don't see it in all Python codes, so when should I use it exactly?
 - Can you give me some examples?
 
Let me try to explain the above to you.

In Python all modules have some built-in attributes. `__name__` is one of them. Now the question is what does `__name__` contain?

Well, that depends actually. It depends on how you use the module. 
 
## Case 1: Running the module directly

If you run the module directly in a standalone program then in that case the value of `__name__` attribute is set to `__main__`.

For example, create a file `main.py` and enter below code.

```python
if __name__ == "__main__":
	print "Directly called from python interpreter"
	print "Value of __name__ attribute is "+__name__
else:
	print "Not directly called"
	print "Value of __name__ attribute is "+__name__
```

Now run the above code as below:

```sh
$ python main.py
```

Output:

```sh
Directly called from Python interpreter
Value of __name__ attribute is __main__
```

Notice that when we ran the program directly from python interpreter the conditional `__name__ == __main__` returned `True` and the print statement inside the if block got executed. 

## Case 2: Using the module with import

If you use the module in another program (using the `import` function), then in that case the value of `__name__` attribute is set to the filename of the module.

Let's try to import the above created `main.py`.

```sh
$ python
>>> import main.py
```

Output:

{ {< highlight python >}}
Not directly called
Value of __name__ attribute is main
{ {< /highlight >}}

```sh
Not directly called
Value of __name__ attribute is main
```

### References

- [\_\_main\_\_ — Top-level script environment](https://docs.python.org/3/library/__main__.html)
- [What does if \_\_name\_\_ == “\_\_main\_\_”: do?](https://stackoverflow.com/questions/419163/what-does-if-name-main-do)