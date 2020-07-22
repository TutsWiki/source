---
date: 2020-07-22
linktitle: Environment Passing in Clojure
title: Environment Passing in Clojure
weight: 10
url: /environment-passing-in-clojure
description: To fix this, we need to “pass the environment” of the calling thread to the computation thread.
keywords:
  - clojure
  - environment
  - env
  - programming
---

<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Environment Passing in Clojure" />
<meta name=”twitter:description” content="To fix this, we need to “pass the environment” of the calling thread to the computation thread." />

Clojure’s [vars](https://clojure.org/reference/vars) provide a nice mechanism for managing dynamically bound state, but you need to be careful with vars when moving computations across thread boundaries. For example, suppose we implement the following timeout function:

```clojure
(import '(java.util.concurrent Future TimeUnit))

(defn timeout1 [millis op]
  (.get (future-call op) millis TimeUnit/MILLISECONDS))
```

The function `timeout1` sends the operation `op` to be computed in a separate, timeout-bound thread. If the operation completes within `millis` milliseconds, `timeout1` returns the result of that operation, otherwise it throws an exception:

```clojure
(timeout1 1000 #(inc 1))
=> 2

(timeout1 1000 #(Thread/sleep 2000))
; throws java.util.concurrent.TimeoutException 
```

This timeout implementation demonstrates the potential problem with using vars and threads:

```clojure
(def a-var 3)

(binding [a-var 4] (inc a-var))
=> 5

(timeout1 1000 #(binding [a-var 4] (inc a-var)))
=> 5

(binding [a-var 4] (timeout1 1000 #(inc a-var)))
=> 4
```

If we rebind `a-var` within the timeout thread, we get the expected result of `5`. But if we rebind `a-var` outside of the timeout thread, the computation sees the root value of `3` for `a-var`, not the thread-local value of `4`.

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

To fix this, we need to “pass the environment” of the calling thread to the computation thread:

```clojure
(defn timeout2 [millis op]
  (let [env    (get-thread-bindings)
        env-op #(with-bindings* env op)]
    (.get (future-call env-op) millis TimeUnit/MILLISECONDS)))

(timeout2 1000 #(binding [a-var 4] (inc a-var)))
=> 5

(binding [a-var 4] (timeout2 1000 #(inc a-var)))
=> 5
```

Here we capture the environment in the context of the caller and inject it into the timeout thread using `with-bindings*`. Now that the timeout thread sees the same thread-local environment as the caller, the external binding of `a-var` works as expected.

Finally, we can extract this pattern into a macro `passing-bindings` that can be used to pass an environment across thread boundaries:

```clojure
(defmacro passing-bindings [[in-env-sym] body]
  `(let [env# (get-thread-bindings)
         ~in-env-sym (fn [op#] #(with-bindings* env# op#))]
     ~body))

(defn timeout3 [millis op]
  (passing-bindings [in-env]
    (.get (future-call (in-env op)) millis TimeUnit/MILLISECONDS)))

(binding [a-var 4] (timeout3 1000 #(inc a-var)))
=> 5
```
## Help improve this content
Feel free to <a href="https://github.com/TutsWiki/source/edit/master/content/blog/env-passing-in-clojure.md">edit this page</a> to fix any typos or add more insights.

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