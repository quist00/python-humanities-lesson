---
title: Starting With Data
teaching: 30
exercises: 30
---

::::::::::::::::::::::::::::::::::::::: objectives

- Navigate the workshop directory and download a dataset.
- Explain what a library is and what libraries are used for.
- Describe what the Python Data Analysis Library (Pandas) is.
- Load the Python Data Analysis Library (Pandas).
- Use `read_csv` to read tabular data into Python.
- Describe what a DataFrame is in Python.
- Access and summarize data stored in a DataFrame.
- Define indexing as it relates to data structures.
- Perform basic mathematical operations and summary statistics on data in a Pandas DataFrame.
- Create simple plots.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I import data in Python?
- What is Pandas?
- Why should I use Pandas to work with data?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Working With Pandas DataFrames in Python

We can automate the processes listed above using Python. It is efficient to spend time
building code to perform these tasks because once it is built, we can use our code
over and over on different datasets that share a similar format. This makes our
methods easily reproducible. We can also share our code with colleagues
so they can replicate our analysis.

#### Starting in the same spot

If you do not already have JupyterLab opened from the workshop directroy, let's do that now. 
Doing so should help us avoid path and file name issues. At this time please
navigate to the workshop directory in your terminal.

*Opening JupyterLab*

```bash
# navigate to or start your terminal from the worskhop directory
$ jupyter lab
# you may see various status messages as it spins up jupyter server and launches your default browser
```

