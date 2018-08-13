---
title: "Creating re-usable code"
teaching: 25
exercises: 15
questions:
- "What are user defined functions?"
- "How can I automate my code for re-use?"
objectives:
- "Describe the syntax for a user defined function"
- "Create and use simple functions"
- "Explain the advantages of using functions"
keypoints:
- "Functions are used to create re-usable sections of code"
- "Using parameters with functions make them more flexible"
- "You can use functions written by others by importing the libraries containing them into your code"
---

## Defining a function

We have already made use of several Python builtin functions like `print`, `list` and `range`.

In addition to the functions provided by Python, you can write your own functions.

Functions are used when a section of code needs to be repeated at various different points in a program. It saves you re-writing it all. In reality you rarely need to repeat the exact same code. Usually there will be some variation in variable values needed. Because of this, when you create a function you are allowed to specify a set of `parameters` which represent variables in the function.

In our use of the `print` function we have provided as a parameter, whatever it is we wanted to `print`. Typically whenever we use the print function we pass a different parameter value.

The ability to specify parameters make functions very flexible.

~~~
def power(num, raisedto):
    '''
    This function takes the value specified in the 'num' parameter and raises it to
    the power of the number specified in the 'raisedto' parameter and returns the result
    '''
    x = 1
    for i in range(raisedto):
        x = x * num
    return x

print(power(2,3))
~~~
{: .language-python}

~~~
8
~~~
{: .output}

Points to note:

1.	The definition of a function (or procedure) starts with the def keyword and is followed by the name of the function with any parameters used by the function in brackets.
2.	The definition clause is terminated with a `:` which causes indentation on the next and subsequent lines. All of these lines form the statements which make up the function. The function ends after the indentation is removed.
3.	Within the function, the parameters behave as variables whose initial values will be those that they were given when the function was called.
4.	functions have a return statement which specifies the value to be returned. This is the value assigned to the variable on the left-hand side of the call to the function. (power in the example above)
5.	You call (run the code) of a function simply by providing its name and values for its parameters the same way you would for any builtin function.
6.  Once the definition of the function has been executed, it becomes part of Python for the current session and can be used anywhere.
7. Like any other builtin function you can use `shift` + `tab` in Jupyter to see the parameters.
8. At the beginning of the function code we have a multiline  `comment` denoted by the `'''` at the beginning and end. This kind of comment is known as a `docstring` and can be used anywhere in Python code as a documentation aid. It is particularly common, and indeed best practice, to use them to give a brief description of the function at the beginning of a function definition in this way. This is because this description will be displayed along with the parameters when you use the help() function or `shift` + `tab` in Jupyter.
9. The variable `x` defined within the function only exists within the function, it cannot be used outside in the main program.


In our `power` function we have two parameters which must be provided every time the function is used. You need to  provide the parameters in the right order or to explicitly name the parameter you are referring to and use the `=` sign to give it a value.

In many cases of functions we want to provide default values for parameters so the user doesn't have to. We can do this in the following way

~~~
def power2(num, raisedto = 2):
    '''
    This function takes the value specified in the 'num' parameter and raises it to
    the power of the number specified in the 'raisedto' parameter and returns the result
    '''
    x = 1
    for i in range(raisedto):
        x = x * num
    return x

power2(3)
~~~
{: .language-python}

~~~
9
~~~
{: .output}

The only change we have made is to provide a default value for the `raisdto` parameter. Now if the user does not provide a value, then the value of 2 will be used. Because `num` is the first parameter we can specify its value by position. We could however have explicitly named the parameters we were referring to.

~~~
print(power2(num = 4))
print(power(raisedto = 6, num = 4))
~~~
{: .language-python}

~~~
16
4096
~~~
{: .output}

> ## Exercise
>
> 1. Write a function definition to calculate the volume of a cuboid. The function will use three parameters `H`, `W` and `L` and return the volume.
>
> 2. Supposing that in addition to the volume I also wanted to calculate the surface area and the sum of all of the edges. Would I (or should I) have three separate functions or could I write a single function to provide all three values together?
>
> > ## Solution
> >
> > ~~~
> > def cuboid(H,W,L):
> >     vol = H*W*L
> >     sa = 2*(H*W + H*L + L*W)
> >     edges = 4*(H + W + L)
> >     return vol, sa, edges
> >
> > volume, surface_area, length_of_edges = cuboid(2,3,4)
> > print("Volume = ", volume, " Surface_area = ", surface_area, " Sum of edges = ", length_of_edges)
> > ~~~
> > {: .language-python}
> >
> > The cuboid function above returns all three values from a single call. This means that you need three variables in which to place the 3 returned values. If you do not provide three variables, you will get an error.
> > Unless you are always going to use the 3 values, it would probably be better in this case to use distinct functions for these three cases.
> >
> {: .solution}
{: .challenge}

## Using libraries

The functions we have created above only exist for the duration of the session in which they have been defined. If you start a new Jupyter notebook you will have to run the code to define them again.

If all of your code is in a single file or notebook this isn't really a problem.

There are however many (thousands) of useful functions which other people have written and have made available to all Python users by creating libraries (also referred to as packages or modules) of functions.

You can find out what all of these libraries are and their contents by visiting the main (python.org) site.

We need to go through a 2-step process before we can use them in our own programs.

Step 1.  use the `pip` command from the commandline. `pip` is installed as part of the Python install and is used to fetch the package from the Internet and install it in your Python configuration.

~~~
> pip install <package name>
~~~
{: .bash}

pip stands for Python install package and is a commandline function. Because we are using the Anaconda distribution of Python, all of the packages that we will be using in this lesson are already installed for us, so we can move straight on to step 2.

Step 2. In your Python code include an `import package-name` statement. Once this is done, you can use all of the functions contained within the package.

As all of these packages are produced by 3rd parties independently of each other, there is the strong possibility that there may be clashes in function names. To allow for this, when you are calling a function from a package that you have imported, you do so by prefixing the function name with the package name. This can make for long-winded function names so the `import` statement allows you to specify an `alias` for the package name which you must then use instead of the package name.

In future episodes, we will be importing the `csv`, `json`, `pandas`, `numpy` and `matplotlib` modules. We will describe their use as we use them.

The code that we will use is shown below

~~~
import csv
import json
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
~~~
{: .language-python}

The first two we don't alias as they have short names. The last three we do. Matplotlib is a very large library broken up into what can be thought of as sub-libraries. As we will only be using the functions contained in the `pyplot` sub-library we can specify that explicitly when we import. This saves time and space. It does not effect how we call the functions in our code.

The `alias` we use (specified after the `as` keyword) is entirely up to us. However those shown here for `pandas`, `numpy` and `matplotlib` are nearly universally adopted conventions used for these popular libraries. If you are searching for code examples for these libraries on the Internet, using these aliases will appear most of the time.
