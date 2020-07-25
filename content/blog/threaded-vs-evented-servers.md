---
date: 2020-07-25
linktitle: Threaded vs Evented Servers
title: Threaded vs Evented Servers
weight: 10
url: /threaded-vs-evented-servers
description: Threaded servers use multiple concurrently-executing threads that each handle one client request, while evented servers run a single event loop that handles events for all connected clients.
keywords:
  - clojure
  - deploy
  - develop
  - programming
  - webapp
  - aws
  - ec2
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Threaded vs Evented Servers" />
<meta name=”twitter:description” content="Threaded servers use multiple concurrently-executing threads that each handle one client request, while evented servers run a single event loop that handles events for all connected clients." />

Broadly speaking, there are two ways to handle concurrent requests to a server. *Threaded* servers use multiple concurrently-executing threads that each handle one client request, while *evented* servers run a single event loop that handles events for all connected clients.

To chose between the threaded and evented approaches you need to consider the load profile of the server. This post describes a simple mathematical model for reasoning about these load profiles and their implications for server design.

Suppose that requests to a server take `c` CPU milliseconds and w wall clock milliseconds to execute. The CPU time is spent actively computing on behalf of the request, while the wall clock time is the total time including that time spent waiting for calls to external resources. For example, a web application request might take 5 ms of CPU time `c` and 95 ms waiting for a database call for a total wall time `w` of 100 ms. Let’s also say that a threaded version of the server can maintain up to `t` threads before performance degrades because of scheduling and context-switching overhead. Finally, we’ll assume single-core servers.

If a server is CPU bound then it will be able to respond to at most

```
(/ 1000 c)
```

requests per second. For example, if each requests takes 2 ms of CPU time then the CPU can only handle

(/ 1000 2)
=> 500

requests per second.

If the server is thread bound then it can handle at most

(* t (/ 1000 w))

requests per second. This expression is similar to the one for CPU time, but here we multiply the result by `t` to account for the `t` concurrent threads.

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

The throughput of a threaded server is the minimum of the CPU and thread bounds since it is subject to both constraints. An evented server is not subject to the thread constraint since it only uses one thread; its throughput is given by the CPU bound. We can express this as follows:

```clojure
(defn max-request-rate [t c w]
  (let [cpu-bound    (/ 1000 c)
        thread-bound (* t (/ 1000 w))]
    {:threaded (min cpu-bound thread-bound)
     :evented  cpu-bound}))
```

Now we'll consider some different types of servers and see how they might perform with threaded and evented implementations.

For the examples below I'll use a `t` value of 25. This is a modest number of threads that most threading implementations can handle.

Let's start with a classic example: an HTTP proxy server. These servers require very little CPU time, so say `c` is 0.1 ms. Suppose that the downstream servers can receive the relay within milliseconds for a wall time `w` of, say, 10 ms. Then we have

```clojure
(max-request-rate 25 0.1 10)
=> {:threaded 2500, :evented 10000}
```

In this case we expect a threaded server to be able to handle 2500 requests per second and an evented server 10000 requests per second. The higher performance of the evented server implies that the thread bound is limiting for the threaded server.

Another familiar example is the web application server. Let's first consider the case where we have a lightweight app that does not access any external resources. In this case the request parsing and response generation might take a few milliseconds; say `c` is 2 ms. Since no blocking calls are made this is the value of `w` as well. Then

```clojure
(max-request-rate 25 2 2)
=> {:threaded 500, :evented 500}
```

Here the threaded server performs as well as the evented server because the workload is CPU bound.

Suppose we have a more heavyweight app that is making calls to external resources like the filesystem and database. In this case the amount of CPU time will be somewhat larger that the previous case but still modest; say `c` is 5 ms. But now that we are waiting on external resources we should expect a `w` value of, say, 100 ms. Then we have

```clojure
(max-request-rate 25 5 100)
=> {:threaded 200, :evented 200}
```

Even though we are making a lot of blocking calls, the workload is still CPU bound and the threaded and evented servers will therefore perform comparably.