A quick aside that there are Python libraries like [OS
Library](https://docs.python.org/3/library/os.html) that can work with our
directory structure, however, that is not our focus today.

#### Alex's Processing

Alex is a researcher who is interested in exploring the collection of fictional works at their university and assessing how representative it is in relation to the student body. Alex was able to find a pre-liminary assembled by a previous researcher from which they want to create some exploratory plots and intermediate datasets in order to determine next steps to expand on this line of inquiry.

Alex coul do some of this work using spreadsheet systems but this can be time consuming to do  and revise. It can lead to mistakes that are hard to detect.

The next steps show how Python can be used to automate some of the processes.

As a result of creating Python scripts, the data can be re-run in the future.

#### Our Data

For this lesson, we will be using the EEBO catalogue data, a subset of the data
from EEBO/TCP
[Early English Books Online/Text Creation Partnership](https://eebo.chadwyck.com/home)

We will be using files from the data folder.
This section will use the `all_works.csv` file that can be found in your data folder.

 The dataset is stored as a comma separated (`.csv`) file, where each row holds information for a single title, and the columns represent diferent aspects (variables) of each entry:

| Column            | Description                                                       | 
| ----------------- | ----------------------------------------------------------------- |
| title             | Title of Work                                                     | 
| subjects          | List of Subjects                                                  | 
| mms_id            | Metadata Management System ID                                     | 
| author            | Author(s) delimited with ';'                                      | 
| publication_date  | Year of Publication                                               | 
| publication_place | Place of Publication                                              | 
| language_code     | Language of Text                                                  |           
| resource_type     | Type of resource, e.g. physical, electronic                       | 
| acquisition_date  | Date of Acquisition                                               | 
| is_dei            | Boolean for whether work meets broadest definition of a DEI work  | 
| checkouts         | Count of number of times                                          | 

If we open the `all_works.csv` data file using a text editor, the first few rows of our first file look like this:

```
title,subjects,mms_id,author,publication_date,publication_place,language_code,resource_type,acquisition_date,is_dei,subject_count,chekcouts
"""A god of justice?"" : the problem of evil in twentieth-century Black literature","['American literature--African American authors--History and criticism', 'African Americans--Intellectual life--20th century', 'African Americans--Intellectual lif', 'American literature--African American author']",991004617919702908,"Whitted, Qiana J., 1974-",2009.0,Charlottesville,eng,Book - Physical,2016-06-26 23:51:01,True,4,
"""Baad bitches"" and sassy supermamas : Black power action films","['Blaxploitation films--United States--History and criticism', 'African American women heroes in motion pictures', 'African American women heroes in motion picture', 'Blaxploitation film', 'Blaxploitation Film']",991003607949702908,"Dunn, Stephane, 1967-",2008.0,Urbana,eng,Book - Physical,2011-12-13 03:51:20,True,5,1.0
"""Codependent lesbian space alien seeks same""","['Lesbians--Drama', 'Lesbians', 'Video recordings for the hearing impaired']",991013190057102908,"Olnek, Madeleine.; Space Aliens, LLC.",2011.0,"[New York, NY?]",eng,Projected medium - Physical,2016-11-10 07:56:09,True,3,1.0
"""Lactilla tends her fav'rite cow"" : ecocritical readings of animals and women in eighteenth-century British labouring-class women's poetry","['English poetry--Women authors--History and criticism', 'Working class writings, English--History and criticism', 'Ecofeminism in literature', 'English poetry--Women authors', 'Working class women in literature', 'Working class writings, English']",991003662979702908,"Milne, Anne.",2008.0,Lewisburg,eng,Book - Physical,2015-10-07 16:31:28,True,8,
```

***

### About Libraries

A library in Python contains a set of tools (functions) that perform different
actions on our data. Importing a library is like getting a set of particular tools
out of a storage locker and setting them up on the bench for use in a project.
Once a library is set up, its functions can be used or called to perform different tasks.

### Pandas in Python

One of the best options for working with tabular data in Python is to use the
[Python Data Analysis Library](https://pandas.pydata.org/) (a.k.a. Pandas Library). The
Pandas library provides structures to sort our data, can produce high quality plots in conjunction with other libraries such as
[matplotlib](https://matplotlib.org/), and integrates nicely with libraries
that use [NumPy](https://www.numpy.org/) (which is another common Python library) arrays.

Python doesn't load all of the libraries available to it by default. We have to
add an `import` statement to our code in order to use library functions required for our project. To import
a library, we use the syntax `import libraryName`, where `libraryName` represents the name of the specific library we want to use. Note that the `import` command calls a library that has been installed previously in our system. If we use the `import` command to call for a library that does not exist in our local system, the command will throw and error when executed. In this case, you can use `pip` command in another termina window to install the missing libraries. [See here for details](https://github.com/resbaz/Intro_Python_Nov2017/blob/master/Python_Installation.md) on how to do this.

Moreover, if we want to give the
library a nickname to shorten the command, we can add `as nickNameHere`.  An
example of importing the pandas library using the common nickname `pd` is:

```python
import pandas as pd
```

Each time we call a function that's in a library, we use the syntax
`LibraryName.FunctionName`. Adding the library name with a `.` before the
function name tells Python where to find the function. In the example above, we
have imported Pandas as `pd`. This means we don't have to type out `pandas` each
time we call a Pandas function.

## Reading CSV Data Using Pandas

We will begin by locating and reading in a data which in CSV format.
We can use Pandas' `read_csv` function to pull either a local (a file in our machine) or a remote (one that is available for downloading from the web) file into a Pandas table or
[DataFrame](https://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe).

In order to read data in, we need to know where the data is stored on our computer or its URL address if the file is available on the web.
It is recommended to place the data files in the same directory as the Jupyter notebook file (ending in .ipynb) if working with local files

```python
# note that pd.read_csv is used because we imported pandas as pd
# note that this assumes that the data file is in the same location
# as the Jupyter notebook
pd.read_csv("all_works.csv")
```

### So What's a DataFrame?

A DataFrame is a 2-dimensional data structure that can store data of different
types (including characters, integers, floating point values, factors and more)
in columns. It is similar to a spreadsheet or a `data.frame` in
R. A DataFrame always has an index (0-based integers by default). An index refers to the position of
an element in the data structure.

The above command yields the **output** similar to below, but formatted differently

```
                                                   title                                           subjects  ...  is_dei checkouts
0      "A god of justice?" : the problem of evil in t...  ['American literature--African American author...  ...    True         0
1      "Baad bitches" and sassy supermamas : Black po...  ['Blaxploitation films--United States--History...  ...    True         1
2           "Codependent lesbian space alien seeks same"  ['Lesbians--Drama', 'Lesbians', 'Video recordi...  ...    True         1
3      "Lactilla tends her fav'rite cow" : ecocritica...  ['English poetry--Women authors--History and c...  ...    True         0
4      "The useless mouths", and other literary writings  ['French drama--20th century--Translations int...  ...    True         1
...                                                  ...                                                ...  ...     ...       ...
14227  ¡Ban c/s this! : the BSP anthology of Xican@ l...  ["Littérature américaine--Auteurs américain...  ...   False         0
14228  ¡Muy pop! : conversations on Latino popular cu...  ['Popular culture--United States', "Littératu...  ...   False         1
14229  ¡Viva la historieta! : Mexican comics, NAFTA, ...  ['Mondialisation dans la littérature', "Mondi...  ...   False         0
14230                 Đời về cơ bản là buồn cười  ['Life--Comic books, strips, etc', 'Life', 'Co...  ...   False         3
14231  Đường vào văn chương : phê bình lý trí ...  ['Literature, Modern--History and criticism--T...  ...   False         0

[14232 rows x 11 columns]
```

We can see that there were 14232 rows parsed. Each row has 11
columns. The first column displayed is the **index** of the DataFrame. The index is used to
identify the position of the data, but it is not an actual column of the DataFrame.
It looks like  the `read_csv` function in Pandas  read our file properly. However,
we haven't saved any data to memory so we can not work with it yet. We need to assign the
DataFrame to a variable so we can call and use the data. Remember that a variable is a name given to a value, such as `x`,
or  `data`. We can create a new  object with a variable name by assigning a value to it using `=`.

Let's call the imported data `all_works_df`:

```python
all_works_df = pd.read_csv("all_works.csv")
```

Notice that when you assign the imported DataFrame to a variable, Python does not
produce any output on the screen. We can print the value of the `all_works_df`
object by typing its name into the cell and running it.  If you were doing this in a script file, you would need to use `print(all_works_df)`. Not needing to do so here is another convenience feature of Jupyter.

```python
all_works_df
```

which prints contents like above

### Manipulating Our Index Data

Now we can start manipulating our data. First, let's check the data type of the
data stored in `all_works_df` using the `type` method. **The `type` method and
`__class__` attribute** tell us that `all_works_df` is `<class 'pandas.core.frame.DataFrame'`.

```python
type(all_works_df)
# this does the same thing as the above!
all_works_df.__class__
```

We can also run `all_works_df.dtypes` in a cell to view the data type for each
column in our DataFrame. `int64` represents numeric integer values - `int64` cells
can not store decimals. `object` represents strings (letters and numbers). `float64`
represents numbers with decimals.

```
all_works_df.dtypes
```

which returns output similar to:

```
title                object
subjects             object
mms_id                int64
author               object
publication_date      int64
publication_place    object
language_code        object
resource_type        object
acquisition_date     object
is_dei                 bool
checkouts             int64
dtype: object
```


#### Useful Ways to View DataFrame objects in Python

We can use attributes and methods provided by the DataFrame object to summarize and access the data stored in it.

To access an attribute, use the DataFrame object name followed by the attribute
name `df_object.attribute`. For example, we can access the [Index object](https://pandas.pydata.org/docs/reference/indexing.html) containing the column names of `all_works_df` by using its `columns` attribute

```python
all_works_df.columns
```

As we will see later, we can use the contents of the Index object to extract (slice) specific records from our DataFrame based on their values.

Methods are called by using the syntax `df_object.method()`. Note the inclusion of open brackets at the end of the method. Python treats methods as **functions** associated with a dataframe rather than just a property of the object as with attributes. Similarly to functions, methods can include optional parameters inside the brackets to change their default behaviour.

As an example, `all_works_df.head()` gets the first few rows in the DataFrame
`all_works_df` using **the `head()` method**. With a method, we can supply extra
information within the open brackets to control its behaviour.

Let's look at the data using these.

:::::::::::::::::::::::::::::::::::::::  challenge

### Challenge - DataFrames

Using our DataFrame `all_works_df`, try out the attributes \& methods below to see
what they return.

1. `all_works_df.columns`

2. `all_works_df.shape` Take note of the output of `shape` - what format does it
  return the shape of the DataFrame in?
  
  HINT: [More on tuples, here](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences).

3. `all_works_df.head()` Also, what does `all_works_df.head(15)` do?

4. `all_works_df.tail()`
  

::::::::::::::::::::::::::::::::::::::::::::::::::

### Calculating Statistics From Data In A Pandas DataFrame

We've read our data into Python. Next, let's perform some quick summary
statistics to learn more about the data that we're working with. We might want
to know how many different authors are included in our dataset, or how many works were published in a given year. We can perform summary stats quickly using groups. But
first we need to figure out what we want to group by.

Let's begin by exploring our data:

```python
# Look at the column names
all_works_df.columns.values
```

which **returns**:

```
array(['title', 'subjects', 'mms_id', 'author', 'publication_date',
       'publication_place', 'language_code', 'resource_type',
       'acquisition_date', 'is_dei', 'checkouts'], dtype=object)
```

Let's get a list of all the publication locations. The `pd.unique` function tells us all of
the unique values in the `publication_place` column.

```python
pd.unique(all_works_df['publication_place'])
```

which **returns**:

```python
array(['Charlottesville', 'Urbana', '[New York, NY?]', ...,
       'Cambridge [UK] ; Medford', 'Arles', '[London]:'], dtype=object)
```

:::::::::::::::::::::::::::::::::::::::  challenge

### Challenge - Statistics

1. Create a list of unique publicaton years found in the data. Call it
  `years`. How many unique years are there in the data?

2. What is the difference between `len(years)` and `all_works_df['publication_date'].nunique()`?
  
  TODO: Solution

::::::::::::::::::::::::::::::::::::::::::::::::::

## Summary Statistics \& Groups in Pandas

We often want to calculate summary statistics for our data.  This can be useful even at the exploratory phase as it can help you understand the ranges of your data and detect possible outliers.


We can calculate basic statistics for all records in a single column using the
syntax below:

```python
all_works_df['checkouts'].describe()
```

gives **output**

```python
count    14232.000000
mean         0.526349
std          1.432189
min          0.000000
25%          0.000000
50%          0.000000
75%          1.000000
max         43.000000
Name: checkouts, dtype: float64
```

We can also extract specific metrics for one or various columns if we wish:

```python
all_works_df['checkouts'].min()
all_works_df['checkouts'].max()
all_works_df['checkouts'].mean()
all_works_df['checkouts'].std()
all_works_df['checkouts'].count()
```

But if we want to summarize by one or more variables, for example checkouts by language_code, we can
use the **`.groupby` method**. When executed, this method creates a DataFrameGroupBy object containing a subset of the original DataFrame. Once we've created it, we
can quickly calculate summary statistics by a group of our choice. For example the following code will group our data by language_code.

```python
# Group data by status
grouped_data = all_works_df.groupby('language_code')
```

If we execute the **pandas function `describe`** on this new object we will obtain descriptive stats for all the numerical columns in `all_works_df` grouped by the different cities available in the `publication_place` column of the DataFrame.

```python
# summary statistics for all numeric columns by place
grouped_data.describe()
# provide the mean for each numeric column by place
grouped_data.mean()
```

`grouped_data.mean(numeric_only=True)` **OUTPUT:**

```python
	mms_id	publication_date	is_dei	checkouts
language_code				
ara	9.910032e+17	2009.000000	0.400000	0.000000
bos	9.910132e+17	2018.000000	0.500000	0.000000
cat	9.910131e+17	2011.000000	0.500000	0.000000
chi	9.910073e+17	2011.400000	0.500000	0.700000
# Truncated for brevity
```

The `groupby` command is powerful in that it allows us to quickly generate
summary stats.

:::::::::::::::::::::::::::::::::::::::  challenge

### Challenge - Summary Data

Create your own groupy object based on publicatin_place and take the mean.

1. What is the mean number of checkout for works published in `[Washington, D.C.]`.
2. What does the summary stats uncover about the quality of data in the publication_place column?
3. What happens when you group by two columns using the following syntax and
  then grab mean values:

- `grouped_data2 = all_works_df.groupby(['resource_type','language_code'])
   grouped_data2.mean(numeric_only=True)`


:::::::::::::::  solution

TODO:

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Quickly Creating Summary Counts in Pandas

Let's next count the number of publications for each author(s). We can do this in a few
ways, but we'll use `groupby` combined with **a `count()` method**.

```python
# count the number of texts by authors / groups of authors
author_counts = all_works_df.groupby('author')['mms_id'].count()
author_counts
```

Or, we can also count just the rows that have the a specifig author like  "Abate, Michelle Ann, 1975-":

```python
author_counts = all_works_df.groupby('author')['mms_id'].count()['Abate, Michelle Ann, 1975-']
author_counts
```


### Basic Math Functions

If we wanted to, we could perform math on an entire numerical column of our data. To demonstrate this, let's add another column that shows what percentage of total checkouts for each work.

```
all_checkouts = all_works_df.checkouts.sum()
all_works_df["checkout_percentage"] = (all_works_df.checkouts / all_checkouts) * 100
all_works_df.checkout_percentage
```

## Quick \& Easy Plotting Data Using Pandas

We can plot our summary stats using Pandas, too.

```python
# group data
is_dei_count = all_works_df.groupby("is_dei")["mms_id"].count() 
# set equal to variable so we can set additional parameters
plot = is_dei_count.plot(kind="bar", title="Checkout by DEI Status")
#lablel the y-axis
plot.set_ylabel("Checkouts")
```


What does this graph show? Let's step through

- `all_works_df.groupby("is_dei")` : This groups the works by the resource type.
- `all_works_df.groupby("is_dei")["mms_id"]` : This chooses a single column to count,
  rather than counting all columns since we only want one number to plot. You could effectively pick an column.
- `all_works_df.groupby("is_dei")["mms_id"].count()` : this counts the instances, i.e. how many works per given resource type?


- `plot = is_dei_count.plot(kind="bar",logy=True)` : this plots a bar chart with the resource type on x axis and count on the y axis.
- `plot.set_ylabel("Checkouts")` : this labels the y-axis

````

:::::::::::::::::::::::::::::::::::::::  challenge
 ## Summary Plotting Challenge

 Create a stacked bar plot, showing the checkouts, per language_code, with
 the is_dei stacked on top of each other for the 10 languages with the most checkouts. Drop any records where is_dei True column is NAN. The language_code should go on the X axis, and the checkouts on the Y axis. Some tips are below
 to help you solve this challenge:

 * [For more on Pandas plots, visit this link.](http://pandas.pydata.org/pandas-docs/stable/visualization.html#basic-plotting-plot)
 * You can use the code that follows to create a stacked bar plot but the data to stack
  need to be in individual columns.  Here's a simple example with some data where
  'a', 'b', and 'c' are the groups, and 'one' and 'two' are the subgroups.

 ```
 d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
 pd.DataFrame(d)
 ```

 shows the following data

 ```
       one  two
   a    1    1
   b    2    2
   c    3    3
   d  NaN    4
 ```

 We can plot the above with

 ```
 # plot stacked data so columns 'one' and 'two' are stacked
 my_df = pd.DataFrame(d)
 my_df.plot(kind='bar', stacked=True, title="The title of my graph")
 ```

 ![Stacked Bar Plot](../fig/stackedBar1.png)

 * You can use the `.unstack()` method to transform grouped data into columns
 for each plotting.  Try running `.unstack()` on some DataFrames above and see
 what it yields.

 Start by transforming the grouped data into an unstacked layout, then create
 a stacked plot.

:::::::::::::::  solution

 First we group data by language_code and then by is_dei. 

 ```python
grouping = all_works_df.groupby(['language_code','is_dei'])
checkouts = grouping['checkouts'].sum()
top_checkouts = checkouts.sort_values(ascending=False).head(10)
 ```

 The last two lines above calculates the sum of checkouts, for each language_code further broken down be DEI boolean flag, as a table and keeps only the top 10 most checked out items.

 ```
language_code  is_dei
is_dei	False	True
language_code		
eng	4441.0	2530.0
spa	95.0	92.0
fre	70.0	62.0
per	27.0	26.0
ita	21.0	NaN
jpn	NaN	20.0
 ```
 
 After that, we use the `.unstack()` function on our grouped data to figure
 out the total contribution of DEI verse non-DEI for each language_code, and then plot the
 data. 
 ```python
 plot = top_checkouts.unstack.plot(kind="bar", stacked=True, title="DEI Checkouts By Language", figsize=(10,5),logy=True)
 plot.set_ylabel("Checkouts")
 ```



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::


