---
date: 2017-05-25
linktitle: What is main function
title: What is if __name__ == "__main__" in Python?
weight: 10
url: /if-name-main-in-python
description: 
keywords:
  - pandas
---

If you are new to python then you may have noticed ```if __name__ == "__main__"``` line in some python codes. 

You may be wondering:
 
 - What does that mean? 
 - What purpose does it serve?
 - I don't see it in all python codes, so when should I use it exactly?
 - Can you give me some examples?
 
 Let me try to explain the above to you.

```python
if __name__ == "__main__":
	print "Directly called from python interpreter"
else:
	print "Not directly called"
```

Run the above code as below:

```sh
$ python main.py
```

Output:

```sh
Directly called from Python interpreter
```

Now run the same code as below:

```sh
$ python
>>> import main.py
```

Output:

```sh
Not directly called
```

### References

- [What does if \_\_name\_\_ == “\_\_main\_\_”: do?](https://stackoverflow.com/questions/419163/what-does-if-name-main-do)
- [A module's \_\_name\_\_](http://ibiblio.org/g2swap/byteofpython/read/module-name.html)