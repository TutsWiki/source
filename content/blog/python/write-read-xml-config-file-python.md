---
date: 2020-09-16
linktitle: Writing and Reading XML config file in Python
title: Writing and Reading XML config file in Python
weight: 10
url: /read-write-xml-config-file-in-python
description: Learn how to write and read XML config files in Python using pyyaml module.
keywords:
  - python
  - config
  - read config file
  - write config file
  - configparser
  - XML
  - BeautifulSoup
  - htmlparser
tags: [Python]
---
We've already covered `.ini`, `.json` and `.yaml` in below articles, in this article we'll focus on `.xml` config files.

- [.ini file in Python](/read-write-config-files-in-python/)
- [.json file in Python](/read-write-json-config-file-in-python/)
- [.yaml file in Python](/read-write-yaml-config-file-in-python/)

## Reading and Writing config data to XML file in Python
`XML`, or `eXtensible Markup Language` is a markup language just like HTML that can be interpreted by both humans and computers easily. XML does not have predefined tags. For instance,

File: tutswiki.xml
```
<mail>
 <subject>
  config file
 </subject>
 <receiver>
  www.tutswiki.com
 </receiver>
 <content>
  This is an article
 </content>
</mail>
```
As we can see, all the tags used are user-defined which makes XML self-explanatory. We will be using the same file for reading and writing data.

For parsing the XML file, we will be using the `BeautifulSoup` module along with `html parser`. First, we need to install the latest `BeautifulSoup4` package using the following command.

```console
pip install BeautifulSoup4
```
We then have to `import BeautifulSoup` module from `bs4` (BeautifulSoup4).

```python
from bs4 import BeautifulSoup
```

Now, we are all set to parse our file.

### Creating XML config file in Python
To add a new tag in our XML file `tutswiki.xml`, we use `new_tag()` method which takes an `XML tag` as a parameter.
 After creating a new tag, we use `insert()` method. This method takes 2 parameters, `tag position`, and the `tag` that we created earlier.

```python
from bs4 import BeautifulSoup

with open("tutswiki.xml", "r") as f:
    content = f.read()
    y = BeautifulSoup(content, features="html.parser")
    new_tag = y.new_tag("h1")
    y.mail.insert(2,new_tag)
f = open("tutswiki.xml", "w")
f.write(y.prettify())
```
So we have created a tag `h1` which will be inserted at position 2, i.e, after `<subject>`.

File: tutswiki.xml
```xml
<mail>
 <subject>
  config file
 </subject>
 <h1>
 </h1>
 <receiver>
  www.tutswiki.com
 </receiver>
 <content>
  This is an article
 </content>
</mail>
```
Here, `prettify()` method is used to convert the data into proper XML format.
If we want to add `attribute` and `content` to the new tag, we write
```
new_tag.string = "This is a heading" # content
new_tag['name'] = 'heading' # attribute
```
which will get
```
<h1 name="heading">
  This is a heading
</h1>
```

### Reading a key from XML config file
For reading, we will have to traverse to the tag which we want to read using the dot (.) operator. Let's say we want to read the content of `<h1>` and the value of the attribute `name`.
```python
from bs4 import BeautifulSoup

with open("tutswiki.xml", "r") as f:
    content = f.read()
    y = BeautifulSoup(content, features="html.parser")
    tag = y.mail.h1
    print(tag.string)
    print(tag['name'])
```
Here `tag.string` fetches the value inside the tag `<h1>`, and we get,

Output:
```console
This is a heading
heading
```

### Updating a key in XML config file
If we want to update tag name `h1` to `h2`, we will use `name` property.

```python
from bs4 import BeautifulSoup

with open("tutswiki.xml", "r") as f:
    content = f.read()
    y = BeautifulSoup(content, features="html.parser")
    tag = y.mail.h1
    tag.name = 'h2'
f = open("tutswiki.xml", "w")
f.write(y.prettify())
```

File: tutswiki.xml
```xml
<mail>
 <subject>
  config file
 </subject>
 <h2 name="heading">
  This is a heading
 </h2>
 <receiver>
  www.tutswiki.com
 </receiver>
 <content name="article">
  This is an article
 </content>
</mail>
```
