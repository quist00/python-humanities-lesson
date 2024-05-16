---
title: Putting It All Together
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learners import, clean, and plot their own data.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What common issues might be encountered with real world data.
- How can plotting and other techniques help with exploratory analysis and getting to know my data?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Putting it all together

Up to this point, we have walked through tasks that are often
involved in handling and processing data using the workshop ready cleaned  
files that we have provided. In this wrap-up exercise, we will perform
many of the same tasks with real data sets. 

As opposed to the previous ones, this lesson does not give step-by-step
directions to each of the tasks. Use the lesson materials you've already gone
through as well as the Python documentation to help you along.

## 1\. Obtain data

There are many repositories online from which you can obtain data. We are
providing you with one data file to use with these exercises (`works-original.csv`), but feel free to
use any data that is relevant to your research.

## 2\. Clean up your data and open it using Python and Pandas

To begin, import your data file into Python using Pandas. Did it fail? Your data
file may have metadata that Pandas does not recognize as part of the data
table. Remove this metadata, but do not simply delete it in a text editor! Use
either a shell script or Python to do this - you wouldn't want to do it by hand
if you had many files to process.

If you are still having trouble importing the data as a table using Pandas,
check the documentation. You can open the docstring in an ipython notebook using
a question mark. For example:

```python
    import pandas as pd
    pd.read_csv?
```

Look through the function arguments to see if there is a default value that is
different from what your file requires (Hint: the problem is most likely the
delimiter or separator. Common delimiters are `','` for comma, `' '` for space,
and `'\t'` for tab).

Create a DataFrame that includes only the values of the data that are useful to
you. You can also change the name of the columns in the DataFrame like this:

```python
df.rename(columns={"A": "Column1", "B": "Column2"}, inplace=True)

```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Lots of plots

Make a variety of plots from your data. Add axis labels, titles, and legends to your figures. Make at least one figure with multiple plots using the function `subplot()`.


::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Final Plot

Display your data using one or more plot types from the example gallery
 ([http://matplotlib.org/gallery.html](https://matplotlib.org/gallery.html)). Which
ones to choose will depend on the content of your own data file. For example, consider
if any of your data would benefit from being presented as a histogram, a bar plot, or
explore the different ways matplotlib can handle dates and times for figures.


::::::::::::::::::::::::::::::::::::::::::::::::::


