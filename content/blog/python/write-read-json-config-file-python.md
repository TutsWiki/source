---
date: 2020-09-16
linktitle: Writing and Reading JSON config file in Python
title: Writing and Reading JSON config file in Python
weight: 10
url: /read-write-json-config-file-in-python
description: Learn how to write and read JSON config files in Python using json module.
keywords:
  - python
  - config
  - read config file
  - write config file
  - configparser
  - json
tags: [Python]
---
A `Config` short for `Configuration` file is a file that stores information such as parameters, settings, configurations, and preferences of an application.

`Config files` are simple plain text files with `.config`, `.ini`, `.json`, `.xml`, `.yaml` file extensions among others that can be created, viewed or edited using any text editor.

For example, the `Web.config` file in Microsoft ASP.NET MVC application contains configuration information that controls the working of the application. It may be for individual pages or the entire application.

Below is a sample `Web.config` file.

```
<configuration>
   <appSettings>
      <add key="tutswiki" value="python" />
      <add key="article" value="Config file" />
   </appSettings>
</configuration>
```

We've already covered `.ini`, `.yaml` and `.xml` in below articles, in this article we'll focus on `.json` config files.

- [.ini file in Python](/read-write-config-files-in-python/)
- [.yaml file in Python](/read-write-yaml-config-file-in-python/)
- [.xml file in Python](/read-write-xml-config-file-in-python/)

## Reading and Writing config data to JSON file in Python

`JSON` or `Javascript Object Notation` file is used to store and transfer data in the form of `arrays` or `Key - Value` pairs. Let's now understand read and write operations to the JSON file.

### Creating JSON config file in Python

There are 2 methods to write in the JSON file.

#### Using `json.dumps()`

`json.dumps()` takes `python object` as parameter.

First, we need to import `json` module.  `json.dumps()` method serializes (Conversion of data into series of bytes) *python object* (`Dictionary` in this case) into *JSON formatted string* and `write()` method writes that formatted JSON string to the file `tutswiki.json`.

```python
import json

article_info = {
    "domain" : "tutswiki",
    "language" : "python",
    "date" : "11/09/2020",
    "topic" : "config file"
}
myJSON = json.dumps(article_info)

with open("tutswiki.json", "w") as jsonfile:
    jsonfile.write(myJSON)
    print("Write successful")
```

Output:
```console
Write successful

Process finished with exit code 0
```

File: tutswiki.json
```console
{
  "domain": "tutswiki",
  "language": "python",
  "date": "11/09/2020",
  "topic": "config file"
}
```

> **Note:** "w" mode creates the file in the current working directory if it does not exists.

#### Using `json.dump()`

Here, unlike `json.dumps()`, we need not serialize python object to JSON string. Instead, `json.dump()` method directly stores the python object as a JSON formatted data into the JSON file.

`json.dump()` takes `python object` and `file pointer` as parameters.

```python
import json

article_info = {
    "domain" : "tutswiki",
    "language" : "python",
    "date" : "11/09/2020",
    "topic" : "config file"
}

with open("tutswiki.json", "w") as jsonfile:
    json.dump(article_info, jsonfile)
```
File: tutswiki.json
```
{
  "domain": "tutswiki", 
  "language": "python", 
  "date": "11/09/2020", 
  "topic": "config file"
}
```
### Reading a key from JSON config file

We can read a JSON file using `json.load()` method which deserializes the JSON object to python object, `dictionary`. This method takes `file pointer` as its parameter.

```python
import json

with open("tutswiki.json", "r") as jsonfile:
    data = json.load(jsonfile)
    print("Read successful")
print(data)
```
Output:
```console
Read successful
{ 
  'domain': 'tutswiki', 
  'language': 'python', 
  'date': '11/09/2020', 
  'topic': 'config file'
}
```

> **Note:** If we want to deserialize a JSON string to a python object directly instead of reading from a file, we use `json.loads()` method which takes a `JSON string` as a parameter. For instance,

```python
import json

s = "{\"domain\": \"tutswiki\", \"language\": \"python\"}"
data = json.loads(s)
print(data)
```

Output:
```js
{
  'domain': 'tutswiki', 
  'language': 'python', 
  'date': '11/09/2020', 
  'topic': 'config file'
}

Process finished with exit code 0
```

### Updating a key in JSON config file

Now let's say we want to update the `date` to `12/09/2020`. 
First, we will read the data, update the required values, and then finally write to the file as done below.

```python
import json

article_info = {
    "domain" : "tutswiki",
    "language" : "python",
    "date" : "11/09/2020",
    "topic" : "config file"
}

with open("tutswiki.json", "r") as jsonfile:
    data = json.load(jsonfile) # Reading the file
    print("Read successful")
    jsonfile.close()
    
data['date'] = '12/09/2020' # Updating, before it was 11/09/2020
print("Date updated from 11/09/2020 to 12/09/2020")
with open("tutswiki.json", "w") as jsonfile:
    myJSON = json.dump(data, jsonfile) # Writing to the file
    print("Write successful")
    jsonfile.close()
```
Output:
```console
Read successful
Date updated from 11/09/2020 to 12/09/2020
Write successful

Process finished with exit code 0
```

File: tutswiki.json
```js
{
  "domain": "tutswiki", 
  "language": "python", 
  "date": "12/09/2020", 
  "topic": "config file"
}
```
> As you can see, `date` in `tutswiki.json` file has been updated successfully.
