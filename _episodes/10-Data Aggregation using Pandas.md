---
title: "Data Aggregation using Pandas"
teaching: 0
exercises: 0
questions:
- "How can I summarise the data in a data frame?"
objectives:
- "Access and summarize data stored in a Data Frame"
- "Perform basic mathematical operations and summary statistics on data in a Pandas Data Frame"
- "Understand missing data"
- "Changing to and from 'NaN' values"
keypoints:
- "Summarising numerical and categorical variables is a very common requirement"
- "Missing data can interfer with how statiscal summaries are calculated"
- "Missing data can be replaced or created depending on requirement"
- "Summarising or aggregation can be done over single or multiple variables at the same time"
---

## Using Pandas functions to summarise data in a Data Frame

For variables which contain numerical values we are often interested in various statisical measures relating to those values. For categorical variable we are often interested in the how many of each unique values are present in the dataset.

We shall use the SAFI_results.csv dataset to demonstrate how we can obtain these pieces of information

~~~
import pandas as pd
df_SAFI = pd.read_csv("SAFI_results.csv") 
df_SAFI
~~~

For numeric variables we can obtain a variety of basic statistical information by using the `describe` method.

~~~
df_SAFI.describe()
~~~

This can be done for the dataframe as a whole, in which case some of the results might have no sensible meaning, or on a single variable basis

~~~
df_SAFI['B_no_membrs'].describe()
~~~

There are also a set of methods which allow us to obtain individual values.

~~~
print(df_SAFI['B_no_membrs'].min())
print(df_SAFI['B_no_membrs'].max())
print(df_SAFI['B_no_membrs'].mean())
print(df_SAFI['B_no_membrs'].std())
print(df_SAFI['B_no_membrs'].count())
print(df_SAFI['B_no_membrs'].sum())
~~~

Unlike the `describe` method which converts the variable to a float (when it was originally an integer), the individual summary methods only does so for the returned result if needed. 

We can do the same thing for the 'E19_period_use' variable

~~~
print(df_SAFI['E19_period_use'].min())
print(df_SAFI['E19_period_use'].max())
print(df_SAFI['E19_period_use'].mean())
print(df_SAFI['E19_period_use'].std())
print(df_SAFI['E19_period_use'].count())
print(df_SAFI['E19_period_use'].sum())
~~~

> ## Exercise
> 
> Compare the count values returned for the 'B_no_membrs' and the 'E19_period_use' variables. 
> 
> 1. Why do you think they are different?
> 2. How does this affect the calculation of the mean values?
> 
> > ## Solution
> > 
> > 1. We know from when we originally displayed the contents of the 'df_SAFI dataframe that there are 131 rows in it. This matches the value for the 'B_no_membrs' count. The count for 'E19_period_use' however is only 92. If you look at the values in the 'E19_period_use' column using 
> > 
> > ~~~
> > df_SAFI['E19_period_use']
> > ~~~
> > 
> > you will see that there are several 'NaN' values. They also occurred when we used `describe()` on the full dataframe. NaN stands for Not a Number, ie. the value is missing. There are only 92 non-missing values and this is what is reported by the `count()` method. This value is also used in the caclulation of the mean and std values.
> > 
> {: .solution}
{: .challenge}

## Dealing with missing values

We can find out how many variables in our dataframe contains any 'NaN' values with the code

~~~
df_SAFI.isnull().sum()
~~~

or for a specific variable 

~~~
df_SAFI['E19_period_use'].isnull().sum()
~~~

Data from most sources has the potential to include missing data. Whether or not this presents a problem at all depends on what you are planning to do. 

We have been using data from two very different sources. 

