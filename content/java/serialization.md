---
date: 2020-09-10
linktitle: Serialization
menu:
  main:
    parent: java
title: Serialization in Java
weight: 5
url: /java/serialization
description: Serialization is the mechanism to convert an object into a sequence of bytes so that it could be used in any external process like sending an object via a network or saving in memory.
keywords:
  - java
  - serialization
tags: [Java]  
---
<meta property="og:image" content="https://tutswiki.com/images/Java/Java-Serialization-Flow.jpg?width=30pc"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Serialization in Java" />
<meta name=”twitter:description” content="Serialization is the mechanism to convert an object into a sequence of bytes so that it could be used in any external process like sending an object via a network or saving in memory." />

## What is Serialization ?

**Serialization** is the mechanism to convert an object into a sequence of bytes so that it could be used in any external process like sending an object via a network or saving in memory. The created sequence of bytes will keep the information that is stored in the object as well as information of Object type and structure, to recreate it again when needed. The mechanism of recreating the object from these sequence of bytes is called **Deserialization**.

### Serialization Example

Let us consider a class `Post` which has two data members `content` and `likes`. It is defined as follows:

```java
class Post {
    private String content;
    private int likes;

    public Post(String content, int likes) {
        this.content = content;
        this.likes = likes;
    }

    public void printPost() {
        System.out.println("Post : " + this.content);
    }
}
```

To Serialize the above class, we will mark the class as *Serializable* by implementing the interface `java.io.Serializable`.

```java
class Post implements Serializable
```

And then we create the following method

```java
public static void serializePost(Post inputPost, String filename) {
    try {
        FileOutputStream file = new FileOutputStream(filename);
        ObjectOutputStream out = new ObjectOutputStream(file);

        out.writeObject(inputPost);

        out.close();
        file.close();
    } catch (IOException ex) {
        System.out.println("IOException occured");
    }
}
```

#### Explanation
This method takes an object of class `Post` as a parameter and then serializes it. After that, it writes the serialized bytes to a file whose name is taken as the second parameter.  

Here, `out.writeObject(inputPost)` does the Serialization. 
After the bytes are written to the file, we close the filehandle by calling `file.close();`

### Deserialization Example
Now, to deserialize the serialized object, we could use the below method.

```java
public static void deserializePost(Post outputPost, String filename) {
    try {
        FileInputStream file = new FileInputStream(filename);
        ObjectInputStream in = new ObjectInputStream(file);

        outputPost = (Post) in .readObject();
    } catch (IOException ex) {
        System.out.print("IOException Occured");
    } catch (ClassNotFoundException ex) {
        System.out.print("ClassNotFound Occured");
    }
}
```
#### Explanation
Here, `deserializePost()` method takes two parameters where outputPost is blank `Post` object ( It could be initialized earlier but original values will be overwritten by this method ) and filename where the deserialized object is saved. 

1. This method will open the file in Input mode.
2. `in.readObject()` will perform Deserialization.
3. Finally, the result will be explicitly typecasted into Post using `(Post)`.

The above method could expect `IOException` if the file does not exist already. And `ClassNotFound` Exception occurs when expected class is not found.

### Serialization and Deserialization Example
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

class Post implements Serializable {
    private String content;
    private int likes;

    public Post(String content, int likes) {
        this.content = content;
        this.likes = likes;
    }

    public void printPost() {
        System.out.println("Post : " + this.content);
    }
}

public class Serialization {

    public static void serializePost(Post inputPost, String filename) {
        try {
            FileOutputStream file = new FileOutputStream(filename);
            ObjectOutputStream out = new ObjectOutputStream(file);

            out.writeObject(inputPost);

            out.close();
            file.close();
        } catch (IOException ex) {
            System.out.println("IOException occured");
        }
    }

    public static Post deserializePost(String filename) {
        try {
            FileInputStream file = new FileInputStream(filename);
            ObjectInputStream in = new ObjectInputStream(file);

            return (Post) in .readObject();
        } catch (IOException ex) {
            System.out.println("IOException Occured");
        } catch (ClassNotFoundException ex) {
            System.out.println("ClassNotFound Occured");
        }
        return null;
    }

