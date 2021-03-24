---
layout: post
current: post
cover:  assets/images/blog/2021-01-01_00_mordor_json_file_imported_cover.jpg
navigation: True
title: Importing a Mordor Dataset with Jupyter Notebooks on Visual Studio Code (Python kernel)
date: 2021-01-01 12:00:00
tags: [mordor,jupyter-notebook,vscode,python]
class: post-template
subclass: 'post'
author: jose
---

In this blogpost, I will share the steps that you can follow in order to import a Mordor dataset to your workspace on Visual Studio Code (VSCode) using a Jupyter Notebook. During the development of this blogpost I used a Python kernel in a Windows computer.

## Pre-requisites
In order to complete the steps of this blogpost, you need to install the following in your windows computer:
* `Visual Studio Code`: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_vscode_windows).
* `Python Extension for Visual Studio Code`: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_python_extension_vscode).
* `Python Interpreter`: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_python_interpreter).

## Importing a Mordor Dataset to my VSCode Workspace
### 1) Creating a Jupyter Notebook in VSCode
* Create a Jupyter Notebook following the steps described on [My First Jupyter Notebook on Visual Studio Code (Python kernel)](https://blog.openthreatresearch.com/first_jupyter_notebook_vscode).

![](assets/images/blog/2021-01-01_01_mordor_new_notebook.jpg)

### 2) Importing Python Liraries
* For the purpose of this blog, we are using the following libraries: `requests`, `io`, and `zipfile`.
* If you have not installed any of this libraries, you can install them using a Jupyter Notebook or the terminal on VSCode. The steps to install a Python library either through a Jupyter Notebook or the terminal in VSCode are described [here](https://blog.openthreatresearch.com/installing_python_library_vscode).
```
import requests
from io import BytesIO
from zipfile import ZipFile
```

![](assets/images/blog/2021-01-01_02_mordor_importing libraries.jpg)

### 3) Getting Mordor Json File
* Let us pick [Exchange ProxyLogon SSRF RCE Vuln POC](https://mordordatasets.com/notebooks/small/windows/02_execution/SDWIN-210314014019.html) Mordor dataset as the example for this blogpost.
* Within the metadata provided for this dataset, we can find a url that links the GitHub raw file. We are assingning this link to the variable `url`.
* We are using the `get` method from the `requests` library, so we can get the content of the raw file.
* We are creating a `ZipFile` object after processing the content of our request. The type of content that we received is `Bytes`.
* We are using the `extract` method from the `zipFile` library, so we can get the `json` file that contains multiple security event logs.
* We are using the `namelist` method from the `zipFile` library, so we can get the name of the file extracted. The name also includes the path or directory where the `json` file is sotred. We are assigning the name of the file to the variable `jsonFilePath`.

```
url = 'https://raw.githubusercontent.com/OTRF/mordor/master/datasets/small/windows/persistence/host/proxylogon_ssrf_rce_poc.zip'
zipFileRequest = requests.get(url)
zipFile = ZipFile(BytesIO(zipFileRequest.content))
jsonFilePath = zipFile.extract(zipFile.namelist()[0])
jsonFilePath
```

* There will be two outputs after running the code of this cell.
* At the bottom of the cell, We are getting the complete path or directory where the `json` file was stored.
* On the left side of your screen, you will see a new file with the `.json` extension. This is the mordor dataset that is stored in our VSCode worspace.

![](assets/images/blog/2021-01-01_03_mordor_json_file_imported.jpg)

## References
* https://mordordatasets.com/introduction.html
* https://mordordatasets.com/notebooks/small/windows/02_execution/SDWIN-210314014019.html
* https://requests.readthedocs.io/en/master/
* https://docs.python.org/3/library/io.html
* https://docs.python.org/3/library/zipfile.html