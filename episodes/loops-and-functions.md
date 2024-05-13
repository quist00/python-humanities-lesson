---
title: Data workflows and automation
teaching: 40
exercises: 50
---

::::::::::::::::::::::::::::::::::::::: objectives

- Describe why for loops are used in Python.
- Employ for loops to automate data analysis.
- Write unique filenames in Python.
- Build reusable code in Python.
- Write functions using conditional statements (if, then, else).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Can I automate operations in Python?
- What are functions and why should I use them?

::::::::::::::::::::::::::::::::::::::::::::::::::

So far, we've used Python and the pandas library to explore and manipulate
individual datasets by hand, much like we would do in a spreadsheet. The beauty
of using a programming language like Python, though, comes from the ability to
automate data processing through the use of loops and functions.

## For loops

Loops allow us to repeat a workflow (or series of actions) a given number of
times or while some condition is true. We would use a loop to automatically
process data that's stored in multiple files (daily values with one file per
year, for example). Loops lighten our work load by performing repeated tasks
without our direct involvement and make it less likely that we'll introduce
errors by making mistakes while processing each file by hand.

Let's write a simple for loop that simulates what a kid might see during a
visit to the zoo:

```python
>>> animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
>>> print(animals)
['lion', 'tiger', 'crocodile', 'vulture', 'hippo']

>>> for creature in animals:
...    print(creature)
lion
tiger
crocodile
vulture
hippo
```

The line defining the loop must start with `for` and end with a colon, and the
body of the loop must be indented.

In this example, `creature` is the loop variable that takes the value of the next
entry in `animals` every time the loop goes around. We can call the loop variable
anything we like. After the loop finishes, the loop variable will still exist
and will have the value of the last entry in the collection:


## Automating data processing using For Loops

Suppose that we were working with a much larger set of books. It might be useful
to split them out into smaller groups, e.g., by date of publication.

Let's start by making a new directory inside our current working folder. Python has
a built-in library called `os` for this sort of operating-system dependent behaviour.

```python
import os

os.mkdir('yearly_files')
```

We can check that the folder was created by listing the contents of the current directory:

```python
os.listdir()
['gender.csv',
 '.DS_Store',
 'ethnicity.csv',
 'Untitled.ipynb',
 'yearly_files',
 'out.csv',
 'all_works.csv',
 '.virtual_documents',
 '.ipynb_checkpoints']
```

In previous lessons, we saw how to use the library pandas to load
data into memory as a DataFrame, how to select a subset of the data using some
criteria, and how to write the DataFrame into a csv file. Let's write a script
that performs those three steps in sequence to write out records for the year
*1948* from *all_works.csv* as a seperate csv in the `yearly_files` directory

```python
import pandas as pd

# Load the data into a DataFrame
all_works_df = pd.read_csv('all_works.csv')

# Select only data for 1948
df_1948 = all_works_df[all_works_df.publication_date == 1948]

# Write the new DataFrame to a csv file
df_1948.to_csv('yearly_files/1948Publications.csv')

# Then check that that file now exists:
os.listdir("yearly_files")
```

To create yearly data files, we could repeat the last two commands over and
over, once for each year of data. Repeating code is neither elegant nor
practical, and is very likely to introduce errors into your code. We want to
turn what we've just written into a loop that repeats the last two commands for
every year in the dataset.

Let's start by writing a loop that simply prints the names of the files we want
to create - the dataset we are using covers 31 years bewteen 1948 and 2024, and we'll create
a separate file for each of those years. Listing the filenames is a good way to
confirm that the loop is behaving as we expect.

We have seen that we can loop over a list of items, so we need a list of years
to loop over. We can get the unique years in our DataFrame with:

```python
all_works_df['publication_year'].unique()
array([2009, 2008, 2011, 2018, 2021, 2014, 2010, 2019, 2020, 2017, 2022,
       2013, 2012, 2015, 2007, 2016, 2023, 2006, 2003, 2001, 2005, 1948,
       1994, 1998, 1990, 1999, 1992, 1973, 1949, 2024, 1981])
```

We can start builing our for loop as follows, and test out our naming convention is working before
writing any actual files to disk.

