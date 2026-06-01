---
title: Creating re-usable code
teaching: 25
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objectives

- Describe the syntax for a user defined function
- Create and use simple functions
- Explain the advantages of using functions

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are user defined functions?
- How can I automate my code for re-use?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Defining a function

We have already made use of several Python built-in functions like `print`, `list`, and `range`. But in addition to the functions provided by Python, you can write your own as well. Functions are used when a section of code needs to be repeated several times in a program, it saves you rewriting it. In reality, you rarely need to repeat the _exact same_ code. Usually there will be some variation, for example in the variables the code needs to be run on. Because of this, when you create a function you are allowed to specify a set of `parameters` or arguments to the function.

When we used the `print` function we provided the text we wanted to `print` as a `parameter`. Typically whenever we use the `print` function, we pass a different `parameter` value. The ability to specify parameters make functions very flexible.

```python
def get_item_count(items_str,sep):
    '''
    This function takes a string with a list of items and the character that separates the items, and returns the number of items in the list

    '''
    items_list = items_str.split(sep)
    num_items = len(items_list)
    return num_items

items_owned = "bicycle;television;solar_panel;table"
print(get_item_count(items_owned,';'))
```

```output
4
```

![](fig/functionAnatomy.png){alt='Python\_Repl'}

Points to note:

1. The definition of a function (or procedure) starts with the keyword _def_ and is followed by the name you wish to give to the function, with any parameters used by the function in between parentheses.
2. The definition clause ends in`:` which causes indentation on the next and subsequent lines. All of these lines are the statements which make up the function. The function ends where the indentation ends.
3. Within the function, the parameters behave as variables whose initial values will be those that were given when the function was called.
4. Functions usually "return" something, which is the result of the procedure applied to the parameters, and is the value assigned to the variable on the left-hand side of the call to the function. This is specified using the `return` keyword.
5. You call (run the code) of a function by providing its name and values for its parameters, the same way you would for any built-in function.
6. Once the definition of the function has been executed, it becomes part of Python for the current session and can be used anywhere.
7. At the beginning of the function code we have a multiline `comment` denoted by the `'''` at the beginning and end. This kind of comment is known as a `docstring` and can be used anywhere in Python code as a documentation aid. It is particularly common, and indeed best practice, to use them to give a brief description of the function. This is because this description will be displayed along with the parameters when you use the help() function or `shift` + `tab` in Jupyter.
8. Variables that are defined within a function only exist within the function itself, they cannot be used outside in the main program.

Our `get_item_count` function has two parameters which must be provided every time the function is called. You need to provide the parameters in the right order or to explicitly name the parameter you are referring to and use the `=` sign to give it a value.

In many cases, there is a value for a certain parameter that is more likely than others. In that case the value can be set as "default", and that is the value that will be taken if the user does not specify a value.

```python
def get_item_count(items_str, sep=';'):
    '''
    This function takes a string with a list of items and the character that they're separated by and returns the number of items
    '''
    items_list = items_str.split(sep)
    num_items = len(items_list)
    return num_items


print(get_item_count(items_owned)) # Note that the separator is not specified
```

```output
4
```

The only change we made is to provide a default value for the `sep` parameter. Now if the user does not provide a value, then the value _;_ will be used. Because `items_str` is the first parameter, we can specify its value by position, without having to explicitly name it, but it could be clearer to explicitly name all parameters.

```python
print(get_item_count(items_owned, sep = ','))
print(get_item_count(items_str = items_owned, sep=';'))
```

```output
1
4
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

1. Write a function definition to create an identifier for each survey participant. 
The function requires three parameters: `first_name`, `surname`, and their six-digit staff ID number (`id`); and returns an identifier formed by the last letter of the first name, the two middle numbers of the staff ID, and the last letter of the surname.

2. Suppose that in addition to the identifier you also wanted to generate a username that each participant could use to log into a platform where you will display their results, formed by their first name initial plus the whole surname, all in lowercase; and also return their full name as one string. 
Would you (or should you) have three separate functions or could you write a single function to provide all three values together?

:::::::::::::::  solution

## Solution

1. A function to calculate the unique identifier as described in the exercise could be:

```python
def generate_identifier(first_name, surname, id):
    """
    Generates an identifier formed by: the last letter of the first name, the two middle numbers of the staff ID, and the length of their surname.
    Takes in first_name, surname, and id; and returns the identifier.
    """
    identifier = first_name[-1] + id[2:4] + str(len(surname))
    return identifier
