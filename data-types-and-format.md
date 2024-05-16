---
title: Data Types and Formats
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Describe how information is stored in a Python DataFrame.
- Define the two main types of data in Python: text and numerics.
- Examine the structure of a DataFrame.
- Modify the format of values in a DataFrame.
- Describe how data types impact operations.
- Define, manipulate, and interconvert integers and floats in Python.
- Analyze datasets having missing/null values (NaN values).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What types of data can be contained in a DataFrame?
- Why is the data type important?

::::::::::::::::::::::::::::::::::::::::::::::::::

The format of individual columns and rows will impact analysis performed on a
dataset read into python. For example, you can't perform mathematical
calculations on a string (text formatted data). This might seem obvious,
however sometimes numeric values are read into python as strings. In this
situation, when you then try to perform calculations on the string-formatted
numeric data, you get an error.

In this lesson we will review ways to explore and better understand the
structure and format of our data.

## Types of Data

How information is stored in a
DataFrame or a python object affects what we can do with it and the outputs of
calculations as well. There are two main types of data that we're explore in
this lesson: numeric and text data types.

### Numeric Data Types

Numeric data types include integers and floats. A **floating point** (known as a
float) number has decimal points even if that decimal point value is 0. For
example: 1.13, 2.0 1234.345. If we have a column that contains both integers and
floating point numbers, Pandas will assign the entire column to the float data
type so the decimal points are not lost.

An **integer** will never have a decimal point. Thus if we wanted to store 1.13 as
an integer it would be stored as 1. Similarly, 1234.345 would be stored as 1234. You
will often see the data type `Int64` in python which stands for 64 bit integer. The 64
simply refers to the memory allocated to store data in each cell which effectively
relates to how many digits it can store in each "cell". Allocating space ahead of time
allows computers to optimize storage and processing efficiency.

### Text Data Type

Text data type is known as Strings in Python, or Objects in Pandas. Strings can
contain numbers and / or characters. For example, a string might be a word, a
sentence, or several sentences. A Pandas object might also be a plot name like
'plot1'. A string can also contain or consist of numbers. For instance, '1234'
could be stored as a string. As could '10.23'. However **strings that contain
numbers can not be used for mathematical operations**!

Pandas and base Python use slightly different names for data types. More on this
is in the table below:

