---
date: 2017-06-05
title: How to print on same line with print in Python
weight: 10
url: /print-same-line-python
description: Use a comma at the end of print function to write the data on same line
keywords:
  - python
  - print
  - python print
tags: [python]
---
In Python, when you use the print function, it prints a new line at the end.

For example:

```python
print "This is some line."
print "This is another line."
```

Output:

````sh
This is some line.
This is another line.
````

What if you want to avoid the newline and want to print both statements on same line? Well, there are 2 possible solutions.

## Add comma at the end of print

```python
print "This is some line.",
print "This is another line."
```

Output:

```sh
This is some line. This is another line.
```

If you are using Python 3 then use the below:

```python
print ("This is some line.", end="")
print ("This is another line.")
```

## Use sys module

```python
import sys
sys.stdout.write("This is some line.")
sys.stdout.write("This is another line.")
```
