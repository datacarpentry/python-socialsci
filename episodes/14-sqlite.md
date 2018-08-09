---
title: "Accessing SQLite Databases "
teaching: 35
exercises: 25
questions:
- "How can I access database  tables using Pandas and Python?"
- "What are the advantages of storing data in a database"

objectives:
- "Use the sqlite3 module to interact with a SQL database"
- "Access data stored in SQLite using Python"
- "Describe the difference in interacting with data stored as a CSV file versus in SQLite"
- "Describe the benefits of accessing data using a database compared to a CSV file"
keypoints:
- "The SQLite database system is directly available from within Python"
- "A database table and a pandas Dataframe can be considered similar structures"
- "Using pandas to return all of the results from a query is simpler than using sqlite3 alone  "
---

## Introducing the sqlite3 module 

SQLite is a relational database system. Despite the 'Lite' in the name it can handle databases in excess of a Terabyte. The 'Lite'part really relates to the fact that it is a 'bare bones' system. It provides the mechanisms to create and query databases via a simple command line interface but not much else. In the SQL lesson we used a Firefox plugin to provide a GUI (Graphical User Interface) to the SQLite database engine.

In this lesson we will use Python code using the sqlite3 module to access the engine. We can use Python code and the sqlite3 module to create, delete and query database tables.

In practice we spend a lot of the time querying database tables. 

## Pandas Dataframe v SQL table

It is very easy and often very convenient to think of SQL tables and pandas Dataframes as being similar types of objects. All of the data manipulations, slicing, dicing, aggragetions and joins associated with SQL and SQL tables can all be accomplished with pandas methods operating on a pandas Dataframe.

The difference is that the pandas Dataframe is held in memory within the Python environment. The SQL table can largely be on disc and when you access it, it is the SQLite database engine which is doing the work. This allows you to work with very large tables which your Python environment may not have the memory to hold completely.  

A typical use case for SQLite databases is to hold large datasets, you use SQL commands from Python to slice and dice and possibly aggregate the data within the database system to reduce the size to something that Python can comfortably process and then return the results to a Dataframe.

## Accessing data stored in SQLite using Python

We will illustrate the use of the `sqlite3` module by connecting to an SQLite database using both core Python and also using pandas.

The database that we will use is SN7577.sqlite This contains the data from the SN7577 dataset that we have used in other lessons.

## Connecting to an SQlite database

The first thing we need to do is import the `sqlite3` library, We will import pandas at the same time for convenience.

~~~
import sqlite3
import pandas as pd
~~~
{: .language-python}

We will start looking at the sqlite3 library by connecting to an existing database and returning the results of running a query.

Initially we will do this without using Pandas and then we will repreat the exercise so that you can see the difference.

The first thing we need to do is to make a connection to the database. An SQLite database is just a file. To make a connection to it we only need to use the sqlite3 `connect()` function and specify the database file as the first parameter.

The connection is assigned to a variable. You could use any variable name, but 'con' is quite commonly used for this purpose

~~~
con = sqlite3.connect('SN7577.sqlite')
~~~
{: .language-python}

The next thing we need to do is to create a `cursor` for the connection and assign it to a variable. We do this using the `cursor` method of the connection object.

The cursor allows us to pass SQL statements to the database, have them executed and then get the results back.

To execute an SQL statement we use the `execute()` method of the cursor object.

The only paramater we need to pass to `execute()` is a string which contains the SQL query we wish to execute.

In our example we are passing a literal string. It could have been contained in a string variable. The string can contain any valid SQL query. It could also be a valid DDL statement such as a "CREATE TABLE ...". In this lesson however we will confine ourseleves to querying exiting database tables.

~~~
cur = con.cursor()
cur.execute("SELECT * FROM SN7577")
~~~
{: .language-python}

~~~
<sqlite3.Cursor at 0x115e10d50>
~~~
{: output}

The `execute()` method doesn't actually return any data, it just indicates that we want the data provided by running the SELECT statement.