```python
for year in sorted(all_works_df['publication_date'].unique()):
    filename = 'yearly_files/{}Publications.csv'.format(year)
    print(filename)

yearly_files/1948Publications.csv
yearly_files/1949Publications.csv
yearly_files/1973Publications.csv
yearly_files/1981Publications.csv
yearly_files/1990Publications.csv
yearly_files/1992Publications.csv
etc.
```

We can now add the rest of the steps we need to create separate text files:

```python

for year in sorted(all_works_df['publication_date'].unique()):
    filename = 'yearly_files/{}Publications.csv'.format(year)
    # Select data for the year
    publish_year = all_works_df[all_works_df.publication_date == year]
    publish_year.to_csv(filename)
```

Look inside the `yearly_files` directory and check a couple of the files you
just created to confirm that everything worked as expected.

## Writing Unique Filenames

Notice that the code above created a unique filename for each year.

```
filename = 'yearly_files/{}Publications.csv'.format(year)
```

Let's break down the parts of this name:

- The first part is simply some text that specifies the directory to store our
  data file in `yearly_files/`
- We want to dynamically insert the value of the year into the filename. We can
  do this by indicating a placeholder location inside the string with `{}`, and
  then specifying the value that will go there with `.format(value)`
- Finally, we specify a fixed portion of the filename and the file type with 
  `Publications.csv`. Since the `.` is inside the string, it doesn't behave the 
  same way as dot notation in Python commands.

Notice the difference between the filename - wrapped in quote marks - and the
variable (`year`), which is not wrapped in quote marks. The result looks like
`'yearly_files/1948Publications.csv'` which contains the path to the new filename
AND the file name itself.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Building loops

1. Build a loop that reads in each publications csv file into a dataframe, counts how
  many records are in the df, and then prints that number out to the screen along with the filename.
  Recommend using the glob library to simplfy this task, but if you are creative you can do it with os library
  instead.


2. Compare your results to the output of `all_works_df['publication_date'].value_counts().sort_index()`

:::::::::: solution
1. 
``` python
import glob
files = sorted(glob.glob('yearly_files/*Publication*.csv'))

for f in files:
    df = pd.read_csv(f)
    print(f[13:17],df.shape[0])

```

:::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Building reusable and modular code with functions

Suppose that separating large data files into individual yearly files is a task
that we frequently have to perform. We could write a **for loop** like the one above
every time we needed to do it but that would be time consuming and error prone.
A more elegant solution would be to create a reusable tool that performs this
task with minimum input from the user. To do this, we are going to turn the code
we've already written into a function.

Functions are reusable, self-contained pieces of code that are called with a
single command. They can be designed to accept arguments as input and return
values, but they don't need to do either. Variables declared inside functions
only exist while the function is running and if a variable within the function
(a local variable) has the same name as a variable somewhere else in the code,
the local variable hides but doesn't overwrite the other.

Every method used in Python (for example, `print`) is a function, and the
libraries we import (say, `pandas`) are a collection of functions. We will only
use functions that are housed within the same code that uses them, but it's also
easy to write functions that can be used by different programs.

Functions are declared following this general structure:

```python
def this_is_the_function_name(input_argument1, input_argument2):
    # The body of the function is indented
    """This is the docstring of the function. Wrapped in triple-quotes,
    it can span across multiple lines. This is what is shown if you ask
    for help about the function like this: help(this_is_the_function_name)
    """
    
    # This function prints the two arguments to screen
    print('The function arguments are:', input_argument1, input_argument2, '(this is done inside the function!)')

    # And returns their product
    return input_argument1 * input_argument2
```

The function declaration starts with the word `def`, followed by the function
name and any arguments in parenthesis, and ends in a colon. The body of the
function is indented just like loops are. If the function returns something when
it is called, it includes a return statement at the end.

This is how we call the function:

