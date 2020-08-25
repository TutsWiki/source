---
date: 2020-08-22
linktitle: Installation
menu:
  main:
    parent: R
next: /r/basic_syntax
title: Installing R and RStudio in Windows & Linux
weight: 26
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

## Installing RStudio IDE in Ubuntu
1. Download the RStudio package for Ubuntu from the [official site](https://rstudio.com/products/rstudio/download/)
![RStudio Ubuntu Package](/images/R/RStudio-ubuntu-deb.png "RStudio Ubuntu Package")
2. Once you have the .deb file, all that is left is to navigate to your downloads folder using  in the command line  cd Downloads and then run the following command to begin the installation process:
```bash
sudo dpkg –i rstudio 1.3.1073-amd64.deb
```
3. You may encounter some dependency problems in this system that may cause your first try to install RStudio to fail, but this has an easy fix. Just run the following command and try again:
```bash
sudo apt –f install
```
«image 21 goes here»
4. When the process of installation finishes, you will have an RStudio shortcut in your Ubuntu app list, but there is an alternative that you will also be able to start RStudio by typing rstudio in the command line.
From the application list of ubuntu:
«image 22 goes here»
From the command terminal:
«image 23 goes here»

## Installing R in CentOS
R packages are not included in CentOS 8 core repositories. We’ll install R from the EPEL repository. To install R on CentOS 8, follow these steps:
1. Enable the EPEL and Power Tools repositories:
«image 24 goes here»
2. Install R by typing:
«image 25 goes here»
3. Verify the installation by printing the R version:
«image 26 goes here»
4. Install the libraries and tools that are used by common R packages:
«image 28 goes here»
5. That’s it! You have successfully installed R your CentOS system, and you can start using it.

## Installing packages from CRAN
1. If the R binary is launched as root or sudo, the packages are installed globally and available for all system users. To set up a personal library for your user, invoke the binary as a regular user.
2. As an example, we’ll install a package named stringr, which provides fast and correct implementations of common string manipulations. Start by opening the R console as root:
«image 29 goes here»
3. Install the stringr package:
«image 30 goes here»
4. You will be asked to select a CRAN mirror:                                                                        «image 31 goes here»   
5. Select the mirror that is closest to your location.
6. The installation will take some time and once completed, load the library by typing:
«image 32 goes here»
7. Let’s take an example and define a character vector named Tutorial:
«image 33 goes here»
8. Run the following function which will print the length of each string:
«image 34 goes here»
9.	The output is given as follows:
«image 35 goes here»

You can install many more such packages from CRAN using `install.packages ()`.