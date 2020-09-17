---
date: 2017-06-05
linktitle: Writing and Reading config files in Python
title: Writing and Reading config files in Python
weight: 10
url: /read-write-config-files-in-python
description: Learn how to write and read .ini config files in Python using configparser module.
keywords:
  - python
  - config
  - read config file
  - write config file
  - configparser
tags: [Python]
---
I'm sure you must be aware about the importance of [configuration files](https://www.wikiwand.com/en/Configuration_file). Config files help creating the initial settings for any project, they help avoiding the hardcoded data. 

Imagine if you migrate your server to a new host and suddenly your application stops working, now you have to go through your code and search/replace IP address of host at all the places. Config file comes to the rescue in such situation. You define the IP address key in config file and use it throughout your code. Later when you want to change any attribute, just change it in the config file. So helpful, isn't it?
  
Let's see how can we create and read config files in Python.

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

We’ve already covered .yaml, .json and .xml in below articles, in this article we’ll focus on .ini config files.

- [.json file in Python](/read-write-json-config-file-in-python/)
- [.yaml file in Python](/read-write-yaml-config-file-in-python/)
- [.xml file in Python](/read-write-xml-config-files-in-python/)

## Creating config file in Python

In Python we have [configparser](https://docs.python.org/3/library/configparser.html) module which can help us with creation of config files (.ini format).

```python
from configparser import ConfigParser

#Get the configparser object
config_object = ConfigParser()

#Assume we need 2 sections in the config file, let's call them USERINFO and SERVERCONFIG
config_object["USERINFO"] = {
    "admin": "Chankey Pathak",
    "loginid": "chankeypathak",
    "password": "tutswiki"
}

config_object["SERVERCONFIG"] = {
    "host": "tutswiki.com",
    "port": "8080",
    "ipaddr": "8.8.8.8"
}

#Write the above sections to config.ini file
with open('config.ini', 'w') as conf:
    config_object.write(conf)
```

Now if you check the working directory, you will notice `config.ini` file has been created, below is its content.

```text
[USERINFO]
admin = Chankey Pathak
password = tutswiki
loginid = chankeypathak

[SERVERCONFIG]
host = tutswiki.com
ipaddr = 8.8.8.8
port = 8080
```

## Reading a key from config file

So we have created a config file, now in your code you have to read the configuration data so that you can use it by "keyname" to avoid hardcoded data, let's see how to do that.

```python
from configparser import ConfigParser

#Read config.ini file
config_object = ConfigParser()
config_object.read("config.ini")

#Get the password
userinfo = config_object["USERINFO"]
print("Password is {}".format(userinfo["password"]))
```

Output:

```sh
Password is tutswiki
```

## Updating a key in config file

Suppose you have updated the password for chankeypathak user. You can update the same in config file using below:

```python
from configparser import ConfigParser

#Read config.ini file
config_object = ConfigParser()
config_object.read("config.ini")

#Get the USERINFO section
userinfo = config_object["USERINFO"]

#Update the password
userinfo["password"] = "newpassword"

#Write changes back to file
with open('config.ini', 'w') as conf:
    config_object.write(conf)
```

Now if you open the `config.ini` file, you will notice that the password has been updated.