```python
product_of_inputs = this_is_the_function_name(2,5)
The function arguments are: 2 5 (this is done inside the function!)

print('Their product is:', product_of_inputs, '(this is done outside the function!)')
Their product is: 10 (this is done outside the function!)
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Functions

1. Change the values of the arguments in the function and check its output
2. Try calling the function by giving it the wrong number of arguments (not 2)
  or not assigning the function call to a variable (no `var_name =`)
3. Declare a variable inside the function and test to see where it exists (Hint:
  can you print it from outside the function?)
4. Explore what happens when a variable both inside and outside the function
  have the same name. What happens to the global variable when you change the
  value of the local variable?
  

::::::::::::::::::::::::::::::::::::::::::::::::::

We can now turn our code for saving yearly data files into a function. There are
many different "chunks" of this code that we can turn into functions, and we can
even create functions that call other functions inside them. Let's first write a
function that separates data for just one year and saves that data to a file:

```python
def one_year_csv_writer(this_year, all_data):
    """
    Writes a csv file for data from a given year.
    
    Parameters
    ----------
    this_year: int
        year for which data is extracted
    all_data: pandas Dataframe
        DataFrame with multi-year data
    
    Returns
    -------
    None
    """

    # Select data for the year
    texts_year = all_data[all_data.publication_date == this_year]

    # Write the new DataFrame to a csv file
    filename = 'yearly_files/function_works' + str(this_year) + '.csv'
    texts_year.to_csv(filename)
```

The text between the two sets of triple quotes is called a docstring and
contains the documentation for the function. It does nothing when the function
is running and is therefore not necessary, but it is good practice to include
docstrings as a reminder of what the code does. Docstrings in functions also
become part of their 'official' documentation:

```python
one_year_csv_writer?
```

```python
one_year_csv_writer(2023,all_works_df)
```

We changed the root of the name of the csv file so we can distinguish it from
the one we wrote before. Check the `yearly_files` directory for the file. Did it
do what you expect?

What we really want to do, though, is create files for multiple years without
having to request them one by one. Let's write another function that replaces
the entire *for* loop by simply looping through a sequence of years and repeatedly
calling the function we just wrote, `one_year_csv_writer`:

```python
def yearly_data_csv_writer(start_year, end_year, all_data):
    """
    Writes separate csv files for each year of data.

    Parameters
    ----------
    start_year: int
        the first year of data we want
    end_year: int
        the last year of data we want
    all_data: pandas Dataframe
        DataFrame with multi-year data

    Returns
    -------
    None
    """
    years = all_data.publication_date.unique()
    # "end_year" is the last year of data we want to pull, so we loop to end_year+1
    for year in range(start_year, end_year+1):
      if year in years:
        one_year_csv_writer(str(year), all_data)
```

Because people will naturally expect that the end year for the files is the last
year with data, the for loop inside the function ends at `end_year + 1`. By
writing the entire loop into a function, we've made a reusable tool for whenever
we need to break a large data file into yearly files. Because we can specify the
first and last year for which we want files, we can even use this function to
create files for a subset of the years available. This is how we call this
function:

```python
# Create csv files
yearly_data_csv_writer(2008, 2023, all_works_df)
```

BEWARE! If you are using IPython Notebooks and you modify a function, you MUST
re-run that cell in order for the changed function to be available to the rest
of the code. Nothing will visibly happen when you do this, though, because
simply defining a function without *calling* it doesn't produce an output. Any
cells that use the now-changed functions will also have to be re-run for their
output to change.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge- More functions

1. Add two arguments to the functions we wrote that take the path of the
  directory where the files will be written and the root of the file name.
  Create a new set of files with a different name in a different directory.
2. How could you use the function `yearly_data_csv_writer` to create a csv file
  for only one year? (Hint: think about the syntax for `range`)
3. Make the functions return a list of the files they have written. There are
  many ways you can do this: either of the
  functions can print to screen, either can use a return statement to give back
  numbers or strings to their function call, or you can use some combination of
  the two. You could also try using the `os` library to list the contents of
  directories.
4. Explore what happens when variables are declared inside each of the functions
  versus in the main (non-indented) body of your code. What is the scope of the
  variables (where are they visible)? What happens when they have the same name
  but are given different values?

::::::: solution
1. 
  ```python
  def one_year_csv_writer(this_year, all_data, folder_to_save, root_name):
      """
      Writes a csv file for data from a given year.

      Parameters
      ---------
      this_year : int
          year for which data is extracted
      all_data: pd.DataFrame
          DataFrame with multi-year data 
      folder_to_save : str
          folder to save the data files
      root_name: str
          root of the filenames to save the data
      """

      # Select data for the year
      texts_year = all_data[all_data.year == this_year]

      # Write the new DataFrame to a csv file
      filename = os.path.join(folder_to_save, ''.join([root_name, str(this_year), '.csv']))
      texts_year.to_csv(filename)