| Pandas Type           | Native Python Type                    | Description                                                                                                                                                   | 
| --------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| object                | string                                | The most general dtype. Will be assigned to your column if column has mixed types (numbers and strings).                                                      | 
| int64                 | int                                   | Numeric characters. 64 refers to the memory allocated to hold this character.                                                                                 | 
| float64               | float                                 | Numeric characters with decimals. If a column contains numbers and NaNs(see below), pandas will default to float64, in case your missing value has a decimal. | 
| datetime64, timedelta[ns] | N/A (but see the [datetime] module in Python's standard library)                     | Values meant to hold time data. Look into these for time series experiments.                                                                                  | 

### Checking the format of our data

Now that we're armed with a basic understanding of numeric and text data
types, let's explore the format of our survey data. 

We alreayd learned that `DataFrame.dtypes` will return info on all the columns,
but we can also check the type of one column in a DataFrame using the syntax
`dataFrameName[column_name].dtype`:

```python
works_df['subjects'].dtype
```

**OUTPUT:** `dtype('O')`

A type 'O' just stands for "object" which in Pandas' world is a string
(text).

```python
works_df['publication_date'].dtype
```

**OUTPUT:** `dtype('int64')`

The type `int64` tells us that python is storing each value within this column
as a 64 bit integer. We can use the `dat.dtypes` command to view the data type
for each column in a DataFrame (all at once).


### Working With Integers and Floats

Integers are the numbers we usually count
with. Floats have fractional parts (decimal places).  Let's next consider how
the data type can impact mathematical operations on our data. Addition,
subtraction, division and multiplication work on floats and integers as we'd expect.

```python
print(5+5)
10

print(24-4)
20
```

If we divide one integer by another, we get a float.
The result on python 3 is different than in python 2, where the result is an
integer (integer division).

```python
print(5/9)
0.5555555555555556

print(10/3)
3.3333333333333335
```

We can also convert a floating point number to an integer or an integer to
floating point number. Notice that Python by default rounds down when it
converts from floating point to integer.

```python
# convert a to integer
a = 7.83
int(a)
7

# convert to float
b = 7
float(b)
7.0
```

## Pandas and type conversion

Getting back to our data, we can modify the format of values within it.
Pandas interpreted the `mms_id` field as and integer, but it is often more
appropriate to treat id fields as strings, so let's do that now.

```python
# convert the record_id field from an integer to a string
works_df['mms_id'] = works_df['mms_id'].astype('str')
works_df['mms_id'].dtype
```

**OUTPUT:** `dtype('O')`

While we only demonstrated this with *str* others like *int*, *float*, etc., but not all conversions
will succeed and some may succeed but discard information, so you should always check on the results of
any conversions.

## Missing Data Values - NaN

Our *all_works.csv* file was carefully constructed to have no missing values in the form of `null` or `NaN`, so 
we will be importing a variant of that file that contains missing values. 

```python

null_df = pd.read_csv('null.csv')
pd.isnull(null_df)
```

A snippet of the output is below:

```python
	title	subjects	mms_id	author
0	False	False	False	False	
1	False	False	False	False
2	False	False	False	False
3	False	False	False	False
4	False	False	False	False	
```

*NaN* (**N**ot **a** **N**umber) values are undefined values that cannot be 
represented mathematically. Pandas, for example, will read an empty cell in
 a CSV or Excel sheet as a NaN. NaNs have some desirable properties: if we
were to average the checkouts column in our *null_df* without replacing NaNs, Pandas would know to skip
over those cells.

```python
null_df['checkouts'].mean()
0.5260636799098337
```

Dealing with missing data values is always a challenge. It's sometimes hard to
know why values are missing - was it because of a data entry error? Or data that
someone was unable to collect? Should the value be 0? We need to know how
missing values are represented in the dataset in order to make good decisions.
If we're lucky, we have some metadata that will tell us more about how null
values were handled.

For instance, in some disciplines, like Remote Sensing, missing data values are
often defined as -9999. Having a bunch of -9999 values in your data could really
alter numeric calculations. Often in spreadsheets, cells are left empty where no
data are available. Pandas will, by default, replace those missing values with
NaN. However it is good practice to get in the habit of intentionally marking
cells that have no data, with a no data value! That way there are no questions
in the future when you (or someone else) explores your data.

### Where Are the NaN's?

#### Finding Columns

Two options for finding what columns contain *NaN* values are as follows: 

```python
# this method provides several columns of information including one that is a count of the not null values in
# each colum
null_df.info()
```
which **returns**:
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 14232 entries, 0 to 14231
Data columns (total 11 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   title              14196 non-null  object 
 1   subjects           14196 non-null  object 
 2   mms_id             14196 non-null  float64
 3   author             8505 non-null   object 
 4   publication_date   14232 non-null  float64
 5   publication_place  14196 non-null  object 
 6   language_code      14196 non-null  object 
 7   resource_type      14196 non-null  object 
 8   acquisition_date   14196 non-null  object 
 9   is_dei             14196 non-null  object 
 10  checkouts          14196 non-null  float64
dtypes: float64(3), object(8)
memory usage: 1.2+ MB
```

```python
# this method takes advantage of the interpretation of true as 1 and false as 0
# to return a series 
null_df.isnull().sum()
```
which **returns**:
```
title                  36
subjects               36
mms_id                 36
author               5727
publication_date        0
publication_place      36
language_code          36
resource_type          36
acquisition_date       36
is_dei                 36
checkouts              36
dtype: int64
```

#### Finding the actual null entries

 We can use the `isnull` method to do this.
The `isnull` method will compare each cell with a null value. If an element
has a null value, it will be assigned a value of  `True` in the output object.

To select the rows where there are null values, we can use
the mask as an index to subset our data as follows:

```python
# To select just the rows with NaN values, we can use the 'any()' method
null_df[pd.isnull(null_df).any(axis=1)]
```

We can run `isnull` on a particular column too. What does the code below do?

```python
# what does this do?
empty_authors = null_df[null_df['author'].isnull()]
print(empty_authors)
```

Let's take a minute to look at the statement above. We are using the Boolean
object `null_df['author'].isnull()` to filter `null_df`. We are
asking Python to select rows that have a `NaN` value of author. While we obtained 
the booleans using a method of the author column, we could have done it using 
`pd.isnull(null_df['author'])` instead if we preferred.

#### Dealing with the NaN's?
Assuming we determine that missing checkouts indeed should be zero, we
 can replace all *NaN* values with zeroes using the `.fillna()` method:

```python
# fill all NaN values with 0
null_df['checkouts'] = null_df['checkouts'].fillna(0)
```


However NaN and 0 yield different analysis results. The mean value when NaN
values are replaced with 0 is different from when NaN values are simply thrown
out or ignored.

```python
null_df['checkouts'].mean()
0.5247329960652052
```

We can fill NaN values with any value that we chose. While a bit silly, the code below fills all
NaN values in the publication_date column with the mean for mean for all publication_dates.  This
is possible because our publications_date is just the 4 digit year with dtype of integer.

```python
 null_df['publication_date'] = null_df['publication_date'].fillna(null_df['publication_date'].mean())
```

We could also choose to create a subset of our data, only keeping rows that do
not contain NaN values.

The point is to make conscious decisions about how to manage missing data. This
is where we think about how our data will be used and how these values will
impact the scientific conclusions made from the data.

Python gives us all of the tools that we need to account for these issues. We
just need to be cautious about how the decisions that we make impact the validity
of any conclusions drawn from it.



## Recap

What we've learned:

- How to explore the data types of columns within a DataFrame
- How to change the data type
- What NaN values are, how they might be represented, and what this means for your work
- How to replace NaN values, if desired

[datetime]: https://doc.python.org/2/library/datetime.html



