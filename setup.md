---
layout: page
title: Setup
---

## Setup instructions for Python

In order to complete the materials for the Python lesson, you will need Python to be installed on your machine. As many of the examples and exercises use Jupyter notebooks, you will need it to be installed as well.

The [Anaconda](https://www.anaconda.com/download/) distribution of Python will allow you to install both Python and Jupyter notebooks as a single install. Anaconda will also install many other commonly used Python packages.

## How to install the Anaconda distribution of python

1.	Follow the Anaconda link above to the Anaconda website. There are versions of Anaconda available for Windows, macOS, and Linux. The website will detect your operating system and provide a link to the appropriate download.
2.	There will be two options, one for Python 2.x and another for Python 3.x. We will take the Python 3.x option. Python 2.x will eventually be phased out but is still provided for backward compatibility with some older optional Python modules. The majority of popular modules have been converted to work with Python 3.x. The actual value of x will vary depending on when you download. At the time of writing I am being offered Python 3.6 or Python 2.7.
3.	For Windows and Linux there is the option of either a 64 bit (default) download or a 32 bit download. Unless you know that you have an old 32 bit pc you should choose the 64 bit installer.
4.	Run the downloaded installer program. Accept the default settings until you are given the option to add Anaconda to your environmental Path variable. Despite the recommendation not to and the subsequent warning, you should select this option. This will make it easier later on to start Jupyter notebooks from any location.
5.	The installation can take a few minutes. When finished you should be able to open a cmd prompt (Type cmd from Windows start and into the cmd window type python. You should get a display similar to that below.  
![Python Install](/fig/Python_install_1.png)
6.	The `>>>` prompt tells you that you are in the Python environment. You can exit Python with the `exit()` command.  



## Running Jupyter Notebooks in Windows

1. From file explorer navigate to where you can select the folder which contains your Jupyter Notebook notebooks (it can be empty initially).
2. Hold down the `shift` key and right-click the mouse
3.	The pop-up menu items will include an option to start a cmd window or in the latest Windows release start a ‘PowerShell’ window. Select whichever appears.
4.	When the window opens, type the command `jupyter notebook`.
5.	Several messages will appear in the command window. In addition your default web browser will open and display the Jupyter notebook home page. The main part of this is a file browser window starting at the folder you selected in step 1.
6.	There may be existing notebooks which you can select and open in a new tab in your browser or there is a menu option to create a new notebook.
![Python Install](/fig/Python_install_2.png)
7.	The Jupyter package creates a small web services and opens your browser pointing at it. If your browser does not open, you can open it manually and specify ‘localhost:8888’ as the URL.
8.	Port 8888 is the default port used by the Jupyter web service, but if it is already in use it will increment the port number automatically. Either way the port number it does use is given in a message in the cmd/powershell window.
9.	Once running, the cmd/powershell window will display additional messages, e.g. about saving notebooks, but there is no need to interact with it directly. The window can be minimized and ignored.
10.	To shut Jupyter down, select the cmd/powershell window and type <kbd>Ctrl</kbd>+<kbd>c</kbd> twice and then close the window.