Suppose now that we are implementing a background service such as an RSS feed fetcher that makes high-latency requests to external services and then performs minimal processing of the results. In this case `c` may be quite low, say 2 ms, but `w` will be high, say 250 ms. Then

```clojure
(max-request-rate 25 2 250)
=> {:threaded 100, :evented 500}
```

Here an evented server will perform better. The CPU load is sufficiently low and the external resource latency sufficiently high that the blocking external calls limit the threaded implementation.

Finally, consider the case of long polling clients. Here clients establish a connection to the server and the server responds only when it has a message it wants to send to the client. Suppose that we have a lightweight app such that `c` is 1 ms, but that response messages are sent to the client after 10 seconds such that the `w` value is 10000 ms. Then

```clojure
(max-request-rate 25 1 10000)
=> {:threaded 2.5, :evented 1000}
```

If the server were really limited to 25 threads and each client required its own thread, we could only allow 2.5 new connections per second if we wanted to avoid exceeding the thread allocation. An evented server on the other hand could saturate the CPU by accepting 1000 requests per second.

Even if we increase the maximum number of threads `t` by an order of magnitude to 250, the evented approach still fares better:

```clojure
(max-request-rate 250 1 10000)
=> {:threaded 25, :evented 1000}
```

Indeed, a threaded server would need to maintain 10000 threads in order to be able to accept requests at the rate of the evented server.

Now that we have seen some specific examples of the model we should step back and note the patterns. In general, an evented architecture becomes more favorable as the ratio of wall time `w` to CPU time `c` increases, i.e. as proportionally more time is spent waiting on external resources. Also, the viability of a threaded architecture depends on the strength of the underlying threading implementation; the higher the thread threshold `t`, the more wait time can be tolerated before eventing becomes necessary.

In addition to the quantitative performance implications captured by this model, there are several qualitative factors that influence the suitability of threaded and evented architectures for particular servers.

One factor is the fit of the server architecture to the work that the server is doing internally. For example, proxying is well suited to evented architectures because the work being done is fundamentally evented: upon receiving an input chunk from the client the chunk is relayed to a downstream server. In contrast, the business logic implemented by web applications is more naturally described in a synchronous style. The callbacks required by an evented architecture become unwieldy in complex application code.

Another consideration is memory coordination and consistency. Evented servers executing in a single event loop do not need to worry about the correctness and performance implications of maintaining consistent shared memory, but this may be a problem for threaded servers. Threaded servers therefore attempt to minimize memory shared among threads. This approach works well for the servers that we discussed above - proxies, web applications, background workers, and long poll endpoints - as none of them need to share state internally across client sessions. But fundamentally stateful servers like caches and databases cannot avoid this problem.

The threaded approach can be a non-starter if the underlying platform does not support proper threading. In these cases blocking calls to external resources prevent the process from using the CPU in other threads, even if the blocker is not itself using the CPU. C Ruby falls into this category. In these cases `t` is effectively 1, making evented architectures relatively more appealing.

In the other extreme, the assumption of `t` being 25 or even 250 may be too modest for some platforms. These low t values are [an artifact of threading implementations](http://www.usenix.org/events/hotos03/tech/full_papers/vonbehren/vonbehren_html/index.html) and not intrinsic to the threading model itself. More scalable threading implementations make threaded servers viable for higher `w` to `c` ratios.

An evented approach can be compromised by a lack of evented libraries for the platform. For evented servers to perform optimally, all external resources must be accessed through nonblocking libraries. Such libraries are not always available, especially on platforms that have typically used threaded/blocking models like the JVM and C Ruby. Fortunately this situation is improving as developers [publish](https://netty.io/) [more](https://github.com/AsyncHttpClient/async-http-client) [nonblocking](http://rubyeventmachine.com/) [libraries](https://github.com/igrigorik/em-http-request) in response to the demand from implementors of evented servers. Indeed, the requirement of pervasive evented libraries for optimal performance is one reason that [node.js](https://nodejs.org/en/) is so compelling for building evented servers.

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
