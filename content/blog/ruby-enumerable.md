---
date: 2020-07-18
linktitle: A Deeper Look at Ruby's Enumerable
title: A Deeper Look at Ruby's Enumerable
weight: 10
url: /ruby-enumerable
description: Ruby’s Enumerable module gives you a way of iterating over collections in a lazy manner, loading only what you need, when you need it. But it gives us so much more than that.
keywords:
  - ruby
  - enumerable
  - each_cons
---

<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="A Deeper Look at Ruby's Enumerable" />
<meta name=”twitter:description” content="Ruby’s Enumerable module gives you a way of iterating over collections in a lazy manner, loading only what you need, when you need it. But it gives us so much more than that." />

Have you ever needed to load a VERY large file in ruby? No I don’t mean your 500 line rails model. I mean a several gig binary or text file.

Try it. Watch your machine become unresponsive. Cry a little inside. Then reach for enumerable.

Ruby’s Enumerable module gives you a way of iterating over collections in a lazy manner, loading only what you need, when you need it. But it gives us so much more than that.

Today I am going to walk you through a couple of highly useful methods from Enumerable that has come up in a few coding challenges I have done over the years.

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

### each_cons, any?, all?, none?

I use these methods when I need to determine the 'distance or difference' in a set of numbers or objects. `each_cons` simply gives us a 'sliding' window of our list so we can compare multiple items in our list.

```ruby
numbers = [1,3,5,8,10,54,99]
cards   = [5,3,4,6,2]

# get only the values where the distance is greater than 10
numbers.each_cons(2).select {|a,b| b-a>10 } #=> [[10, 54], [54, 99]]

# determine if the hand is a straight
cards.sort.each_cons(5).all? do |series| 
             (series.first..series.last).to_a == series
           end #=> true
```
Our first example shows how to get the elements in the numbers list that are over 10 units in difference. I do this by using `each_cons` to give me a sliding window of 2 elements at a time. This give me:

At each step on the way I check b-a and see if that is greater than 10. If so, the `select` will return those groups of elements.

The second example simply sorts a set of 'cards' and then looking at all 5 cards through `each_cons` and comparing the scores to see if they are in order and in sequence `(a+1==b)`.

Let’s play off this example a bit more, this time with dice. If we have to implement small and large straights, we have a similar problem, with a similar solution:

For the small straight we get 2 windows, becuase there are 5 values in the list and we are calling `each_cons(4)`. Therefore we want a postfix of `any`? because either of the windows can be a run of 4.

In the second example we use `all`? to be more explicit, yet `any`? would have worked as well, because in the large straight, we only have one sliding window, 2,3,4,5,6.

### any? all? none?

These have to be a few of my favorite methods in enumerable.

Given no args, they take a list of bool values and returns a bool. So `[true, true, false].all? #=> false`.

Kinda useful, but not really. If we pass it a block of code however.. now we can do something useful. Consider this example:

`.any?` and `.none?` do what you probably think. Where `.all?` only returns true if ALL the conditions and elements match up, `.any?` returns true if ANY of them match, and `.none?` returns true if there were NO matches.

### Weird things like look-ahead?

Say you have a file to read in line by line. You need to compare the current line to the next line to look for duplicates. `each_cons(2)` to the rescue!

This will tell us of the current line and the following line are the same.

 - More uses for each_cons
 - averages
 - distances
 - smoothing plots
 - graphing
 - geometry

## Help improve this content
Feel free to <a href="https://github.com/TutsWiki/source/edit/master/content/blog/rest-api-in-clojure.md">edit this page</a> to fix any typos or add more insights.

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
