---
title: Plotting
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Create plot objects
- Set universal plot settings
- Modify an existing plot object
- Change the aesthetics of a plot such as colour
- Edit the axis labels
- Build complex plots using a step-by-step approach
- Create scatter plots, box plots, and time series plots
- Use the `facet_wrap` and `facet_grid` commands to create a collection of plots splitting the data by a factor variable
- Create customized plot styles to meet their needs

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Can I use Python to create plots?
- How can I customize plots generated in Python?

::::::::::::::::::::::::::::::::::::::::::::::::::

##### Disclaimer

Pandas has powerful built-in plotting capabilities based off of `matplotlib`, but
more flexibility is available to us if access `matplotlib ` directly.  We will explore
that here first, but there are other libraries like `seaborn` and `bokeh` that can simplify the steps required and /or add additional functionality. We will make some plots with the [`bokeh`](https://docs.bokeh.org/en/latest/) package to exemplify the differences.  The most important take away, though, is that you have plotting
options and that you should not let some differences in syntax dissuade you from finding the right fit for
your needs.

## Plotting with Matplotlib

Matplotlib is a Python library that can be used to visualize data. The
toolbox `matplotlib.pyplot` is a collection of functions that make matplotlib
work like MATLAB. In most cases, this is all that you will need to use, but
there are many other useful tools in matplotlib that you should explore.

We will cover a few basic commands for formatting plots in this lesson. A great
resource for help styling your figures is the matplotlib gallery
([http://matplotlib.org/gallery.html](https://matplotlib.org/gallery.html)), which includes plots in many different styles and the source code that creates them. 

The simplest of plots is the 2 dimensional line plot.   These next few examples walk through the
 basic commands for making line plots using pyplots.

### Using pyplot:

First, import the pyplot toolbox:

```python
    import matplotlib.pyplot as plt
```

We can start by plotting the values of a list of numbers (matplotlib can handle
many types of numeric data, including numpy arrays and pandas DataFrames - we
are just using a list as an example!):

```python
    list_numbers = [1.5, 4, 2.2, 5.7]
    plt.plot(list_numbers)
    plt.show()
```

The command `plt.show()` prompts Python to display the figure. Without it, it
creates an object in memory but doesn't produce a visible plot. 

If you provide the `plot()` function with only one list of numbers, it assumes
that it is a sequence of y-values and plots them against their index (the first
value in the list is plotted at `x=0`, the second at `x=1`, etc). If the
function `plot()` receives two lists, it assumes the first one is the x-values
and the second the y-values. The line connecting the points will follow the list
in order:

```python
    plt.plot(list_numbers,[6.8, 4.3, 3.2, 8.1])
    plt.show()
```

A third, optional argument in `plot()` is a string of characters that indicates
the line type and color for the plot. The default value is a continuous blue
line. For example, we can make the line red (`'r'`), with circles at every data
point (`'o'`), and a dot-dash pattern (`'-.'`). Look through the matplotlib
gallery for more examples.

```python
    plt.plot(list_numbers,[6.8, 4.3, 3.2, 8.1], 'ro-.')
    plt.axis([0,6,0,10])
    plt.show()
```

The command `plt.axis()` sets the limits of the axes from a list of `[xmin, xmax, ymin, ymax]` values (the square brackets are needed because the argument
for the function `axis()` is one list of values, not four separate numbers!).


A single figure can include multiple lines, and they can be plotted using the
same `plt.plot()` command by adding more pairs of x values and y values (and
optionally line styles) The functions `xlabel()` and `ylabel()` will label the axes, and `title()` will
write a title above the figure:

```python
    import numpy as np

    # create a numpy array between 0 and 10, with values evenly spaced every 0.5
    t = np.arange(0., 10., 0.5)

    # red dashes with no symbols, blue squares with a solid line, and green triangles with a dotted line
    plt.plot(t, t, 'r--', t, t**2, 'bs-', t, t**3, 'g^:')

    plt.xlabel('This is the x axis')
    plt.ylabel('This is the y axis')
    plt.title('This is the figure title')

    plt.show()
```

We can include a legend by adding the optional keyword argument `label=''` in
`plot()`. Caution: We cannot add labels to multiple lines that are plotted
simultaneously by the `plt.plot()` command like we did above because Python
won't know to which line to assign the value of the argument label. Multiple
lines can also be plotted in the same figure by calling the `plot()` function
several times:

```python
    # red dashes with no symbols, blue squares with a solid line, and green triangles with a dotted line
    plt.plot(t, t, 'r--', label='linear')
    plt.plot(t, t**2, 'bs-', label='square')
    plt.plot(t, t**3, 'g^:', label='cubic')

    plt.legend(loc='upper left', shadow=True, fontsize='x-large')

    plt.xlabel('This is the x axis')
    plt.ylabel('This is the y axis')
    plt.title('This is the figure title')

    plt.show()
```

The function `legend()` adds a legend to the figure, and the optional keyword
arguments change its style. By default [typing just `plt.legend()`], the legend
is on the upper right corner and has no shadow.

Like MATLAB, pyplot is stateful; it keeps track of the current figure and
plotting area, and any plotting functions are directed to those axes. To make
more than one figure, we use the command `plt.figure()` with an increasing
figure number inside the parentheses:

```python
    # this is the first figure
    plt.figure(1)
    plt.plot(t, t, 'r--', label='linear')

    plt.legend(loc='upper left', shadow=True, fontsize='x-large')
    plt.title('This is figure 1')

    plt.show()

    # this is a second figure
    plt.figure(2)
    plt.plot(t, t**2, 'bs-', label='square')

    plt.legend(loc='upper left', shadow=True, fontsize='x-large')
    plt.title('This is figure 2')

    plt.show()
```

A single figure can also include multiple plots in a grid pattern. The
`subplot()` command specifies the number of rows, the number of columns, and
the number of the space in the grid that particular plot is occupying:

```python
    plt.figure(1)

    plt.subplot(2,2,1) # two row, two columns, position 1
    plt.plot(t, t, 'r--', label='linear')

    plt.subplot(2,2,2) # two row, two columns, position 2
    plt.plot(t, t**2, 'bs-', label='square')

    plt.subplot(2,2,3) # two row, two columns, position 3
    plt.plot(t, t**3, 'g^:', label='cubic')

    plt.show()
```
:::::::::::::::::::::::::::::::::::::::  challenge
### Make other types of plots:

1. Create a histogram of checkouts.

2. Matplotlib can make many other types of plots in much the same way that it makes
 2 dimensional line plots. Look through the examples in
 [http://matplotlib.org/users/screenshots.html](https://matplotlib.org/users/screenshots.html) and try a few of them (click on the
 "Source code" link and copy and paste into a new cell in ipython notebook or
 save as a text file with a `.py` extension and run in the command line).

::::::::::::::::::::::::::::::::::::::::::::::::::

## Plotting with bokeh

We will make the same plot using the `bokeh` package.

`bokeh` is a plotting package that makes it simple to create complex plots
from data in a dataframe. It uses default settings, which help creating
publication quality plots with a minimal amount of settings and tweaking.

bokeh graphics are built step by step by adding new elements.

To build a bokeh plot we need to:

- bind the plot to a specific data frame using the `data` argument

- define figure (`figure`), by selecting the variables to be plotted and the variables to define the presentation
  such as plotting size, title etc.,

We also set some notebook settings with a "output\_notebook()" statement to get interactive
and exportable plots

```python
from bokeh.plotting import figure, output_file, show
from bokeh.io import output_notebook
```

```python
works_plot_df = pd.read_csv( 'all_works.csv', index_col='mms_id')
output_notebook()
p = figure(width=400, height=400)

```

![](fig/figure01.png){alt='png'}

We can add simple points to create a scatter plot using circle.

```python
list_dates = works_plot_df['publication_date']
list_numbers = works_plot_df['checkouts']
p.circle(list_dates, list_numbers)
show(p)
```

![](fig/figure02.png){alt='png'}

## Building your plots

We can add extra arguments into circle's argument.

For comparison, we create a new figure and then add the
alpha argument to circle to change the opacity of the points.

```python
p1 = figure(width=400, height=400)

p1.circle(list_dates, list_numbers, alpha=0.1)

show(p1)
```

![](fig/figure03.png){alt='png'}

We can also add colors for all the points.

```python
p2 = figure(width=400, height=400)

p2.circle(list_dates, list_numbers, color="blue", alpha=0.1)

show(p2)
```

![](fig/figure04.png){alt='png'}

## Plotting time series data

Let's plot the number of items per each publication year since 2008. To do that we need
to group data first and count records within each group.

```python
yearly = works_plot_df[works_plot_df['publication_date'] >= 2008][['publication_date','publication_place','checkouts']].groupby(['publication_date','publication_place']).count().reset_index()
p3 = figure(width=800, height=250)

p3.line(yearly['publication_date'], yearly['checkouts'], color='navy', alpha=0.5)

show(p3)
```

## Customization

Now, let's look at how to add a titles to a figure:

```python
from bokeh.models import ColumnDataSource, Range1d, LabelSet, Label

p4 = figure(title="Publications by Year", width=400, height=400)

p4.circle(list_dates, list_numbers)

p4.xaxis[0].axis_label = 'Year'
p4.yaxis[0].axis_label = 'Publications Count'
p4.xaxis[0].axis_label_text_font_size = "20pt"
p4.yaxis[0].axis_label_text_font_size = "20pt"
show(p4)
```

![](fig/figure06.png){alt='png'}



With all of this information in hand, please take another five minutes to either
improve one of the plots generated in this exercise or create a beautiful graph
of your own.

Here are some ideas:

- Can you find a way to change its labels?
- Use a different color palette.

After creating your plot, you can save it to a file as a png file:

```python
from bokeh.io import export_png

export_png(p4, filename="plot.png")
```


