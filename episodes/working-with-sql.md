---
title: Accessing SQLite Databases Using Python & Pandas
teaching: 20
exercises: 25
---

::::::::::::::::::::::::::::::::::::::: objectives

- Use the sqlite3 module to interact with a SQL database.
- Access data stored in SQLite using Python.
- Describe the difference in interacting with data stored as a CSV file versus in SQLite.
- Describe the benefits of accessing data using a database compared to a CSV file.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Python and SQL

When you open a CSV in python, and assign it to a variable name, you are using
your computers memory to save that variable. Accessing data from a database like
SQL is not only more efficient, but also it allows you to subset and import only
the parts of the data that you need.

In the following lesson, we'll see some approaches that can be taken to do so.

### The `sqlite3` module

The [sqlite3] module provides a straightforward interface for interacting with
SQLite databases. A connection object is created using `sqlite3.connect()`; the
connection must be closed at the end of the session with the `.close()` command.
While the connection is open, any interactions with the database require you to
make a cursor object with the `.cursor()` command. The cursor is then ready to
perform all kinds of operations with `.execute()`.

```python
import sqlite3

# Create a SQL connection to our SQLite database
con = sqlite3.connect("all_works.db")

cur = con.cursor()

# the result of a "cursor.execute" can be iterated over by row
for row in cur.execute('SELECT * FROM all_works LIMIT 100;'):
    print(row)

#Be sure to close the connection.
con.close()
```

### Queries

One of the most common ways to interact with a database is by querying:
retrieving data based on some search parameters. Use a SELECT statement string.
The query is returned as a single tuple or a tuple of tuples. Add a WHERE
statement to filter your results based on some parameter.

```python
import sqlite3

# Create a SQL connection to our SQLite database
con = sqlite3.connect("all_works.db")

cur = con.cursor()

# Return all results of query
cur.execute('SELECT mms_id FROM checkouts WHERE checkouts>0')
checked_out = cur.fetchall()

# Return first result of query
cur.execute('SELECT checkouts FROM checkouts WHERE mms_id="991000199359702908"')
checkout_for_id = cur.fetchone()

#Be sure to close the connection.
con.close()
```

## Accessing data stored in SQLite using Python and Pandas

Using pandas, we can import results of a SQLite query into a dataframe.

```python
import pandas as pd
import sqlite3

# Read sqlite query results into a pandas DataFrame
con = sqlite3.connect("all_works.db")
df = pd.read_sql_query("SELECT * from all_works", con)

# verify that result of SQL query is stored in the dataframe
print(df.head())

con.close()
```
:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - SQL

1. Create a query that contains title data published between 2008 and 2015 that
  includes book's Title, Author, and mms_id. How many records are returned?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Storing data: CSV vs SQLite

Storing your data in an SQLite database can provide substantial performance
improvements when reading/writing compared to CSV. The difference in performance
becomes more noticable as the size of the dataset grows (see for example [these
benchmarks][these benchmarks]).

## Storing data: Create new tables using Pandas

We can also us pandas to create new tables within an SQLite database. Here, we will filter the dataframe to only works published in 2010, and then save it out to its own table so we can work with it on its own later.

```python
import pandas as pd
import sqlite3

# Establish connection to the SQLite database
with sqlite3.connect("all_works.db") as con:
    # Load the data into a DataFrame
    books_df = pd.read_sql_query("SELECT * from all_works", con)

    # Select only data for 2010
    titles_2010 = books_df[books_df.publication_date == '2010']
with sqlite3.connect("all_works.db") as con:
    # Write the new DataFrame to a new SQLite table
    titles_2010.to_sql("titles_2010", con, if_exists="replace")
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Saving your work

1. Create your own filter; modify the code to save
  results to its own tables in the all_works database.

2. What are some of the reasons you might want to save the results of your queries back into the
  database? What are some of the reasons you might avoid doing this.
  

::::::::::::::::::::::::::::::::::::::::::::::::::

[sqlite3]: https://docs.python.org/3/library/sqlite3.html
[these benchmarks]: https://sebastianraschka.com/Articles/2013_sqlite_database.html#results-and-conclusions



