---
layout: post
current: post
cover:  assets/images/blog/2021-01-01_00_notebook_cover.jpg
navigation: True
title: My First Jupyter Notebook on Visual Studio Code (Python kernel)
date: 2021-01-01 12:00:00
tags: [jupyter-notebook,vscode,python]
class: post-template
subclass: 'post'
author: jose
---

In this blogpost, I will share the steps that you can follow in order to generate and use a Jupyter Notebook on Visual Studio Code (VSCode). During the development of this blogpost I used a Python kernel in a Windows computer.

## 0. Pre-requisites
In order to complete the steps of this blogpost, you need to install the following in your windows computer:
* **Visual Studio Code**: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_vscode_windows).
* **Python Extension for Visual Studio Code**: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_python_extension_vscode).
* **Python Interpreter**: you can find the steps to install it [here](https://blog.openthreatresearch.com/installing_python_interpreter).

## 1. Creating a Workspace on VSCode
* The first step of this guide will be to create a folder to store all our Jupyter Notebook projects. For this blog post, I am creating the folder **Jupyter-Notebooks** in the following directory ```C:\Users\User1\Documents\Cyb3rPandaH```.

![](assets/images/blog/2021-01-01_01_notebook_project_directory.jpg)

* Open the **VSCode** application.

![](assets/images/blog/2021-01-01_02_notebook_vscode_application.jpg)

* Go to the **File** menu and select **Open Folder**.

![](assets/images/blog/2021-01-01_03_notebook_open_folder.jpg)

* Search for the **Jupyter-Notebooks** that we created previously and click on **Select Folder**.

![](assets/images/blog/2021-01-01_04_notebook_select_folder.jpg)

* By opening the Jupyter-Notebook folder, it becomes your **workspace** within Visual Studio Code. We are now ready to create our first Jupyter Notebook file.

![](assets/images/blog/2021-01-01_05_notebook_vscode_workspace.jpg)

## 2. Creating a Jupyter Notebook
* Click on the **New File** icon, next to the name of our workspace.

![](assets/images/blog/2021-01-01_06_notebook_new_file.jpg)

* Type a name for your file using the **ipynb** extension and press enter.
* By using this extension, we are telling VSCode to process this file as a **Jupyter Notebook** file.

![](assets/images/blog/2021-01-01_07_notebook_extension.jpg)

* After hitting the **Enter** key, you will see a message at the bottom left of your screen that says **Python Extension Loading**. 

![](assets/images/blog/2021-01-01_08_notebook_python_extension_loading.jpg)

* In additon, because this is your first time creating a Jupyter Notebook using VSCode, you will get an additional message that says **Python requires ipykernel to be installed**. This message is telling us that we need to install a Kernel.
* A **kernel** is a process that runs interactive code in a particular language such as Python or R and return output to us.
* In this example we are using **Python** language, therefore you need to install the **ipykernel** Python kernel for VSCode. Click on **Install**.

![](assets/images/blog/2021-01-01_09_notebook_ipykernel.jpg)

* VSCode will open a terminal and install the **ipykernel** component for you.
* Wait for the installation process to complete.

![](assets/images/blog/2021-01-01_10_notebook_ipykernel_installation.jpg)

* After the installation process is completed successfully, VSCode will connect your **Jupyter Notebook** to the **ipykernel** Python kernel.

![](assets/images/blog/2021-01-01_11_notebook_ipykernel_successful_installation.jpg)

* You can close the VSCode terminal window by clicking on the **Close Panel** button that is located at the top right of the terminal window.
* The installation process for the **ipykernel** Python kernel will not be required when creating future Jupyter Notebooks because we just installed it in VSCode.

![](assets/images/blog/2021-01-01_12_notebook_close_terminal.jpg)

* Awesome!! VSCode has created a **Jupyter Notebook server** locally in your computer. We now have our first Jupyter Notebook file created with the Python extension loaded and connected to a Python Kernel.

![](assets/images/blog/2021-01-01_13_notebook_ready_to_start.jpg)

## 3. Basic Interaction With Jupyter Notebooks
### a. Inserting and Deleting Cells
* You can add a new cell by clicking on the **Insert Cell Bellow** ➕ (At the bottom left of each cell) button or by hitting the **B** key one time after selecting a cell.

![](assets/images/blog/2021-01-01_14_notebook_add_cell.jpg)

* You can delete a cell by clicking on the **Delete Cell** button 🗑️ (At the right of each cell) or by hitting the **D** key two times after selecting a cell.

![](assets/images/blog/2021-01-01_15_notebook_delete_cell.jpg)

### b. Switch Cell Content Type
* By default, the content type of a cell is **code**. You can change the content type to **narrative text** by clicking on the **Change to Markdown** button **M**⬇️ (At the top left of each cell).

![](assets/images/blog/2021-01-01_16_notebook_to_markdown.jpg)

```
# This is my first Jupyter Notebook
Thank you!
```

![](assets/images/blog/2021-01-01_17_notebook_to_markdown_example.jpg)

* You can change the content type of a cell back to **code** by clicking on the **Change to Code** buttom **{ }** (At the top left of each cell).

![](assets/images/blog/2021-01-01_18_notebook_to_code_example.jpg)

### c. Running Markdown and Code Cells
* After adding content to a **Markdown** cell, you can get the output of this type of cell just by clicking on anywhere outside the cell or by hitting **Shift + Enter** keys combination (Using the second option will also add a new cell below).

![](assets/images/blog/2021-01-01_19_notebook_markdown_cell_run_example.jpg)

* After adding content to a **Code** cell, you can get the output of this type of cell just by clicking on the **Run Cell** button ▶️ (At the top left of each cell) or by hitting **Shift + Enter** keys combination (Using either both options will add a new cell below).

```
# Importing Python library
import pandas as pd

# Initializing a Dictionary
data = {'User': ['Cyb3rPandaH','Cyb3rCuy'], 'Country': ['Peru','United Sttes']}

# Creating a Pandas Dataframe
pd.DataFrame.from_dict(data)
```

![](assets/images/blog/2021-01-01_20_notebook_code_cell_run.jpg)

![](assets/images/blog/2021-01-01_22_notebook_code_cell_run_example.jpg)

## 4. Importing Non-Installed Python Libraries
When trying to import a non-installed Python library, we will get an error similar to the one showed in the image below.

```
# Importing Python library
import requests
```

![](assets/images/blog/2021-01-01_23_notebook_code_cell_run_importing_library_error.jpg)

You can find all the steps required to install a Python library using **pip** in VSCode [here](https://blog.openthreatresearch.com/installing_python_library_vscode). After completing these steps, you can try and run the code within the cell again.


## References
* https://code.visualstudio.com/docs/python/jupyter-support
* https://blog.openthreatresearch.com/installing_vscode_windows
* https://blog.openthreatresearch.com/installing_python_extension_vscode
* https://blog.openthreatresearch.com/installing_python_interpreter
* https://blog.openthreatresearch.com/installing_python_library_vscode


