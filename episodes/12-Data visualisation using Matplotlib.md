---
title: "Data visualisation using Matplotlib"
teaching: 0
exercises: 0
questions:
- "How can I create simple visualisations of my data?"
objectives:
- "Import pyplot from the matplotlib library" 

- "Create simple plots using pyplot"
keypoints:
- "Graphs can be drawn directly from pandas, but it still uses matplotlib"
- "Different graph types have different data requirements"
- "Graphs are created from a variety of discrete components placed on a 'canvas', you don't have to use them all"
- "Plotting multiple graphs on a single 'canvas' is possible"
---

## Matplotlib

Matplotlib is a Python graphical library that can be used to produce a variety of different graph types. 

pandas contains very tight integration with matplotlib. pandas includes function that automatically call matplotlib functions to produce graphs. 

Although we are using Matplotlib in this lesson, pandas can mke use of several other graphical libraries available from within Python such as ggplo2 and seaborn.

## Importing matplotlib

The matplotlib library can be imported just like any other library. Like pandas it is almost invariably given an alias. In this case 'plt'. Almost any example code using matplotlib will use 'plt' as the alias.

In addition to importing the library, in a Jupyter notebook environment we need to tell Jupyter that when we produce a graph we want it to be display in a cell in the notebook just like any other results. To do this we use the '%matplotlib inline' directive.  

If you forget to to this your graphs will not appear.

~~~
import matplotlib.pyplot as plt
%matplotlib inline
~~~


## Numpy 

Numpy is another Python library. It is used for multi-dimensional array processing. In our case we just want to use it for its useful random number generation functions which we will use to create some fake data to demonstrate some of the graphing functions of matplotlib.

numpy is usually given the alias of 'np', a convention we will follow.

## Bar charts

~~~
np.random.rand(20)
~~~

will generate 20 random numbers between 0 and 1.

We are using these to create a pandas Series of values. 

A bar chart only needs a single set of values. Each 'bar' represents the value from the Series of values.
A pandas Series (and a DataFrame) have a method called 'plot'. We only need to to tell plot what kind of graph we want.

The 'x' axis represents the index values of the Series


~~~
import numpy as np
import pandas as pd

np.random.seed(12345)            # set a seed value to ensure reproducibility of the plots 
s = pd.Series(np.random.rand(20) )
#s
# plot the bar chart
s.plot(kind='bar')

~~~

Internally the pandas 'plot' method has called the 'bar' method of matplotlib and provided a set of parameters, including the pandas.Series s to generate the graph.

We can use matplotlib directly to produce a similar graph. In this case we need to pass two parameters, the number of bars we need and the pandas Series holding the values.

We also have to explicitly call the 'show' function to produce the graph.

~~~
plt.bar(range ( len ( s )), s) 
plt.show ()

~~~

> ## Exercise
> 
> Compare the two graphs we have just drawn. How do they differ? Are the differences significant?
> 
> > ## Solution
> > 
> > Most importantly the data in the graphs is the same. There are cosmetic differentces in the scale points in the x and y axis and in the width of the bars.
> > 
> > The width of the bars can be changed with a parameter in the 'bar' function
> > 
> > ~~~
> > plt.bar(range ( len ( s )), s, width = 0.5)   # the default width is 0.8
> > 
> > ~~~
> > 
> {: .solution}
{: .challenge}

## Histograms

We can plot histograms in a similar ways, directly from pandas and also from Matplotlib

The pandas way

~~~
s = pd.Series(np.random.rand(20))
# plot the bar chart
s.plot(kind='hist')
~~~

and the matplotlib way

~~~
plt.hist(s) 
plt.show()
~~~

For the Histogram, each data point is allocated to one of 10 (by default) equal 'bins' of equal size (range of numbers) which are indicated along the x axis and the number of points (frequency) is shown on the y axis.

In this case the graphs are almost identical. The only difference being in the first graph the y axis has a label 'Frequency' associated with it.

We can fix this with a call to the 'ylabel' function

~~~
plt.ylabel('Frequency')
plt.hist(s)
plt.show()
~~~

In general most graphs can be broken down into a series of elelments which, although typically related in some way, can all exist independently of each other. This allows us to create the graph in a rather piecemeal fashion.

The labels (if any) on the x and y axis are independent of the data values being represented. The title and the legend are also independent objects within the overall graph.

In matplotlib you create the graph by providing values for all of the individual components you choose to include. When you are ready, you call the 'show' function.

Using this same approach we can plot two sets of data on the same graph

We will use a scatter plot to demonstrate some of the available features.

For a scatter plot we need two sets of data points one for the x values 
and the other for the y values.

## Scatter plot

The scatter plot requires the x and y coordinates of each of the points being plotted.
To provide this we will generate two series of randon data one for the x coordinates and the other for the y coordinates

We will generate two sets of points and plot them on the same graph.

we will also add other common features like a title, a legend and labels on the x and y axis.

~~~
# Generate some date for 2 sets of points.
x1 = pd.Series(np.random.rand(20) - 0.5 )
y1 = pd.Series(np.random.rand(20) - 0.5 )

x2 = pd.Series(np.random.rand(20) + 0.5 )
y2 = pd.Series(np.random.rand(20) + 0.5 )


# Add some features
plt.title('Scatter Plot')
plt.ylabel('Range of y values')
plt.xlabel('Range of x values')

# plot the points in a scatter plot
plt.scatter(x1,y1, c='red', label='Red Range' )  # 'c' parameter is the colour and 'label' is the text forthe legend
plt.scatter(x2,y2, c='blue', label='Blue Range')

plt.legend( loc=4 )  # the locations 1,2,3 and 4 are top-right, top-left, bottom-left and bottom-right
# Show the graph with the two sets of points
plt.show()
~~~

In the call to the `scatter` method, the 'label' parameter values are used by the 
