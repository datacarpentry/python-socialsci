---
title: "Data Aggregation using Pandas"
teaching: 20
exercises: 10
questions:
- "How can I summarise the data in a data frame?"
objectives:
- "Access and summarize data stored in a Data Frame"
- "Perform basic mathematical operations and summary statistics on data in a Pandas Data Frame"
- "Understand missing data"
- "Changing to and from 'NaN' values"
keypoints:
- "Summarising numerical and categorical variables is a very common requirement"
- "Missing data can interfere with how statistical summaries are calculated"
- "Missing data can be replaced or created depending on requirement"
- "Summarising or aggregation can be done over single or multiple variables at the same time"
---

## Using Pandas functions to summarise data in a Data Frame

For variables which contain numerical values we are often interested in various statistical measures relating to those values. For categorical variable we are often interested in the how many of each unique values are present in the dataset.

We shall use the SAFI_results.csv dataset to demonstrate how we can obtain these pieces of information

~~~
import pandas as pd
df_SAFI = pd.read_csv("SAFI_results.csv")
df_SAFI
~~~
{: .language-python}

For numeric variables we can obtain a variety of basic statistical information by using the `describe()` method.

~~~
df_SAFI.describe()
~~~
{: .language-python}

This can be done for the Dataframe as a whole, in which case some of the results might have no sensible meaning. If there are any missing values, represented in the display as `NaN` you will get a warning message.

You can also `.describe()` on a single variable basis.

~~~
df_SAFI['B_no_membrs'].describe()
~~~
{: .language-python}

There are also a set of methods which allow us to obtain individual values.

~~~
print(df_SAFI['B_no_membrs'].min())
print(df_SAFI['B_no_membrs'].max())
print(df_SAFI['B_no_membrs'].mean())
print(df_SAFI['B_no_membrs'].std())
print(df_SAFI['B_no_membrs'].count())
print(df_SAFI['B_no_membrs'].sum())
~~~
{: .language-python}

~~~
2
19
7.190839694656488
3.1722704895263734
131
942
~~~
{: .output}

Unlike the `describe()` method which converts the variable to a float (when it was originally an integer), the individual summary methods only does so for the returned result if needed.

We can do the same thing for the `E19_period_use` variable

~~~
print(df_SAFI['E19_period_use'].min())
print(df_SAFI['E19_period_use'].max())
print(df_SAFI['E19_period_use'].mean())
print(df_SAFI['E19_period_use'].std())
print(df_SAFI['E19_period_use'].count())
print(df_SAFI['E19_period_use'].sum())
~~~
{: .language-python}

~~~
1.0
45.0
12.043478260869565
8.583030848015385
92
1108.0
~~~
{: output}

> ## Exercise
>
> Compare the count values returned for the `B_no_membrs` and the `E19_period_use` variables.
>
> 1. Why do you think they are different?
> 2. How does this affect the calculation of the mean values?
>
> > ## Solution
> >
> > 1. We know from when we originally displayed the contents of the `df_SAFI` Dataframe that there are 131 rows in it. This matches the value for the `B_no_membrs` count. The count for `E19_period_use` however is only 92. If you look at the values in the `E19_period_use` column using
> >
> > ~~~
> > df_SAFI['E19_period_use']
> > ~~~
> > {: .language-python}
> >
> > you will see that there are several `NaN` values. They also occurred when we used `describe()` on the full Dataframe. `NaN` stands for Not a Number, ie. the value is missing. There are only 92 non-missing values and this is what is reported by the `count()` method. This value is also used in the calculation of the mean and std values.
> >
> {: .solution}
{: .challenge}

## Dealing with missing values

We can find out how many variables in our Dataframe contains any `NaN` values with the code

~~~
df_SAFI.isnull().sum()
~~~
{: .language-python}

~~~
Column1                             0
A01_interview_date                  0
A03_quest_no                        0
A04_start                           0
...
~~~
{: output}

or for a specific variable

~~~
df_SAFI['E19_period_use'].isnull().sum()
~~~
{: .language-python}

~~~
39
~~~
{: output}

Data from most sources has the potential to include missing data. Whether or not this presents a problem at all depends on what you are planning to do.

We have been using data from two very different sources.