The SN7577 dataset is provided by the [UK Data Service](https://www.ukdataservice.ac.uk). Datasets from the UK data Service, have already been 'cleaned' and it is unlikely that there will be any genuinely missing data. However you may find that data which was missing has been replaced with a vlaue such as '-1' or 'Not Specified'. In cases like these it may be appropriate to replace these values with 'NaN' before you try to process the data further.

The SAFI dataset we have been using comes from a project called 'Studying African Farmer-led Irrigation'. The data for this project is questionnaire based, but rather than using a paper-based questionnaire, it has been created and is completed electronically via an app on a smartphone. This provides flexibility in the design and presentation of the questionnaire; a section of the questionnaire may only be presented depending on the answer given to some preceding question. This means that there can quite legitimately be a set of 'NaN' values in a record (one complete questionnaire) where you would still consider the record to be complete.

We have already seen how we can check for missing values. There are three other actions we need to be able to do;

1. Remove complete rows which contain NaN
2. Replace NaN with a value of our choice
3. Replace specific values with NaN


With these options we can ensure that the data is suitable for the further processing we have planned.


### Completely remove rows with NaNs

The `dropna` method will delete all rows if 'any' of the variables contain an NaN. For some datasets this may be acceptable. You will need to take care that you have enough rows left for your analysis to have meaning.

~~~
df_SAFI = pd.read_csv("SAFI_results.csv") 
print(df_SAFI.shape)
df_SAFI.dropna(inplace=True)
print(df_SAFI.shape)
~~~

Because there are variables in the SAFI dataset which are all NaN using the `dropna` method effectively deletes all of the rows from the dataframe, probably not what you wanted. Instead we can use the `notnull()` method as a row selection criteria and delete the rows where a specific variable has NaN values.

~~~
df_SAFI = pd.read_csv("SAFI_results.csv") 
print(df_SAFI.shape)
df_SAFI = df_SAFI[(df_SAFI['E_no_group_count'].notnull())]
print(df_SAFI.shape)
~~~

### Replace NaN with a value of our choice

The 'E19_period_use' variBLE answers the question; 'For how many years have you been irrigating the land?'. In some cases the land is not irrigated and these are represented by NaN in the dataset. So when we run 

~~~
df_SAFI['E19_period_use'].describe()
~~~

we get a count value of 92 and all of the other statistics are based on this count value.

Now supposing that instead of NaN the interviewer entered a value of 0 to indicated the land which is *not* irrigated has been irrigated for 0 years, technically correct.

To see what happens we can convert all of the NaN values in the 'E19_period_use' column to 0 with the following code;

~~~
df_SAFI['E19_period_use'].fillna(0, inplace=True)
~~~

If we now run the `describe()` again you can see that all of the statistic have been changed because the calculations are NOW based on a count of 131. Probably not what we would have wanted.

Conveniently this allows us to demonstrate our 3rd action.


### Replace specific values with NaN

Although we can recognise 'NaN' with methods like `isnull` or `dropna` actually creating a 'NaN' value and putting it into a dataframe, requires the numpy module. The following code will replace our 0 values with NaN. We can demonstrate that this has occurred by running the `describe` again and see that we now have our original values back.

~~~
import numpy as np
df_SAFI['E19_period_use'].replace(0, np.NaN, inplace = True)
df_SAFI['E19_period_use'].describe()
~~~

## Categorical variables

For categorical variables, nuumerical statistics don't make any sense.
For a categorical cariable we can obtain a list of unique values used by the variable by using the `unique` method. 

~~~
pd.unique(df_SAFI['C01_respondent_roof_type'])
~~~

Knowing all of the unique values is useful but what is more useful is knowing how many occurences of each there are. In order to do this we can use the `groupby` method. 

Having performed the `groupby` we can them `describe()` the results. The format is similar to that which we have seen before except that the 'grouped by' variable appears to the left and there is a set of statistics for each unique value of the variable.

~~~
grouped_data = df_SAFI.groupby('C01_respondent_roof_type')
grouped_data.describe()
~~~

You can group by more than on variable at a time by providing them as a list.

~~~
grouped_data = df_SAFI.groupby(['C01_respondent_roof_type', 'C02_respondent_wall_type'])
grouped_data.describe()
~~~

You can also obtain individual statistics if you want.

~~~
A11_years_farm = df_SAFI.groupby(['C01_respondent_roof_type', 'C02_respondent_wall_type'])['A11_years_farm'].count()
A11_years_farm
~~~

## Joining dataframes

### Why do we want to do this


There are many occasions when we have related data spread across multiple files.

The data can be realted to each other in different ways. How they are related will how and how completely we can join the data from the datasets. 

In this episode we will consider different scenarios and show we might join the data. We will use csv files and in all cases the first step will be to read the datasets into a pandas dataframe from where we will do the joining. The csv files we are using are cut down versions of the SN7577 dataset just to make the displays more manageable.



### Scenario 1 - Two data sets contining the same columns but different rows of data

Here we want to add the rows from one dataframe to the rows of the other dataframe. In order to do this we can use the `concat` method.

~~~

import pandas as pd

df_SN7577i_a = pd.read_csv("SN7577i_a.csv")
df_SN7577i_b = pd.read_csv("SN7577i_b.csv")
~~~

Have a quick look at what these dataframes look like with 

~~~
print(df_SN7577i_a)
print(df_SN7577i_b)
~~~

The `concat` method appends the rows from the two dataframes to create the df_all_rows dataframe. When you list this out you can see that all of the data rows are there, however there is a problem with the `index`.

~~~
df_all_rows = pd.concat([df_SN7577i_a, df_SN7577i_b])
df_all_rows
~~~

We didn't explicitly set an index for any of the dataframes we have used. For 'df_SN7577i_a' and 'df_SN7577i_b' default indexes would have been created by pandas. When we concatenated the dataframes the indexes were also concatenated resulting in duplicate entries.

This is really only a problem if you need to access a row by its index. We can fix the problem with the following code.

~~~
df_all_rows=df_all_rows.reset_index(drop=True)
df_all_rows
~~~

What if the columns in the dataframes are not the same?

~~~
df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
df_all_rows = pd.concat([df_SN7577i_aa, df_SN7577i_bb])
df_all_rows
~~~

In this case 'df_SN7577i_aa' has no Q4 column and 'df_SN7577i_bb' has no Q3 column. When they are concatenated, the resulting dataframe has a column for for Q3 and Q4. For the rows corresponding to 'df_SN7577i_aa' the values in the Q4 column are missing and denoted by 'NaN'. The same applies to Q3 for the 'df_SN7577i_bb' rows 


### Scenario 2 - Adding the columns from one dataframe to those of another dataframe

~~~
df_SN7577i_c = pd.read_csv("SN7577i_c.csv")
df_SN7577i_d = pd.read_csv("SN7577i_d.csv")
df_all_cols = pd.concat([df_SN7577i_c, df_SN7577i_d], axis = 1)
df_all_cols
~~~

We use the `axis=1` parameter to indicate that it is the columns that need to be joined together. Notice that the 'Id' column appears twice, because it was a column in each dataset. This is not particularly desirable, but also not necessarily a problem. However there are better ways of combining columns from two dataframes which avoid this problem.

### Scenario 3 - Using merge to join columns

We can join columns from two dataframes using the `merge` method. This is similar to the SQL 'join' functionality.

A detailed discussion of different join types is given in the [SQL lesson](./episodes/sql...)

In order to `merge` the dataframes we need to identify a column common to both of them.

~~~
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner')
df_cd
~~~

In fact if there is only one column with the same name in each dataframe, it will be assumed to be the one you want to join on. In this example the 'Id' column

Leaving the join column to default in this way is not best practice. It is better to explicitly name the column using the `on` parameter.

~~~
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner', on = 'Id')
~~~

In many circumstances, the column names that you wish to join on are not the  same in both dataframes, in which case you can use the 'left_on' and 'right_on' parameters to specify them separately.

~~~
df_cd = pd.merge(df_SN7577i_c, df_SN7577i_d, how='inner', left_on = 'Id', right_on = 'Id')
~~~

You specify the type of join you want using the `how` parameter. The default is the 'inner' join which returns the columns from both tables where the 'key' or common column values match in both dataframes.

The possible values of the `how` parameter are shown in the picture below (taken from the Pandas documentation)

![pandas_join_types](../fig/pandas_join_types.png)

The different join types behave in the same way as they do in SQL. In Python/pandas, any missing values are shown as 'NaN'


> ## Exercises
> 
> 1. Examine the contents of the 'SN7577i_aa' and 'SN7577i_bb' csv files using Excel or equivalent.
> 2. Using the 'SN7577i_aa' and 'SN7577i_bb' csv files, create a dataframe which is the result of an outer join using the 'Id' column to join on.
> 3. What do you notice about the column names in the new dataframe?
> 4. Using shift+ tab in Jupyter examine the possible parameters for the `merge` method.
> 5. re-write the code so that the columns names which are common to both files have suffixes indicating the filename from which they come
> 6. If you add the parameter 'indicator=True', what additional information is provided in the resulting dataframe?
> 
> > ## Solution
> > 
> > ~~~
> > 
> > df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
> > df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
> > df_aabb = pd.merge(df_SN7577i_aa, df_SN7577i_bb, how='outer', on = 'Id')
> > df_aabb
> > 
> > ~~~
> > 
> > ~~~
> > 
> > df_SN7577i_aa = pd.read_csv("SN7577i_aa.csv")
> > df_SN7577i_bb = pd.read_csv("SN7577i_bb.csv")
> > df_aabb = pd.merge(df_SN7577i_aa, df_SN7577i_bb, how='outer', on = 'Id',suffixes=('_aa', '_bb'), indicator = True)
> > df_aabb
> > 
> > ~~~
> > 
> {: .solution}
{: .challenge}

## Wide and long data formats

In the SN7577 dataset that we have been using there is a group of columns which record which daily newspapers each respondent reads. Despite the un-informative names like 'daily1' each column refers to a current UK daily national or local newspaper. 

Whether the paper is read or not is recorded using the values of 0 or 1 as a boolean indicator. The advantage of using a column for eah paper means that should a respondent read multiple newpapers, all of the required information can still be recorded in a single record.

Recording information in this 'wide' format is not always beneficial when trying to analyse the data.

Pandas provides methods for converting data from 'wide' to 'long' format and from 'long' to 'wide' format

The SN7577 dataset does not contain a variable that can be used to uniquely identify a row. This is often referred to as a 'primary key' field (or column).

A dataset doesn't need to have such a key. None of the work we have done so far has required it. 

When we create a pandas dataframe by importing a csv file, we have seen that pandas will create an index  for the rows. This index can be used a bit like a key field, but as we have seen there can be problems with the index when we concatenate two dataframes together.

In the version of SN7577 that we are going to use to demonstrate long and wide formats we will add a new variable with the name of 'Id' and we will restrict the other columns to those starting with the word 'daily'.

~~~
import pandas as pd
df_SN7577 = pd.read_csv("SN7577.tab", sep='\t')
~~~

We will create a new dataframe with a single column of 'Id'. 

~~~
# create an 'Id' column
df_papers1 = pd.DataFrame(pd.Series(range(1,1287)),index=None,columns=['Id'])
~~~

Using the range function I can create values of Id starting with 1 and going upto 1286 (remember the second parameter to re=ange is one past the last value used.) I have explicitly coded this value because I knew how many rows were in the dataset. If I didn't, I could have used 

~~~
len(df_SN7577.index) +1
~~~


We will create a 2nd dataframe, based on SN7577 but containing only the columns starting with the word 'daily'. 

There are several ways odf doing this.

we could use the `iloc` method and provide the index values of the range of columns we want.

~~~
df_papers2 = df_SN7577.iloc[:,118:143]
~~~

This isn't really very practical. I would need to know the position of all of the columns of interest. They may not be contiguous.

We could use a regular expression. Regular expressions is a very complex topic which we won't be covering. Using the `filter` method the code

~~~
df_papers2 = df_SN7577.filter(regex= '^daily')
~~~

will do want we want. '^daily' is a simple regular exprerssion which says 'startswith the characters 'daily'

A simpler way is to use the 'like' parameter of the `filter` method.

~~~
df_papers2 = df_SN7577.filter(like= 'daily')
~~~

The value supplied to 'like' can occur anywhere in the column name to be matched (and therefore selected)

To create the dataframe that we will use, we will concatenate the two dataframes we have created. 

~~~
df_papers = pd.concat([df_papers1, df_papers2], axis = 1)
print(df_papers.index)
print(df_papers.columns)
~~~

We use 'axis = 1' because we are joining by columns not rows which is the default.


## From 'wide' to 'long'

Just to make the displays more maneagable we will use only the first eight 'daily' columns

~~~
## using df_papers
daily_list = ['daily1', 'daily2','daily3', 'daily4','daily5', 'daily6','daily7', 'daily8']

df_daily_papers_long = pd.melt(df_papers, id_vars = ['Id'], value_vars = daily_list)

# by default the new columns created will be called 'variable' which is the name of the 'daily'
# and 'value' which is the value of that 'daily' for that 'Id'. So we will rename the columns

df_daily_papers_long.columns = ['Id','Daily_paper','Value']
df_daily_papers_long
~~~

We now have a dataframe that we can `groupby`. 

We want to `groupby` the 'Daily_paper' and them sum the 'Value'.

~~~
a = df_daily_papers_long.groupby('Daily_paper')['Value'].sum()
a
~~~

## From Long to Wide 

The process can be reversed by using the `pivot` method. 
Here we need to indicate which column (or columns) remain fixed (this will become an index in the new dataframe), which column contains the values which are to become column names and which column contains the values for the columns.

In our case we want to use the 'Id' column as the fixed column, the 'Daily_paper' column contains the column names and the 'Value' column contains the values.

~~~
df_daily_papers_wide = df_daily_papers_long.pivot(index = 'Id', columns = 'Daily_paper', values = 'Value')
~~~

We can change our 'Id' index back to an ordinary column with 

~~~
df_daily_papers_wide.reset_index(level=0, inplace=True)
~~~

> ## Exercise
> 
> 1. Find out how many people take each of the daily newspapers by Title.
> 2. Which titles don't appear to be read by anyone?
> 
> There is a file called Newspapers.csv which lists all of the newpapers Titles along with the corresponding 'daily' value
> 
> Help : Newspapers.csv cotains both daily and Sunday newspapers. you can filter out the Sunday papers with the following code;
> 
> 
> ~~~
> df_newspapers = df_newspapers[(df_newspapers.Column_name.str.startswith('daily'))]
> ~~~
> 
> > ## Solution
> > 
> > 1. Read in Newspapers.csv file and keep only the dailies.
> > 
> > ~~~
> > df_newspapers = pd.read_csv("Newspapers.csv")
> > df_newspapers = df_newspapers[(df_newspapers.Column_name.str.startswith('daily'))]
> > df_newspapers
> > ~~~
> > 
> > 2. Create the df_papers dataframe as we did before.
> > 
> > ~~~
> > import pandas as pd
> > df_SN7577 = pd.read_csv("SN7577.tab", sep='\t')
> > #create an 'Id' column
> > df_papers1 = pd.DataFrame(pd.Series(range(1,1287)),index=None,columns=['Id'])
> > df_papers2 = df_SN7577.filter(like= 'daily')
> > df_papers = pd.concat([df_papers1, df_papers2], axis = 1)
> > df_papers
> > 
> > ~~~~
> > 
> > 3. Create a list of all of the dailies, one way would be
> > 
> > ~~~
> > 
> > daily_list = []
> > for i in range(1,26):
> >     daily_list.append('daily'+str(i))
> >     
> > ~~~
> > 
> > 4. Pass the list as the 'value_vars' parameter to the `melt` method
> > 
> > 
> > ~~~
> > #use melt to create df_daily_papers_long  
> > df_daily_papers_long = pd.melt(df_papers, id_vars = ['Id'], value_vars = daily_list )
> > #Change the column names
> > df_daily_papers_long.columns = ['Id','Daily_paper','Value']
> > ~~~
> > 
> > 5. `merge` the two dataframes with a left join, because we want all of the Newspaper Titles to be included.
> > 
> > ~~~
> > df_papers_taken = pd.merge(df_newspapers, df_daily_papers_long, how='left', left_on = 'Column_name',right_on = 'Daily_paper')
> > ~~~
> > 
> > 6. Then `groupby` the 'Title' and sum the 'Value'
> > 
> > ~~~
> > df_papers_taken.groupby('Title')['Value'].sum()
> > ~~~
> > 
> {: .solution}
{: .challenge}
