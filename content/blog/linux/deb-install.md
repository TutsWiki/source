---
date: 2020-11-01
linktitle: Install deb file from command line
title: Install deb file from command line (dpkg, apt, gdebi, software center)
weight: 10
url: /install-deb-command-line-dpkg-apt-gdebi-eddy
description: Find out various ways to install deb package in Ubuntu/Debian using dkpg, apt, gdebi, eddy or software center.
keywords:
  - deb
  - install
  - apt
  - linux
tags: [Linux]  
---
<meta property="og:image" content="https://tutswiki.com/images/DSA/radix-sort-example-array-1.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Install deb file from command line in Ubuntu" />
<meta name=”twitter:description” content="Find out various ways to install deb package in Ubuntu/Debian using dkpg, apt, gdebi, eddy or software center." />

## What is a deb file?
Deb is a collection of archived files that is managed by Debian Package Management System. Some softwares in Ubuntu is available through the deb package, these files have a .deb extension. In simple words, to install files in linux we have a software center but it does not have all the softwares, so these softwares are needed to be installed through the deb package. We will go through a number of steps through which we can install softwares from the deb package.

In this article we will be installing [Google Chrome](https://www.google.com/chrome/) in Ubuntu.

- First we have to download chrome from their official website. Follow this link https://www.google.com/chrome/
![Installation in ubuntu](/images/blog/deb_1.png "download")

- Now click on the Download button and select your linux type.
![Installation in ubuntu](/images/blog/deb_2.png "download-start")

- Your download will be available in download folder
![Installation in ubuntu](/images/blog/deb_3.png "folder")

## Ways to install deb package in Ubuntu
Now we will see multiple ways of installing Google Chrome in Ubuntu as an example.

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

### 1. Software Center
- First simplest way to install this file is to just double click on it and it will open it in software center like this
![Installation in ubuntu](/images/blog/deb_4.png "software-center")

- Then you just need to click on install and enter password. Installation will start 
![Installation in ubuntu](/images/blog/deb_5.png "install-start")

### 2. Installing .deb file using gdebi
While installing through software center, sometimes we can face some error like dependency error. This is because our software is dependent on some other piece of software or library. This occurs because while preparing the deb package, sometimes the developer may assume that some other required library or software is already available, but if it is not there then the system gives an error message.

To handle this we have gdebi, which is a lightweight GUI application. Its sole purpose is to install deb packages. It detects the dependencies and tries to install them along the package. Lets see how it works 

- First we have to install gdebi
Open Command prompt (`CTRL+ALT+T`), then type command:
```console
sudo apt install gdebi
```
and enter the password to your computer. gdebi package installer application will be installed.
![Installation in ubuntu](/images/blog/deb_6.png "gdebi")

- Right click on the downloaded file and select open with “ gdebi package installer “. Then click Install Package
![Installation in ubuntu](/images/blog/deb_7.png "install")

- Then google chrome will be installed
![Installation in ubuntu](/images/blog/deb_8.png "install-google")

### 3. Installing deb files using dpkg command
For dpkg we will use -i parameter

- Then type command {`sudo dpkg -i path_to_the_file`} in the terminal window and enter password.
```console
sudo dpkg -i ./Downloads/google-chrome-stable_current_amd64.deb 
```
![Installation in ubuntu](/images/blog/deb_11.png "install-through-dpkg")

- If we get error of missing dependencies, we can use this apt command to fix them
```console
sudo apt install -f
```
![Installation in ubuntu](/images/blog/deb_12.png "fix")

- To remove the package we have to use `-r` parameter
```console
sudo dpkg -r google-chrome-stable
```
![Installation in ubuntu](/images/blog/deb_13.png "remove")

### 4. Installing deb file using apt command
To install using apt command, first we must have apt installed. Apt command uses dpkg underneath. It is more easy and popular.

- Then type command {`sudo apt install path_to_the_file`} in the terminal window.
```console
sudo apt install ./Downloads/google-chrome-stable_current_amd64.deb 
```
![Installation in ubuntu](/images/blog/deb_9.png "install-through-apt")

- You can also remove this file by command
```console
sudo apt remove google-chrome-stable
```
![Installation in ubuntu](/images/blog/deb_10.png "remove-through-apt")
