---
title: "Processing data from a file"
teaching: 25
exercises: 15
questions:
- "How can I read and write files?"
- "What kind of data files can I read?"

objectives:
- "Describe a file handle"
- "Use open to open files for reading"
- "Create and open files for writing or appending"
- "Use close to close files"
- "Explain what is meant by a record"

keypoints:
- "First key point."
---

## Reading and Writing datasets

In all of our examples so far, we have directly allocated values to variables in the code we have written before using the variables.

Python does have an `input()` function which will ask for input from the user. But for any large amounts of data this will be an  impractical way of collecting data. 

The reality is that most of the data that your program uses will be read from a file. Additionally, apart from when you are developing, most of your program output will be written to a file. 

In this episode we will look at how to read and write files of data in Python. 

There are in fact many different approaches to reading data files and which one you choose will depend on such things as the size of the file and the data format of the file.

In this episode we will;

* We will read a file which is in .csv (Comma Seperated Values) format. 
* We will use standard core Python functions to do this
* We will read the file one line at a time ( line = record = row of a table)
* We will perform simple processing of the file data and print the output
* We will split the file into smaller files based on some processing

The file we will be using is only a small file (131 data records), but the approach we are using will work for any size of file. Imagine 131M records. This is because we only process one record at a time so the memory requirements of the programs will be very small. Of course the larger the file the more the processing time required.

Other approaches to reading files will typically expect to read the whole file in one go. This can be efficient when you subsequently process the data but it require large amounts of memory to hold the entire file. We will look at this approach later when the look at the Pandas package.

For our examples in this episode we are going to use the SAFI_results.csv file. This is available for download [here](../data/SAFI_results.csv) and the description of the file is available [here](../data/SAFI_results_desc.txt)

The code assumes that the file is in the same directory as your notebook

We will build up our programs in simple steps

### Step 1 - Open the file , read through it and close the file

~~~
filename = "SAFI_results.csv"
f = open(filename, "r")    # open the file whose name is in filename, the 'r' means we want to read from the file
                           # the open function returns what is called a file handle. we use this to refer to the file
for line in f:             # We use a for loop to iterate through the file one line at a time. 
    print(line)            # we simply print the line

f.close                    # Always close th file at the end.
~~~

You can think of the file as being a list of strings. Each string in the list is one complete line from the file.

If you look at the output, you can see that the first record in the file is a header record containing column names. When we read a file in this way, the column headings have no significance, they are just another record in the file.

### Step 2 - Select a specific 'column' from the records in the file

We know that the first record in the file is a header record and we want to ignore it. To do this we call the readline() method of the file handle f. We don't need to assign the line that it will return to a variable as we are not going to use it. 

As we read the file the line variable is a string containing a complete record. The fields or columns of the record are seperated by each other by "," as it is a csv file.

As line is a string we can use the split() method to convert it to a list of column values. We are specicically going to select the column which is the 19th entry in the list (remember the list index starts at 0). This refers to the C01_respondent_roof_type column. We are going to examine the different roof types.

~~~
filename = "SAFI_results.csv"
f = open(filename, "r") 

# first line is a header so ignore it
f.readline()

for line in f:
    print(line.split(",")[18])    # index 18, the 19th column is C01_respondent_roof_type

f.close()
~~~

Having a list of the roof types from all of the records is one thing, but it is more likely that we would want a count of each type. By scanning up and down the previous output, there appears to be 3 different type, but we will play safe and assume there may be more.

### Step 3 - How many of each different roof types are there?

~~~
# 1
filename = "SAFI_results.csv"
f = open(filename, "r") 

# 2
f.readline()

# 3
grass_roof = 0
mabatisloping_roof = 0
mabatipitched_roof = 0
roof_type_other = 0

for line in f:
# 4
    roof_type = line.split(",")[18]
# 5    
    if roof_type == 'grass' :
        grass_roof += 1
    elif roof_type == 'mabatisloping' :
        mabatisloping_roof += 1
    elif roof_type == 'mabatipitched' :
        mabatipitched_roof += 1
    else :
        roof_type_other += 1
#6
f.close()

#7
print("There are ", grass_roof, " grass roofs")
print("There are ", mabatisloping_roof, " mabatisloping roofs")
print("There are ", mabatipitched_roof, " mabatipitchedg roofs")
print("There are ", roof_type_other, " other roof types")
~~~

What are we  doing here?

1. Open the file
2. Ignore the headerline
3. Initialise roof type variables to 0
4. Extract the C01_respondent_roof_type information from each record
5. Increment the appropriate variable
6. close the file
7. Print out the results

Instead of just printing out the counts of the roof types, you may want to extract all of one particular roof type to a seperate file. Let us assume we want all of the grass roof records to be writtent to a file.

~~~
# 1
filename = "SAFI_results.csv"
fr = open(filename, "r") 

filename = "SAFI_grass_roof.csv"
fw = open(filename, "w") 

for line in fr:
# 2    
    if line.split(",")[18] == 'grass' :
        fw.write(line)
    
#3
fr.close()
fw.close()
~~~

What are we  doing here?

1. Open the files. Because there are now two files, each has its own file handle. fr for the file we read and fw for the file we are going to write. (They are just variable names so you can use anything you like). For the file we are going to write to we use 'w' for the second parameter. If the file does not exist it will be created. If it does exist, then the contents will be overwritten. If we want to append to a file we can use 'a'; as the second parameter.
2. Because we are just testing a specific field from the record to have a certain value, we don't need to put it into a variable first. If the expression is True, then we use write() method to write the complete line just as we read it to the output file.
3. Close the files

In this example we didn't bother skipping the header line as it would fail the test in the if statement. If we did want to include it  we could have added the line 

~~~

fw.write(fr.readline())
~~~

before the for loop

> ## Exercise
> 
> From the SAFI_results.csv file extract all of the records where the C01_respondent_roof_type (index 18) has a value of 'grass' and the C02_respondent_wall_type (index 19) has a value of 'muddaub' and write them to a file. Within the same program write all of the records where C01_respondent_roof_type (index 18) has a value of 'grass' and the C02_respondent_wall_type (index 19) has a value of 'burntbricks' and write them to a seperate file. In both files include the header record.
> 
> > ## Solution
> > 
> > ~~~
> > filename = "SAFI_results.csv"
> > fr = open(filename, "r") 
> > 
> > filename = "SAFI_grass_roof_muddaub.csv"
> > fw1 = open(filename, "w") 
> > filename = "SAFI_grass_roof_burntbricks.csv"
> > fw2 = open(filename, "w") 
> > 
> > headerline = fr.readline()
> > fw1.write(headerline)
> > fw2.write(headerline)
> > 
> > for line in fr:
> >     if line.split(",")[18] == 'grass' :
> >         if line.split(",")[19] == 'muddaub' :
> >             fw1.write(line)
> >         if line.split(",")[19] == 'burntbricks' :
> >             fw2.write(line)    
> > 
> > fr.close()
> > fw1.close()
> > fw2.close()
> > ~~~
> > 
> {: .solution}
{: .challenge}
