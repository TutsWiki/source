---
date: 2020-08-22
linktitle: Syntax
menu:
  main:
    parent: R
next: /r/install-env-setup
title: Basic Syntax
weight: 26
url: /r/basic_syntax
description: 
keywords:
  - R
tags: [R]  
---
## Basic Syntax
We have already discussed everything regarding the [installation of R and RStudio](/r/install-env-setup/) in various operating systems. Now as a convention, we will start learning basics of R programming by writing a "Hello, World!" program. Depending upon the needs and availability of operating system available to the user, one can prefer programming either at *R command prompt* or one can also use an *R Script file* to write their own program (using RStudio IDE). Let’s begin with the R command prompt first.

## R Command Prompt
- Once you an R environment set up, then it’s easy to start your R command prompt by getting into the designated path where both `R.exe` and `RScript.exe` file is located.
![R and R Script](/images/R/R-exe-windows.png "R Executables")
- Clicking on the above application will launch an R interpreter and you will get a prompt `>` where you can start typing your program as given below
![Rterm Hello World](/images/R/Rterm-hello-world.png "Rterm Hello World")
- Here first statement defines a string variable `name`, where we have assigned a `vector` (objects of similar classes) of strings `"Hello, World!"` and `"Tutswiki"` and then in the next statement `print()` function is being used to print the value stored in variable `name`.
- You can also print the same using `R Script.exe` file located on the same folder.

## R Script File from RStudio

- Open the downloaded RStudio software that may be either 32 bit or 64 bit software located in your computer system depending upon the system type.
![RStudio Binary](/images/R/RStudio-binary.png "RStudio Binary")
- Click on the above to write your first program in R language. Let’s have a look at the series of steps
- Set your working directory where you want to store your file as shown in the image below
![RStudio Set Working Dir](/images/R/RStudio-working-dir.png "RStudio Set Working Dir")
- A sample query has been presented as an example demonstrating the execution and result of program in R Studio.
- Press `Ctrl + S` to save the file and name it as `HELLO.R`

## Comments
Comments are like helping text in your R program that provides clarity to the R source code allowing others to better understand what the code was intended to accomplish and greatly helping in debugging the code. Comments helps to indicate the meaning of each line of code being executed using `#` in the beginning of the statement as shown in below image. Such comments are called as single line comments.

### Multi-line comments in R
R does not support multi-line comments but you can perform a trick as shown below that uses `if(FALSE)`
![Multi line comment in R](/images/R/multi-line-comment-R.png "Multi line comment in R")

Though above comments will be executed by R interpreter, they will not interfere with your actual program. Such comments should always be enclosed within a single or double quote or you need to use bunch of `#` at once.
