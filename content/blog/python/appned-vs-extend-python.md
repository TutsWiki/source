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
tags: [Python]
---
`append` and `extend` are list methods in Python which can be used to combine multiple lists. But what is the difference between them? When should you use one over another, let's find out.

The [official documentation](https://docs.python.org/2/tutorial/datastructures.html#more-on-lists) describes them as:

- `list.append(x)`: Add **an item** to the end of the list; equivalent to `a[len(a):] = [x]`.

- `list.extend(L)`: Extend the list by appending **all the items** in the given list; equivalent to `a[len(a):] = L`.

Notice the bold words in the definition. append adds `an item` and extend `appends all the items of the given list`.

Let's take an example:

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

## append a list to another list in Python

```python
some_list = ['string', 1, 'another string', 6.5]
another_list = ['new data', 12]
some_list.append(another_list)
print some_list
```

Output:

```sh
['string', 1, 'another string', 6.5, ['new data', 12]]
```

As you can see, the `append` method took `an item` i.e. the object of another list and added it to `some_list` as it is. 

## extend a list with another list in Python

```python
some_list = ['string', 1, 'another string', 6.5]
another_list = ['new data', 12]
some_list.extend(another_list)
print some_list
```

Output:

```sh
['string', 1, 'another string', 6.5, 'new data', 12]
```

In case of `extend` you can see that all the items from `another_list` were appended to `some_list` one by one. So you can say extend method concatenates one list with another list.

## Quiz

Q: What will be the output of below code:

```python
some_list = ['string', 1, 'another string', 6.5]
data = 'hello'
some_list.extend(data)
print some_list
```

A: Output is

```sh
['string', 1, 'another string', 6.5, 'h', 'e', 'l', 'l', 'o']
```

Explanation: If you notice carefully, `data` is a string and `strings in Python are iterable`. Therefore the `extend` method iterated over all characters in the string one by one and appended them to the list.

So based on above we can say that below 2 code snippets are equivalent:

```python
for data in iterator:
    some_list.append(data)
```

```python
some_list.extend(iterator)
```
