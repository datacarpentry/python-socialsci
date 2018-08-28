---
layout: page
title: "Instructor Notes"
permalink: /guide/
---

## Setup

The setup instruction for installing the Anaconda distribution of Python is in the [setup](../setup.html) file.
The Anaconda distribution contains all of the 3rd party libraries used.

PIP is referred to in the text but it shouldn't need to be used.

It is assumed that Jupyter notebooks will be used for all of the coding. (The shell is used in explaining REPL)

How to start Jupyter is included in the setup instructions.


## The datasets used

All of the datasets used have been placed in the data folder.

They should be downloaded to the local machine before use.

The code examples are written on the assumption that the data files are in the same folder as the notebook.



## The Lessons

It is assumed that all of the work will be done in Jupyter notebooks. There are no specific instructions when to start a notebook, at each new episode would be appropriate.

If a new notebook is created or a new Jupyter session is started. Any mudules that need to be used will have to be imported again.

Each section of code within an episode will not necessarily re import modules that are needed for that section.

[Introduction to Python](link)

An introduction to the benefits of the Python language.

Comparison between interpreted and compiled languages.

An explanation and example of using REPL.

An Introduction to using Jupyter notebooks.



[Python basics ](link)

Using Jupyter - creating new cells, hiding output, changing cell type.

Python variables and data types.

Using Print and builtin functions.

Python Help  and Internet help for Python.

Strings and String functions.

Lists and range function (Dictionaries kept for later, when needed).

[Python control structures](link)

Explanation of typical program structure and the need for control structures.

Explanation and examples of the `if`, `while` and `for` constructs.

[Creating re-usable code](link)

Explanation of functions and why we use them.

Creating functions.

Using parameters in functions.

Python libraries and how to install and use them.

[Processing data from a file](link)

This episode starts to use some of the control structures and files to create complete small programs with
proper 'Input - Processing - Output' structure.

Different approaches to reading files ie. one record at a time  v reading the whole file.

Opening and closing files and file handles.

Description of a csv file being a list of strings.

Iterating over complete files.

Use of the `split` function to parse a line from a csv file and extracting specific elements.

Writing extracted information to a file.

The Python dictionary explanation and examples.

Creating and populating a dictionary on the fly from data read from a file.

[Date and Time](link)

Need to import the datetime module.

Explanation of format strings to provide a representation of the dates and times.

Need to convert date/time strings to a datetime structure before using them

The ability to extract individual components from  and do arithmetic with datetime structures

[Processing JSON data](link)

This episode is programatically the most complex. However the examples are built up as gently as possible.
The solution to the 2nd exercise needs to be understood to make it worthwhile continuing with the coding.

The episode starts with further details of the Dictionary object and how it can be used to represent nested data structures.

The JSON data format is described and compared to a Python dictionary.

An example of using the json module to read a file in JSON format is explained.

How to make the printed JSON more presentable using `json.dumops` parameters is demonstrated.

Systematically  extracting single items from a dictionary is covered followed by extracting all such entries
from a file of JSON.

The overall aim is to demonstrate extracting a set of specific fields from a JSON formatted file and writing them to a flat structured csv file making subsequent processing more straight forward.


[Pandas](link)

This episode introduces the pandas module and explains the advantages of using it for data analysis.

The two key pandas data structures are introduced.

Examples of reading a csv file into a dataframe, optionally selecting only specific columns.

Various methods for extract information about the dataframe are covered.


[Extracting rows and columns](link)

Basic pandas methods for extracting row and columns from a dataframe are covered.

For row selection the emphasis is on specifying criteria, although random selection is also covered.

[Aggregations_and_missing_data](link)

Explain why we want to summarise data.

Introduce the basic aggregation methods for a dataframe and individual columns.

The examples of the aggregations will show up 'NaN' values. Missing data and its representations are discussed.

The effects of missing data in summarisation is discussed.

Ways of recognising and dealing with missing data are discussed and demonstrated.

Summarising categorical variables using the `groupby` method is discussed and demonstrated.

[Joins](link)

Explain why we want to join dataframes and the necessary conditions for doing so.

Simple concatenation of rows using `concat` and the effect of the columns not being the same.

The downside of using `concat` to join by columns.

The use of `merge` and its similarity with the SQL JOIN clause.

The different types of joins available with `merge`

Discussion and examples of what different join types tell you about the data.

[Long_and_wide_data_formats](link)

The difference between wide and long formats

Use and examples of `melt` and `pivot` methods to convert between them.

Because the SN7577 dataset has no 'key' column, the setup for the examples is more complex, but it makes use of previously used techniques as well as introducing different ways of selecting columns from a dataframe.

The final exercise is quite long, but brings together the concepts learned in this and the previous two episodes.

[Data visualisation using Matplotlib](link)

The basic aim of this episode is to illustrate how simple graphs can be produced using matplotlib.

This episode uses entirely random data produced by numpy methods

Four commonly used graph types are demonstrated; Bar charts, Histograms, Scatter plots and Boxplots.

The tight integration between pandas and matplotlib is discussed and illustrated.

How graphs can be built up from individual components is covered.

Saving the produced graph to a file is explained and demonstrated.


[Accessing SQLite Databases](link)

A comparison between an relational database table and a pandas dataframe is made.

The possible advantages of using data from a database (SQLite in this case) rather than having it in a dataframe is explained.

The construction of an SQLite database as a single file is covered

The sqlite3 module is introduced and connection strings are explained.

The use of the connection and the cursor are explained.

The `execute` method is used to run an SQL query on the database system is demonstrated.

Various ways of retrieving the results of the query are covered.

Pandas is used to run a similar query, its greater simplicity is explained.

The problem with running DML statements with pandas is illustrated in the final exercise.
