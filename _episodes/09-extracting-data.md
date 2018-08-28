---
title: 'Extracting row and columns'
teaching: 15
exercises: 15
questions:
- 'How can I extract specific rows and columns from a Dataframe?'
- 'How can I add or delete columns from a Dataframe?'
- 'How can I find and change missing values in a Dataframe?'
objectives:
- 'Define indexing as it relates to data structures'
- 'Select specific columns from a data frame'
- 'Select specific rows from a data frame based on conditional expressions'
- 'Using indexes to access rows and columns'
- 'Copy a data frame'
- 'Add columns to a data frame'
- 'Analyse datasets having missing/null values'
keypoints:
- 'First key point.'

---

We will continue this episode from where we left off in the last episode. If you have restarted Jupyter or you want to use a new notebook make sure that you import pandas and have read the SN7577.tab dataset into a Dataframe.

~~~
import pandas as pd
df_SN7577 = pd.read_csv("SN7577.tab", sep='\t')
~~~
{: .language-python}

### Selecting rows and columns from a pandas Dataframe

If we know which columns we want before we read the data from the file we can tell `read_csv()` to only import those columns by specifying columns either by their index number (starting at 0) as a list to the `usecols` parameter. Alternatively we can also provide a list of column names.  

~~~
df_SN7577_some_cols = pd.read_csv("SN7577.tab", sep='\t', usecols= [0,1,2,173,174,175])
print(df_SN7577_some_cols.shape)
print(df_SN7577_some_cols.columns)
df_SN7577_some_cols = pd.read_csv("SN7577.tab", sep='\t', usecols= ['Q1', 'Q2', 'Q3', 'sex', 'age', 'agegroups'])
print(df_SN7577_some_cols.columns)
~~~
{: .language-python}

~~~
(1286, 6)
Index(['Q1', 'Q2', 'Q3', 'sex', 'age', 'agegroups'], dtype='object')
Index(['Q1', 'Q2', 'Q3', 'sex', 'age', 'agegroups'], dtype='object')
~~~
{: .output}

Let us assume for now that we read in the complete file which is now in the Dataframe `df_SN7577`, how can we now refer to specific columns?

There are two ways of doing this using the column names (or labels):

~~~
# Both of these statements are the same
print(df_SN7577['Q1'])
# and
print(df_SN7577.Q1)
~~~
{: .language-python}

~~~
0        1
1        3
2       10
3        9
...
~~~
{: .output}

If we are interested in more than one column, the 2nd method above cannot be used. However in the first, although we used a string with the value of `'Q1'` we could also have provided a list of strings. Remember that lists are enclosed in `[]`.

~~~
print(df_SN7577[['Q1', 'Q2', 'Q3']])
~~~
{: .language-python}

~~~
Q1  Q2  Q3
0      1  -1   1
1      3  -1   1
2     10   3   2
3      9  -1  10
...
~~~
{: .language-python}
> ## Exercise  
>
> What happens if you:
>
> 1. List the columns you want out of order from the way they appear in the file?
> 2. Put the same column name in twice?
> 3. Put in a non-existing column name? (a.k.a Typo)
>
> > ## Solution
> >
> > ~~~
> > print(df_SN7577[['Q3', 'Q2']])
> > print(df_SN7577[['Q3', 'Q2', 'Q3']])
> > print(df_SN7577[['Q33', 'Q2']])
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Filtering by Rows

You can filter the Dataframe by rows by specifying a range in the form of `a:b`. `a` is the first row and `b` is one beyond the last row required.

~~~
# select row with index of 1, 2 and 3 (rows 2, 3 and 4 in the Dataframe)
df_SN7577_some_rows = df_SN7577[1:4]
df_SN7577_some_rows
~~~
{: .language-python}

> ## Exercise
>
> What happens if we ask for a single row instead of a range?
>
> > ## Solution
> >
> > ~~~
> > df_SN7577[1]
> > ~~~
> > {: .language-python}
> >
> > You get an error if you only specify `1`. You need to use `:1` or `0:1` to get the first row returned. The `:` is always required. You can use `:` by itself to return all of the rows.
> >
> {: .solution}
{: .challenge}


## Using criteria to filter rows

It is more likely that you will want to select rows from the Dataframe based on some criteria, such as "all rows where the value for Q2 is -1".


~~~
df_SN7577_some_rows = df_SN7577[(df_SN7577.Q2 == -1)]
df_SN7577_some_rows
~~~
{: .language-python}

The criteria can be more complex and isn't limited to a single column's values:

~~~
df_SN7577_some_rows = df_SN7577[ (df_SN7577.Q2 == -1) & (df_SN7577.numage > 60)]
df_SN7577_some_rows
~~~
{: .language-python}

We can combine the row selection with column selection:

~~~
df_SN7577_some_rows = df_SN7577[ (df_SN7577.Q2 == -1) & (df_SN7577.numage > 60)][['Q1', 'Q2','numage']]
df_SN7577_some_rows
~~~
{: .language-python}

Selecting rows on the row index is of limited use unless you need to select a contiguous range of rows.

There is however another way of selecting rows using the row index:

~~~
df_SN7577_some_rows = df_SN7577.iloc[1:4]
df_SN7577_some_rows
~~~
{: .language-python}

Using the `iloc` method gives the same results as our previous example.

However, now we can specify a single value and more importantly we can use the `range()` function to indicate the records that we want. This can be useful for making pseudo-random selections of rows from across the Dataframe.

~~~
# Select the first row from the Dataframe
df_SN7577_some_rows = df_SN7577.iloc[0]
df_SN7577_some_rows
# select every 100th record from the Dataframe.
df_SN7577_some_rows = df_SN7577.iloc[range(0, len(df_SN7577), 100)]
df_SN7577_some_rows
~~~
{: .language-python}

You can also specify column ranges using the `iloc` method again using the column index numbers:

~~~
# columns 0,1,2 and 3
df_SN7577_some_rows = df_SN7577.iloc[range(0, len(df_SN7577), 100),0:4]
df_SN7577_some_rows
# columns 0,1,2,78 and 95
df_SN7577_some_rows = df_SN7577.iloc[range(0, len(df_SN7577), 100),[0,1,2,78,95]]
df_SN7577_some_rows
~~~
{: .language-python}

There is also a `loc` method which allows you to use the column names.

~~~
# columns 0,1,2,78 and 95 using the column names and changing 'iloc' to 'loc'
df_SN7577_some_rows = df_SN7577.loc[range(0, len(df_SN7577), 100),['Q1', 'Q2', 'Q3', 'Q18bii', 'access6' ]]
df_SN7577_some_rows
~~~
{: .language-python}

## Sampling

Pandas does have a `sample` method which allows you to extract a sample of the records from the Dataframe.

~~~
df_SN7577.sample(10, replace=False)             # ten records, do not select same record twice (this is the default)
df_SN7577.sample(frac=0.05, random_state=1)     # 5% of records , same records if run again
~~~
{: .language-python}
