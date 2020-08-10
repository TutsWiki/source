---
date: 2020-08-10
linktitle: Approximating Randomness
title: Approximating Randomness
weight: 10
url: /approximating-randomness
description: Generating random numbers from computers is essential to the development
of certain kinds of software. Anything from modelling the environment, to a
lottery machine, to determining the value of loot in a chest in an RPG, will
require random number generation.
keywords:
  - random
  - programming
  - languages
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Approximating Randomness" />
<meta name=”twitter:description” content="Generating random numbers from computers is essential to the development
of certain kinds of software. Anything from modelling the environment, to a
lottery machine, to determining the value of loot in a chest in an RPG, will
require random number generation." />

Generating random numbers from computers is essential to the development
of certain kinds of software. Anything from modelling the environment, to a
lottery machine, to determining the value of loot in a chest in an RPG, will
require random number generation. At first it may seem strange that computers,
which are capable of producing massive amounts of digits in a short time,
would not be able to produce random numbers. The difficulty is that the
computers we use, are constructed specifically to follow logical steps
deterministically, so to generate numbers that are truly random from a system
like our computers is virtually impossible. True randomness cannot be obtained
using arithmetic operations, which is exactly what our computers perform. When
some sequence of numbers is random then it is not possible to predict what the
next digit will be, and since computers use a set of logical steps to create
any new number it is theoretically possible to predict it. Nonetheless
programmers take advantage of features for creating random numbers all the
time, the most widely used programming languages, provide libraries that can
generate 'random' values. Any role-playing game for example, that you might
use, needs random values to determine the number of gold you'll find from some
locations, what items dead enemies will drop, etc. So how is it possible that
even though the very nature of computers makes it impossible to exhibit random
behaviour, that many programs include simulations of randomness?      

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

The answer is pseudo-random number generators(PRNG), these are programs that
follow a certain algorithm to simulate random behaviour. The numbers they
generate, cannot be by definition truly random, however for many uses it is a
good enough approximation. The libraries in many of todays programming
languages that generate random digits use PRNG's. The basic idea is to perform
some arithmetic operations on a number in sequence, to make it seem like it's
random. A PRNG always starts with an initial value called a seed. It will then
perform operations on the seed to make the final number different from the
original value. So when the PRNG is supplied with seeds that are in sequence,
it will return numbers that seem random, because the operations that are
performed on each number should change it to make it so the returned numbers
are no longer following any pattern. PRNG's are also periodical, they have a certain
amount of numbers they produce, and after that the sequence starts to repeat
itself. It is usually not a problem to make the period very large, so that the
application that relies on it will never start to get repeated sequences.      

A very well known PRNG is the Mersenne Twister(1), it is the default PRNG for
Python, Ruby, PHP and many others. It has a period of 2^(19937−1), which is a
Mersenne prime number, hence the name of the generator. Invented in 1997 it
was largely superior to PRNG's like C's rand or Java's Random. Furthermore it
passes certain statistical tests for randomness so it is a very reliable
generator as it is very successful at simulating genuine randomness.    

These programs have been used extensively and are relied upon by many applications,
it is important to note that when truly random behaviour is needed then PRNG's
are not sufficient. John von Neumann famously said: 

> Anyone who considers arithmetical methods of producing random digits is, of course, in a state of sin.

In this situation he was referring to depending on PRNG's or other
arithmetic approaches to create truly random digits, which is not a good idea.
Rather it is possible to use true random number generators, that are based on
the idea of extracting physical phenomena that are believed to be random and
using them to generate random numbers. For example atmospheric noise, or
radioactive decay, etc. Even though it is possible to extract truly random
numbers from the outside world, most applications that use random numbers use
PRNG's. It is because of their convenience, there is no need for any extra
devices, or input, all it needs is a sequence of numbers that it later
transforms into a pseudo-random sequence.

1: Link to the implementation of Mersenne Twister. This is the latest version from the original creators: 
http://www.math.sci.hiroshima-u.ac.jp/~m-mat/MT/MT2002/CODES/mt19937ar.c