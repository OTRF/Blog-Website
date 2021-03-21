---
layout: post
current: post
cover:  assets/images/blog/2020-12-01_00_jekyll_installation_cover.jpg
navigation: True
title: Installing Jekyll - Windows
date: 2020-12-01 12:00:00
tags: [jekyll]
class: post-template
subclass: 'post'
author: jose
---

In this quick blogpost, I will share the steps that you can follow in order to install jekyll on a Windows computer using Firefox as web browser and the command prompt. After completing all the steps of this blog, you should be able to deploy a website locally in your computer.

## 1. Downloading Ruby + Devkit Executable
* Go to [Ruby Installers Downloads](https://rubyinstaller.org/downloads/) website.
* Click on **Ruby+Devkit 2.7.2-1 (x64)** under the **WITH DEVKIT** section.

![](assets/images/blog/2020-12-01_01_jekyll_ruby_download.jpg)

## 2. Running Ruby + Devkit Executable
* Click on **Save File**.

![](assets/images/blog/2020-12-01_02_jekyll_save_file.jpg)

* In your **Downloads** directory, right click on the Ruby + Devkit executable and select **Open**.

![](assets/images/blog/2020-12-01_03_jekyll_open_file.jpg)

## 3. Completing the Ruby + Devkit Setup

* **Accept** the license and click on **Next**.

![](assets/images/blog/2020-12-01_04_jekyll_accept_license.jpg)

* Select the installation **Destination** folder and click on **Install**.

![](assets/images/blog/2020-12-01_05_jekyll_installation_destination.jpg)

* Select the components that should be installed. Check **Ruby RI** and **MSYS2 development toolchain** boxes.
* Click on **Next**

![](assets/images/blog/2020-12-01_06_jekyll_select_components.jpg)

* Wait for the installation process to complete.

![](assets/images/blog/2020-12-01_07_jekyll_waiting_installation.jpg)

* Check the **Run ridk install** box and click on **Finish**.
* A command prompt window will be opened.

![](assets/images/blog/2020-12-01_08_jekyll_finish_installation.jpg)

## 3. Installing MSYS2

* Type **1** and press enter in order to install the **MYSYS2 base** component.

![](assets/images/blog/2020-12-01_09_jekyll_msys2_installer.jpg)

* Wait for the installation process to complete.

![](assets/images/blog/2020-12-01_10_jekyll_msys2_waiting_installation.jpg)

* After the installation process is completed, **close** the command prompt window.

![](assets/images/blog/2020-12-01_11_jekyll_msys2_installation_complete.jpg)

## 4. Installing Jekyll

* **Open** a new command prompt window and run **gem install jekyll**.

![](assets/images/blog/2020-12-01_11_jekyll_gem_install_jekyll.jpg)

* Wait for the installation process to complete.

![](assets/images/blog/2020-12-01_12_jekyll_gem_install_jekyll_waiting.jpg)

* After the installation process is completed, run **jekyll -v** to validate that the installation was successful.

![](assets/images/blog/2020-12-01_13_jekyll_installation_validation.jpg)

* Close the command prompt window.

## 5. Installing Bundler

* **Open** a new command prompt window and run **gem install bundler**.

![](assets/images/blog/2020-12-01_14_jekyll_gem_install_bundler.jpg)

* Wait for the installation process to complete.

![](assets/images/blog/2020-12-01_15_jekyll_gem_install_bundler_waiting.jpg)

* After the installation process is completed, run **bundle -v** to validate that the installation was successful.

![](assets/images/blog/2020-12-01_16_jekyll_gem_install_bundler_validation.jpg)

* Close the command prompt window.

## References

* https://jekyllrb.com/
* https://rubyinstaller.org/downloads/