def yearly_data_csv_writer(start_year, end_year, all_data, folder_to_save, root_name):
    """
    Writes separate csv files for each year of data.

    Parameters
    ----------
    start_year: int
        the first year of data we want
    end_year: int
        the last year of data we want
    all_data: pandas Dataframe
        DataFrame with multi-year data
    folder_to_save : str
          folder to save the data files
    root_name: str
          root of the filenames to save the data
    Returns
    -------
    None
    """
    years = all_data.publication_date.unique()
    # "end_year" is the last year of data we want to pull, so we loop to end_year+1
    for year in range(start_year, end_year+1):
      if year in years:
        one_year_csv_writer(str(year), all_data, folder_to_save, root_name)

2. 

3. 

```


:::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

The functions we wrote demand that we give them a value for every argument.
Ideally, we would like these functions to be as flexible and independent as
possible. Let's modify the function `yearly_data_csv_writer` so that the
`start_year` and `end_year` default to the full range of the data if they are
not supplied by the user. Arguments can be given default values with an equal
sign in the function declaration. Any arguments in the function without default
values (here, `all_data`) is a required argument and MUST come before the
argument with default values (which are optional in the function call).
 
```python
def yearly_data_csv_writer(all_data, start_year=1948, end_year=2024):
    """
    Writes separate csv files for each year of data.

    Parameters
    ----------
    start_year: int
        the first year of data we want
    end_year: int
        the last year of data we want
    all_data: pandas Dataframe
        DataFrame with multi-year data

    Returns
    -------
    None
    """
    years = all_data.publication_date.unique()
    # "end_year" is the last year of data we want to pull, so we loop to end_year+1
    for year in range(start_year, end_year+1):
      if year in years:
        one_year_csv_writer(str(year), all_data)
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Optional Variables

1. What happens if you only include a value for `start_year` in the function
  call? Can you write the function call with only a value for `end_year`? (Hint:
  think about how the function must be assigning values to each of the arguments -
  this is related to the need to put the arguments without default values before
  those with default values in the function definition!)
  

::::::::::::::::::::::::::::::::::::::::::::::::::

## If Loops

The body of our function used a conditional *if* loop to
check that values of `year` are in the dataframe. *If* loops execute the body of
the loop when some condition is met. They commonly look something like this:

```python
a = 5

if a<0: # meets first condition?

    # if a IS less than zero
    print('a is a negative number')

elif a>0: # did not meet first condition. meets second condition?

    # if a ISN'T less than zero and IS more than zero
    print('a is a positive number')

else: # met neither condition

    # if a ISN'T less than zero and ISN'T more than zero
    print('a must be zero!')
```

Which would return:

```
a is a positive number
```

Change the value of `a` to see how this function works. The statement `elif`
means "else if", and all of the conditional statements must end in a colon.


:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Modifying functions

1. The code below checks to see whether a directory exists and creates one if it
  doesn't. Add some code to your function that writes out the CSV files, to check
  for a directory to write to.

```Python
  if 'dir_name_here' in os.listdir('.'):
      print('Processed directory exists')
  else:
      os.mkdir('dir_name_here')
      print('Processed directory created')
```
:::::::: solution
  ```python
  def one_year_csv_writer(this_year, all_data, folder_to_save, root_name):
      """
      Writes a csv file for data from a given year.

      Parameters
      ---------
      this_year : int
          year for which data is extracted
      all_data: pd.DataFrame
          DataFrame with multi-year data 
      folder_to_save : str
          folder to save the data files
      root_name: str
          root of the filenames to save the data
      """
      if folder_to_save in os.listdir('.'):
        print('Output directory exists')
      else:
        os.mkdir(folder_to_save)
        print('folder_to_save, directory created')
      # Select data for the year
      texts_year = all_data[all_data.year == this_year]

      # Write the new DataFrame to a csv file
      filename = os.path.join(folder_to_save, ''.join([root_name, str(this_year), '.csv']))
      texts_year.to_csv(filename)
      ```

:::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::