> ## Exercise 
> 
> 1. What happens if you if you ask for a non existent table?, field within a table? or just any kind of syntax error?
> 
> > ## Solution
> > 
> > ~~~
> > cur = con.cursor()
> > # notice the mistyping of 'SELECT'
> > cur.execute("SELET * FROM SN7577")
> > ~~~
> > {: .language-python}
> > 
> > In all cases an error message is returned. The error message is not from Python but from SQLite. It is the same error message that you would have got had you made the same errors in the SQLite plugin.
> >
> {: .solution}
{: .challenge}


Before we can make use of the results of the query we need to use the `fetchall()` method of the cursor. 

The `fetchall()` method returns a list. Each item in the list is a tuple containing the values from one row of the table. You can iterate through the items in a tuple in the same way as you would do so for a list.

~~~
cur = con.cursor()
cur.execute("SELECT * FROM SN7577")
rows = cur.fetchall()
for row in rows:
    print(row)
~~~
{: .language-python}

~~~
(1, -1, 1, 8, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 3, 3, 3, 2, 3, 3, 4, 1, 4, 2, 2, 2, 2, 1, 0, 0, 0, 3, 2, 3, 3, 1, 4, 2, 3
...
~~~
{: output}

The output is the data only, you do not get the column names.

The column names are available from the 'description' of the cursor.

~~~
colnames = []
for description in cur.description :
    colnames.append(description[0])

print(colnames)
~~~
{: .language-python}

~~~
['Q1', 'Q2', 'Q3', 'Q4', 'Q5ai', 'Q5aii', 'Q5aiii', 'Q5aiv', 'Q5av', 'Q5avi', 'Q5avii', 'Q5aviii', 'Q5aix', 'Q5ax', 'Q5axi', 'Q5axii', 'Q5axiii', 'Q5axiv', 'Q5axv', 'Q5bi', 'Q5bii', 'Q5biii', 'Q5biv', 'Q5bv', 'Q5bvi', 'Q5bvii', 'Q5bviii', 'Q5bix', 'Q5bx', 'Q5bxi', 'Q5bxii', 'Q5bxiii', 'Q5bxiv', 'Q5bxv', 'Q6', 'Q7a', 'Q7b', 'Q8', 'Q9', 'Q10a', 'Q10b', 'Q10c', 'Q10d', 'Q11a',
...
~~~
{: output}

One reason for using a database is the size of the data involved. Consequently it may not be practial to use `fetchall()` as this will return the complete result of your query.

An alternative is to use the `fetchone()` method, which as the name suggestrs returns only a single row. The cursor keeps track of where you are in the results of the query, so the next call to `fetchone()` will return the next record. When there are no more records it will return 'None'.

~~~
cur = con.cursor()
cur.execute("SELECT * FROM SN7577")
row = cur.fetchone()
print(row)
row = cur.fetchone()
print(row)
~~~
{: .language-python}

~~~
(1, -1, 1, 8, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 3, 3, 3, 2, 3, 3, 4, 1, 4, 2, 2, 2, 2, 1, 0, 0, 0, 3, 2, 3, 3, 1, 4, 2, 3, 2, 4, 4, 2, 2, 2, 4, 2, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0
~~~
{: output}

> ## Exercise 
> 
> Can you write code to return the first 5 records from the SN7577 table in two different ways?
> 
> > ## Solution
> > 
> > ~~~
> > import sqlite3
> > con = sqlite3.connect('SN7577.sqlite')
> > cur = con.cursor()
> > 
> > # we can use the SQLite 'limit' clause to restrict the number of rows returned and then use 'fetchall'
> > cur.execute("SELECT * FROM SN7577 Limit 5")
> > rows = cur.fetchall()
> > 
> > for row in rows:
> >     print(row)
> > 
> > # we can use 'fetchone' in a for loop
> > cur.execute("SELECT * FROM SN7577")
> > for i in range(1,6):
> >     print(cur.fetchone())
> > 
> > # a third way would be to use the 'fetchmany()' method
> > 
> > cur.execute("SELECT * FROM SN7577")
> > rows = cur.fetchmany(5)
> > 
> > for row in rows:
> >     print(row)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Using Pandas to read a database table.

When you use Pandas to read a database table, you connect to the database in the same way as before using the SQLite3 `connect()` function and providing the filename of the database file.

Pandas has a method `read_sql_query` to which you provide both the string containing the SQL query you wish to run and also the connection variable.

The results from running the query are placed in a pandas Dataframe with the table column names automatically added.

~~~
con = sqlite3.connect('SN7577.sqlite')
df = pd.read_sql_query("SELECT * from SN7577", con)

# verify that result of SQL query is stored in the Dataframe
print(type(df))
print(df.shape)
print(df.head())

con.close()
~~~
{: .language-python}

## Saving a Dataframe as an SQLite table

There may be occasions when it is convenient to save the data in you pandas Dataframe as an SQLite table for future use or for access to other systems. This can be done using the `to_sql()` method.

~~~
con = sqlite3.connect('SN7577.sqlite')
df = pd.read_sql_query("SELECT * from SN7577", con)

# select only the row where the response to Q1 is 10 meaning undecided voter
df_undecided = df[df.Q1 == 10]
print(df_undecided.shape)

# Write the new Dataframe to a new SQLite table
df_undecided.to_sql("Q1_undecided", con)

# If you want to overwrite an existing SQLite table you can use the 'if_exists' parameter
#df_undecided.to_sql("Q1_undecided", con, if_exists="replace")
con.close()
~~~
{: .language-python}

~~~
(335, 202)
~~~
{: output}

## Deleting an SQLite table

If you have created tables in an SQLite database, you may also want to delete them.
You can do this by using the sqlite3 cursor `execute()` method

~~~
con = sqlite3.connect('SN7577.sqlite')
cur = con.cursor()

cur.execute('drop table if exists Q1_undecided')

con.close()
~~~
{: .language-python}


> ## Exercise
> 
> The code below creates an SQLite table as we have done in previous examples. Run this code to create the table.
> 
> ~~~
> con = sqlite3.connect('SN7577.sqlite')
> df_undecided = df[df.Q1 == 10]
> df_undecided.to_sql("Q1_undecided_v2", con)
> con.close()
> ~~~
> 
> Try using the following pandas code to delete (drop) the table.
> 
> ~~~
> pd.read_sql_query("drop table Q1_undecided_v2", con)
> ~~~
> {: .language-python}
> 
> 1. What happens?
> 2. Run this line of code again, What is different?
> 3. Can you explain the difference and does the table now exist or not?
> 
> 
> > ## Solution
> > 
> > 1. When the line of code is run the first time you get an error message : 'NoneType' object is not iterable.
> > 
> > 2. When you run it a second time you get a different error message:
> > DatabaseError: Execution failed on sql 'drop table Q1\_undecided\_v2': no such table: Q1\_undecided\_v2
> > 
> > 3. the `read_sql_query()` method is designed to send the SQL containing your query to the SQLite execution engine, which will execute the SQL and return the output to pandas which will create a Dataframe from the results.  
> > 
> > The SQL statement we sent is valid SQL but it doesn't return rows from a table, it simply reports success of failure (in dropping the table in this case). The first time we run it the table is deleted and a response to the effect is returned. The resonse cannot be converted to a Dataframe, hence the first error message, which is a pandas error.
> > 
> > When we run it for the second time, the table has already has already been dropped, so this time the error message is from SQLite saying the table didn't exist. Pandas recognises that this is an SQLite error message and simply passes it on to the user.
> > 
> > The moral of the story: pandas may be better for getting data returned into a Dataframe, but there are some things best left to the sqlite functions directly.
> >
> {: .solution}
{: .challenge}
