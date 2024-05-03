---
title: Plotting with bokeh
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Create a ggplot object
- Set universal plot settings
- Modify an existing ggplot object
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

Python has powerful built-in plotting capabilities such as `matplotlib`, but
for this exercise, we will be using the [`bokeh`](https://docs.bokeh.org/en/latest/)
package, which facilitates the creation of highly-informative plots of
structured data.

```python
works_plot_df = pd.read_csv( 'all_works.csv', index_col='mms_id')
```


```python
from bokeh.plotting import figure, output_file, show
from bokeh.io import output_notebook
```

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


