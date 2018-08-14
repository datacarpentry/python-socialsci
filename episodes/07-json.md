---
title: "Processing JSON data"
teaching: 30
exercises: 15
questions:
- "What is JSON format?"
- "How can I extract specific data items from a JSON record?"
- "How can I convert an array of  JSON record into a table?"
objectives:
- "Describe the JSON data format"
- "Understand where JSON is typically used"
- "Appreciate some advantages of using JSON over tabular data"
- "Appreciate some dis-advantages of processing JSON documents"
- "Compare JSON to the Python Dict data type"
- "Use the JSON package to read a JSON file"
- "Display formatted JSON"
- "Select and display specific fields from a JSON document"
- "Write tabular data from selected elements from a JSON document to a csv file"
keypoints:
- "JSON is a popular data format for transferring data used by a great many Web based APIs"
- " The JSON data format is very similar to the Python Dictionary structure."
- "The complex structure of a JSON document means that it cannot easily be 'flattened' into tabular data"
- "We can use Python code to extract values of interest and place them in a csv file"
---

## More on Dictionaries

In the last episode we introduced the dictionary object.

We created dictionaries and we added `key : value` pairs to the dictionary.

In all of the examples that we used, the `value` was always a simple data type like an integer or a string.

The `value` associated with a `key` in a dictionary can be of any type including  a `list` or even another `dictionary`.

We created a simple dictionary object with the following code:

~~~
personDict = {'Name' : 'Peter'}
personDict['Location'] = 'Manchester'

print(personDict)
~~~
{: .language-python}

~~~
{'Name': 'Peter', 'Location': 'Manchester'}
~~~
{: .output}

So far the keys in the dictionary each relate to a single piece of information about the person. What if we wanted to add a list of items?

~~~
personDict['Children'] = ['John', 'Jane', 'Jack']
personDict['Children_count'] = 3
print(personDict)
~~~
{: .language-python}

~~~
{'Name': 'Peter', 'Children': ['John', 'Jane', 'Jack'], 'Children_count': 3, 'Location': 'Manchester'}
~~~
{: .output}

Not only can I have a key where the value is a list, the value could also be another dictionary object. Suppose I want to add some telephone numbers

~~~
personDict['phones'] = {'home' : '0102345678', 'mobile' : '07770123456'}
print(personDict.values())

# adding another phone
personDict['phones']['business'] =  '0161234234546'
print(personDict)
~~~
{: .language-python}

~~~
dict_values(['Peter', ['John', 'Jane', 'Jack'], {'home': '0102345678', 'mobile': '07770123456'}, 3, 'Manchester'])
{'Name': 'Peter', 'Children': ['John', 'Jane', 'Jack'], 'phones': {'home': '0102345678', 'mobile': '07770123456', 'business': '0161234234546'}, 'Children_count': 3, 'Location': 'Manchester'}
~~~
{: .output}

> ## Exercise
>
> 1. Using the personDict as a base add information relating to the persons home and work addresses including postcodes.
> 2. Print out the postcode for the work address.
> 3. Print out the names of the children on seperate lines (i.e. not as a list)
>
> > ## Solution
> >
> > ~~~
> > personDict['Addresses'] = {'Home' : {'Addressline1' : '23 acacia ave.', 'Addressline2' : 'Romford', 'PostCode' : 'RO6 5WR'},
> >                           'Work' : {'Addressline1' : '19 Orford Road.', 'Addressline2' : 'London', 'PostCode' : 'EC4J 3XY'}
> >                           }
> >
> > print(personDict['Addresses']['Work']['PostCode'])
> >
> > for child in personDict['Children']:
> >     print(child)
> >     
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

The ability to create dictionaries containing lists and other dictionaries, makes the dictionary object very versatile, you can create an arbitrarily complex data structure of dictionaries within dictionaries.

In practice you will not be doing this manually, instead like most data you will read it in from a file.

## The JSON data format

The JSON data format was designed as a way of allowing different machines or processes within machines to communicate with each other by sending messages constructed in a well defined format. JSON is now the preferred data format used by APIs (Application Programming Interfaces).

The JSON format although somewhat verbose is not only Human readable but it can also be mapped very easily to a Python dictionary object.

We are going to read a file of data formatted as JSON, convert it into a dictionary object in Python then selectively extract Key-Value pairs and create a csv file from the extracted data.

The JSON file we are going to use is the [SAFI.json](../data/SAFI.json) file. This is the output file from an electronic survey system called ODK. The JSON represents the answers to a series of survey questions. The questions themselves have been replaced with unique Keys, the values are the answers.

