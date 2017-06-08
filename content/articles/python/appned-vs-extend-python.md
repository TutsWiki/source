---
date: 2017-06-08
title: Difference between append and extend in Python
weight: 10
url: /append-vs-extend-python
description: What is the difference between list methods append and extend in Python?
keywords:
  - python
  - list
  - append
  - extend
  - combine
  - merge
tags: [python]
---
`append` and `extend` are list methods in Python which can be used to combine multiple lists. But what is the difference between them? When should you use one over another, let's find out.

The official documentation describes them as:

- `list.append(x)`: Add an item to the end of the list; equivalent to `a[len(a):] = [x]`.

- `list.extend(L)`: Extend the list by appending all the items in the given list; equivalent to `a[len(a):] = L`.


