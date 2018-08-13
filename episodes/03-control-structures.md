---
title: "Python control structures"
teaching: 20
exercises: 25
questions:
- "What constructs are available for changing the flow of a program?"
- "How can I repeat an action many times?"
- "How can I perform the same task(s) on a set of items?"
objectives:
- "Change program flow using available language constructs"
- "Demonstrate how to execute a section of code a fixed number of times"
- "Demonstrate how to conditionally execute a section of code"
- "Demonstrate how to execute a section of code on a list of items"
keypoints:
- "Most programs will require 'Loops' and 'Branching' constructs."
- "The `if`, `elif`, `else` statements allow for branching in code."
- "The `for` and `while` statements allow for looping through sections of code"
- "The programmer must provide a condition to end a `while` loop."
---

## Programs are rarely linear

Most programs do not work by executing a simple sequential set of statements. The code is constructed so that decisions and different paths through the program can be taken based on changes in variable values.

To make this possible all programming language have a set of control structures which allow this to happen.

In this episode we are going to look at how we can create loops and branches in our Python code.
Specifically we will look at three control structures, namely:

* If..Else..
* While...
* For ...

## The `if` statement and variants

The simple `if` statement allows the program to branch based on the evaluation of an expression

The basic format of the `if` statement is:

~~~
if expression :
    statement 1
    statement 2
    ...
    statement n

statement always executed
~~~
{: .language-python}

If the expression evaluates to `True` then the statements 1 to n will be executed followed by `statement always executed` . If the expression is `False`, only `statement always executed` is executed. Python knows which lines of code are related to the `if` statement by the indentation, no extra syntax is necessary.

Below are some examples:

~~~
print("\nExample 1\n")

a= 5
b= 4
print("a is", a, "b is",b)
if a > b :
    print(a, "is bigger than ", b)

print("\nExample 2\n")

a= 3
b= 4
print("a is", a, "b is",b)
if a > b :
    print(a , "is bigger than ", b)

print("\nExample 3\n")

a= 4
b= 4
print("a is", a, "b is",b)
if a == b :
    print(a, "is equal to", b)
~~~
{: .language-python}

~~~
Example 1

a is 5 b is 4
5 is bigger than  4

Example 2

a is 3 b is 4

Example 3

a is 4 b is 4
4 is equal to 4
~~~
{: .output}

In the examples above there are three things to notice:

1.	The colon `:` at the end of the `if` line. Missing this out is a common error.
2.	The indentation of the print statement. If you remembered the `:` on the line before, Jupyter (or any other Python IDE) will automatically do the indentation for you. All of the statements indented at this level are considered to be part of the `if` statement. This is a feature fairly unique to Python, that it cares about the indentation. If there is too much, or too little indentation, you will get an error.
3.	The `if` statement is ended by removing the indent. There is no explicit end to the `if` statement as there is in many other programming languages

In the last example, notice that in Python the operator used to check equality is `==`.

> ## Exercise
>
> Add another if statement to example 2 that will check if b is greater than or equal to a
>
> > ## Solution
> >
> > ~~~
> > print("\nExample 2a\n")
> >
> > a= 3
> > b= 4
> > print("a is", a, "b is",b)
> > if a > b :
> >     print(a, "is bigger than ", b)
> > if a <= b :
> >     print(b, "is bigger than or equal to ", a)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

Instead of using two separate `if` statements to decide which is larger we can use the `if ... else ...` construct

~~~
# If ... Else ...

a = 4
b = 5
print("a = ", a, "and b = ", b)

if a > b :
    print(a, " is greater than ", b)
else :
    print(a, " is NOT greater than ", b)
~~~
{: .language-python}

~~~
a = 4 and b = 5
4 is NOT greater than 5
~~~
{: .output}

> ## Exercise
>
> Repeat above with different operators '<' , '=='
{: .challenge}

A further extension of the `if` statement is the `if ... elif ...else` version.

The example below allows you to be more specific about the comparison of a and b.

~~~
# If ... Elif ... Else ... EndIf

a = 5
b = 4
print("a = ", a, "and b = ", b)

if a > b :
    print(a, " is greater than ", b)
elif a == b :
    print(a, " equals ", b)
else :
    print(a, " is less than ", b)
~~~
{: .language-python}

~~~
a = 5 and b = 4
5 is greater than 4
~~~
{: .output}

The overall structure is similar to the `if ... else` statement. There are three additional things to notice:

1.	Each `elif` clause has its own test expression.
2.	You can have as many `elif` clauses as you need
3.	Execution of the whole statement stops after an `elif` expression is found to be True. Therefore the ordering of the `elif` clause can be significant.


## The `while` loop

The while loop is used to repeatedly execute lines of code until some condition becomes False.

For the loop to terminate, there has to be something in the code which will potentially change the condition.

~~~
# while loop
n = 10
cur_sum = 0
# sum of n  numbers
i = 1
while  i <= n :
    cur_sum = cur_sum + i
    i = i + 1
print("The sum of the numbers from 1 to", n, "is ", cur_sum)
~~~
{: .language-python}

~~~
The sum of the numbers from 1 to 10 is 55
~~~
{: .output}

Points to note:

