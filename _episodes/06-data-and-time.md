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
- "You can use foramt strings to have dates/times displayed in any representation you like"
- "Internally date and times are stored in special data structures which allow you to access the component parts of dates and times"
---

## Date and Times in Python

Python can be very flexible in how it interprets 'strings' which you want to be considered as a date, time or date and time.

All you have to do is tell Python how the various parts of the date and/or time are represented in your 'string'.

You can do this by creating a format. In a format different case sensitive letters preceded by the '%' character act as placeholders for parts of the date/time. 

A full list of the letters used and what they represennt can be found towards the end of [this](https://docs.python.org/3/library/datetime.html) section of the official Python documentation.

To use the date and time functions you need to import the datetime module.

There is a `today()` method which allows you to get the current date and time. 
By default it is displayed in a format similar to the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard format.

~~~
from datetime import datetime

today = datetime.today()
print('ISO     :', today)
~~~

We can however use our own formatting, for example if we wanted words instead of number and the 4 digit year at the end we could use the following.

~~~
format = "%a %b %d %H:%M:%S %Y"

s = today.strftime(format)
print('strftime:', s)
print(type(s))

d = datetime.strptime(s, format)
print('strptime:', d.strftime(format))
print(type(d))
~~~

`strftime` converts a datetime object to a string and `strptime` creates a datetime object from a string. 
When you print them using the same format string, they look the same.

The format of the date fields in the SAFI_results.csv file have been generated automatically to comform to the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard.

When we read the file and extract the date fields, they are of type string. Before we can use them as dates, we need to convert them into Python date objects.

In the format string we use below, the '-' , ':' , 'T'and 'Z' characters are just that, caharacters in the string representing the date/time. 
Only the character preceded with '%' have special meanings.

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

### Components of dates and times

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


> ## Exercise
> 
> How do you interpret the last result?
> 
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

> ## Exercise
> 
> 1. In the SAFI_results.csv file the A01_interview_date field (index 1) contains a date in the form of 'dd/mm/yyyy'. Read the file and calculate the differences in days (because the interview date is only given to the day) between the A01_interview_date values and the A04_start values. You will need to create a format string for the A01_interview_date field.
> 
> 2. Looking at the results here and from the previous section of code. Do you think the use of the smartphone data entry system for the survey was being used in real time?
> 
> > ## Solution
> > 
> > ~~~
> > 
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
> > 
> > ~~~
> > 
> {: .solution}
{: .challenge}
