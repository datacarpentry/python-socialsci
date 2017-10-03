---
title: "Python basics"
teaching: 0
exercises: 0
questions:
- "How do I assign values to variables?"
- "How do I do arithmetic?"
- "What is a built-in function?"
- "How do I see results?"
- "What data types are supported in Python?"

objectives:
- "Create variables and assign values to them"

- "Perform simple arithmetic operations"

- "Recognise built-in functions"

- "Understand how to specify parameters when using built-in functions"

- "Know how to get help for built-in functions and any other aspect of the Python language"

- "Use the Jupyter environment to show or hide output"

- "Define datatype and understand how Python uses them"

- "List the native data types in Python"

keypoints:
- "First key point."
---


## Python Basics

##### In this episode we will look at the following

* Create variables and assign values to them
* List the native data types in Python
* Perform simple arithmetic operations
* Recognise built-in functions
* Understand how to specify parameters when using built-in functions
* Know how to get help for built-in functions and any other aspect of the Python language
* Use the Jupyter environment to show or hide output
* Use the Jupyter environment to insert cells
* Use the Jupyter environment to change the cell type
* Define datatype and understand how Python uses them 
* Check the type of a variable
* Convert from one datatype to another

## Using the Jupyter environment

### New cells
from the insert menu item you can insert a new cell anywhere in the notebook either above or below the current cell. You can also us the + button on the toolbar to insert a new cell below.

### Change cell type

from the cell menu item you can change the type of a cell from code to markdown. By default new cells are created as code cells.

### Hiding output

When you run cells of code the out put is displayed immediately below the cell. In general this is convenient. The output is associated with the cell that produced it and remains a part of the notebook. So if you copy or move the notebook the output stays with the code.

However lots of output can mke the notebook look cluttered and more difficult to move around. So there is an option available from the `cell` menu item to 'toggle' or 'clear' the output associated either with an individual cel or all cells in the notebook.

## Creating variables and assigning values

### Variables and Types 

In Python variables are created when you first assign values to them.

~~~
a = 2
b = 3.142
~~~

All variables have a data type associated with them.
The datatype is an indication of the type of data contained in a variable. 
If you want to know the type of a variable you can use the built-in type() function.

~~~
type(a)
type(b)
s = "Hello World"
type(s)
~~~

There are many more data types available, a full list is available in the Python documentation.
We will be looking a few of them later on.

## Simple arithmetic operations

For now we will stick with the numeric types and do some arithmetic. 
All of the usual arithmetic operators are available.

In the examples below we also introduce the Python comment symbol '#'. 
Anything to the right of the '#' symbol is treated as a comment. To a large extent using markdown cells in a notebook reduces the need for comments in the code, but occasionally they can be useful. - Don't over do them.

We also make use of the built-in 'print()' function.

~~~
print("a = ", a, "and b = " , b)
print(a + b)      # addition
print(a * b)      # multiplication
print(a - b)      # subtraction
print(a / b)      # division
print(b ** a)     # exponentiation
print(2 * a % b)  # modulus - returns the remainder
~~~

We need to use the print function because by default only the last output from a cell is displayed in the output cell.

The first call to the print function is passed four different parameters, each seperated by a comma. A string "a = " followed by a followed by the string "b = " and then the variable b.

The output is what you would probably have guessed at. 

All of the other calls to print are only passed a single parameter. Although it may look like 2 or 3, the expressions are evaluated first and it is only the single result which is seen as the parameter value and printed. 

In the last expression 'a' is multiplied by 2 and then the modulus of the result is taken. Had I wanted to calculate a % b and then multiply the result by two I could have done so by using brackets to make the order of calculation clear.

Arithmetic expressions can be arbitarily complex, but remember people have to read and understand them as well.

> ### Exercise
> 
> 1. Create a new cell and paste into it the code cell above. Remove all of the calls to the print function and run the code. What is returned?
> 
> 2. Now remove all but the first line (with the 4 items in it) and run the cell again. How does this ourput differ from when we used the print function?
> 
> 3. Practice assigning values to variables using as many different operators as you can think of.
> 4. Create some expressions to be evaluated using brackets to enforce the precedence that you require
> 
> 
> > ## Solution
> > 
> > A complete set of Python operators can be found in the [official documentation](https://docs.python.org/3.5/library/operator.html) . The documentataion may appear a bit confusing as it initially talks about operators as functions whereas we generally use them as 'inplace ' operators. Section 10.3.1 provides a table which list all of the available operators, not all of which are relevant to basic arithmetic.
> >
> {: .solution}
{: .challenge}

## Using built-in functions

Python has a reasonable number of built-in functions. You can find a complete list in the [offical documentation](https://docs.python.org/3/library/functions.html)

Additional functions are provided by 3rd party packages which we will look at later on.

For any function, a common question to ask is; Wwhat parameters does this function take?

In order to answer this from Jupyter, you can type the function name and then type `shift`+`tab` and a pop-up window will provide you with various details about the function including the parameters.

> ### Exercise
> 
> For the print function find out what parameters can be provided
> 
> > ### Solution
> > Type 'print' into a code cell and then type `shift`+`tab`. The following pop-up should appear.
> > 
> > ![Print parameter information](..\\..\\Pictures\\Python_function_parameters_9.png) 
> > 
> > 
> {: .solution}
{: .challenge}

## Getting Help for Python

You can get help on any Python function by using the help function. It takes a single parameter of the function name for which you want the help

~~~
help(print)
~~~
~~~
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
    
~~~



## Datatype and how Python uses them
