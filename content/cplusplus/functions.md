---
date: 2020-10-13
linktitle: Functions
menu:
  main:
    parent: cplusplus
next: /cplusplus/overview
title: Functions in C++
weight: 5
url: /cplusplus/functions
description: Functions provide reusability and modularity to code, instead of writing the same piece of code multiple times we can define it once at a single place in a function.
keywords:
  - cplusplus
  - functions
  - methods
  - oop
tags: [C++]  
---
<meta property="og:image" content="https://tutswiki.com/images/cplusplus/process-address-space.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Functions in C++" />
<meta name=”twitter:description” content="Functions provide reusability and modularity to code, instead of writing the same piece of code multiple times we can define it once at a single place in a function." />

## What is a function?
A function is a set of instructions defined by a programmer to perform a specific task. Suppose if there is a task that is to be performed multiple times in a program, so we can define the task in a function and we can call this function every time we need to use that task.

## General format of a function
```c
Return_type function_name(parameter_type parameter_name)
{
Code;
}
```
## Parts of a function
- Return type: This tells about the type of value that will be returned from the function. When we have nothing to return from the function so we set the return type as “void”.
- Function name: This is the name by which we will be calling this function whenever we need it.
- Parameter type: This is the type of parameters that will be passed in this function, it can be integer, strings, vectors etc. We need these parameters to compute what our function intends to. There can also be functions in which we don't need parameters.
- Parameter name: This can be different from the name that is used while calling that function.
![Functions in C++](/images/cplusplus/functions.png "Functions")

**Note:** It is not necessary that every time a function will be returning something, it can also be a void function.

## Why do we need functions?
Functions provide reusability and modularity to code i.e. instead of writing the same piece of code multiple times we can define it once at a single place in a function. It makes code more readable. It helps to reduce the size of code and also helps to make debugging easy.

**Note:** Every CPP program should have the main function. Every time a user runs a program, the `main()` function of CPP is called by the Operating System.

### Example: A function to return the square of a number
```c
#include <bits/stdc++.h>
using namespace std;
int square(int x)	// Function defined with name square
{
	int a = x * x;
	return a;	// Returning output of function back from where it is called
}

int main()
{
	int p = 5;
	int ans = square(p);	// Call function passing parameter that is to be squared and storing output in
	variable ans
	cout << ans;
	return 0;
}
```
Output:
```console
25
```
### Example: A function to increment all values of vector by 5, and a void function to print a vector
```c
#include <bits/stdc++.h>
using namespace std;
vector<int> add_five(vector<int> v)	// Defining a function to increment all values of vector by 5
{
	for (int i = 0; i < v.size(); i++)
	{
		v[i] += 5;
	}

	return v;
}

void print(vector<int> v)	// Defining a function to print the vector
{
	for (int i = 0; i < v.size(); i++)
	{
		cout << v[i] << " ";
	}

	cout << endl;
}

int main()
{
	vector<int> v = { 1, 2, 3, 4, 5 };
	cout << "Before : ";
	print(v);
	vector<int> p = add_five(v);	// Passing vector in the function
	cout << "After : ";
	print(p);
	return 0;
}
```
Output:
```console
Before : 1 2 3 4 5
After : 6 7 8 9 10
```
**Note:** We have to declare a function before we use it.

## Types of functions
There are two types of functions

- Built-in Functions
- User-defined Functions

### Built-in functions
These are the library functions and are provided by C++ so that we can use some functionalities directly like sort, math operations etc. These functions are abstractly written and we are not able to access its implementation. We use header files that give us direct access to those functions. You can check the header files and there use here: https://en.cppreference.com/w/cpp/header

#### Example
```c
#include <iostream>	// iostream is used for data types and input/ output functions
using namespace std;
#include <string>	// string library to use perform string operations in program
#include <vector>	// vector library helps us in defining and operating on the vector
#include <math.h >	// math library helps in math operation
#include <queue>	// queue library helps us to use queue operations like push, pop directly

int main()
{
	vector<string> v;
	string str = "";
	for (int i = 0; i < 26; i++)
	{
		str += i + 97;
	}

	cout << "String is: " << str << endl;
	int a = 5;
	int ans = pow(5, 3);
	cout << "5 cube is: " << ans << endl;
	queue<int> q;
	q.push(2);
	q.push(4);
	q.push(6);
	q.push(8);
	cout << "Queue Elements are : ";
	while (!q.empty())
	{
		cout << q.front() << " ";
		q.pop();
	}

	return 0;
}
```
Output :
```console
String is: abcdefghijklmnopqrstuvwxyz
5 cube is: 125
Queue Elements are : 2 4 6 8
```
### User defined functions
The functions that are defined by users themselves are called user-defined functions. We can define functions specific to tasks that we want them to perform. The functions must be declared before calling them.
#### Example
```c
#include <iostream>
using namespace std;
int func_gcd(int x, int y)	// Function to calculate gcd of two numbers
{
	if (x == 0)
		return y;
	if (y == 0)
		return x;
	if (x == y)
		return x;
	if (x > y)
		return func_gcd(x - y, y);
	return func_gcd(x, y - x);
}

int main()
{
	int p = 32, q = 12;
	cout << "Greatest Common Divisor of " << p << " and " << q << " is " << func_gcd(p, q);
	return 0;
}
```
Output :
```console
Greatest Common Divisor of 32 and 12 is 4
```
## Passing parameters to a function
Furthermore, there are two ways of passing parameters to a function

- Pass by Value
- Pass by Reference

**Note:** Formal parameters are the parameters that function receives and actual parameters are the parameters (the actual values) that are passed to the function.
### Pass by Value
In this method formal parameters and actual parameters are stored in two different memory locations i.e. formal parameters have a copy of actual parameters. So, any operation or update that we perform inside a function does not change the value of actual parameters.

#### Example
A program to increment all values of vector by 10
```c
#include <bits/stdc++.h>
using namespace std;
void func(vector<int> v)	// Function is called by value so change in vector will not be reflected in main
function
{
	for (int i = 0; i < v.size(); i++)
	{
		v[i] += 10;
	}
}

void print(vector<int> v)
{
	for (int i = 0; i < v.size(); i++)
	{
		cout << v[i] << " ";
	}

	cout << endl;
}

int main()
{
	vector<int> v = { 1, 2, 4, 8, 13 };
	func(v);
	cout << ”Values in the vector are: “;
	print(v);
	return 0;
}
```
Output :
```console
Values in the vector are: 1 2 4 8 13
```
### Pass by Reference
In this actual and formal parameters point to the same memory location. So, any operation or update that we perform inside a function also updates the value of actual parameters.

#### Example
A program to increment all values of vector by 10
```c
#include <bits/stdc++.h>
using namespace std;
void func(vector<int> &v)	// Function is called by reference so change in vector will be reflected in main
function
{
	for (int i = 0; i < v.size(); i++)
	{
		v[i] += 10;
	}
}

void print(vector<int> v)
{
	for (int i = 0; i < v.size(); i++)
	{
		cout << v[i] << " ";
	}

	cout << endl;
}

int main()
{
	vector<int> v = { 1, 2, 4, 8, 13 };
	func(v);
	cout << ”Values in vector are: “;
	print(v);
	return 0;
}
```
Output:
```console
Values in vector are: 11 12 14 18 23
```
## Conclusion
Functions are a very important part of programming in CPP. It provides us with modularity and reusability. It makes our code simple and more readable. It further helps in advanced data structures like recursion, backtracking, tree implementation, graphs, etc. So we can say that functions are the most important pillars of programming in CPP.
