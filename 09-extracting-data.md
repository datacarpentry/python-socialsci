---
title: Extracting row and columns
teaching: 15
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objectives

- Define indexing as it relates to data structures
- Select specific columns from a data frame
- Select specific rows from a data frame based on conditional expressions
- Using indexes to access rows and columns
- Copy a data frame
- Add columns to a data frame
- Analyse datasets having missing/null values

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I extract specific rows and columns from a Dataframe?
- How can I add or delete columns from a Dataframe?
- How can I find and change missing values in a Dataframe?

::::::::::::::::::::::::::::::::::::::::::::::::::

We will continue this episode from where we left off in the last episode. If you have restarted Jupyter or you want to use a new notebook make sure that you import pandas and have read the SN7577.tab dataset into a Dataframe.

```python
import pandas as pd
df_SN7577 = pd.read_csv("SN7577.tab", sep='\t')
```

### Selecting rows and columns from a pandas Dataframe

If we know which columns we want before we read the data from the file we can tell `read_csv()` to only import those columns by specifying columns either by their index number (starting at 0) as a list to the `usecols` parameter. Alternatively we can also provide a list of column names.

```python
df_SN7577_some_cols = pd.read_csv("SN7577.tab", sep='\t', usecols= [0,1,2,173,174,175])
print(df_SN7577_some_cols.shape)
print(df_SN7577_some_cols.columns)
df_SN7577_some_cols = pd.read_csv("SN7577.tab", sep='\t', usecols= ['Q1', 'Q2', 'Q3', 'sex', 'age', 'agegroups'])
print(df_SN7577_some_cols.columns)
```

```output
(1286, 6)
Index(['Q1', 'Q2', 'Q3', 'sex', 'age', 'agegroups'], dtype='object')
Index(['Q1', 'Q2', 'Q3', 'sex', 'age', 'agegroups'], dtype='object')
```

Let us assume for now that we read in the complete file which is now in the Dataframe `df_SN7577`, how can we now refer to specific columns?

There are two ways of doing this using the column names (or labels):

```python
# Both of these statements are the same
print(df_SN7577['Q1'])
# and
print(df_SN7577.Q1)
```

```output
0        1
1        3
2       10
3        9
...
```

If we are interested in more than one column, the 2nd method above cannot be used. However in the first, although we used a string with the value of `'Q1'` we could also have provided a list of strings. Remember that lists are enclosed in `[]`.

```python
print(df_SN7577[['Q1', 'Q2', 'Q3']])
```

```python
Q1  Q2  Q3
0      1  -1   1
1      3  -1   1
2     10   3   2
3      9  -1  10
...
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

What happens if you:

1. List the columns you want out of order from the way they appear in the file?
2. Put the same column name in twice?
3. Put in a non-existing column name? (a.k.a Typo)

:::::::::::::::  solution

## Solution

```python
print(df_SN7577[['Q3', 'Q2']])
print(df_SN7577[['Q3', 'Q2', 'Q3']])
print(df_SN7577[['Q33', 'Q2']])
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Filtering by Rows

You can filter the Dataframe by rows by specifying a range in the form of `a:b`. `a` is the first row and `b` is one beyond the last row required.

```python
# select row with index of 1, 2 and 3 (rows 2, 3 and 4 in the Dataframe)
df_SN7577_some_rows = df_SN7577[1:4]
df_SN7577_some_rows
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

What happens if we ask for a single row instead of a range?

:::::::::::::::  solution

## Solution

```python
df_SN7577[1]
```

You get an error if you only specify `1`. You need to use `:1` or `0:1` to get the first row returned. The `:` is always required. You can use `:` by itself to return all of the rows.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Using criteria to filter rows

It is more likely that you will want to select rows from the Dataframe based on some criteria, such as "all rows where the value for Q2 is -1".

```python
df_SN7577_some_rows = df_SN7577[(df_SN7577.Q2 == -1)]
df_SN7577_some_rows
```

The criteria can be more complex and isn't limited to a single column's values:

```python
df_SN7577_some_rows = df_SN7577[ (df_SN7577.Q2 == -1) & (df_SN7577.numage > 60)]
df_SN7577_some_rows
```

We can combine the row selection with column selection:

```python
df_SN7577_some_rows = df_SN7577[ (df_SN7577.Q2 == -1) & (df_SN7577.numage > 60)][['Q1', 'Q2','numage']]
df_SN7577_some_rows
```

Selecting rows on the row index is of limited use unless you need to select a contiguous range of rows.

There is however another way of selecting rows using the row index:

```python
df_SN7577_some_rows = df_SN7577.iloc[1:4]
df_SN7577_some_rows
```

Using the `iloc` method gives the same results as our previous example.

However, now we can specify a single value and more importantly we can use the `range()` function to indicate the records that we want. This can be useful for making pseudo-random selections of rows from across the Dataframe.

```python
# Select the first row from the Dataframe
df_SN7577_some_rows = df_SN7577.iloc[0]
df_SN7577_some_rows
# select every 100th record from the Dataframe.
df_SN7577_some_rows = df_SN7577.iloc[range(0, len(df_SN7577), 100)]
df_SN7577_some_rows
```

You can also specify column ranges using the `iloc` method again using the column index numbers:

```python
# columns 0,1,2 and 3
df_SN7577_some_rows = df_SN7577.iloc[range(0, len(df_SN7577), 100),0:4]
df_SN7577_some_rows
# columns 0,1,2,78 and 95
df_SN7577_some_rows = df_SN7577.iloc[range(0, len(df_SN7577), 100),[0,1,2,78,95]]
df_SN7577_some_rows
```

There is also a `loc` method which allows you to use the column names.

```python
# columns 0,1,2,78 and 95 using the column names and changing 'iloc' to 'loc'
df_SN7577_some_rows = df_SN7577.loc[range(0, len(df_SN7577), 100),['Q1', 'Q2', 'Q3', 'Q18bii', 'access6' ]]
df_SN7577_some_rows
```

## Sampling

Pandas does have a `sample` method which allows you to extract a sample of the records from the Dataframe.

```python
df_SN7577.sample(10, replace=False)             # ten records, do not select same record twice (this is the default)
df_SN7577.sample(frac=0.05, random_state=1)     # 5% of records , same records if run again
```

:::::::::::::::::::::::::::::::::::::::: keypoints

- Import specific columns when reading in a .csv with the `usecols` parameter
- We easily can chain boolean conditions when filtering rows of a pandas dataframe
- The `loc` and `iloc` methods allow us to get rows with particular labels and at particular integer locations respectively
- pandas has a handy `sample` method which allows us to extract a sample of rows from a dataframe

::::::::::::::::::::::::::::::::::::::::::::::::::


