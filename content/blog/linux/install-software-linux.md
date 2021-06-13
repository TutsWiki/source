---
date: 2020-08-17
linktitle: How to install software in Linux
title: How to install software in Linux (RPM/DEB systems)
weight: 10
url: /install-software-linux-yum-rpm-apt-dpkg
description: In this tutorial we'll be covering packages, package managers and how to find, install and remove software for most popular Linux distributions. RPM/DEB based.
keywords:
  - install
  - yum
  - rpm
  - apt
  - dpkg
  - software
  - linux
tags: [Linux] 
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="How to install software in Linux (RPM/DEB systems)" />
<meta name=”twitter:description” content="In this tutorial we'll be covering packages, package managers and how to find, install and remove software for most popular Linux distributions. RPM/DEB based." />

In this tutorial, we'll be covering packages, package managers and how to find, install and remove software for most popular Linux distributions.

## Package
Typically when you install software in a Linux system you do so with a package. A package is just a collection of files that make up an application. Additionally, a package contains data about the application as well as any steps required to successfully install and remove that application. 

### Data / Metadata
The data or metadata that is contained within a package can include such information as the description of the application, the version and the list of dependencies or other packages that this particular application needs in order to function.

## Package Manager
A package manager is used to install, upgrade and remove packages. The package manager uses a package's metadata to automatically install any required dependencies. Package managers keep track of what files belong to what packages, what packages are installed and what versions of those packages are installed.

## Installing Software on RPM Distros
Here's a list of distributions that are based on the RPM package format. RPM stands for RedHat Package Manager.

- RedHat
- CentOS
- Fedora
- Oracle Linux
- Scientific Linux

### yum
The `yum` command-line utility is a package management program for those Linux distributions that use `rpm` packages. 

**Note:** Installing or removing software requires `root` or `superuser` privileges.

#### Find packages to install using yum
```bash
yum search string
```

#### Display information about a package using yum
```bash
yum info [package]
```

#### Install a package with yum
```bash
yum install [package]
```
yum will typically ask you to review your request and say yes or no. If you want to continue to automatically answer yes to yum's question use:
```bash
yum install [-y] [package]
```

#### Remove a package with yum
```bash
yum remove [package]
```

### rpm
In addition to the `yum` command, you can also use the `rpm` command to interact with the package manager.  

#### List installed packages using rpm
```bash
rpm -qa
```

#### List the file's package usign rpm
rpm -qf with path to a file will tell you what file a package belongs to.
```bash
rpm -qf /path/to/file
```

#### List package's files
List all the files that belong to that particular package
```bash
rpm -ql [package]
```

#### Install package using rpm
```bash
rpm -ivh package.rpm
```

#### Erase (uninstall) pacakge using rpm
```bash
rpm -e [package]
```

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

## Installing Software on DEB Distros
Another popular package format is the Debian package format. In addition to Debian, distributions like Mint and Ubuntu use deb packages.

- Debian
- Linux Mint
- Ubuntu

### apt - Advanced Packacking Tool
The Debian-based distributions use a package manager called `apt` (advanced packaging tool). `apt` is comprised of a few small utilities with the two most commonly used being `apt-cache` and `apt-get`.

#### Find packages to install using apt-cache
```bash
apt-cache search string
```

#### Install a package with apt-get
```bash
apt-get install [-y] [package]
```

#### Remove a package but keep configurations
```bash
apt-get remove [package]
```

#### Remove a package and delete configurations
```bash
apt-get purge [package]
```

#### Display information about a package using apt-cache
```bash
apt-cache show [package]
```

### dpkg
In addition to the app utilities, you can use the dpkg command to interact with the package manager.

#### List installed packages using dpkg
```bash
dpkg -l
```

#### List file's package using dpkg
```bash
dpkg -S /path/to/file
```

#### List all files in a package using dpkg
```bash
dpkg -L [package]
```

#### Install a package with dpkg
```bash
dpkg -i package.deb
```

{{% notice tip %}}
Watch the video for live examples of searching, installing and removing some actual software like Dropbox using the above mentioned methods.
{{% /notice %}}
{{< youtube K3tdWIJhg8I >}}

## Summary
Packages are used to install software on Linux system. You can manipulate packages with a package manager. Two of the most popular package formats are RPM and Debian. For RPM-based distributions use the `yum` and `rpm` commands to manage packages. For Debian-based distributions use `apt` and `dpkg` to manage packages.

Also see: [Install deb file from command line](/install-deb-command-line-dpkg-apt-gdebi/)
