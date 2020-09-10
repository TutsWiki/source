---
date: 2020-09-10
linktitle: Serialization
menu:
  main:
    parent: java
title: Serialization in Java
weight: 5
url: /java/serialization
description:
keywords:
  - java
  - serialization
tags: [Java]  
---
## What is Serialization ?

**Serialization** is the mechanism to convert an object into a sequence of bytes so that it could be used in any external process like sending an object via a network or saving in memory.
<br/>
The created Sequence of bytes will keep the information stored in the Object as well as information of Object type and structure, to recreate it again when needed.
<br/>
The mechanism of recreating the object from these sequence of bytes is called __Deserialisation__.

### Example

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

__Example__:  Replacing above class definition by
```java
    class Post implements Serializable
```

Now to Serialize the above class we could use the following method.

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

### Explanation : 
This method takes an object of class Post as a parameter and then serialize it. Then, write the object in the file whose name is also taken as a parameter.
<br/>
Here, `out.writeObject(inputPost)` does the Serialization.
<br />
Always, close the file after use.


Now, to deserialize the serialized object, we could use the below method.

```java
    public static void deserializePost(Post outputPost, String filename) {
        try {
            FileInputStream file = new FileInputStream(filename);
            ObjectInputStream in = new ObjectInputStream(file);

            outputPost = (Post) in.readObject();
        } catch (IOException ex) {
            System.out.print("IOException Occured");
        } catch (ClassNotFoundException ex) {
            System.out.print("ClassNotFound Occured");
        }
    }
```
### Explanation :
Here, `deserializePost()` method takes two parameters where outputPost is blank `Post` object ( It could be initialized earlier but original values will be overwritten by this method ) and filename where the deserialized object is saved.
<br />
This method opens the file in Input mode and then `in.readObject()` performs deserialization and the result will be typecast into `Post` by explicit type casting using `(Post)`.
<br />
The above method could expect `IOException` if the file does not exist already. And `ClassNotFound` Exception occurs when expected class is not found.

By merging the above code into one example we get,
## Example
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

    public static void deserializePost(Post outputPost, String filename) {
        try {
            FileInputStream file = new FileInputStream(filename);
            ObjectInputStream in = new ObjectInputStream(file);

            outputPost = (Post) in.readObject();
        } catch (IOException ex) {
            System.out.print("IOException Occured");
        } catch (ClassNotFoundException ex) {
            System.out.print("ClassNotFound Occured");
        }
    }

    public static void main(String[] args) {
        Post randomPost = new Post("This is my first post", 0);
        final String filename = "demo.bin";

        // Serialization

        System.out.println("Before Serialization : ");
        randomPost.printPost();

        serializePost(randomPost, "demo.bin");

        // Deserialization

        Post postFromFile = null;
        deserializePost(postFromFile, "demo.bin");

        System.out.println("After Serialization : ");
        randomPost.printPost();
    }
}
```

### Output
```console
Before Serialization : 
Post : This is my first post
After Serialization : 
Post : This is my first post
```

## Key Terms

- __Byte Stream__: A stream is a sequence of Objects. And a byte stream is a sequence of byte (8-bits) of data. Streams are generally used to Input and Output data.

## Need of Serialization

- To transfer objects via a network.
- To store java objects in memory.
- To store java objects in files.

![Need of Serialization](https://drive.google.com/uc?id=1VQhiPMfnTHGMnI9CU-twalFVBjeeohti)

## Features of Serialisation

- __Machine Independent__: Any object serialised on any one machine can be deserialised by any other machine.
- __Inheritance__: If a parent class is marked as Serializable then all its sub classes will also become Serializable.

## Things to remember

- The serialized object will be a snapshot of the original object when it is serialized. It will NOT automatically sync with changes done to the original object, like reassigning values to data members. To reflect the new changes, the user will need to serialize the object again.
- If the class intended for serialization does not implement `Serializable` interface then it will lead to `NotSerializableException`. User can try this by removing `implements Serializable` from class declaration.

## Credits
Icons for above image by [Flaticon](https://www.flaticon.com/)