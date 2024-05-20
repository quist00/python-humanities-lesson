---
title: Plotting
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Create plot objects
- Change the aesthetics of a plot such as colour
- Edit the axis labels
- Create customized plot styles to meet their needs

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Can I use Python to create plots?
- How can I customize plots generated in Python?

::::::::::::::::::::::::::::::::::::::::::::::::::

##### Disclaimer

  

## Plotting with Matplotlib


Matplotlib is a Python library that can be used to visualize data.
Pandas has powerful built-in plotting capabilities based off of `matplotlib`, but
more flexibility is available to us if access `matplotlib` directly. The
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
    list_numbers = [2, 4, 6, 8]
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
    plt.plot(list_numbers,[200, 400, 800, 1600])
    plt.show()
```

A third, optional argument in `plot()` is a string of characters that indicates
the line type and color for the plot. The default value is a continuous blue
line. For example, we can make the line red (`'r'`), with circles at every data
point (`'o'`), and a dot-dash pattern (`'-.'`). Look through the matplotlib
gallery for more examples.

```python
    plt.plot(list_numbers,[200, 400, 800, 1600], 'ro-.')
    plt.axis([0,6,0,10])
    plt.show()
```

The command `plt.axis()` sets the limits of the axes from a list of `[xmin, xmax, ymin, ymax]` values (the square brackets are needed because the argument
for the function `axis()` is one list of values, not four separate numbers!).


A single figure can include multiple lines. We can include a legend by adding the optional keyword argument `label=''` in `plot()` and then call `plt.legend()`. The functions `xlabel()` and `ylabel()` will label the axes, and `title()` will write a title above the figure:

```python
# convert acquistion to datetime data type
works_df.acquisition_date = pd.to_datetime(works_df['acquisition_date'])

# subset on flag
dei = works_df[works_df['is_dei'] == True]
non_dei = works_df[works_df['is_dei'] == False]

# Group by acquisition year (or another suitable period)
acquisition_dei = dei.resample('Y', on='acquisition_date').size()
acquisition_non_dei = non_dei.resample('Y', on='acquisition_date').size()
works = works_df.resample('Y', on='acquisition_date').size()

# Plotting
plt.plot(acquisition_dei.index, acquisition_dei.values,"r--",label="DEI")
plt.plot(acquisition_non_dei.index, acquisition_non_dei.values,"g^",label="non-DEI")
plt.plot(works.index, works.values,"bo-",label="Combined",alpha=.25)
plt.title('Number of Acquisitions Over Time')
plt.xlabel('Year')
plt.ylabel('Number of Acquisitions')
plt.grid(True)
plt.legend()
plt.show()
```

Like MATLAB, pyplot is stateful; it keeps track of the current figure and
plotting area, and any plotting functions are directed to those axes. To make
more than one figure, we use the command `plt.figure()` with an increasing
figure number inside the parentheses:

```python
    # this is the first figure
    plt.figure(1)
    plt.plot(acquisition_dei.index, acquisition_dei.values,"r--",label="DEI")

    plt.legend(loc='upper left', shadow=True, fontsize='x-large')
    plt.title('This is figure 1')

    plt.show()

    # this is a second figure
    plt.figure(2)
    plt.plot(acquisition_non_dei.index, acquisition_non_dei.values,"g^",label="non-DEI")

    plt.legend(loc='lower right', shadow=True, fontsize='x-large')
    plt.title('This is figure 2')

    plt.show()
```

A single figure can also include multiple plots in a grid pattern. The
`subplot()` command specifies the number of rows, the number of columns, and
the number of the space in the grid that particular plot is occupying:

```python
    plt.figure(1)

    plt.subplot(2,2,1) # two row, two columns, position 1
    plt.plot(works.index, works.values,"bo-",label="Combined",alpha=.25)

    plt.subplot(2,2,2) # two row, two columns, position 2
    plt.plot(acquisition_dei.index, acquisition_dei.values,"r--",label="DEI")

    plt.subplot(2,2,3) # two row, two columns, position 3
    plt.plot(acquisition_non_dei.index, acquisition_non_dei.values,"g^",label="non-DEI")

    plt.show()
```

:::::::::::::::::::::::::::::::::::::::  challenge
## What can we and can't we ask?

- What other DEI related questions could you ask of the data as is?  
- What questions could you answer by reformulating the data currently in the dataset.  
- What is a question you have that cannot be answered with the current data?  
    - How would you go about getting the data needed.
    - How much would that data cost in time, effort, dollars?



:::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge
### Make other types of plots:

1. Create a histogram of checkouts.

2. Run the following code:
```python
ethnicity_df = pd.read_csv('ethnicity.csv')
ethnicity_df.loc[ethnicity_df.intl.isnull(),['intl']]= ethnicity_df.intl.min()
ethnicity_df.intl = ethnicity_df.intl.astype('int')

plot_df = ethnicity_df[ethnicity_df.term == 'Fall']
plot_df.set_index('year',inplace=True)
plot_df = plot_df.sort_index()
plot_df
plot_df[['amind', 'black', 'hispanic', 'asian','pacific_islander', 'multi', 'white', 'unknown', 'intl']].plot()
```
Use a *for* loop to recreate that plot using matplotlib. Make sure you have a title, legend, and label your axis.
**Hint:** You can reuse plot_df.

3. Matplotlib can make many other types of plots in much the same way that it makes
 2 dimensional line plots. Look through the examples in
 [http://matplotlib.org/users/screenshots.html](https://matplotlib.org/users/screenshots.html) and try a few of them (click on the
 "Source code" link and copy and paste into a new cell in ipython notebook or
 save as a text file with a `.py` extension and run in the command line).

:::::::: solution
1.
```python
plt.figure(figsize=(10, 6))
plt.hist(works_df['checkouts'], bins=len(works_df['checkouts'].unique()), log=True, edgecolor='black')
plt.title('Histogram of Checkouts (Logarithmic Y-axis)')
plt.xlabel('Checkouts')
plt.ylabel('Frequency (log scale)')
plt.grid(True)
plt.show()
```
2.
```python
cols = ['amind', 'black', 'hispanic', 'asian',
       'pacific_islander', 'multi', 'white', 'unknown', 'intl']
styles = ['r-', 'b--', '-.', ':', 'r--', 'b-', 'g-', 'g--', 'r-.']
for ethnicity in enumerate(cols):
    plt.plot(plot_df.index,plot_df[ethnicity[1]],styles[ethnicity[0]],label = ethnicity[1],alpha=.5)

plt.legend()
plt.ylabel('Number of Students')
plt.xlabel('Year')
plt.title('Students by Ethnicity')
```
::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Plotting with other libraries.

`matplotlib` is not your only option. There are many other libraries that can simplify the steps required or add additional functionality with `seaborn` and `bokeh` being just two examples. There is no way to cover them
all here, but be aware that you have options and that you should not let differences in syntax dissuade
 you from exploring to finding the right fit for your needs.