```

2. It depends. As a rule-of-thumb, functions should __do one thing and one thing only, and do it well.__
If you always need these three pieces of information together, the 'one thing' could be
'for each participant, generate an identifier, username, and full name'. In that case, your function could look like this:

```python
# Method 1 - single function
def generate_user_attributes(first_name, surname, id):
    """
    Generates attributes needed for survey participant.
    Takes in first_name, surname, and id; and returns an identifier, username, and full name.
    """
    identifier = first_name[-1] + id[2:4] + str(len(surname))
    username = (first_name[0] + surname).lower()
    full_name = first_name + " " + surname
    return identifier, username, full_name
```

It may be better, however, to break your function down: one for each piece of information you are
generating. Your functions could look like this:

```python
# Method 2 - separate functions
def gen_identifier(first_name, surname, id):
    """
    Generates an identifier formed by: the last letter of the first name, the two middle numbers of the staff ID, and the length of their surname.
    Takes in first_name, surname, and id; and returns the identifier.
    """
    identifier = first_name[-1] + id[2:4] + str(len(surname))
    return identifier


def gen_username(first_name, surname):
    """
    Generates an username formed by: their first name initial plus the whole surname, all in lowercase.
    Takes in first_name and surname, and returns the username.
    """   
    username = (first_name[0] + surname).lower()
    return username


def display_full_name(first_name, surname):
    """
    Displays the participant's full name.
    Takes in first_name and surname, and returns the full name.
    """   
    full_name = first_name + " " + surname
    return full_name
```

We could then rewrite our first function that returns all attributes needed:

```python
def gen_attributes(first_name, surname, id):
    """
    Generates attributes needed for survey participant.
    Takes in first_name, surname, and id; and returns an identifier, username, and full name.
    """
    identifier = gen_identifier(first_name, surname, id)
    username = gen_username(first_name, surname)
    full_name = display_full_name(first_name, surname)

    return identifier, username, full_name
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Using libraries

The functions we have created above only exist within the Jupyter notebook in which they have been defined, and only for the duration of the session. If you start a new Jupyter notebook you will have to copy and paste the functions in to define them again. If all of your code is in a single file or notebook this isn't really a problem. But if your project gets larger, it can be hard to keep track of where each function is saved. 

There are many (thousands) of useful functions which other people have written and have made available to all Python users by creating libraries (also referred to as packages or modules) of functions. You can find out more about existing Python packages by visiting [pypi.org/](https://pypi.org/).

There are several ways to install third party packages to be able to use them in your own code. If you have Python 3.4 or later, it includes by default a package installer called [pip](https://pypi.org/project/pip/), which can be used to install packages. From a Jupyter notebook, you would use the syntax:

```python
!pip install <package_name>
```

After installing the package, you still need to "import" the package into your notebook to be able to use the functions contained within the package. This is done by running:
```python
import <package_name>
```

As all of these packages are produced by third parties independently of each other, there is the strong possibility that there may be clashes in function names, this is there are functions in two different packages that have the exact same name. Therefore, when you are calling a function from a package that you have imported, you can prefix the function name with the package name, which makes it clear which function you are expecting to run. This can make for long-winded function names, though! The `import` statement allows you to also specify an "alias" for the package, which you must then use instead of the full package name. For example:
```python
import numpy as np
```

Many aliases (specified after the `as` keyword) are nearly universally adopted conventions used for very popular libraries, and you will almost certainly come across them when searching for example code. In future lessons, we will be importing the `csv`, `json`, `pandas`, `numpy`, and `matplotlib` modules, which we will describe as we use them. The code that we will use to import these packages is:

```python
import csv
import json
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

Matplotlib is a very large library broken up into what can be thought of as sub-libraries. As we will only be using the functions contained in the `pyplot` sub-library we can specify that explicitly when we import. This saves time and space, and does not affect how we call the functions in our code.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Functions are used to create re-usable sections of code
- Using parameters with functions make them more flexible
- You can use functions written by others by importing the libraries containing them into your code

::::::::::::::::::::::::::::::::::::::::::::::::::