Because detailed surveys are by nature nested structures making it possible to record different levels of detail or selectively ask a set of specific questions based on the answer given a previous question, the structure of the answers for the survey can not only be complex and convoluted, it could easily be different from one survey respondent's set of answers to another.

### Advantages of JSON

* Very popular data format for APIs (e.g. results from an Internet search)
* Human readable
* Each record (or document as they are called) is self contained. The equivalent of the column name and column values are in every record.
* Documents do not all have to have the same structure within the same file
* Document structures can be complex and nested

### Dis-advantages of JSON

* It is more verbose than the equivalent data in csv format
* Can be more difficult to process and display than csv formatted data

## Use the JSON package to read a JSON file

~~~
import json

with open('SAFI.json') as json_data:
    d = json.load(json_data)
    print(type(d))
    print(type(d[0]))
    print(json.dumps(d[0], indent=2))
~~~
{: .language-python}

~~~
<class 'list'>
<class 'dict'>
{
  "G02_months_lack_food": [
    "Jan"
  ],
  "G01_no_meals": 2,
  "E_no_group_count": "2",
  "A03_quest_no": "01",
...
~~~
{: .output}

Points to note:

1. We import the json package with an import statement.
2. We have chosen to use the `with` statement to open the SAFI.json file. Notice the `:` at the end of the line and the subsequent indentation. The `with` statement is in effect until we un-indent. At which time the file will automatically be closed. So we don't need to do so explicitly.
3. 'json_data' is the file handle.
4. The `json.load` method is passed the file handle and reads the complete file.
5. The variable `d` is a list of dictionaries. (When we read the csv file we considered it to be a list of strings).
6. The `json.dumps` method can be used to print either the entire file or a specific dictionary from the list in a formatted manner by using the indent parameter)


By default the order in which the keys of the dictionary are printed is not guaranteed. If we want them in sorted order we can have them sorted by using the `sort_keys` parameter

~~~
print(json.dumps(d[0], indent=2, sort_keys=True))
~~~
{: .language-python}

~~~
{
  "A01_interview_date": "2016-11-17",
  "A03_quest_no": "01",
  "A04_start": "2017-03-23T09:49:57.000Z",
  "A05_end": "2017-04-02T17:29:08.000Z",
  "A06_province": "province1",
...
}
~~~
{: .output}

## Extracting specific fields from a JSON document

If we want to extract fields from a JSON document, the first step isto convert the JSON document into a Python dictionary. We have in fact already done this with the

~~~
d = json.load(json_data)
~~~
{: .language-python}

line. `d` a list object and each entry in the list is a Dictionary object.

## Extract the fields we want into a flat format

Despite the arbitrary complexity of a JSON document or a Python dictionary object, we can adopt a very systematic approach to extracting individual fields form the structure.

The story so far: Our JSON file has been read into a variable `d`. We know that `d` is a list of dictionaries. Each dictionary represents a JSON document ( a record).

We can print the contents of the first dictionary in the list with

~~~
print(json.dumps(d[0], indent=2, sort_keys=True))
~~~
{: .language-python}

> ## Exercise
>
> 1. In the output from the code above there is a key with the name of `D_curr_crop`. Find it and by looking at the indentation and the `[` (lists) and `{` (dictionaries) describe in English how you could find the first occurrence of  `D_curr_crop` starting with `d`.
>
> 2. Use a print statement to find out what it is.
>
> > ## Solution
> >
> > - `d` is a list of dictionaries
> > - `d[0]` is the first dictionary
> > - within `d[0]` there is a key `D_plots` whose value is a list and contains dictionaries
> > - `d[0]['D_plots'][0]` is the first dictionary in the list
> > - within `d[0]['D_plots'][0]` there is a key `D_crops` which is also a list of dictionaries
> > - `d[0]['D_plots'][0]['D_crops'][0]` is the first dictionary in the list
> > - within this dictionary there is a key `D_curr_crop`
> >
> > Being able to start at the outermost level and work your way in is very important when you need to extract specific items.
> >
> > ~~~
> > print(d[0]['D_plots'][0]['D_crops'][0]['D_curr_crop'])
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}


Being able to drill down in this way is very useful in helping you get a feel for the JSON data structure. In practice it is more likely that instead of returning the first occurrence of `D_curr_crop` you will want to return all of them. This requires a little more programming and to be aware of two potential problems.

1. `D_curr_crop` may not exist in any particular dictionary within `D_crops`
2. any of the lists `D_plots` or `D_crops` could be missing or just empty lists (`[]`)

In our first attempt we will ignore these problems. 

