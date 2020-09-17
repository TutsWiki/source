---
date: 2020-09-16
linktitle: Writing and Reading YAML config file in Python
title: Writing and Reading YAML config file in Python
weight: 10
url: /read-write-yaml-config-file-in-python
description: Learn how to write and read YAML config files in Python using pyyaml module.
keywords:
  - python
  - config
  - read config file
  - write config file
  - configparser
  - yaml
  - pyyaml
tags: [Python]
---
We've already covered `.ini`, `.json` and `.xml` in below articles, in this article we'll focus on `.yaml` config files.

- [.ini file in Python](/read-write-config-files-in-python/)
- [.json file in Python](/read-write-json-config-file-in-python/)
- [.xml file in Python](/read-write-xml-config-file-in-python/)

## Reading and Writing config data to YAML file in Python
`YAML` or `YAML Ain't Markup Language` is a case sensitive and human-friendly data serialization language used mainly for configurations.

For reading and writing to the YAML file, we first need to install the `PyYAML` package by using the following command.

```console
$ pip install pyyaml
```
Let's now dive into read and write operations to the `YAML` file.

### Creating YAML config file in Python
We can write data to the `YAML` file using `yaml.dump()` method which takes `python object` and `file pointer` as parameters.

In the below example, we are writing the dictionary `article_info` to the YAML file `tutswiki.yaml`.
```python
import yaml

article_info = [
    {
        'Details': {
        'domain' : 'www.tutswiki.com',
        'language': 'python',
        'date': '11/09/2020'
        }
    }
]

with open("tutswiki.yaml", 'w') as yamlfile:
    data = yaml.dump(article_info, yamlfile)
    print("Write successful")
```

Output:
```console
Write successful

Process finished with exit code 0
```

File: tutswiki.yaml
```
- Details:
    date: 11/09/2020
    domain: www.tutswiki.com
    language: python
```

> **Note:** `sort_keys` parameter in `dump()` method can be used to sort the keys in ascending order.

```python
sorted = yaml.dump(data, sort_keys = True)
```

### Reading a key from YAML config file

We can read the data using `yaml.load()` method which takes `file pointer` and `Loader` as parameters. 
FullLoader handles the conversion from YAML scalar values to the Python dictionary.

To read the value from the file, we write the below code
```python
import yaml

with open("tutswiki.yaml", "r") as yamlfile:
    data = yaml.load(yamlfile, Loader=yaml.FullLoader)
    print("Read successful")
print(data)
```

Output:
```console
Read successful
[{'Details': {'date': '11/09/2020', 'domain': 'www.tutswiki.com', 'language': 'python'}}]

Process finished with exit code 0
```

Now if we want to read the value of `domain`, we would write

```python
print(data[0]['Details']['domain'])
```
The index `[0]` is used to select the tag we want to read. And we move further using the key values.

Output:
```console
www.tutswiki.com
```

### Updating a key in YAML config file
 What if we want to update the language `python` to `java`? For that, we will have to first read the file, reach to the key `language` and update its value and write the data to the file as done below.
```python
import yaml

article_info = [
    {
        'Details': {
        'domain' : 'www.tutswiki.com',
        'language': 'python',
        'date': '11/09/2020'
        }
    }
]

with open("tutswiki.yaml", "r") as yamlfile:
    data = yaml.load(yamlfile, Loader=yaml.FullLoader)
    print(data)
    print("Read successful")
    data[0]['Details']['language'] = 'java'
    print("Value of language updated from 'python' to 'java'")
    yamlfile.close()

with open("tutswiki.yaml", 'w') as yamlfile:
    data1 = yaml.dump(data, yamlfile)
    print(data)
    print("Write successful")
    yamlfile.close()
```

Output:
```console
[{'Details': {'date': '11/09/2020', 'domain': 'www.tutswiki.com', 'language': 'python'}}]
Read successful
Value of language updated from 'python' to 'java'
[{'Details': {'date': '11/09/2020', 'domain': 'www.tutswiki.com', 'language': 'java'}}]
Write successful

Process finished with exit code 0
```
File: tutswiki.yaml
```console
- Details:
    date: 11/09/2020
    domain: www.tutswiki.com
    language: java
```
As you can see, `language` in `tutswiki.yaml` file has been updated successfully.
