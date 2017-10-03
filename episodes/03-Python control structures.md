---
title: "Python control structures"
teaching: 0
exercises: 0
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
- "First key point."
---

## Programs are rarely linear

Most programs do not work by executing a simple sequential set statements. The code is constructed so that decisions and different paths through the program can be taken based on changes in variable values.

To make this possible all programming language have a set of control structures which allow this to happen.

In this section we are going to look at how we can create loops and branches in our Python code
Specifically we will look at three control structures, namely; 

* If..Else..
* While...
* For ...
 
## The `if` statement and variants 

The Simple `if` statement allows the program to branch based on the evaluation of an expression

The basic format of the `if` statement is:

`if` expression :
    statement 1
    statement 2
    ...
    statement n
next statement

Below are some examples

~~~
# if 

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

In the examples above there are three things to notice;

1.	The colon ‘:’ at the end of the ‘if’ line. Missing this out is a common error. 
2.	The indentation of the print statement. If you remembered the ‘:’ on the line before, Jupyter (or any other Python IDE) will automatically do the indentation for you. All of the statement indented at this level are considered to be part of the ‘if’ statement
3.	The `if` statement is ended by removing the indent. There is no explicit end to the `if` statement as there is in many other programming languages

In the last example, notice that in Python the operator used to check equality is ‘==’. 

> ## Exercise 
> 
> Add another if statement to example two that will check if b is greater than or equal to a 
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
> > 
> {: .solution}
{: .challenge}

Instead of using two seperate `if` statements to decide which is larger we can use the `if ... else ...` construct

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

The overall structure is similar to the ‘if … Else’ statement. There are three additional things to notice;

1.	Each ‘Elif’ clause has its own test expression.
2.	You can have as many ‘Elif’ clauses as you need
3.	Execution of the whole statement stops after an ‘Elif’ expression is found to be True. Therefore the ordering of the ‘Elif’ clause can be significant.


## The `while` loop

The while loop is used to repeatedly execute lines of code until some condition becomes False.

For the loop to terminate, there has to be something in the code which will potentially change the condition.

~~~
# while loop
n = 10
sum = 0
# sum of n  numbers
i = 1
while  i <= n :
    sum = sum + i
    i = i + 1
print("The sum of the numbers from 1 to", n, "is ", sum)
~~~

Points to note;

1.	The condition clause (i <= n) in the while statement can be anything which when evaluated would return a Boolean value of either True of False. Initially i has been set to 1 (before the start of the loop) and therefore the condition is `True`.
2.	The clause can be made more complex by use of parentheses and `and` and `or`  operators amongst others
3.	The statements after the while clause are only executed if the condition evaluates as True. 
4.	Within the statements after the while clause there should be something which potentially will make the condition evaluate as `False` next time around. If not the loop will never end.
5.  In this case the last statement in the loop changes the value of i which is part of the condition clause, so hopefully the loop will end.

> ## Exercise - Things that can go wrong with while loops
> 
> In the examples below, without running them try to decide why we will not get the required answer.
> Run the first 3 to confirm your suspicions and then correct them.
> Why should you not run the last example in it's current form?
> 
> ~~~
> # while loop - summing the numbers 1 to 10
> n = 10
> sum = 0
> # sum of n  numbers
> i = 0
> while  i <= n :
>     i = i + 1
>     sum = sum + i
>     
> print("The sum of the numbers from 1 to", n, "is ", sum)
> 
> # while loop - summing the numbers 1 to 10
> n = 10
> sum = 0
> boolvalue = False
> # sum of n  numbers
> i = 0
> while  i <= n and boolvalue:
>     sum = sum + i
>     i = i + 1
>     
> print("The sum of the numbers from 1 to", n, "is ", sum)
> 
> # while loop - summing the numbers 1 to 10
> n = 10
> sum = 0
> # sum of n  numbers
> i = 0
> while  i != n :
>     sum = sum + i
>     i = i + 1
>     
> print("The sum of the numbers from 1 to", n, "is ", sum)
> 
> # while loop - summing the numbers 1.1 to 9.9 i. steps of 1.1 - no NOT run
> #n = 9.9
> #sum = 0
> ## sum of n  numbers
> #i = 0
> #while  i != n :
> #    sum = sum + i
> #    i = i + 1.1
> #    
> #print("The sum of the numbers from 1.1 to", n, "is ", sum)
> ~~~
> 
> > ## Solution
> > 
> > 1. Because i is incremented before the sum, you are summing 1 to 11.
> > 2. The Boolean value is set to False the lopp will never be executed.
> > 3. When i does equal 10 the expression is False and the lop does not execute so we have only summed 1 to 9
> > 4. Because you cannot guarantee the internal representation of Float, you should never try to compare them for eqaulity. In this particular case the i never 'equals' n and so the loop never ends. - If you did try running this, you can stop it using `Ctrl+c`
> > {: .solution}
> {: .challenge}



## The `for` loop