1.	The condition clause (i <= n) in the while statement can be anything which when evaluated would return a Boolean value of either True of False. Initially i has been set to 1 (before the start of the loop) and therefore the condition is `True`.
2.	The clause can be made more complex by use of parentheses and `and` and `or`  operators amongst others
3.	The statements after the while clause are only executed if the condition evaluates as True.
4.	Within the statements after the while clause there should be something which potentially will make the condition evaluate as `False` next time around. If not the loop will never end.
5.  In this case the last statement in the loop changes the value of i which is part of the condition clause, so hopefully the loop will end.
6. We called our variable `cur_sum` and not `sum` because `sum` is a builtin function (try typing it in, notice the editor
changes it to green).  If we define `sum = 0` now we can't use the function `sum` in this Python session.

> ## Exercise - Things that can go wrong with while loops
>
> In the examples below, without running them try to decide why we will not get the required answer.
> Run each, one at a time, and then correct them. Remember that when the input next to a notebook cell
> is [*] your Python interpreter is still working.
>
> ~~~
> # while loop - summing the numbers 1 to 10
> n = 10
> cur_sum = 0
> # sum of n  numbers
> i = 0
> while  i <= n :
>     i = i + 1
>     cur_sum = cur_sum + i
>     
> print("The sum of the numbers from 1 to", n, "is ", cur_sum)
> ~~~
> {: .language-python}
>
> ~~~
> # while loop - summing the numbers 1 to 10
> n = 10
> cur_sum = 0
> boolvalue = False
> # sum of n  numbers
> i = 0
> while  i <= n and boolvalue:
>     cur_sum = cur_sum + i
>     i = i + 1
>     
> print("The sum of the numbers from 1 to", n, "is ", cur_sum)
> ~~~
> {: .language-python}
>
> ~~~
> # while loop - summing the numbers 1 to 10
> n = 10
> cur_sum = 0
> # sum of n  numbers
> i = 0
> while  i != n :
>     cur_sum = cur_sum + i
>     i = i + 1
>
> print("The sum of the numbers from 1 to", n, "is ", cur_sum)
> ~~~
> {: .language-python}
>
> ~~~
> # while loop - summing the numbers 1.1 to 9.9 i. steps of 1.1
> n = 9.9
> cur_sum = 0
> # sum of n  numbers
> i = 0
> while  i != n :
>     cur_sum = cur_sum + i
>     i = i + 1.1
>     print(i)
>     
> print("The sum of the numbers from 1.1 to", n, "is ", sum)
> ~~~
> {: .language-python}
>
> > ## Solution
> >
> > 1. Because i is incremented before the sum, you are summing 1 to 11.
> > 2. The Boolean value is set to False the loop will never be executed.
> > 3. When i does equal 10 the expression is False and the loop does not execute so we have only summed 1 to 9
> > 4. Because you cannot guarantee the internal representation of Float, you should never try to compare them for equality. In this particular case the i never 'equals' n and so the loop never ends. - If you did try running this, you can stop it using <kbd>Ctrl</kbd>+<kbd>c</kbd> in a terminal or going to the kernel menu of a notebook and choosing interrupt.
> {: .solution}
{: .challenge}



## The `for` loop

The for loop, like the while loop repeatedly executes a set of statements. The difference is that in the for loop we know in at the outset how often the statements in the lop will be executed. We don't have to rely on a variable being changed within the looping statements.

The basic format of the `for` statement is

~~~
for variable_name in some_sequence :
    statement1
    statement2
    ...
    statementn
~~~
{: .language-python}

The key part of this is the `some_sequence`. The phrase used in the documentation is that it must be 'iterable'. That means, you can count through the sequence, starting at the beginning and stopping at the end.

There are many examples of things which are iterable some of which we have already come across.

* Lists are iterable - they don't have to contain numbers, you iterate over the elements in the list.
* The `range()` function
* The characters in a string

~~~
print("\nExample 1\n")
for i in [1,2,3] :
    print(i)

print("\nExample 2\n")
for name in ["Tom", "Dick", "Harry"] :
    print(name)

print("\nExample 3\n")
for name in ["Tom", 42, 3.142] :
    print(name)

print("\nExample 4\n")
for i in range(3) :
    print(i)

print("\nExample 5\n")
for i in range(1,4) :
    print(i)

print("\nExample 6\n")
for i in range(2, 11, 2) :
    print(i)

print("\nExample 7\n")
for i in "ABCDE" :
    print(i)

print("\nExample 8\n")
longString = "The quick brown fox jumped over the lazy sleeping dog"
for word in longString.split() :
    print(word)
~~~
{: .language-python}

~~~
Example 1

1
2
3

Example 2

Tom
Dick
Harry

Example 3

Tom
42
3.142

Example 4

0
1
2

Example 5

1
2
3

Example 6

2
4
6
8
10

Example 7

A
B
C
D
E

Example 8

The
quick
brown
fox
jumped
over
the
lazy
sleeping
dog
~~~
{: .output}

> ## Exercise
>
> In example8 the split method is used to break the `longString` variable down into a list of individual words which are then iterated through.
>
> Suppose that we have a string containing a set of 4 different values separated by `,`  like this:
>
> ~~~
> variablelist = "01/01/2010,34.5,Yellow,True"
> ~~~
> {: .language-python}
>
> Research the `split()` method and then rewrite example 8 so that it prints the 4 components of `variablelist`
>
> > ## Solution
> >
> > ~~~
> > variablelist = "01/01/2010,34.5,Yellow,True"
> > for word in variablelist.split(",") :
> >     print(word)
> > ~~~
> > {: .language-python}
> >
> > The format of `variablelist` is very much like that of a record in a csv file. In later episodes we will see how we can extract these values and assign them to variables for further processing rather than printing them out.
> >
> {: .solution}
{: .challenge}
