---
date: 2017-05-31
linktitle: How to run a Python module as script?
title: How to run a Python module as script?
weight: 10
url: /run-module-as-script-python
description: If you want to run the module itself as a script then you should use the __name__ variable.
keywords:
  - python
  - name
  - main
  - script
  - module
  - import
tags: [python]
---
Suppose you have a module named `mymath.py`, which has a couple of functions. You can import this module in your script and call these functions.

```python
def int_sum(a, b):
    print a+b

def some_other_function():
    pass
```

But, what if you want to run the module itself as a script?

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Well, if you want to use a Python module as script then you just have to use the conditional for `__name__`.

```python
def int_sum(a, b):
    print a+b

if __name__ == "__main__":
    import sys
    int_sum(int(sys.argv[1]),int(sys.argv[2]))
```

Now you can run the above as:

```sh
$ python mymath.py 1 2
3
```

This works because the value of built-in `__name__` variable is set to `__main__` if the Python code is executed directly through the interpreter. If you use the above module in a script using `import` then in that case the value of `__name__` is the filename of module.

### Also see
- [How to convert a Python script to module](https://tutswiki.com/convert-python-script-to-module)
- [PEP 338 -- Executing modules as scripts](https://www.python.org/dev/peps/pep-0338/)
- [What is if __name__ == "__main__" in Python?](/if-name-main-in-python/)
- [What is the difference between a module and a script in Python?](https://stackoverflow.com/questions/2996110/what-is-the-difference-between-a-module-and-a-script-in-python)