    public static void main(String[] args) {
        Post randomPost = new Post("This is my first post", 0);
        final String filename = "demo.bin";

        System.out.println("Before Serialization : ");
        randomPost.printPost();

        // Serialization
        serializePost(randomPost, filename);

        // Deserialization
        Post postFromFile = deserializePost(filename);

        System.out.println("After Serialization : ");
        postFromFile.printPost();
    }
}
```

#### Output
```console
Before Serialization : 
Post: This is my first post
After Serialization : 
Post: This is my first post
```

#### Content of Serialized File
```console
$ head demo.bin
��srPost�7ޗ�[lIlikesLcontenttLjava/lang/String;xptThis is my first post
```

## Serialization with Inheritance
When a parent class is marked Serializable then all its subclasses will also be marked Serializable without explicitly implementing Serializable interface.

#### Example
```java
class Post implements Serializable {
    private String content;
    private int likes;

    public Post(String content, int likes) {
        this.content = content;
        this.likes = likes;
    }

    public void printPost() {
        System.out.println("Post : " + this.content);
    }
}

class FBPost extends Post {
    private String comments;

    public addComment(String newComment) {
        if (this.comments == null) comments = "";
        this.comments += (newComment + ", ");
    }

    public getComment() {
        return this.comments;
    }
}
```

### Explanation
The above example has two classes `Post` and `FBPost`. 
Here, `FBPost` inherits `Post` and it also has `comments` data member which consists of all the comments made to the post.
<br/>
Here, `Post` implements Serializable and thus `FBPost` will also become Serializable by the concept of Multilevel Inheritance.


> But this concept does not work the other way round, which means when a subclass implements Serializable interface it will not affect the Parent class.

## Serialization with Aggregation
In many use cases, there are one or more objects that reside in an object as data members, then this relationship is known as Aggregation. 
These objects act as properties for the aggregating object. And to make an aggregating object serializable, all the inner objects are required to be Serializable.
<br/>
If any of the data members are not Serialiazabile then the `NotSerializableException` will be thrown while serializing the object.

### Example
```java
class Post {
    private String title;
    private String content;

    public String getPost() {
        return this.content + " : " + this.post;
    }
}

class FBPost {
    private Post post;
    private int likes;
}
```
#### Explanation
In the above example, if the user tries to serialize the `FBPost` object, then `NotSerizalizableException` will be thrown because of the `post` object since it is not Serializable.

## Key Terms

- **Byte Stream**: A stream is a sequence of Objects. And a byte stream is a sequence of byte (8-bits) of data. Streams are generally used to Input and Output data.
- **Multilevel Inheritance**: As the name suggests, there are multiple levels of inheritance in this type of Inheritance. This means that one class inherits a class which already inherits another class, thus creating multiple levels of inheritance.

## Cases when properties are not Serialized

### 1. Static Members
Static members of a class are not serialized because they do not belong to any individual object.

#### Example
```java
class Post implements Serializable {
    public String content;
    public int likes;
    public static int postCount;

    public Post(String content, int likes) {
        this.content = content;
        this.likes = likes;
    }

    public void printPost() {
        System.out.println(String.format("Post : \"%s\" and Current Count for posts = %d.", this.content, Post.postCount));
    }
}
```

#### Explanation
In the above example, we have added `postCount` as a class member or static member. Thus this variable will not be serialized with other data members.

To see this example in effect, user need to create two different programs for Serialization and Deserialization.
By doing so the static member will not remember the actual value while serializing and the user will get postCount as 0 (Default Integer value) after deserialization.


### 2. Transient members
In some use cases, the user themselves doesn't want to serialize all the data members of any object. 
For this use case, Java provides the keyword `transient`, any data member which is marked as transient will have a default value for their datatype in the serialized object.

#### Example
```java
class Post implements Serializable {
    public String content;
    public transient int likes;

    public Post(String content, int likes) {
        this.content = content;
        this.likes = likes;
    }

    public void printPost() {
        System.out.println(String.format("Post : \"%s\" with %d likes.", this.content, this.likes));
    }
}

// Serialization class is the same as the second example
```

#### Output
```console
Before Serialization : 
Post: "This is my first post" with 5 likes.
After Serialization : 
Post: "This is my first post" with 0 likes.
```

#### Explanation
In the above case, the number of likes went from 5 to 0 after serialization. 
This happened because the `likes` are marked as `transient` and thus its value is initialised with the default value of `int` datatype that is `0`.

#### Use Case
Sometimes, some variables consist of large values which are very costly to transport over a network. 
Thus those values can be marked as `transient`.
Also, sometimes the user does not want to share or save the value of the variable because it is either too trivial or confidential, those variables could also be marked as `transient`.

## Need for Serialization

- To transfer objects via a network
- To store Java objects in memory
- To store Java objects in files

![Java Serialization Flow](/images/Java/Java-Serialization-Flow.jpg "Java Serialization Flow") [Icons credit: flaticon]

## Features of Serialization

- **Machine Independent**: Any object serialized on any one machine can be deserialized by any other machine.
- **Inheritance**: If a parent class is marked as Serializable then all its subclasses will also become Serializable.

## Things to remember

- The serialized object will be a snapshot of the original object when it is serialized. 
  It will NOT automatically sync with changes done to the original object, like reassigning values to data members. 
  To reflect the new changes, the user will need to serialize the object again.
- If the class intended for serialization does not implement `Serializable` interface or any data member within it is not Serializable, then it will lead to `NotSerializableException` in runtime. 
  User can try this by removing `implements Serializable` from class declaration.
