---
title: "Data visualisation using Matplotlib"
teaching: 25
exercises: 25
questions:
- "How can I create visualisations of my data?"
objectives:
- "Import pyplot from the matplotlib library"
- "Create simple plots using pyplot"
keypoints:
- "Graphs can be drawn directly from Pandas, but it still uses Matplotlib"
- "Different graph types have different data requirements"
- "Graphs are created from a variety of discrete components placed on a 'canvas', you don't have to use them all"
- "Plotting multiple graphs on a single 'canvas' is possible"
---

## Plotting in Python

There are a wide variety of ways to plot in Python, like many programming languages.  Some do more of the design work for you and others let you customize the look of the plots and all of the little details yourself. `Pandas` has basic plots built into it that reduce the amount of syntax, if your data is already in a DataFrame.
Matplotlib is a Python graphical library that can be used to produce a variety of different graph types, it is fully controllable down to basic elements and includes a module `pylab` that is somewhere in between (designed to feel like matlab plotting, if you happen to have done that before).


The [Pandas][pandas-web] library contains very tight integration with [Matplotlib][matplotlib-web].
There are functions in Pandas that automatically call Matplotlib functions to produce graphs.

The Matplotlib library can be imported using any of the import techniques we have seen. As Pandas is generally imported
with `import Pandas as pd`, you will find that `matplotlib` is most commonly imported with `import matplotlib as plt` where 'plt' is the alias.

In addition to importing the library, in a Jupyter notebook environment we need to tell Jupyter that when we produce a
graph, we want it to be display the graph in a cell in the notebook just like any other results. To do this we use the `%matplotlib inline` directive.

Although we are using Matplotlib in this episode, Pandas can make use of several other graphical libraries available
from within Python such as [ggplot2][ggplot2-web] and [Seaborn][seaborn-web]. Seaborn has some very powerful features
and advanced plot types.  One of its most useful features is formatting.

## Plotting with Pandas

To plot with Pandas we have to import it as we have done in past episodes.  We can also use the `%matplotlib inline` notebook magic to reduce syntax otherwise.  Without that we need to a `show()` command

~~~
import pandas as pd
%matplotlib inline
~~~
{: .language-python}

We also need data to work with loaded into a DataFrame and it's helpful to look at a few rows to remember what's there.

~~~
df = pd.read_csv("data/SAFI_full_shortname.csv")
df.head()
~~~
{: .language-python}

Next, we can plot the a histogram of a variable.

~~~
df['years_liv'].hist()
~~~
{: .language-python}

![png](output_4_1.png)


We can change the number of bins to make it look how we would like, for example

~~~
df['years_liv'].hist(bins=20)
~~~
{: .language-python}

We can also specify the column as a parameter and a groupby column with the `by` keyword. there are a lot of keywords available to make it look better, we can see some of the most likely ones (as decided by Pandas developers) by using <kbd>shift</kbd> + <kbd>tab<kbd>. Lets try `layout`, `figsize`, and `sharex`.

~~~
df.hist(column='years_liv',by='village',layout=(1,3),figsize=(12,3),sharex=True)
~~~
{: .language-python}

## Scatter plot

The scatter plot requires the x and y coordinates of each of the points being plotted.
To provide this we will generate two series of random data one for the x coordinates and the other for the y coordinates

We will generate two sets of points and plot them on the same graph.

We will also add other common features like a title, a legend and labels on the x and y axis.

~~~
{: .language-python}
df.plot.scatter(x='gps:Latitude', y='gps:Longitude', c='gps:Altitude', colormap="viridis", figsize=[4,4])
~~~~~~
{: .language-python}

![png](output_10_1.png)

