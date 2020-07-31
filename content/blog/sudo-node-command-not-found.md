---
date: 2020-07-22
linktitle: sudo and node
title: How to fix sudo node command not found error
weight: 10
url: /how-to-fix-sudo-node-command-not-found-error
description: To fix this, we need to “pass the environment” of the calling thread to the computation thread.
keywords:
  - sudo
  - node
  - debugging
---

Getting permission errors when installing a module?

Are 'sudo: node: command not found' errors taking away your precious sleeping hours?

## How to fix?

You have to remove any trace of node on your system, and reinstall it. It will soothe your pain:

```bash
echo 'export PATH=$HOME/local/bin:$PATH' >> ~/.bashrc
. ~/.bashrc
mkdir ~/local && ~/node-latest-install && ~/node-latest-install
curl http://nodejs.org/dist/node-latest.tar.gz | tar xz --strip-components=1
./configure --prefix=~/local && make install
curl https://npmjs.org/install.sh | sh
rm -rf ~/node-latest-install
```
You are one/two steps/commands away from success. Just replace `/path/to/your/home/` with the **absolute** path to your home directory (leaving the rest of the path as is):

```bash
sudo ln -s /path.to/your/home/local/bin/node /usr/local/bin/
sudo ln -s /path.to/your/home/local/bin/npm /usr/local/bin/
```

And now...

![now kiss meme](/images/now-kiss.png "now kiss meme")

Also see: [install node and npm without having to sudo](https://gist.github.com/isaacs/579814#file-node-and-npm-in-30-seconds-sh)

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
