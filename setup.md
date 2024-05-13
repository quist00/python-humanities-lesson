---
title: Setup
permalink: /setup/
---

::::::::::::::::::::::::::::::::::::::::::  prereq

## Data

The sample data used in this lesson is from the fiction holdings of an academic library along with available information about the ethnicity and gender identity for available years.

You can download these [example files][dataset] as a compressed zip file. You should save it to a memorable location, and then unzip the data within. If you are not already comfortable with file paththing, we recommend you store the data folder on your desktop to closely match the examples provided. On the other hand, if you are already comfortable with pathing file pathing, you are free to store it at your discretion.
    

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::  prereq

## Software

[Python](https://python.org) is a popular language for scientific computing, and great for
general-purpose programming as well. Installing all of the scientific packages we use in this lesson
individually can be a bit cumbersome, and therefore we recommend the all-in-one
installer [anaconda](https://www.anaconda.com/).

Regardless of how you choose to install it, please make sure you get a recent version of Python 3.  If you are not using the latest version, consult [Status of Python versions
](https://devguide.python.org/versions/) to ensure your version is still getting security updates.



::::::::::::::::::::::::::::::::::::::::::::::::::

## Install Anaconda or Miniconda & Required Packages

### Anaconda installation

Anaconda will install the workshop packages for you. 
Download and install [Anaconda](https://www.anaconda.com/distribution/).
Remember to download and install the installer for Python 3.x.

### Miniconda installation

Skip this step if you used the Anaconda installer recommended above. Miniconda is a "light" version of Anaconda. If you install and use Miniconda you will also need to install the  packages for the workshop listed below for informational purposed.  The actual commands to install them are further down.

#### Required Python Packages for this workshop

- [Pandas](https://pandas.pydata.org/)
- [Jupyter Lab](https://jupyter.org/)
- [Numpy](https://www.numpy.org/)
- [Matplotlib](https://matplotlib.org/)
- [Bokeh](https://bokeh.org/)

#### Download and install Miniconda

Download and install [Miniconda](https://conda.pydata.org/miniconda.html)
following the instructions. Remember to download and run the installer for the
Python 3 version.

#### Check the installation of Miniconda

From the terminal, type:

```
conda list
```

### Install the required workshop packages with conda

From the terminal, type:

```
conda install -y numpy pandas matplotlib
conda install -y -c conda-forge bokeh juptyerlab
```

## Launch a JupyterLab

After installing either Anaconda or Miniconda and the workshop packages,
launch a JupyterLab by typing this command from the terminal:

```
jupyter lab
```

The notebook should open automatically in your browser. If it does not or you
wish to use a different browser, open this link: [http://localhost:8888](https://localhost:8888).

***

## Overview of the Jupyter lab (Optional)

### How the Jupyter notebook works

After typing the command `jupyter lab`, the following happens:

- A JupyterLab server is automatically created on your local machine.

- The JupyterLab server runs locally on your machine only and does not
  use an internet connection.

- The JupyterLab server opens the JupyterLab client, also known
  as the notebook user interface, in your default web browser.

- The JupyterLab server will be dependent on the terminal window from which it was launched. Leave it open for as long as you want to use JupyterLab, as closing the window will terminate the server and lead to errors in the browser client. Information will be logged to the terminal window as you use JupyterLab.  This is expected behavior and can be ignored under normal circumstances.

- When you can create a new notebook and type code into the browser, the web
  browser and the JupyterLab server communicate with each other.
  

- The Jupyter Notebook server does the work and calculations, and the web
  browser renders the notebook.


The JupyterLab interface has several advantages:

- You can easily type, edit, and copy and paste blocks of code.
- Tab completion allows you to easily access the names of things you are using
  and learn more about them.
- It allows you to annotate your code with links, different sized text,
  bullets, etc. to make information more accessible to you and your
  collaborators.
- It allows you to display figures next to the code that produces them
  to tell a complete story of the analysis.


### How the notebook is stored

- The notebook file is stored in a format called JSON and has the suffix
  `.ipynb`.
- Just like HTML for a webpage, what's saved in a notebook file looks
  different from what you see in your browser.
- But this format allows Jupyter to mix software (in several languages) with
  documentation and graphics, all in one file.

### Notebook modes: Control and Edit

The notebook has two modes of operation: Control and Edit. Control mode lets
you edit notebook level features; while, Edit mode lets you change the
contents of a notebook cell. Remember a notebook is made up of a number of
cells which can contain code, markdown, html, visualizations, and more.

### Help and more information

Use the **Help** menu and its options when needed.


[dataset]: episodes/files/data.zip 