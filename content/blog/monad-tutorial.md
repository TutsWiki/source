---
date: 2020-07-22
linktitle: Yet another lousy monad tutorial
title: Yet another lousy monad tutorial
weight: 10
url: /yet-another-lousy-monad-tutorial
description: I'm not a big fan of monads, but I understand them.
keywords:
  - monad
  - fp
  - functionalprogramming
  - programming
---

<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Yet another lousy monad tutorial" />
<meta name=”twitter:description” content="I'm not a big fan of monads, but I understand them." />
I'm not a big fan of monads, but I understand them. They're not rocket science. Let me try to write a monad tutorial that would've helped my past self understand what the fuss was about. I like concrete explanations that start with practical examples, without any annoying metaphors, and especially without any Haskell code. So here's five examples that have something in common:

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

1) If a function has type A → B, and another function has type B → C, then we can "compose" them into a new function of type A → C.

2) Let's talk about functions that can return multiple values. We can model these as functions of type A → List<B>. There is a natural way to "compose" two such functions. If one function has type A → List<B>, and another function has type B → List<C>, then we can "compose" them into a new function of type A → List<C>. The "composition" works by joining together all the intermediate lists of values into one. This is similar to MapReduce, which also collects together lists of results returned by individual workers.

3) Let's talk about functions that either return a value, or fail somewhere along the way. We can model these as functions of type A → Option<B>, where Option<B> is a type that contains either a value of type B, or a special value None. There is a natural way to "compose" two such functions. If one function has type A → Option<B>, and another function has type B → Option<C>, then we can "compose" them into a new function of type A → Option<C>. The "composition" works just like regular function composition, except if the first function returns None, then it gets passed along and the second function doesn't get called. This way you can have a "happy path" in your code, and check for None only at the end.

4) Let's talk about functions that call a remote machine asynchronously. We can model these as functions of type A → Promise<B>, where Promise<B> is a type that will eventually contain a value of type B, which you can wait for. There is a natural way to "compose" two such functions. If one function has type A → Promise<B>, and another function has type B → Promise<C>, then we can "compose" them into a new function of type A → Promise<C>. The "composition" is an asynchronous operation that waits for the first promise to return a value, then calls the second function on that value. This is known in some languages as "promise pipelining". It can sometimes make remote calls faster, because you can send both calls to the remote machine in the same request.

5) Let's talk about functions that do input or output in a pure functional language, like Haskell. We can define IO<A> as the type of opaque "IO instructions" that describe how to do some IO and return a value of type A. These "instructions" might eventually be executed by the runtime, but can also be freely passed around and manipulated before that. For example, to create instructions for reading a String from standard input, we'd have a function of type Void → IO<String>, and to create instructions for writing a String to standard output, we'd have String → IO<Void>. There is a natural way to "compose" two such functions. If one function has type A → IO<B>, and another function has type B → IO<C>, then we can "compose" them into a new function of type A → IO<C>. The "composition" works by just doing the IO in sequence. Eventually the whole program returns one huge complicated IO instruction with explicit sequencing inside, which is then passed to the runtime for execution. That's how Haskell works.

Another thing to note is that each of the examples above also has a natural "identity" function, such that "composing" it with any other function F gives you F again. For ordinary function composition, it's the ordinary identity function A → A. For lists, it's the function A → List<A> that creates a single-element list. For options, it's the function A → Option<A> that takes a value and returns an option containing that value. For promises, it's the function A → Promise<A> that takes a value and makes an immediately fulfilled promise out of it. And for IO, it's the function A → IO<A> that doesn't actually do any IO.

At this point we could go all mathematical and talk about how "compose" is like number multiplication, and "identity" is like the number 1, and then go off into monoids and categories and functors and other things that are frankly boring to me. So let's not go there! Whew!

Instead, to stay more on the programming track, let's use a Java-like syntax to define an interface Monad<T> with two methods "identity" and "compose". The five examples above will correspond to five different concrete classes that implement that interface, for different choices of the type parameter T.

The main complication is that the type parameter T must not be a simple type, like String. Instead it must be itself a generic type, like List, Option or Promise. The reason is that we want to have a single implementation of Monad<Option>, not separate implementations like Monad<Option<Integer>>, Monad<Option<String>> and so on. Java and C# don't support generic types whose parameters are themselves generic types (the technical term is "higher-kinded types"), but C++ has some support for them, called "template template parameters". Some functional languages have higher-kinded types, like Haskell, while others don't have them, like ML.

Anyway, here's what it would look like in Java, if Java supported such things:

```java
interface Monad<T> {
  <A> Function<A, T<A>> identity();
  <A, B, C> Function<A, T<C>> compose(
    Function<A, T<B>> first,
    Function<B, T<C>> second
  );
} 

class OptionMonad implements Monad<Option> {
  public <A> Function<A, Option<A>> identity() {
    // Implementation omitted, figure it out
  }
  public <A, B, C> Function<A, Option<C>> compose(
    Function<A, Option<B>> first,
    Function<B, Option<C>> second
  ) {
    // Implementation omitted, do it yourself
  }
}
```

Defining Monad as an interface allows us to implement some general functionality that will work on all monads. For example, there's a well known function "liftM" that converts a function of type A → B into a function of type List<A> → List<B>, or Promise<A> → Promise<B>, or anything else along these lines. For different monads, liftM will do different useful things, e.g. liftM on lists is just the familiar "map" function in disguise. The implementation of liftM with lambda expressions would be very short, though a little abstract:

```java
<T, A, B> Function<T<A>, T<B>> liftM(
  Function<A, B> func,
  Monad<T> monad
) {
  return (T<A> ta) -> monad.compose(
    () -> ta,
    (A a) -> monad.identity().apply(func.apply(a))
  ).apply(void);
}
```
Or if you don't like lambda expressions, here's a version without them:

```java
<T, A, B> Function<T<A>, T<B>> liftM(
  Function<A, B> func,
  Monad<T> monad
) {
  return new Function<T<A>, T<B>>() {
    public T<B> apply (T<A> ta) {
      return monad.compose(
        new Function<Void, T<A>>() {
          public T<A> apply() {
            return ta;
          }
        },
        new Function<A, T<B>>() {
          public T<B> apply(A a) {
            return monad.identity().apply(func.apply(a));
          }
        }
      ).apply(void);
    }
  }
}
```

Monads first became popular as a nice way to deal with IO instructions in a pure language, treating them as first-class values that can be passed around, like in my example 5. Imperative languages don't need monads for IO, but they often face a similar problem of "option chaining" or "error chaining", like in my example 3. For example, the option types in Rust or Swift would benefit from having a "happy path" through the code and a centralized place to handle errors, which is exactly the thing that monads would give you.

Some people think that monads are about side effects, like some sort of escape hatch for Haskell. That's wrong, because you could always do IO without monads, and in fact the first version of Haskell didn't use them. In any case, only two of my five examples involve side effects. Monads are more like a design pattern for composing functions, which shows up in many places. I think the jury is still out on whether imperative/OO languages need to use monads explicitly, but it might be useful to notice them when they appear in your code implicitly.

## Help improve this content
Feel free to <a href="https://github.com/TutsWiki/source/edit/master/content/blog/monad-tutorial.md">edit this page</a> to fix any typos or add more insights.

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