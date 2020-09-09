---
date: 2020-08-22
linktitle: Installation
menu:
  main:
    parent: R
next: /r/basic_syntax
title: Installing R and RStudio in Windows & Linux
weight: 2
url: /r/install-env-setup
description: Detailed steps with screenshots on how to install R and RStudio IDE in Windows, Ubuntu and CentOS.
keywords:
  - R
tags: [R]  
---
R is a very popular and most interactive programming language and has been an important toolbox for Data Scientists and Business Analysts. To work and create projects on R, you just need to install two important tools– **R** and **RStudio**. Both of these software work in parallel to create various projects and Markdown Documents in R.

Installing R to a local computer consists of various easy steps. The steps for installation of R vary with different operating systems like Windows, Linux and MacOS. The official site for the installation [cloud.r-project.org](https://cloud.r-project.org) provides various pre-compiled binary versions and libraries for the above mentioned various operating systems. To install R, you can get the package either from the given official site or can use commands from the terminal or command prompt. In some Linux Distributions, R is installed by default, which you can easily verify by entering R in the console.

## Installing R in Windows

There are some steps you need to follow to install R and RStudio. Let's have a look at those steps.

1. Firstly you have to download R setup by directly clicking on the link https://cloud.r-project.org OR https://cran.r-project.org. You will be directed to a page containing downloads for various operating systems.  
![Download R for Windows](/images/R/download-R-windows.png "Download R for Windows")
2. Click on "base" and then click on R for the first time, it will direct you to the page containing the latest version of R. Click on **Download R 4.0.2 for Windows** (This is the latest version as of this writing).
![Download R 4.0.2 for Windows](/images/R/download-R-402-windows.png "Download R 4.0.2 for Windows")
3. Once the download is finished, run the setup file.
4. License information will be displayed on the next screen. R uses GPL license. Click on Next. 
![R GPL license](/images/R/R-license.png "R GPL license")
5. Select the path where we want to download our R Software and proceed to Next.                                   ![Download R 4.0.2 for Windows](/images/R/R-install-path.png "Download R 4.0.2 for Windows")
6. Select all the components like 32 bit file, 64 bit file and core files which you want to install and then click on Next.                                                                                                                    ![R 32 and 64 bit files](/images/R/R-32-64-bit.png "R 32 and 64 bit files")
7. In the next step, we have to select either customized startup or we go on with the default and proceed to Next.
![R customized startup](/images/R/R-customized-startup.png "R customized startup")
8. On clicking next, the installation of R in our system will get started in the designated path.
![R Installing](/images/R/R-installing-step.png "R Installing")
9. At last, we will click on Finish to successfully install R in our system.
![R Setup Finished](/images/R/R-setup-finished.png "R Setup Finished")

## Installing RStudio IDE in Windows
1. [Download RStudio](https://rstudio.com/products/rstudio/download/) from the official website.
2. Download the free version by clicking on **Download RStudio**.
![R Setup Finished](/images/R/R-studio-various-versions.png "R Setup Finished")
3. In the next step, we will select the appropriate installer. When we select the installer, our downloading of RStudio will take place and setup will start.
![RStudio free version](/images/R/R-studio-free-version.png "RStudio free version")
4. Once the setup file is downloaded, run it and click on Next.
![RStudio Setup](/images/R/R-studio-install.png "RStudio Setup")
5. Click on Install and proceed for extracting files and installing RStudio.
![RStudio Installing](/images/R/R-studio-installing.png "RStudio Installing")
6. Click on Finish at last.
![RStudio Installed](/images/R/R-studio-installed.png "RStudio Installed")
7. Open RStudio from the installed location or lookup in Start Screen.
![RStudio Welcome Screen](/images/R/R-studio-welcome-screen.png "RStudio Welcome Screen")
8. Now as a demonstration, a sample query has been written in the R script (`File` -> `New File` -> `R Script`). The result will appear in the console section below.
![RStudio Hello World](/images/R/R-studio-hello-world.png "RStudio Hello World")

## Installing R in Ubuntu 20.04/19.04/18.04/16.04
1. Let's first update the packages
```bash
sudo apt update
sudo apt –y upgrade
```
2. Once that's done, install **r-base**
```bash
sudo apt –y install r-base
```