> ## Exercise
> 1. Make a scatter plot of 'years_farm' vs 'years_liv' and color the points by 'buildings_in_compound'
> 2. Make a bar plot of the mean number of rooms per wall type
>
> Compare the two graphs we have just drawn. How do they differ? Are the differences significant?
>
> > ## Solution
> >
> > Most importantly the data in the graphs is the same. There are cosmetic differences in the scale points in the x and y axis and in the width of the bars.
> >
> > The width of the bars can be changed with a parameter in the 'bar' function
> >
> > ~~~
> > plt.bar(range(len(s)), s, width = 0.5)   # the default width is 0.8
> > ~~~
> > {: .language-python}
> {: .solution}
> Extension: try plotting by by wall and roof type?
{: .challenge}

## Boxplot

A boxplot provides a simple representation of a variety of statistical qualities of a single set of data values.

![box_plot](../fig/vis_boxplot_01.png)

A common use of the boxplot is to compare the statistical variations across a set of variables.

The variables can be an independent series or columns of a Dataframe using the Pandas plot method

~~~
df.boxplot(by ='village',column=['buildings_in_compound'])
~~~
{:.language-python}

We can make it look prettier with seaborn, much more easily than fixing components manually with Matplotlib.
~~~
import seaborn as sns
sns.boxplot(data=df,x ='village',y='buildings_in_compound')
~~~
{: .language-python}


~~~
sns.lmplot(x='years_farm', y='years_liv',data=df,hue='village')
~~~
{: .language-python}

In general, most graphs can be broken down into a series of elements which, although typically related in some way, can all exist independently of each other. This allows us to create the graph in a rather piecemeal fashion.

The labels (if any) on the x and y axis are independent of the data values being represented. The title and the legend are also independent objects within the overall graph.

In Matplotlib you create the graph by providing values for all of the individual components you choose to include. When you are ready, you call the `show` function.

Using this same approach, we can plot two sets of data on the same graph.

We will use a scatter plot to demonstrate some of the available features.

## Fine-tuning figures with Matplotlib

If we want to do more advanced or lower level things with our plots, we need to use Matplotlib directly, not through Pandas.  First we need to import it.


The Matplotlib library can be imported using any of the import techniques we have seen. As `pandas` is generally imported with `import pandas as pd`, you will find that `matplotlib` is most commonly imported with `import matplotlib.pylab as plt` where 'plt' is the alias.

In addition to importing the library, in a Jupyter notebook environment we need to tell Jupyter that when we produce a graph we want it to be display the graph in a cell in the notebook just like any other results. To do this we use the `%matplotlib inline` directive.

If you forget to do this, you will have to add `plt.show()` to see the graphs.

~~~
# Generate some date for 2 sets of points.
x1 = pd.Series(np.random.rand(20) - 0.5)
y1 = pd.Series(np.random.rand(20) - 0.5)

x2 = pd.Series(np.random.rand(20) + 0.5)
y2 = pd.Series(np.random.rand(20) + 0.5)


# Add some features
plt.title('Scatter Plot')
plt.ylabel('Range of y values')
plt.xlabel('Range of x values')

# plot the points in a scatter plot
plt.scatter(x1, y1, c='red', label='Red Range')  # 'c' parameter is the colour and 'label' is the text for the legend
plt.scatter(x2, y2, c='blue', label='Blue Range')

plt.legend(loc=4)  # the locations 1,2,3 and 4 are top-right, top-left, bottom-left and bottom-right
# Show the graph with the two sets of points
plt.show()
~~~
{: .language-python}

