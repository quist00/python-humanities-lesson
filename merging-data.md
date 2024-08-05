---
title: Combining DataFrames with pandas
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Combine data from multiple files into a single DataFrame using merge and concat.
- Combine two DataFrames using a unique ID found in both DataFrames.
- Employ `to_csv` to export a DataFrame in CSV format.
- Join DataFrames using common fields (join keys).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Can I work with data from multiple sources?
- How can I combine data from different data sets?

::::::::::::::::::::::::::::::::::::::::::::::::::

In many "real world" situations, the data that we want to use come in multiple
files. We often need to combine these files into a single DataFrame to analyze
the data. The pandas package provides [various methods for combining
DataFrames](https://pandas.pydata.org/pandas-docs/stable/merging.html) including
`merge` and `concat`.

In order to get more practice with slicing and subsetting, we will opt to create subsets
of the works_df instead of loading in data from multiple files.

```python
top_concat_df = works_df.head()
bottom_concat_df = works_df.tail().reset_index()
left_concat_df = works_df.iloc[:,0:3]
right_concat_df = works_df.iloc[:,3:]
left_merge_df = works_df[['mms_id','title']]
left_merge_df = left_merge_df.drop_duplicates()
right_merge_df = works_df[['mms_id','author']].sample(frac=.8,random_state=42).drop_duplicates()
```

## Concatenating DataFrames

We can use the `concat` function in Pandas to append either columns or rows from
one DataFrame to another.  When we concatenate DataFrames, we need to specify the axis.
 `axis=0` tells Pandas to stack the second DataFrame under the first one. It will 
 automatically detect whether the column names are the same and will stack accordingly.
`axis=1` will stack the columns in the second DataFrame to the RIGHT of the
first DataFrame. To stack the data vertically, we need to make sure we have the
same columns and associated column format in both datasets. When we stack
horizonally, we want to make sure what we are doing makes sense (ie the data are
related in some way).

```python
# stack the DataFrames on top of each other
vertical_stack = pd.concat([top_concat_df, bottom_concat_df], axis=0)

# place the DataFrames side by side
horizontal_stack = pd.concat([left_concat_df, right_concat_df], axis=1)
```

#### Row Index Values and Concat

Have a look at the `vertical_stack` dataframe? Notice anything unusual?
The row indexes for the two data frames `top_concat_df` and `bottom_concat_df`
have been repeated. We can reindex the new dataframe using the `reset_index()` method.

```python
# reset index and drop previous index values rather than convert to data column
vertical_stack.reset_index(drop=True,inplace=True)
```

### Writing Out Data to CSV

We can use the `to_csv` command to do export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash to the file
`vertical_stack.to_csv('foldername/out.csv')`. We use the 'index=False' so that
pandas doesn't include the index number for each line.

```python
# Write DataFrame to CSV
vertical_stack.to_csv('out.csv', index=False)
```

Check out your working directory to make sure the CSV wrote out properly, and
that you can open it! If you want, try to bring it back into python to make sure
it imports properly.

```python
# read our output back into python and make sure all looks good
new_output = pd.read_csv('out.csv')
new_output
```

:::::::::::::::::::::::::::::::::::::::  challenge

### Challenge - Combine Data

Make your own subsets and combine them however you like.  Output at least once and read it back in.
Did you get what you expected?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Joining DataFrames

When we concatenated our DataFrames we simply added them to each other -
stacking them either vertically or side by side. Another way to combine
DataFrames is to use columns in each dataset that contain common values (a
common unique id). Combining DataFrames using a common field is called
"joining". The columns containing the common values are called "join key(s)".
Joining DataFrames in this way is often useful when one DataFrame is a "lookup
table" containing additional data that we want to include in the other.

NOTE: This process of joining tables is similar to what we do with tables in an
SQL database.

### Identifying join keys

To identify appropriate join keys we first need to know which field(s) are
shared between the files (DataFrames). We might inspect both DataFrames to
identify these columns. If we are lucky, both DataFrames will have columns with
the same name that also contain the same data. If we are less lucky, we need to
identify a (differently-named) column in each DataFrame that contains the same
information.


In our example, the join key is the column containing the identifier, which is called `mms_id`.

Now that we have a common key, we are almost ready to join our data. However, since there are
[different types of joins](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/), we
also need to decide which type of join makes sense for our analysis.

### Inner joins

The most common type of join is called an *inner join*. An inner join combines
two DataFrames based on a join key and returns a new DataFrame that contains
**only** those rows that have matching values in *both* of the original
DataFrames.

Inner joins yield a DataFrame that contains only rows where the value being
joins exists in BOTH tables. 

The pandas function for performing joins is called `merge` and an Inner join is
the default option:

```python
merged_inner = pd.merge(left=left_merge_df,right=right_merge_df, on='mms_id')

# what's the size of the output data?
print(merged_inner.shape)
merged_inner
```

**OUTPUT:**

```
(12848, 3)
mms_id	title	author
0	991004617919702908	"A god of justice?" : the problem of evil in t...	Whitted, Qiana J., 1974-
1	991004617919702908	"A god of justice?" : the problem of evil in t...	Whitted, Qiana J., 1974-
2	991003607949702908	"Baad bitches" and sassy supermamas : Black po...	Dunn, Stephane, 1967-
3	991003607949702908	"Baad bitches" and sassy supermamas : Black po...	Dunn, Stephane, 1967-
4	991013190057102908	"Codependent lesbian space alien seeks same"	Olnek, Madeleine.; Space Aliens, LLC.
```

The result of an inner join of `left_merge_df` and `right_merge_df` is a new DataFrame
that contains the combined set of columns from those tables. It
*only* contains rows that have *mms_id* codes that are the same in
both DataFrames. 

The two DataFrames that we want to join were passed to the `merge` function using `on` 
since the key colums in boht dataframes was the same.  If they had been different, 
we could still do it but we would have to use `left_on` and `right_on` arguments
with the appropriate column names instead of just one. 


Notice that `merged_inner` has fewer rows than `left_merge_df`. This is an
indication that there were rows in `left_merge_df` with value(s) for `mms_id` that
do not exist as value(s) for `mms_id` in `right_merge_df`.

### Left joins

What if we wanted to merge these two  without losing any of the information 
from `left_merge_df`? In this case, we use a different
type of join called a "left outer join", or a "left join".

Like an inner join, a left join uses join keys to combine two DataFrames. Unlike
an inner join, a left join will return *all* of the rows from the `left`
DataFrame, even those rows whose join key(s) do not have values in the `right`
DataFrame.  Rows in the `left` DataFrame that are missing values for the join
key(s) in the `right` DataFrame will simply have null (i.e., NaN or None) values
for those columns in the resulting joined DataFrame.

Note: a left join will still discard rows from the `right` DataFrame that do not
have values for the join key(s) in the `left` DataFrame.

A left join is performed in pandas by calling the same `merge` function used for
inner join, but using the `how='left'` argument:

```python
merged_left = pd.merge(left=left_merge_df,right=right_merge_df, on='mms_id',how='left')

# what's the size of the output data?
print("shape:", merged_left.shape)
print("Missing Authors:", merged_left.author.isnull().sum())
merged_left

**OUTPUT:**
shape: (14232, 3)
Missing Authors: 1384
mms_id	title	author
0	991004617919702908	"A god of justice?" : the problem of evil in t...	Whitted, Qiana J., 1974-
1	991003607949702908	"Baad bitches" and sassy supermamas : Black po...	Dunn, Stephane, 1967-
2	991013190057102908	"Codependent lesbian space alien seeks same"	Olnek, Madeleine.; Space Aliens, LLC.
3	991003662979702908	"Lactilla tends her fav'rite cow" : ecocritica...	Milne, Anne.
4	991008089169702908	"The useless mouths", and other literary writings	Beauvoir, Simone de, 1908-1986.
...	...	...	...
14227	991013597402302908	¡Ban c/s this! : the BSP anthology of Xican@ l...	Rivera, Santino J., editor.
14228	991010673989702908	¡Muy pop! : conversations on Latino popular cu...	Stavans, Ilan.
14229	991004452179702908	¡Viva la historieta! : Mexican comics, NAFTA, ...	Campbell, Bruce, 1964- author.
14230	991013093859602908	Đời về cơ bản là buồn cười	Lê, Bích, author.
14231	991013054957702908	Đường vào văn chương : phê bình lý trí ...	Đặng, Phùng Quân, 1942- author.
14232 rows × 3 columns
```

The result DataFrame from a left join (`merged_left`) looks very much like the
result DataFrame from an inner join (`merged_inner`) in terms of the columns it
contains. However, unlike `merged_inner`, `merged_left` contains the **same
number of rows** as the original `left_merge_df` DataFrame. When we inspect
`merged_left`, we find there are rows where the information that should have
come from `right_merge_df` (i.e., `author`) is missing (they contain NaN values):

### Other join types

The pandas `merge` function supports two other join types:

- Right (outer) join: Invoked by passing `how='right'` as an argument. Similar
  to a left join, except *all* rows from the `right` DataFrame are kept, while
  rows from the `left` DataFrame without matching join key(s) values are
  discarded.
- Full (outer) join: Invoked by passing `how='outer'` as an argument. This join
  type returns the all pairwise combinations of rows from both DataFrames; i.e.,
  the result DataFrame will `NaN` where data is missing in one of the dataframes. This join type is very rarely used.
  
### Making Composite Keys
Sometimes a data file might not have an obvious key in a single column, but we may be able to generate a
suitable key by combining two or more columns into what is known as a *composite* key.

Let's see an example of this by importing the *ethnicity.csv* and *gender.csv*, and then examining one of them.  

```python
ethicity_df = pd.read_csv('ethnicity.csv')
gender_df = pd.read_csv('gender.csv')
print(gender_df.head())
print(ethicity_df.head())
```
They both have *term* and *year* columns that contain duplication.  However, we could combine these colums into a composite key that would be unique for each dataframe. We will also strip away any potential leading
 or trailing whitespace as a precaution.


```python
gender_df["key"] = gender_df.year.astype(str).str.strip() + gender_df.term.str.strip()
ethnicity_df["key"] = ethnicity_df.year.astype(str).str.strip() + ethnicity_df.term.str.strip()
gender_df
```

Now that we have a shared key, we will drop the *term* and *year* columns from one of the dataframes as we merge.  Otherwise, we would end up with duplicate columns that are automatically renamed with 
a prefix to prevent naming collision.  Simply dropping won't always be the best approach, but is safe to
do so in our case using a inner join.

```python
gender_ethnicity_df = pd.merge(gender_df.drop(columns=["term","year"]), ethnicity_df, on='key')
print(gender_ethnicity_df.shape)
gender_ethnicity_df
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Joins

Create a new DataFrame by joining the contents of the `gender.csv` and
`ethnicity.csv` tables. Make it a left join where gender is the left dataframe.

 Calculate the:

1. Number of ethnicity records that were dropped
2. Number of rows where ethnicity data is NaN

:::::::::::::::  solution

### Solution to challenge

```python
merged = pd.merge(
                  left=pd.read_csv("gender.csv"),
                  right=pd.read_csv("ethnicity.csv"),
                  on="['year','term']"
                  )
# Part 1: Number of ethnicity records that were dropped
ethnicity_df.shape[0] - merged.shape[0]
# Part 2: Number of rows where ethnicity data is NaN
merged.isnull().sum() # this will find all columns with null values and effectively count them
# you can *or* the columns with *nulls* to filter the dataframe and take the first value of shape
merged[merged.intl.isnull()| merged.pacific_islander.isnull()].shape[0]

```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::


