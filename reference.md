---
layout: reference
permalink: /reference/
---

## Glossary

0-based indexing
:   is a way of assigning indices to elements in a sequential, ordered data structure
    starting from 0, i.e. where the first element of the sequence has index 0.

attribute
:   a property of an object that can be viewed, accessed with a `.` but no `()` ex: `df.dtypes`

boolean
:   a data type that can be `True` or `False`

cast
:   the process of changing the type of a variable, in python the data type names operate as functions for casting, for example `int(3.5)`

CSV (file)
:   is an acronym which stands for Comma-Separated Values file. CSV files store
    tabular data, either numbers, strings, or a combination of the two, in plain
    text with columns separated by a comma and rows by the carriage return character.

database
:   is an organized collection of data.

DataFrame
:   is a two-dimensional labeled data structure with columns of (potentially)
    different type.

data structure
:   is a particular way of organizing data in memory.

data type
:   is a particular kind of item that can be assigned to a variable, defined by
    by the values it can take, the programming language in use and the operations
    that can be performed on it. examples: `int` (integer), `str` (string), float, boolean, list

docstring
:   is an recommended documentation string to describe what a Python function does.

faceting
:   is the act of plotting relationships between set variables in multiple subsets
    of the data with the results appearing as different panels in the same figure.

float
:   is a Python data type designed to store positive and negative decimal numbers
    by means of a floating point representation.

function
:   is a group of related statements that perform a specific task.

integer
:   is a Python data type designed to store positive and negative integer numbers.

interactive mode
:   is an online mode of operation in which the user writes the commands directly
    on the command line one-by-one and execute them immediately by pressing a button
    on the keyword, usually Enter.

join key
:   is a variable or an array representing the column names over which pandas.DataFrame.join()
    merge together columns of different data sets.

library
:   is a set of functions and methods grouped together to perform some specific
    sort of tasks.

list
:   is a Python data structure designed to contain sequences of integers, floats,
    strings and any combination of the previous. The sequence is ordered and indexed
    by integers, starting from 0. Elements of a list can be accessed by their index
    and can be modified.

loop
:   is a sequence of instructions that is continually repeated until a condition
    is satisfied.

method
:   a function that is specific to a type of data, accessed via `.` and requires `()` to run, for example `df.sum()`

NaN
:   is an acronym for Not-a-Number and represents that either a value is missing or
    the calculation cannot output any meaningful result.

None
:   is an object that represents no value.

scripting mode
:   is an offline mode of operation in which the user writes the commands to be
    executed in a text file (with .py extension for Python) which is then compiled
    or interpreted to run the program. Notes that Python interprets script on
    run-time and compiles a binary version of the program to speed up the execution time.

sequential (data structure)
:   is an ordered group of objects stored in memory which can be accessed specifying
    their index, i.e. their position, in the structure.

string
:   is a Python data type designed to store sequences of characters.

tuple
:   is a Python data structure designed to contain sequences of integers, floats,
    strings and any combination of the previous. The sequence is ordered and indexed
    by integers, starting from 0. Elements of a tuple can be accessed by their index
    but cannot be modified.

variable
:   a named quantity that can store a value, a variable can store any type, but always one type for a given value.  



## Jupyter Notebook Hints


`Esc` will take you into command mode where you can navigate around your notebook with arrow keys.

### While in command mode:
 - <kbd>A</kbd> to insert a new cell above the current cell,
 - <kbd>B</kbd> to insert a new cell below.
 - <kbd>M</kbd> to change the current cell to Markdown,
 - <kbd>Y</kbd> to change it back to code
 - <kbd>D</kbd> + <kbd>D</kbd> (press the key twice) to delete the current cell
 - <kbd>Enter</kbd> will take you from command mode back into edit mode for the given cell.

### while in edit mode:
 - <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>-</kbd> will split the current cell into two from where your cursor is.
 - <kbd>Shift</kbd> + <kbd>Enter</kbd> will run the current cell

### Full Shortcut Listing
<kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> (or <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> on Linux and Windows)
