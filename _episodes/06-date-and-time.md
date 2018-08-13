---
title: "Dates and Time"
teaching: 15
exercises: 10
questions:
- "How are dates and time represented in Python?"
- "How can I manipulate dates and times?"
objectives:
- "Describe some of the datetime functions available in Python"
- "Describe the use of format strings to describe the layout of a date and/or time string"
- "Make use of date arithmetic"
keypoints:
- "Date and Time functions in Python come from the datetime library, which needs to be imported"
- "You can use format strings to have dates/times displayed in any representation you like"
- "Internally date and times are stored in special data structures which allow you to access the component parts of dates and times"
---

## Date and Times in Python

Python can be very flexible in how it interprets 'strings' which you want to be considered as a date, time, or date and time, but you have to tell Python how the various parts of the date and/or time are represented in your 'string'.

To use the date and time functions you need to import the `datetime` module.
You can do this by creating a `format`. In a `format`, different case sensitive characters preceded by the `%` character act as placeholders for parts of the date/time, for example `%Y` represents year formatted as 4 digit number such as 2014.

A full list of the characters used and what they represent can be found towards the end of the [datetime](https://docs.python.org/3/library/datetime.html) section of the official Python documentation.


There is a `today()` method which allows you to get the current date and time.
By default it is displayed in a format similar to the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard format.

~~~
from datetime import datetime

today = datetime.today()
print('ISO     :', today)
~~~
{: .language-python}

~~~
ISO     : 2018-04-12 16:19:17.177441
~~~
{: .output}

We can use our own formatting instead. For example, if we wanted words instead of number and the 4 digit year at the end we could use the following.

~~~
format = "%a %b %d %H:%M:%S %Y"

s = today.strftime(format)
print('strftime:', s)
print(type(s))

d = datetime.strptime(s, format)
print('strptime:', d.strftime(format))
print(type(d))
~~~
{: .language-python}

~~~
strftime: Thu Apr 12 16:19:17 2018
<class 'str'>
strptime: Thu Apr 12 16:19:17 2018
<class 'datetime.datetime'>
~~~
{: .output}

`strftime` converts a datetime object to a string and `strptime` creates a datetime object from a string.
When you print them using the same format string, they look the same.

The format of the date fields in the SAFI_results.csv file have been generated automatically to comform to the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard.

When we read the file and extract the date fields, they are of type string. Before we can use them as dates, we need to convert them into Python date objects.

In the format string we use below, the `-` , `:` , `T` and `Z` characters are just that, characters in the string representing the date/time.
Only the character preceded with `%` have special meanings.

Having converted the strings to datetime objects, there are a variety of methods that we can use to extract different components of the date/time.

~~~
from datetime import datetime


format = "%Y-%m-%dT%H:%M:%S.%fZ"
f = open('SAFI_results.csv', 'r')

#skip the header line
line = f.readline()

# next line has data
line = f.readline()

strdate_start = line.split(',')[3]  # A04_start
strdate_end = line.split(',')[4]    # A05_end

print(type(strdate_start), strdate_start)
print(type(strdate_end), strdate_end)


# the full date and time
datetime_start = datetime.strptime(strdate_start, format)
print(type(datetime_start))
datetime_end = datetime.strptime(strdate_end, format)

print('formatted date and time', datetime_start)
print('formatted date and time', datetime_end)


# the date component
date_start = datetime.strptime(strdate_start, format).date()
print(type(date_start))
date_end = datetime.strptime(strdate_end, format).date()

print('formatted start date', date_start)
print('formatted end date', date_end)

# the time component
time_start = datetime.strptime(strdate_start, format).time()
print(type(time_start))
time_end = datetime.strptime(strdate_end, format).time()

print('formatted start time', time_start)
print('formatted end time', time_end)


f.close()
~~~
{: .language-python}

~~~
<class 'str'> 2017-03-23T09:49:57.000Z
<class 'str'> 2017-04-02T17:29:08.000Z
<class 'datetime.datetime'>
formatted date and time 2017-03-23 09:49:57
formatted date and time 2017-04-02 17:29:08
<class 'datetime.date'>
formatted start date 2017-03-23
formatted end date 2017-04-02
<class 'datetime.time'>
formatted start time 09:49:57
formatted end time 17:29:08
~~~
{: .output}

## Components of dates and times

For a date or time we can also extract individual components of them.
They are held internally in the datetime datastructure.

~~~
# date parts.
print('formatted end date', date_end)
print(' end date year', date_end.year)
print(' end date month', date_end.month)
print(' end date day', date_end.day)
print (type(date_end.day))

# time parts.

print('formatted end time', time_end)
print(' end time hour', time_end.hour)
print(' end time minutes', time_end.minute)
print(' end time seconds', time_end.second)
print(type(time_end.second))
~~~
{: .language-python}

~~~
formatted end date 2017-04-02
 end date year 2017
 end date month 4
 end date day 2
<class 'int'>
formatted end time 17:29:08
 end time hour 17
 end time minutes 29
 end time seconds 8
<class 'int'>
~~~
{: .output}

## Date arithmetic

We can also do arithmetic with the dates.

~~~
date_diff = datetime_end - datetime_start
date_diff
print(type(datetime_start))
print(type(date_diff))
print(date_diff)

date_diff = datetime_start - datetime_end
print(type(date_diff))
print(date_diff)
~~~
{: .language-python}

~~~
<class 'datetime.datetime'>
<class 'datetime.timedelta'>
10 days, 7:39:11
<class 'datetime.timedelta'>
-11 days, 16:20:49
~~~
{: .output}

> ## Exercise
>
> How do you interpret the last result?
{: .challenge}

The code below calculates the time difference between supposedly starting the survey and ending the survey (for each respondent).


~~~
from datetime import datetime

format = "%Y-%m-%dT%H:%M:%S.%fZ"

f = open('SAFI_results.csv', 'r')

line = f.readline()

for line in f:
    #print(line)
    strdate_start = line.split(',')[3]
    strdate_end = line.split(',')[4]

    datetime_start = datetime.strptime(strdate_start, format)
    datetime_end = datetime.strptime(strdate_end, format)
    date_diff = datetime_end - datetime_start
    print(datetime_start, datetime_end, date_diff )


f.close()
~~~
{: .language-python}

~~~
2017-03-23 09:49:57 2017-04-02 17:29:08 10 days, 7:39:11
2017-04-02 09:48:16 2017-04-02 17:26:19 7:38:03
2017-04-02 14:35:26 2017-04-02 17:26:53 2:51:27
2017-04-02 14:55:18 2017-04-02 17:27:16 2:31:58
2017-04-02 15:10:35 2017-04-02 17:27:35 2:17:00
2017-04-02 15:27:25 2017-04-02 17:28:02 2:00:37
2017-04-02 15:38:01 2017-04-02 17:28:19 1:50:18
2017-04-02 15:59:52 2017-04-02 17:28:39 1:28:47
2017-04-02 16:23:36 2017-04-02 16:42:08 0:18:32
...
~~~
{: .output}

> ## Exercise
>
> 1. In the SAFI\_results.csv file the `A01_interview_date field` (index 1) contains a date in the form of 'dd/mm/yyyy'. Read the file and calculate the differences in days (because the interview date is only given to the day) between the `A01_interview_date` values and the `A04_start` values. You will need to create a format string for the `A01_interview_date` field.
>
> 2. Looking at the results here and from the previous section of code. Do you think the use of the smartphone data entry system for the survey was being used in real time?
>
> > ## Solution
> >
> > ~~~
> > from datetime import datetime
> >
> > format1 = "%Y-%m-%dT%H:%M:%S.%fZ"
> > format2 = "%d/%m/%Y"
> >
> > f = open('SAFI_results.csv', 'r')
> >
> > line = f.readline()
> >
> > for line in f:
> >     A01 = line.split(',')[1]
> >     A04 = line.split(',')[3]
> >    
> >     datetime_A04 = datetime.strptime(A04, format1)
> >     datetime_A01 = datetime.strptime(A01, format2)
> >     date_diff = datetime_A04 - datetime_A01
> >     print(datetime_A04, datetime_A01, date_diff.days )
> >      
> > f.close()
> > ~~~
> > {: .language-python}
> >
> {: .solution}
{: .challenge}
