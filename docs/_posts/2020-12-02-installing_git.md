---
layout: post
current: post
cover:  assets/images/blog/2020-12-02_00_git_installation_cover.jpg
navigation: True
title: Installing Git - Windows
date: 2020-12-02 12:00:00
tags: [git]
class: post-template
subclass: 'post'
author: jose
---

In this quick blogpost, I will share the steps that you can follow in order to install git(2.31.0) on a Windows computer using Firefox as web browser. After completing all the steps of this blog, you should be able to execute git commands using the command prompt.

## 1. Downloading Git Executable
* Go to [Git Downloads](https://git-scm.com/downloads) website.
* Click on **Windows** under the **Downloads** section.

![](assets/images/blog/2020-12-02_01_git_download_executable.jpg)

## 2. Running Git Executable
* Click on **Save File**.

![](assets/images/blog/2020-12-02_02_git_save_file.jpg)

* In your **Downloads** directory, right click on the Git executable and select **Open**.

![](assets/images/blog/2020-12-02_03_git_open_file.jpg)

* Allow the app to make changes clicking on **Yes**.

![](assets/images/blog/2020-12-02_04_git_allow_changes.jpg)

## 3. Completing the Git Setup

* Click on **Next**.

![](assets/images/blog/2020-12-02_05_git_license.jpg)

* Select **Destination** folder and Click on **Next**.

![](assets/images/blog/2020-12-02_06_git_destination_folder.jpg)

* Select **Components** that will be installed and Click on **Next**.

![](assets/images/blog/2020-12-02_07_git_components.jpg)

* Select **Start Menu** folder and Click on **Next**.

![](assets/images/blog/2020-12-02_08_git_start_menu_folder.jpg)

* Select **Default Editor** application and Click on **Next**.

![](assets/images/blog/2020-12-02_09_git_editor_application.jpg)

* Select **Let Git Decide** the name of the initial branch in new repositories and Click on **Next**.

![](assets/images/blog/2020-12-02_10_git_let_git_decide.jpg)

* Select the **Recommended** option for adjusting your PATH environment and Click on **Next**.

![](assets/images/blog/2020-12-02_11_git_adjust_path_environment.jpg)

* Select **Use the OpenSSL libray** and Click on **Next**.

![](assets/images/blog/2020-12-02_12_git_https_transport_backend.jpg)

* Select **Checkout Windows-style, commit Unix-style line endings** (Recommended setting on Windows) and Click on **Next**.

![](assets/images/blog/2020-12-02_13_git_line_ending_conversion.jpg)

* Select **Use MinTTY** (Default terminal of MSYS2) and Click on **Next**.

![](assets/images/blog/2020-12-02_14_git_use_git_bash.jpg)

* Select **Default** (fast-forward or merge) and Click on **Next**.

![](assets/images/blog/2020-12-02_15_git_behavior_git_pull.jpg)

* Select **Git Credential Manager Core** and Click on **Next**.

![](assets/images/blog/2020-12-02_16_git_credential_helper.jpg)

* Select **Enable file system caching** and Click on **Next**.

![](assets/images/blog/2020-12-02_17_git_extra_options.jpg)

* Click on **Install**.

![](assets/images/blog/2020-12-02_18_git_install.jpg)

* Wait for the installation process to complete.

![](assets/images/blog/2020-12-02_19_git_install_waiting.jpg)

* Uncheck the **View Release Notes** box and click on **Finish**.

![](assets/images/blog/2020-12-02_20_git_install_finish.jpg)

## 4. Validating Git Installation

* Open a command prompt window and run **git --version**.

![](assets/images/blog/2020-12-02_21_git_install_validation.jpg)