~~~
for farms in d:
    plot = farms['D_plots']
    for crops in plot:
        crop = crops['D_crops']
        for curr_crops in crop:
            print(curr_crops['D_curr_crop'])
~~~
{: .language-python}

~~~
maize
maize
maize
tomatoes
vegetable
maize
maize
maize
sorghum
...
~~~
{: .output}

In this version we test if all of the keys exist.
This could be extended to check that the lists are not empty.

~~~
for farms in d:
    if 'D_plots' in farms :
        plot = farms['D_plots']
        for crops in plot:
            if 'D_crops' in crops :
                crop = crops['D_crops']
                for curr_crops in crop:
                    if 'D_curr_crop' in curr_crops:
                        print(curr_crops['D_curr_crop'])
~~~
{: .language-python}

We can now produce a list of all of the crops in all of the plots in all of the farms.

We can also create a unique set of all of the crops grown using the Python `set` data structure as shown in the code below. A set is like a list but does not allow duplicate values (but doesn't raise an error if you try to add a duplicate).

~~~
unique_crops = set()
for farms in d:
    if 'D_plots' in farms :
        plot = farms['D_plots']
        for crops in plot:
            if 'D_crops' in crops :
                crop = crops['D_crops']
                for curr_crops in crop:
                    if 'D_curr_crop' in curr_crops:
                        #print(curr_crops['D_curr_crop'])
                        unique_crops.add(curr_crops['D_curr_crop'])
print(unique_crops)
~~~
{: .language-python}

~~~
{'peanut', 'potatoes', 'tomatoes', 'other', 'vegetable', 'amendoim', 'sunflower', 'bananas', 'sesame', None, 'cucumber', 'onion', 'sorghum', 'piri_piri', 'baby_corn', 'cabbage', 'ngogwe', 'maize', 'pigeonpeas', 'beans'}
~~~
{: .output}

Simply having a list of all of the crops is unlikely to be enough. What you are really interested in is which farm grows which crops in which plot.

We can accumulate this information as we move through the list of dictionary objects. At the top level, `farm`, there is a unique identifier `A03_quest_no` which we can use. for the plot and the crop within the plot we will create our own simple indexing system (`plot_no` and `crop_no`). At the end instead of just printing the crop name, we also print the details of where this crop is being grown.

~~~
for farms in d:
    plot_no = 0
    id = farms['A03_quest_no']
    if 'D_plots' in farms :
        plot = farms['D_plots']
        for crops in plot:
            crop_no = 0
            plot_no += 1
            if 'D_crops' in crops :
                crop = crops['D_crops']
                for curr_crops in crop:
                    crop_no += 1
                    if 'D_curr_crop' in curr_crops:
                        print("Farm no ", id," grows ", curr_crops['D_curr_crop']," in plot", plot_no , " and it is crop number ", crop_no)
~~~
{: .language-python}

~~~
Farm no 01 grows maize in plot 1 and it is crop number 1
Farm no 01 grows maize in plot 2 and it is crop number 1
Farm no 01 grows maize in plot 1 and it is crop number 1
Farm no 01 grows tomatoes in plot 2 and it is crop number 1
Farm no 01 grows vegetable in plot 3 and it is crop number 1
...
~~~
{: .output}

The final stage of this data extraction process is to save the extracted data to a file for subsequent use.

Rather than manually appending all of the information items into a string with `,` seperating each, we can use the `csv` module.

To do this we need to create a `csv.writer` object and use it to write complete rows of data at a time. `csv.writer` expects the data to be provided as a list of items.

For the header row we provide a list of strings containing the colmn names we want and at the end we proivide the data items in a list as well.

~~~
import csv
filename = "SAFI_crops.csv"
fw = open(filename, 'w')
cf = csv.writer(fw, lineterminator='\n')

# write the header
cf.writerow(["Farm","plot_no","plot_area","crop_no","crop_name"])

for farms in d:
    plot_no = 0
    id = farms['A03_quest_no']
    if 'D_plots' in farms :
        plot = farms['D_plots']
        for crops in plot:
            crop_no = 0
            plot_no += 1
            if 'D_crops' in crops :
                plot_area = crops['D02_total_plot']
                crop = crops['D_crops']
                for curr_crops in crop:
                    crop_no += 1
                    if 'D_curr_crop' in curr_crops:
                        #print( id, plot_no , plot_area , crop_no, curr_crops['D_curr_crop'])
                        cf.writerow([id, plot_no , plot_area , crop_no, curr_crops['D_curr_crop']])

fw.close()
~~~
{: .language-python}
