---
title: Joining Pandas Dataframes
teaching: 25
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand why we would want to join Dataframes
- Know what is needed for a join to be possible
- Understand the different types of joins
- Understand what the joined results tell us about our data

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I join two Dataframes with a common key?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Joining Dataframes

### Why do we want to do this

There are many occasions when we have related data spread across multiple files.

The data can be related to each other in different ways. How they are related and how completely we can join the data
from the datasets will vary.

In this episode we will consider different scenarios and show we might join the data. We will use csv files and in all
cases the first step will be to read the datasets into a pandas Dataframe from where we will do the joining. The csv
files we are using are cut down versions of the SN7577 dataset to make the displays more manageable.

First, let's download the datafiles. They are listed in the [setup page][setup-page] for the lesson. Alternatively,
you can download the [GitHub repository for this lesson][gh-repo]. The data files are in the
*data* directory. If you're using Jupyter, make sure to place these files in the same directory where your notebook
file is.

### Scenario 1 - Two data sets containing the same columns but different rows of data

Here we want to add the rows from one Dataframe to the rows of the other Dataframe.
In order to do this we can use the `pd.concat()` function.

```python
import pandas as pd

df_SN7577i_a = pd.read_csv("SN7577i_a.csv")
df_SN7577i_b = pd.read_csv("SN7577i_b.csv")
```

Have a quick look at what these Dataframes look like with

```python
print(df_SN7577i_a)
print(df_SN7577i_b)
```

```
  Id  Q1  Q2  Q3  Q4
0   1   1  -1   1   8
1   2   3  -1   1   4
2   3  10   3   2   6
3   4   9  -1  10  10
...

  Id  Q1  Q2  Q3  Q4
0  1277  10  10   4   6
1  1278   2  -1   5   4
2  1279   2  -1   4   5
3  1280   1  -1   2   3
...
```

{: output}

The `concat()` function appends the rows from the two Dataframes to create the df\_all\_rows Dataframe. When you list this out you can see that all of the data rows are there, however, there is a problem with the `index`.

```python
df_all_rows = pd.concat([df_SN7577i_a, df_SN7577i_b])
df_all_rows
```

We didn't explicitly set an index for any of the Dataframes we have used. For `df_SN7577i_a` and `df_SN7577i_b` default
indexes would have been created by pandas. When we concatenated the Dataframes the indexes were also concatenated resulting in duplicate entries.

This is really only a problem if you need to access a row by its index. We can fix the problem with the following code.

```python
df_all_rows=df_all_rows.reset_index(drop=True)

# or, alternatively, there's the `ignore_index` option in the `pd.concat()` function:
df_all_rows = pd.concat([df_SN7577i_a, df_SN7577i_b], ignore_index=True)

df_all_rows
```

What if the columns in the Dataframes are not the same?

```python
df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
df_all_rows = pd.concat([df_SN7577i_aa, df_SN7577i_bb])
df_all_rows
```

In this case `df_SN7577i_aa` has no Q4 column and `df_SN7577i_bb` has no `Q3` column. When they are concatenated, the
resulting Dataframe has a column for `Q3` and `Q4`. For the rows corresponding to `df_SN7577i_aa` the values in the `Q4`
column are missing and denoted by `NaN`. The same applies to `Q3` for the `df_SN7577i_bb` rows.

### Scenario 2 - Adding the columns from one Dataframe to those of another Dataframe

```python
df_SN7577i_c = pd.read_csv("SN7577i_c.csv")
df_SN7577i_d = pd.read_csv("SN7577i_d.csv")
df_all_cols = pd.concat([df_SN7577i_c, df_SN7577i_d], axis = 1)
df_all_cols
```

We use the `axis=1` parameter to indicate that it is the columns that need to be joined together. Notice that the `Id`
column appears twice, because it was a column in each dataset. This is not particularly desirable, but also not
necessarily a problem. However, there are better ways of combining columns from two Dataframes which avoid this problem.

### Scenario 3 - Using merge to join columns

We can join columns from two Dataframes using the `merge()` function. This is similar to the SQL 'join' functionality.

A detailed discussion of different join types is given in the [SQL lesson](./episodes/sql...).

You specify the type of join you want using the `how` parameter. The default is the `inner` join which returns the
columns from both tables where the `key` or common column values match in both Dataframes.

The possible values of the `how` parameter are shown in the picture below (taken from the Pandas documentation)

![](fig/pandas_join_types.png){alt='pandas\_join\_types'}

The different join types behave in the same way as they do in SQL. In Python/pandas, any missing values are shown as `NaN`

In order to `merge` the Dataframes we need to identify a column common to both of them.

```python
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner')
df_cd
```

In fact, if there is only one column with the same name in each Dataframe, it will be assumed to be the one you want to
join on. In this example the `Id` column

Leaving the join column to default in this way is not best practice. It is better to explicitly name the column using
the `on` parameter.

```python
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner', on = 'Id')
```

In many circumstances, the column names that you wish to join on are not the same in both Dataframes, in which case you
can use the `left_on` and `right_on` parameters to specify them separately.

```python
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner', left_on = 'Id', right_on = 'Id')
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Practice with data

1. Examine the contents of the `SN7577i_aa` and `SN7577i_bb` csv files using Excel or equivalent.
2. Using the `SN7577i_aa` and `SN7577i_bb` csv files, create a Dataframe which is the result of an outer join using the `Id` column to join on.
3. What do you notice about the column names in the new Dataframe?
4. Using `shift`\+`tab` in Jupyter examine the possible parameters for the `merge()` function.
5. re-write the code so that the columns names which are common to both files have suffixes indicating the filename from which they come
6. If you add the parameter `indicator=True`, what additional information is provided in the resulting Dataframe?

:::::::::::::::  solution

## Solution

```python
df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
df_aabb = pd.merge(df_SN7577i_aa, df_SN7577i_bb, how='outer', on = 'Id')
df_aabb
```

```python
df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
df_aabb = pd.merge(df_SN7577i_aa, df_SN7577i_bb, how='outer', on = 'Id',suffixes=('_aa', '_bb'), indicator = True)
df_aabb
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

[setup-page]: https://datacarpentry.org/python-socialsci/setup.html
[gh-repo]: https://github.com/datacarpentry/python-socialsci/archive/gh-pages.zip


:::::::::::::::::::::::::::::::::::::::: keypoints

- You can join pandas Dataframes in much the same way as you join tables in SQL
- The `concat()` function can be used to concatenate two Dataframes by adding the rows of one to the other.
- `concat()` can also combine Dataframes by columns but the `merge()` function is the preferred way
- The `merge()` function is equivalent to the SQL JOIN clause. 'left', 'right' and 'inner' joins are all possible.

::::::::::::::::::::::::::::::::::::::::::::::::::


