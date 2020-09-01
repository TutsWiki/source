---
date: 2020-09-01
linktitle: Regular Expressions
menu:
  main:
    parent: java
title: Regular Expressions in Java
weight: 8
url: /java/regex
description: finally is the block that will always get executed irrespective of the fact whether the exception is handled or not.
keywords:
  - java
  - regex
  - regexp
tags: [Java]  
---
### 1.1 Introduction
Regular expression in simple terms is a special sequence of symbols and alphanumeric characters that defines a search pattern.

### 1.2 Uses
- These are used to describe what you are looking for when you search for data in text, article etc.    
- **Data Validation** like checking if the user has inserted a valid e-mail address or not.
- **Data Scraping/Web scraping** like when you want to scrape particular data from the internet.
- These are also used in search engines, string processing etc.

#### Example
```java
^[A-Za-z0-9+_.-]+@(.+)$
```
This pattern is used to validate email addresses.

1. A-Z characters allowed.
2. a-z characters allowed.
3. 0-9 numbers allowed.
4. Additionally, email may contain a dot, dash and underscore.
5. Rest all characters are not allowed

### 2.1 RegExp in Java
Java Regex is an **API** which is used for searching and manipulating strings from a given text which matches the pattern defined in the API.

### 2.2 Java Regex Package
Java provides the `java.util.regex` for pattern matching with regular expressions. This package consists of three main classes:

- Matcher Class
- Pattern Class
- PatternSyntaxException class

#### 2.2.1 Matcher Class
Matcher class interprets the pattern defined by pattern class and is used to perform matching functions on the given char sequence.

Some functions in the matcher class are:

- boolean  matches()
  - Used to test whether the given string matches the given regex pattern or not.
- boolean  find()
  - Used to find the next string expression which matches the given regex pattern in the given text.
- boolean find(int start)
  - Used to search the next expression which matches the given regex pattern from the given start number.

#### 2.2.2 Pattern Class
It is the compiled form of regular expression which is used to define the pattern for a regex engine.
One of the functions in Pattern class is:

- Pattern.compile(String regex)
  - Its job is to compile the given regex pattern and return the instance of a pattern.

#### 2.2.3 PatternSyntaxException Class
It is used to find errors in regular expression patterns.

#### Example
```java
import java.util.regex. * ;
public class reg {
	Public static void main(String[] args) {

		String email = "abcd99@gmail.com";

		String pat = " ^ [a - zA - Z0 - 9_ ! #$ % &'*+/=?`{|}~^.-]+@[a-zA-Z0-9.-]+$";
         // Pattern for email validation with constraints

          Pattern p = Pattern.compile(pat);  // Pattern Object

        Matcher m = p.matcher(email);     // Matcher Object

         System.out.println(m.matches());

         // Prints true if the email is valid and false for invalid, here it will print true
     }
}
```
### 2.3 REGEX QUANTIFIERS
Regex quantifiers are used in regular expressions to define how often an element can occur.

- `*`   -   occurrences should be zero or more times.
- `+`  -    occurrences should be one or more times.
- `?`  -  occurrences should be zero or one time.
- `{X}` -  occurrences should be X number of times
- `{X,Y}` - occurrences should be between X and Y number of times.
- `{X,}` - occurrences should be X or more than X.

### 2.4 REGEX CHARACTER CLASS
Java Character Class has predefined sets which can be directly used as regex expressions.

- `[xyz]` - checks occurrence of x, y or z.
- `[^xyz]` - check the occurrence of any character other than x,y or z.
- `[a-zA-Z]` - check occurrence of characters from a-z or A-Z
- `[a-z&&[xyz]]` - check the occurrence of characters from a-z with x, y or z.
- `[a-z&&[^xy]]` - check occurrence of characters from a-z except x or y.
- `[a-z&&[^m-p]]` - check the occurrence of characters from a-z and not m through p.

#### Example
```java
import java.util.regex. * ;

public class Reg {

	public static void main(String[] args) {

		Pattern p = Pattern.compile("[abc]{3}");
		Matcher m = p.matcher("bcc");
		Matcher ma = p.matcher("acz");

		System.out.println(m.matches());
		System.out.println(ma.matches());

		//Output - true
		false
	}
}
```
This program will only print true under the following conditions:

- String length is equal to 3.
- String only contains a b or c.

### 2.5 REGEX METACHARACTERS
These characters have special meaning in computer programs and are used for making regular expressions.

- **\d** - denotes you can enter any digit. It is a short form of [0-9].
- **\D** - denotes you can enter any non digit .It is short form [^0-9].
- **\w** - denotes you can enter any word character .It is short form of [a-zA-Z_0-9].
- **\W** - denotes you can enter any non-word character. It is short form of [^\w].
- **\b** - denotes the word boundary.
- **\B** - denotes a non-word boundary.

#### Example
```java
import java.util.regex. * ;
public class Reg {

	public static void main(String[] args) {
		Pattern p = Pattern.compile("\\D{3}");
		Matcher m = p.matcher("123");
		Matcher ma = p.matcher("aaa");

		System.out.println(m.matches());
		System.out.println(ma.matches());

		//Output - false
		true

	}
}
```