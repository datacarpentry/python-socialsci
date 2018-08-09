---
title: "Reading data from a file using Pandas"
teaching: 15
exercises: 5
questions:
- "What is Pandas?"
- "How do I read files using Pandas?"
- "What is the difference between reading files using Pandas and other methods of reading files?"

objectives:
- "Explain what a module is and how they are used in Python"
- "Describe what the Python Data Analysis Library (pandas) is"
- "Load the Python Data Analysis Library (pandas)"
- "Use read_csv to read tabular data into Python"

keypoints:
- "pandas is a Python library containing functions and data structures to assist in data analysis"
- "pandas data structures are the Series (like a vector) and the Dataframe (like a table)"
- "the pandas `read_csv` function allows you to read an entire `csv` file into a Dataframe"
---

## What is Pandas?

pandas is a Python library containing a set of functions and specialised data structures that have been designed to help Python programmers to perform data analysis tasks in a structured way.

Most of the things that pandas can do can be done with basic Python, but the collected set of pandas functions and data structure makes the data analysis tasks more consistent in terms of syntax and therefore aids readabilty.

Particular features of pandas that we will be looking at over this and the next couple of episodes include:


* Reading data stored in CSV files (other file formats can be read as well)
* Slicing and subsetting data in Dataframes (tables!)
* Dealing with missing data
* Reshaping data (long -> wide,  wide -> long)
* Inserting and deleting columns from data structures
* Aggregating data using data grouping facilities using the split-apply-combine paradigm
* Joining of datasets (after they have been loaded into Dataframes)


If you are wondering why I write pandas with a lower case 'p' it is because it is the name of the package and Python is case sensitive.


## Importing the pandas library

Importing the pandas library is done in exactly the same way as for any other library. In almost all examples of Python code using the pandas library, it will have been imported and given an alias of `pd`. We will follow the same convention.


~~~
import pandas as pd
~~~
{: .language-python}

## Pandas data structures

There are two main data structure used by pandas, they are the Series and the Dataframe. The Series equates in general to a vector or a list. The Dataframe is equivalent to a table. Each column in a pandas Dataframe is a pandas Series data structure.

We will mainly be looking at the Dataframe.

We can easily create a Pandas Dataframe by reading a .csv file

## Reading a csv file

When we read a csv dataset in base Python we did so by opening the dataset, reading and processing a record at a time and then closing the dataset after we had read the last record. Reading datasets in this way is slow and places all of the responsibility for extracting individual data items of information from the records on the programmer.

The main advantage of this approach, however, is that you only have to store one dataset record in memory at a time. This means that if you have the time, you can process datasets of any size.

In Pandas, csv files are read as complete datasets. You do not have to explicitly open and close the dataset. All of the dataset records are assembled into a Dataframe. If your dataset has column headers in the first record then these can be used as the Dataframe column names. You can explicitly state this in the parameters to the call, but pandas is usually able to infer that there ia a header row and use it automatically.


We are going to read in our SN7577.tab file. Although this is a tab delimited file we will still use the pandas `read_csv` method, but we will explicitly tell the method that the separator is the tab character and not a comma which is the default.

~~~
df_SN7577 = pd.read_csv("SN7577.tab", sep='\t')
~~~
{: .language-python}

> ## Exercise
>
> What happens if you forget to specify `sep='\t'` when reading a tab delimited dataset
>
> > ## Solution
> >
> > ~~~
> > df_SN7577_oops = pd.read_csv("SN7577.tab")
> > print(df_SN7577_oops.shape)
> > print(df_SN7577_oops)
> > ~~~
> > {: .language-python}
> >
> > If you allow pandas to assume that your columns are separated by commas (the default) and there aren't any, then each record will be treated as a single column. So the shape is given as 1286 rows (correct) but only one column.
> > When the contents is display the only column name is the complete first record. Notice the `\t` used to represent the tab characters in the output. This is the same format we used to specify the tab separator when we correctly read in the file.
> >
> >
> {: .solution}
{: .challenge}

##  Getting information about a Dataframe

You can find out the type of the variable `df_SN7577` by using the `type` function.

~~~
print(type(df_SN7577))
~~~
{: .language-python}

~~~
<class 'pandas.core.frame.DataFrame'>
~~~
{: .output}

You can see the contents by simply entering the variable name. You can see from the output that it is a tabular format. The column names have been taken from the first record of the file. On the left hand side is a column with no name. The entries here have been provided by pandas and act as an index to reference the individual rows of the Dataframe.

The `read_csv()` function has an `index_col` parameter which you can use to indicate which of the columns in the file you wish to use as the index instead. As the SN7577 dataset doesn't have a column which would uniquely identify each row we cannot do that.

Another thing to notice about the display is that it is truncated. By default you will see the first and last 30 rows. For the columns you will always get the first few columns and typically the last few depending on display space.

~~~
df_SN7577
~~~
{: .language-python}

Similar information can be obtained with `df_SN7577.head()` But here you are only returned the first 5 rows by default.

~~~
df_SN7577.head()
~~~
{: .language-python}

> ## Exercise
>
> 1. As well as the `head()` method there is a `tail()` method. What do you think it does? Try it.
> 2. Both methods accept a single numeric parameter. What do you think it does? Try it.
> 
{: .challenge}

You can obtain other basic information about your Dataframe of data with:

~~~
# How many rows?
print(len(df_SN7577))
# How many rows and columns - returned as a tuple
print(df_SN7577.shape)
#How many 'cells' in the table
print(df_SN7577.size)
# What are the column names
print(df_SN7577.columns)
# what are the data types of the columns?
print(df_SN7577.dtypes)
~~~
{: .language-python}

~~~
1286
(1286, 202)
259772
Index(['Q1', 'Q2', 'Q3', 'Q4', 'Q5ai', 'Q5aii', 'Q5aiii', 'Q5aiv', 'Q5av',
       'Q5avi',
       ...
       'numhhd', 'numkid', 'numkid2', 'numkid31', 'numkid32', 'numkid33',
       'numkid34', 'numkid35', 'numkid36', 'wts'],
      dtype='object', length=202)
Q1             int64
Q2             int64
Q3             int64
...
Length: 202, dtype: object
~~~
{: .output}

> ## Exercise
>
> When we asked for the column names and their data types, the output was abridged, i.e. we didn't get the values for all of the columns. Can you write a small piece of code which will return all of the values
>
> > ## Solution
> >
> > ~~~
> > for name in df_SN7577.columns:
> >     print(name)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
