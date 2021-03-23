---
layout: post
current: post
cover:  assets/images/blog/2020-12-04_00_pip_cover.jpg
navigation: True
title: Installing a Python Library in Visual Studio Code - Windows
date: 2020-12-04 12:00:00
tags: [jupyter-notebook,vscode,python]
class: post-template
subclass: 'post'
author: jose
---

In this quick blogpost, I will share the steps that you can follow in order to install a Python library using pip through either the **Terminal** or a **Jupyter Notebook** in Visual Studio Code (VSCode) on a Windows computer.

## Pre-requisites
In order to complete the steps of this blogpost, you need to install the following in your windows computer:
* **Visual Studio Code**: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_vscode_windows).
* **Python Extension for Visual Studio Code**: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_python_extension_vscode).
* **Python Interpreter**: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_python_interpreter).

## Installing a Python Library Using the Terminal in VSCode
### 1. Accessing Visual Studio Code Terminal
* Open VSCode application

![](assets/images/blog/2020-12-04_01_pip__vscode_application.jpg)

* Go to the **Terminal** menu and select **New Terminal**.

![](assets/images/blog/2020-12-04_02_pip_new_terminal.jpg)

* A new **terminal** (PowerShell based) window is opened.

![](assets/images/blog/2020-12-04_03_pip_new_terminal_window.jpg)

### 2. Importing a Python Library
* Run ```pip --version``` to validate that pip is installed in your computer.

![](assets/images/blog/2020-12-04_04_pip_installation_validation.jpg)

* Let us say that you want to install **Pandas** Python library.
* Run ```pip install pandas```

![](assets/images/blog/2020-12-04_05_pip_install_pandas.jpg)

* Pandas library is now ready to be imported by any python application. You can repeat this process for any Python library.

## Installing a Python Library Using a Jupyter Notebook in VSCode
### 1. Creating a Jupyter Notebook in VSCode
* Create a Jupyter Notebook following the steps of [My First Jupyter Notebook on Visual Studio Code (Python kernel)](https://blog.openthreatresearch.com/first_jupyter_notebook_vscode)

![](assets/images/blog/2020-12-04_06_pip_vscode_jupyter_notebook.jpg)

### 2. Importing a Python Library
* Run ```!pip --version``` to validate that pip is installed in your computer.

![](assets/images/blog/2020-12-04_07_pip_vscode_jupyter_notebook_pip_version.jpg)

* Let us say that you want to install **Pandas** Python library.
* Run ```!pip install pandas```

![](assets/images/blog/2020-12-04_08_pip_vscode_jupyter_notebook_pip_install.jpg)

* Pandas library is now ready to be imported by any python application. You can repeat this process for any Python library.