In the call to the `scatter` method, the `label` parameter values are used by the _legend_.
The `c` or `color` parameter can be set to any color Matplotlib recognises. Full details of the available colours are available in the [Matplotlib](http://matplotlib.org/api/colors_api.html) website. The [markers](http://matplotlib.org/api/markers_api.html) section will tell you what markers you can use instead of the default 'dots'. There is also an `s` (size) parameter which allows you to change the size of the marker.

> ## Exercise
>
> In the scatterplot the s parameter determines the size of the dots. s can be a simple numeric value, say s=100, which will produce dots all of the same size. However, you can pass a list of values (or a Pandas Series) to provide sizes for the individual dots. This approach is very common as it allows us to provide an extra variable worth of information on the graph.
>
> 1. Modify the code we used for the scatter plot to include a size value for each of the points in the series being plotted.
> (The downside is that some of the smaller dots may be completely covered by the larger dots. To try and highlight when this has happened, we can change the opacity of the dots.)
>
> 2. Find out which parameter controls the opacity of the dots (clue - it is not called opacity), add it to you code and set it > to a reasonable value.
>
> > ## Solution
> >
> > ~~~
> > # Generate some data for 2 sets of points.
> > # and additional data for the sizes - suitably scaled
> > x1 = pd.Series(np.random.rand(20) - 0.5)
> > y1 = pd.Series(np.random.rand(20) - 0.5)
> > z1 = pd.Series(np.random.rand(20) * 200)
> >
> > x2 = pd.Series(np.random.rand(20) + 0.5)
> > y2 = pd.Series(np.random.rand(20) + 0.5)
> > z2 = pd.Series(np.random.rand(20) * 200)
> >
> > # Add some features
> > plt.title('Scatter Plot')
> > plt.ylabel('Range of y values')
> > plt.xlabel('Range of x values')
> >
> > # plot the points in a scatter plot
> > plt.scatter(x1, y1, c='red', label='Red Range', s=z1, alpha=0.5)  # 's' parameter is the dot size
> > plt.scatter(x2, y2, c='blue', label='Blue Range', s=z2, alpha=0.5) # 'alpha' is the opacity
> >
> > plt.legend(loc=4)
> > plt.show()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}


Internally the Pandas 'plot' method has called the 'bar' method of Matplotlib and provided a set of parameters, including the pandas.Series s to generate the graph.

We can use Matplotlib directly to produce a similar graph. In this case we need to pass two parameters, the number of bars we need and the Pandas Series holding the values.

We also have to explicitly call the `show()` function to produce the graph.

## Saving Plots

~~~
plt.savefig("rooms.png")
plt.savefig("rooms.pdf", bbox_inches="tight", dpi=600)
plt.show()
~~~
{: .language-python}


For the Histogram, each data point is allocated to 1 of 10 (by default) equal 'bins' of equal size (range of numbers) which are indicated along the x axis and the number of points (frequency) is shown on the y axis.

In this case the graphs are almost identical. The only difference being in the first graph the y axis has a label 'Frequency' associated with it.

We can fix this with a call to the `ylabel` function

~~~
plt.ylabel('Frequency')
plt.hist(s)
plt.show()
~~~
{: .language-python}


In general most graphs can be broken down into a series of elements which, although typically related in some way, can
all exist independently of each other. This allows us to create the graph in a rather piecemeal fashion.

The labels (if any) on the x and y axis are independent of the data values being represented.
The title and the legend are also independent objects within the overall graph.

In Matplotlib you create the graph by providing values for all of the individual components you choose to
include. When you are ready, you call the `show` function.

Using this same approach we can plot two sets of data on the same graph. We will use a scatter plot to
demonstrate some of the available features.

> ## Exercise
>
> Revisit your favorite plot we've made so far, or make one with your own data then:
>
> 1. add axes labels
> 2. add a title
> 3. add a legend
> 4. save it in two different formats
>
> extension: try plotting by by wall and roof type?
>
{: .challenge}


## Saving a graph

If you wish to save your graph as an image you can do so using the `savefig()` function. The image can be saved as a pdf, jpg or png file by changing the file extension.

~~~
df = pd.DataFrame(np.random.normal(size=(100,5)), columns=list('ABCDE'))
df.plot(kind = 'box', return_type='axes')

plt.title('Box Plot')
plt.xlabel('xlabel')
plt.ylabel('ylabel')
#plt.show()
plt.savefig('boxplot_from_df.pdf')
~~~
{: .language-python}

[matplotlib-web]: http://matplotlib.org/
[pandas-web]: http://pandas.pydata.org/
[ggplot2-web]: http://ggplot2.tidyverse.org/
[seaborn-web]: https://seaborn.pydata.org/