The SN7577 dataset is provided by the [UK Data Service](https://www.ukdataservice.ac.uk). Datasets from the UK data Service, have already been 'cleaned' and it is unlikely that there will be any genuinely missing data. However you may find that data which was missing has been replaced with a value such as '-1' or 'Not Specified'. In cases like these it may be appropriate to replace these values with 'NaN' before you try to process the data further.

The SAFI dataset we have been using comes from a project called 'Studying African Farmer-led Irrigation'. The data for this project is questionnaire based, but rather than using a paper-based questionnaire, it has been created and is completed electronically via an app on a smartphone. This provides flexibility in the design and presentation of the questionnaire; a section of the questionnaire may only be presented depending on the answer given to some preceding question. This means that there can quite legitimately be a set of 'NaN' values in a record (one complete questionnaire) where you would still consider the record to be complete.

We have already seen how we can check for missing values. There are three other actions we need to be able to do:

1. Remove complete rows which contain `NaN`
2. Replace `NaN` with a value of our choice
3. Replace specific values with `NaN`


With these options we can ensure that the data is suitable for the further processing we have planned.


### Completely remove rows with NaNs

The `dropna()` method will delete all rows if *any* of the variables contain an `NaN`. For some datasets this may be acceptable. You will need to take care that you have enough rows left for your analysis to have meaning.

~~~
df_SAFI = pd.read_csv("SAFI_results.csv")
print(df_SAFI.shape)
df_SAFI.dropna(inplace=True)
print(df_SAFI.shape)
~~~
{: .language-python}

~~~
(131, 55)
(0, 55)
~~~
{: output}

Because there are variables in the SAFI dataset which are all `NaN` using the `dropna()` method effectively deletes all of the rows from the Dataframe, probably not what you wanted. Instead we can use the `notnull()` method as a row selection criteria and delete the rows where a specific variable has `NaN` values.

~~~
df_SAFI = pd.read_csv("SAFI_results.csv")
print(df_SAFI.shape)
df_SAFI = df_SAFI[(df_SAFI['E_no_group_count'].notnull())]
print(df_SAFI.shape)
~~~
{: .language-python}

~~~
(131, 55)
(39, 55)
~~~
{: output}

### Replace NaN with a value of our choice

The `E19_period_use` variable answers the question: "For how many years have you been irrigating the land?". In some cases the land is not irrigated and these are represented by NaN in the dataset. So when we run

~~~
df_SAFI['E19_period_use'].describe()
~~~

we get a count value of 92 and all of the other statistics are based on this count value.

Now supposing that instead of NaN the interviewer entered a value of 0 to indicate the land which is *not* irrigated has been irrigated for 0 years, technically correct.

To see what happens we can convert all of the NaN values in the `E19_period_use` column to 0 with the following code:

~~~
df_SAFI['E19_period_use'].fillna(0, inplace=True)
~~~
{: .language-python}

If we now run the `describe()` again you can see that all of the statistic have been changed because the calculations are NOW based on a count of 131. Probably not what we would have wanted.

Conveniently this allows us to demonstrate our 3rd action.


### Replace specific values with NaN

Although we can recognise `NaN` with methods like `isnull()` or `dropna()` actually creating a `NaN` value and putting it into a Dataframe, requires the `numpy` module. The following code will replace our 0 values with `NaN`. We can demonstrate that this has occurred by running the `describe()` again and see that we now have our original values back.

~~~
import numpy as np
df_SAFI['E19_period_use'].replace(0, np.NaN, inplace = True)
df_SAFI['E19_period_use'].describe()
~~~
{: .language-python}

## Categorical variables

For categorical variables, numerical statistics don't make any sense.
For a categorical variable we can obtain a list of unique values used by the variable by using the `unique()` method.

~~~
df_SAFI = pd.read_csv("SAFI_results.csv")
pd.unique(df_SAFI['C01_respondent_roof_type'])
~~~
{: .language-python}

~~~
array(['grass', 'mabatisloping', 'mabatipitched'], dtype=object)
~~~
{: output}

Knowing all of the unique values is useful but what is more useful is knowing how many occurrences of each there are. In order to do this we can use the `groupby` method.

Having performed the `groupby()` we can them `describe()` the results. The format is similar to that which we have seen before except that the 'grouped by' variable appears to the left and there is a set of statistics for each unique value of the variable.

~~~
grouped_data = df_SAFI.groupby('C01_respondent_roof_type')
grouped_data.describe()
~~~
{: .language-python}

You can group by more than one variable at a time by providing them as a list.

~~~
grouped_data = df_SAFI.groupby(['C01_respondent_roof_type', 'C02_respondent_wall_type'])
grouped_data.describe()
~~~
{: .language-python}

You can also obtain individual statistics if you want.

~~~
A11_years_farm = df_SAFI.groupby(['C01_respondent_roof_type', 'C02_respondent_wall_type'])['A11_years_farm'].count()
A11_years_farm
~~~
{: .language-python}

~~~
C01_respondent_roof_type  C02_respondent_wall_type
grass                     burntbricks                 22
                          muddaub                     42
                          sunbricks                    9
mabatipitched             burntbricks                  6
                          muddaub                      3
...
~~~
{: output}

> ## Exercise
>
> 1. Read in the SAFI_results.csv dataset.
> 2. Get a list of the different `C01_respondent_roof_type` values.
> 3. Groupby `C01_respondent_roof_type` and describe the results.
> 4. Remove rows with NULL values for `E_no_group_count`.
> 5. repeat steps 2 & 3 and compare the results.
>
> > ## Solution
> >
> > ~~~
> > # Steps 1 and 2
> > import numpy as np
> > df_SAFI = pd.read_csv("SAFI_results.csv")
> > print(df_SAFI.shape)
> > print(pd.unique(df_SAFI['C01_respondent_roof_type']))
> > ~~~
> > {: .language-python}
> >
> > ~~~
> > # Step 3
> > grouped_data = df_SAFI.groupby('C01_respondent_roof_type')
> > grouped_data.describe()
> > ~~~
> > {: .language-python}
> >
> > ~~~
> > # steps 4 and 5
> > df_SAFI = df_SAFI[(df_SAFI['E_no_group_count'].notnull())]
> > grouped_data = df_SAFI.groupby('C01_respondent_roof_type')
> > print(df_SAFI.shape)
> > print(pd.unique(df_SAFI['C01_respondent_roof_type']))
> > grouped_data.describe()
> > ~~~
> > {: .language-python}
> >
> > `E_no_group_count` is related to whether or not farm plots are irrigated or not. It has no obvious connection to farm buildings.
> > By restricting the data to non-irrigated plots we have accidentally? removed one of the roof_types completely.
> >
> {: .solution}
{: .challenge}
