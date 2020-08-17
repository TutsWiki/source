---
date: 2020-07-25
linktitle: noprocrast
title: noprocrast
weight: 10
url: /noprocrast
description: I wrote a command line script called noprocrast. This script updates /etc/hosts to make distracting websites unreachable from my machine.
keywords:
  - noprocrast
  - productivity
  - procastination
tags: [Linux]  
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="noprocrast" />
<meta name=”twitter:description” content="I wrote a command line script called noprocrast. This script updates /etc/hosts to make distracting websites unreachable from my machine:" />

I wrote a command line script called `noprocrast`. This script updates `/etc/hosts` to make distracting websites unreachable from my machine:

```bash
sudo cp /etc/noprocrast_hosts /etc/hosts
```

The corresponding `/etc/noprocrast_hosts`:

```bash
127.0.0.1 localhost db001 db002 db003 db004 web030 web048
255.255.255.255 broadcasthost
::1             localhost
fe80::1%lo0     localhost

127.0.0.1 news.ycombinator.com
127.0.0.1 reddit.com www.reddit.com
127.0.0.1 twitter.com
127.0.0.1 nytimes.com www.nytimes.com
```

In case I really need to visit one of these sites, I use the following `procrast` script:

```bash
sudo cp /etc/procrast_hosts /etc/hosts
echo `date` >> ~/.procrasts
```

Then if I want to see how I have been doing with procrastination, I `tail ~/.procrasts`.

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

The `/etc/procrast_hosts` file is just:

```bash
127.0.0.1 localhost db001 db002 db003 db004 web030 web048
255.255.255.255 broadcasthost
::1             localhost
fe80::1%lo0     localhost
```

If you haven't tried an `/etc/hosts` hack like this, I suggest giving it a try. You may be surprised by how much time you save.

Also see

 - [get-shit-done](https://github.com/viccherubini/get-shit-done)
 - [noprocrast (ruby gem)](https://github.com/rfwatson/noprocrast)
 
