---
title: "Joining Pandas dataframes"
teaching: 25
exercises: 10
questions:
- "How can I join two dataframes with a common key?"
objectives:
- "Understand why we would want to join dataframes"
- "Know what is needed for a join to be possible"
- "Understand the different types of joins"
- "Understand what the joined results tell us about our data"
keypoints:
- "You can join pandas dataframes in much the same way as you join tables in SQL"
- "The `concat` method can be used to concatenate two dataframes by adding the rows of one to the other."
- "`concat` can also combine dataframes by columns but the `merge` method is the preferred way"
- "The `merge` method is equivalent to the SQL JOIN clause. 'left', 'right' and 'inner' joins are all possible."
---

## Joining dataframes

### Why do we want to do this


There are many occasions when we have related data spread across multiple files.

The data can be related to each other in different ways. How they are related and how completely we can join the data from the datasets will vary.

In this episode we will consider different scenarios and show we might join the data. We will use csv files and in all cases the first step will be to read the datasets into a pandas dataframe from where we will do the joining. The csv files we are using are cut down versions of the SN7577 dataset to make the displays more manageable.

### Scenario 1 - Two data sets continuing the same columns but different rows of data

Here we want to add the rows from one dataframe to the rows of the other dataframe. In order to do this we can use the `concat` method.

~~~
import pandas as pd

df_SN7577i_a = pd.read_csv("SN7577i_a.csv")
df_SN7577i_b = pd.read_csv("SN7577i_b.csv")
~~~
{: .language-python}

Have a quick look at what these dataframes look like with

~~~
print(df_SN7577i_a)
print(df_SN7577i_b)
~~~
{: .language-python}


~~~
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
~~~
{: output}

The `concat` method appends the rows from the two dataframes to create the df_all_rows dataframe. When you list this out you can see that all of the data rows are there, however there is a problem with the `index`.

~~~
df_all_rows = pd.concat([df_SN7577i_a, df_SN7577i_b])
df_all_rows
~~~
{: .language-python}

We didn't explicitly set an index for any of the dataframes we have used. For `df_SN7577i_a` and `df_SN7577i_b` default indexes would have been created by pandas. When we concatenated the dataframes the indexes were also concatenated resulting in duplicate entries.

This is really only a problem if you need to access a row by its index. We can fix the problem with the following code.

~~~
df_all_rows=df_all_rows.reset_index(drop=True)
df_all_rows
~~~
{: .language-python}

What if the columns in the dataframes are not the same?

~~~
df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
df_all_rows = pd.concat([df_SN7577i_aa, df_SN7577i_bb])
df_all_rows
~~~
{: .language-python}

In this case `df_SN7577i_aa` has no Q4 column and `df_SN7577i_bb` has no Q3 column. When they are concatenated, the resulting dataframe has a column for for Q3 and Q4. For the rows corresponding to `df_SN7577i_aa` the values in the Q4 column are missing and denoted by `NaN`. The same applies to Q3 for the `df_SN7577i_bb` rows.


### Scenario 2 - Adding the columns from one dataframe to those of another dataframe

~~~
df_SN7577i_c = pd.read_csv("SN7577i_c.csv")
df_SN7577i_d = pd.read_csv("SN7577i_d.csv")
df_all_cols = pd.concat([df_SN7577i_c, df_SN7577i_d], axis = 1)
df_all_cols
~~~
{: .language-python}

We use the `axis=1` parameter to indicate that it is the columns that need to be joined together. Notice that the 'Id' column appears twice, because it was a column in each dataset. This is not particularly desirable, but also not necessarily a problem. However there are better ways of combining columns from two dataframes which avoid this problem.

### Scenario 3 - Using merge to join columns

We can join columns from two dataframes using the `merge` method. This is similar to the SQL 'join' functionality.

A detailed discussion of different join types is given in the [SQL lesson](./episodes/sql...)

In order to `merge` the dataframes we need to identify a column common to both of them.

~~~
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner')
df_cd
~~~
{: .language-python}

In fact if there is only one column with the same name in each dataframe, it will be assumed to be the one you want to join on. In this example the 'Id' column

Leaving the join column to default in this way is not best practice. It is better to explicitly name the column using the `on` parameter.

~~~
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner', on = 'Id')
~~~
{: .language-python}

In many circumstances, the column names that you wish to join on are not the same in both dataframes, in which case you can use the 'left_on' and 'right_on' parameters to specify them separately.

~~~
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner', left_on = 'Id', right_on = 'Id')
~~~
{: .language-python}

You specify the type of join you want using the `how` parameter. The default is the 'inner' join which returns the columns from both tables where the 'key' or common column values match in both dataframes.

The possible values of the `how` parameter are shown in the picture below (taken from the Pandas documentation)

![pandas_join_types](../fig/pandas_join_types.png)

The different join types behave in the same way as they do in SQL. In Python/pandas, any missing values are shown as `NaN`


> ## Exercises
>
> 1. Examine the contents of the `SN7577i_aa` and `SN7577i_bb` csv files using Excel or equivalent.
> 2. Using the `SN7577i_aa` and `SN7577i_bb` csv files, create a dataframe which is the result of an outer join using the 'Id' column to join on.
> 3. What do you notice about the column names in the new dataframe?
> 4. Using shift+ tab in Jupyter examine the possible parameters for the `merge` method.
> 5. re-write the code so that the columns names which are common to both files have suffixes indicating the filename from which they come
> 6. If you add the parameter 'indicator=True', what additional information is provided in the resulting dataframe?
>
> > ## Solution
> >
> > ~~~
> > df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
> > df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
> > df_aabb = pd.merge(df_SN7577i_aa, df_SN7577i_bb, how='outer', on = 'Id')
> > df_aabb
> > ~~~
> > {: .language-python}
> >
> > ~~~
> > df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
> > df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
> > df_aabb = pd.merge(df_SN7577i_aa, df_SN7577i_bb, how='outer', on = 'Id',suffixes=('_aa', '_bb'), indicator = True)
> > df_aabb
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
