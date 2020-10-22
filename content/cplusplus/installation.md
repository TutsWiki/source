---
date: 2020-10-22
linktitle: Installation
menu:
  main:
    parent: cplusplus
next: /cplusplus/overview
title: C++ Installation and Environment Setup
weight: 2
url: /cplusplus/installation
description: To start coding we need to first set up the environment to compile and run our C++ programs. We can also use online IDEs for the compilation if we don't want to set up the environment locally.
keywords:
  - cplusplus
  - install
  - ide
  - gcc
tags: [C++]  
---
<meta property="og:image" content="https://tutswiki.com/images/cplusplus/cplusplus-editor.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="C++ Installation and Environment Setup" />
<meta name=”twitter:description” content="To start coding we need to first set up the environment to compile and run our C++ programs. We can also use online IDEs for the compilation if we don't want to set up the environment locally." />

C++ is a general-purpose programming language that is popularly used worldwide. C++ is efficient in terms of both memory and performance that makes it handy in competitive programming. C++ provides features like reusability of code, abstraction, polymorphism, inheritance, etc.

C++ can be run on many platforms like Linux, Windows and macOS. To start coding we need to first set up the environment to compile and run our C++ programs. We can also use online IDEs for the compilation if we don't want to set up the environment locally. We will go through both the processes in this article.

## Online IDEs for C++
Many online IDEs provide us with the comfort of running our programs online without setting up the environment locally. Examples are GFG IDE, Codechef IDE etc.

### 1. [Geeks for Geeks](https://ide.geeksforgeeks.org/)
We can choose from other languages also from the left sidebar. We can write our code and then hit the RUN button at the bottom. We can also provide input at the bottom from beside the run button.

![C++ Installation and Environment Setup](/images/cplusplus/GFG_IDE.png "GFG")

**Output:** We will get our output at bottom of the page like this
![C++ Installation and Environment Setup](/images/cplusplus/GFG_Output.png "GFG")


### 2. [Codechef IDE](https://www.codechef.com/ide)
In this also we can choose from multiple languages. We can also provide a custom input from the bottom next to the RUN button.

![C++ Installation and Environment Setup](/images/cplusplus/Codechef_IDE.png "Codechef")

**Output:** Output will be displayed at the bottom 
![C++ Installation and Environment Setup](/images/cplusplus/Codechef_output.png "Codechef")

### 3. [Ideone](https://ideone.com/)

![C++ Installation and Environment Setup](/images/cplusplus/Ideone_IDE.png "Ideone")

**Output:** After compilation we an see output like this
![C++ Installation and Environment Setup](/images/cplusplus/Ideone_output.png "Ideone")


## Setting up the local environment for C++
To run C++ code on a local machine we need two main things.

- Text Editor 
- C++ compiler

### 1. Text Editor 
You'll need a text editor in which you can write your program. Feel free to use any text editor (Notepad++, Sublime, Atom etc.). One important thing that is to be considered here is that the file must be saved with a .cpp extension so that the compiler can consider it as a C++ program.

### 2. C++ Compiler
A compiler is a program that converts the source code of high-level programming language to machine-understandable low-level instructions. We will see how to set up our compiler in Linux. 

#### Installing GNU GCC in Linux

- First of all, open the terminal (Shortcut for Ubuntu: CTRL + ALT + T). Then run these following commands.
```console
sudo apt-get update
```
- Enter the password if prompted
![sudo apt-get update in Ubuntu](/images/cplusplus/sudo-apt-get-update.png "Update packages index")

- Now to install GCC compiler use below command 
```console
sudo apt-get install GCC
```
![Installing GCC in Ubuntu](/images/cplusplus/sudo-gcc.png "Install GCC")

- Then run this command that will install all libraries required to compile and run the C++ programs
```console
sudo apt-get install build-essential
```
![Installing build-essential in Ubuntu](/images/cplusplus/build-essential.png "Install build-essential")

- Now check if the GCC is installed correctly by passing `--version` argument
```console
g++ --version
```
![GCC version](/images/cplusplus/gplusplus-version.png "GCC version")

## How to compile and run C++ program

### Example 1: Hello World in C++

- First we have to open a text editor and write our code 
![C++ Hello World Program](/images/cplusplus/cplusplus-editor.png "Hello World Program")

- Save the file with name lets say here `first_programme` on the desktop with `.cpp` extension.
![Ubuntu Save As](/images/cplusplus/save-as.png "Save As")

- Then open the terminal (Remember CTRL + ALT + T?)

- Then move to the directory where our program file is saved by using `cd` command.
![Change directory cd command](/images/cplusplus/cd-command.png "Change directory")

- To compile a C++ program with gcc we use below syntax where `filename.cpp` is source code file. `output-file-name` is the name given to executable file which will be created by the compiler

```console
g++ filename.cpp -o output-file-name
```

- In this case we make a output file with name `Output_file_1`
![gcc compile cplusplus](/images/cplusplus/gcc-compile-cplusplus.png "gcc compile cplusplus")

- On running this command an executable file with name `Output_file_1` will be saved in the same directory where our source code is saved.
![Ubuntu Desktop](/images/cplusplus/ubuntu-desktop.png "Desktop")

- To run the program we need to write this command in the terminal window which will show us the output of the program.
![Execute C++ code](/images/cplusplus/execute-cplusplus.png "Output")

- Output "Hello World" is printed in the terminal window.

### Example 2: C++ program with input values

Let's write a program that takes radius as an input and prints the area of the circle.

- Open a text editor and write program
![C++ program for area of circle](/images/cplusplus/area-of-circle-cplusplus.png "Area of Cirlce program")

- Then save the program with name lets say `second_programme` on the desktop with `.cpp` extension.
![Ubuntu Save As](/images/cplusplus/save-as-ubuntu.png "Save As")

- Go to the program directory in terminal and compile the code
```console
g++ second_programme.cpp -o Output_file_2
```
- Then run the executable file
![STDIN for C++ executable](/images/cplusplus/execute-cplusplus-binary.png "Execute")

- The terminal will wait for the input of radius, here let us provide **5** and hit enter. We will get output area as 78.5.
![C++ Output](/images/cplusplus/cplusplus-execute.png "Output")
