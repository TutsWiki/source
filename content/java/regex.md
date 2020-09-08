---
date: 2020-09-01
linktitle: Regular Expressions
menu:
  main:
    parent: java
title: Regular Expressions in Java
weight: 8
url: /java/regex
description: Regular expression in simple terms is a special sequence of symbols and alphanumeric characters that defines a search pattern.
keywords:
  - java
  - regex
  - regexp
tags: [Java]  
---
### 1.1 Introduction
Regular Expression in simple terms is a special sequence of symbols and alphanumeric characters that defines a search pattern.

### 1.2 Uses
- These are used to describe what you are looking for when you search for data in text, article etc.      
- It is used for **Data Validation** e.g. checking if the user has inserted a valid e-mail address or not while registering in a website.
- It is used for **Data Scraping/Web scraping** e.g. when you want to scrape particular data from a website.
- These are also used in search engines, string processing etc.

## 2. Regex in Java
### 2.1 RegExp in Java
Java Regex can be defined as an **API** which is used for searching and manipulating (like replacing the matched word with some other word) strings from a given text which matches the pattern defined by the API.

### 2.2 Java Regex Package
Java provides a package known as `java.util.regex` for pattern matching with regular expressions. This package consists of three main classes

- **Matcher Class**
- **Pattern Class**
- **PatternSyntaxException class**

#### 2.2.1 Matcher Class
Matcher class' job is to interpret the pattern defined by pattern class and is used to perform matching functions on the given character sequence.

Some functions in the matcher class are:

- boolean matches()
  - Used to test whether the given string matches the given regex pattern or not and returns a boolean. If the string matches it returns `true` else `false`.
- boolean find()
  - Used to find the next string expression which matches the given regex pattern in the given text. If any string matches the pattern it returns `true` else `false`.
- boolean find(int start)
  - Used to search the next string expression from the given text which matches the given regex pattern from the given start number. It also returns a boolean value.

#### 2.2.2 Pattern Class
Pattern class job is to compile the given regular expression which is used to define the pattern for a regex engine. This class accepts the pattern input provided by the user and compiles it so that matcher class can perform its tasks. One of the functions in Pattern class is

- Pattern.compile(String regex)
  - Its job is to compile the given regex pattern and return the instance of a pattern.

#### 2.2.3 PatternSyntaxException Class
It simply is used to find errors in regular expression patterns which are provided by the user.

#### Example
```java
import java.util.regex.*;
public class reg {
	Public static void main(String[] args) {

		String email = "abcd99@gmail.com";

		// Pattern for email validation with constraints
		String pat = " ^ [a - zA - Z0 - 9_ ! #$ % &'*+/=?`{|}~^.-]+@[a-zA-Z0-9.-]+$";  

		Pattern p = Pattern.compile(pat);  // Pattern Object

		Matcher m = p.matcher(email);     // Matcher Object

		System.out.println(m.matches());

     }
}
```
Output: Prints true if the email is valid and false for invalid, here it will print true
### 2.3 Regex Quantifiers
Regex quantifiers are used in regular expressions to define how often an element can occur in the strings which the user is searching for.
Some examples are

- `*` - it symbolizes that occurrences of the element should be zero or more times
- `+` - it symbolizes that occurrences of the element should be one or more times
- `?` - it symbolizes occurrence of the element should be zero or one.
- `{X}` - it symbolizes occurrences of the element should be X number of times
- `{X, Y}` - it symbolizes occurrences of the element should be between `X` and `Y` number of times
- `{X,}` - it symbolizes occurrences of the element should be `X` or more than `X`

### 2.4 Regex Character Class
Java Character Class has predefined sets of expressions which can be directly used as regex expressions and can be used to create our required pattern.

- `[xyz]` - checks occurrence of x, y or z
- `[^xyz]` - checks occurrence of  any character other than x, y or z
- `[a-zA-Z]` - checks occurrence of characters from a-z or A-Z
- `[a-z&&[xyz]]` - checks occurrence of characters from a-z with xor y or z
- `[a-z&&[^xy]]` - checks occurrence of characters from a-z except x or y or z
- `[a-z&&[^m-p]]` - checks occurrence of characters from a-z and should not have characters m through p

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
	}
}
```
Output: 
```
true
false
```
This program will only print true under the following conditions:

- String length is equal to 3.
- String only contains a, b or c.

### 2.5 Regex Metacharacters
These are ordinary characters which have special meaning in computer programs and are used for making regular expressions.  

Some examples are:

- **\d** - denotes you can enter any digit. It is a short form of the pattern [0-9]
- **\D** - denotes you can enter any non-digit. It is a short form of the pattern [^0-9]
- **\w** - denotes you can enter any word character. It is a short form of the pattern [a-zA-Z_0-9]
- **\W** - denotes you can enter any non-word character. It is a short form of the pattern [^\w]
- **\b** - denotes a word boundary which means a position where a word begins or ends
- **\B** - denotes a non-word boundary same as \b but it is used for non-word characters.

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
	}
}
```
Output: 
```
false
true
```
