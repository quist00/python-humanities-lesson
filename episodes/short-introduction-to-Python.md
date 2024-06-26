---
title: Short Introduction to Programming in Python
teaching: 30
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Describe the advantages of using programming vs. completing repetitive tasks by hand.
- Define the following data types in Python: strings, integers, and floats.
- Perform mathematical operations in Python using basic operators.
- Define the following as it relates to Python: lists, tuples, and dictionaries.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What is Python?
- Why should I learn Python?

::::::::::::::::::::::::::::::::::::::::::::::::::

## The Basics of Python

Python is a general purpose programming language that supports rapid development
of scripts and applications.

Python's main advantages:

- Open Source software, supported by Python Software Foundation
- Available on all platforms
- It is a general-purpose programming language
- Supports multiple programming paradigms
- Very large community with a rich ecosystem of third-party packages

## Interpreter

Python is an interpreted language which can be used in multiple ways:

- **"Interactive" Mode:** It functions like an "advanced calculator" Executing
  one command at a time:

```python
user:host:~$ python
Python 3.5.1 (default, Oct 23 2015, 18:05:06)
[GCC 4.8.3] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 2 + 2
4
>>> print("Hello World")
Hello World
```

- **"Scripting" Mode:** Executing a series of "commands" saved in text file,
  usually with a `.py` extension after the name of your file:

```bash
user:host:~$ python my_script.py
Hello World
```

- **"Cell" Mode:** Commands are stored in specially formatted and interpreted files that contain independent cells that can be run seperately in any order we choose.  The **Scripting** and **Interactive** modes are native to python and always available with any installation. **Cell** mode, on the other hand, requires software seperate from python itself like JupyterLab or VS Code with extensions.  We will begin using **Cell** mode shortly after a few more examples using the built in modes.

## Introduction to Python built-in data types

### Strings, integers and floats

Python has built-in numeric types for integers, floats, and complex numbers.
Strings are a built-in textual type.:

```python
>>>text = "Data Carpentry"
>>> number = 42
>>> pi_value = 3.1415
```

Here we've assigned data to variables, namely `text`, `number` and `pi_value`,
using the assignment operator `=`. The variable called `text` is a string which
means it can contain letters and numbers. Notice that in order to define a
string you need to have quotes around your text. To print out the value
stored in a variable we can simply type the name of the variable into the
interpreter:

```python
>>> text
"Data Carpentry"
```

However, in a script, a `print` function is needed to output the `text`:

*example.py*

```python
# A Python script file
# Comments in Python start with #
# The next line uses the print function to print out the text string
text = "Data Carpentry"
number = 42
pi_value = 3.1415
print(text)
```

*Running the script*

```bash
$ python example.py
Data Carpentry
```

**Tip**: The `print` function is a built-in function in Python. Later in this
lesson, we will introduce methods and user-defined functions. The Python
documentation is excellent for reference on the differences between them.

### Operators

We can perform mathematical calculations in Python using the basic operators
`+, -, /, *, %`:

```python
>>> 2 + 2   #  addition
4
>>> 6 * 7   #  multiplication
42
>>> 2 ** 16  # power
65536
>>> 13 % 5  # modulo
3
```

We can also use comparison and logic operators:
`<, >, ==, !=, <=, >=` and statements of identity such as
`and, or, not`. The data type returned by this is
called a *boolean*.

```python
>>> 3 > 4
False
>>> True and True
True
>>> True or False
True
```

## Sequential types: Lists and Tuples

### Lists

**Lists** are a common data structure to hold an ordered sequence of
elements. Each element can be accessed by an index.  Note that Python
indexes start with 0 instead of 1:

```python
>>> numbers = [1, 2, 3]
>>> numbers[0]
1
```

A `for` loop can be used to access the elements in a list or other Python data
structure one at a time:

```python
>>> for num in numbers:
...     print(num)
...
1
2
3
>>>
```

**Indentation** is very important in Python. Note that the second line in the
example above is indented. Just like three chevrons `>>>` indicate an
interactive prompt in Python, the three dots `...` are Python's prompt for
multiple lines. This is Python's way of marking a block of code. [Note: you
do not type `>>>` or `...`.]

To add elements to the end of a list, we can use the `append` method. Methods
are a way to interact with an object (a list, for example). We can invoke a
method using the dot `.` followed by the method name and a list of arguments
in parentheses. Let's look at an example using `append`:

```python
>>> numbers.append(4)
>>> print(numbers)
[1, 2, 3, 4]
>>>
```

To find out what methods are available for an
object, we can use the built-in `help` command:

```python
help(numbers)

Help on list object:

class list(object)
 |  list() -> new empty list
 |  list(iterable) -> new list initialized from iterable's items
 ...
```

### Tuples

A tuple is similar to a list in that it's an ordered sequence of elements.
However, tuples can not be changed once created (they are "immutable"). Tuples
are created by placing comma-separated values inside parentheses `()`.

```python
# tuples use parentheses
a_tuple= (1, 2, 3)
another_tuple = ('blue', 'green', 'red')
# Note: lists use square brackets
a_list = [1, 2, 3]
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Tuples

1. What happens when you type `a_tuple[2]=5` vs `a_list[1]=5` ?
2. Type `type(a_tuple)` into python - what is the object type?

:::::::::::::::  solution

1. As a tuple is immutable, it does not support item assignment. Elements in a list can be altered individually.

2. `tuple`


::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::




## Dictionaries

A **dictionary** is a container that holds pairs of objects - keys and values.

```python
>>> translation = {'one': 1, 'two': 2}
>>> translation['one']
1
```

Dictionaries work a lot like lists - except that you index them with *keys*.
You can think about a key as a name for or a unique identifier for a set of values
in the dictionary. Keys can only have particular types - they have to be
"hashable". Strings and numeric types are acceptable, but lists aren't.

```python
>>> rev = {1: 'one', 2: 'two'}
>>> rev[1]
'one'
>>> bad = {[1, 2, 3]: 3}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

In Python, a "Traceback" is an multi-line error block printed out for the
user.

To add an item to the dictionary we assign a value to a new key:

```python
>>> rev = {1: 'one', 2: 'two'}
>>> rev[3] = 'three'
>>> rev
{1: 'one', 2: 'two', 3: 'three'}
```

Using `for` loops with dictionaries is a little more complicated. We can do
this in two ways:

```python
>>> for key, value in rev.items():
...     print(key, '->', value)
...
1 -> one
2 -> two
3 -> three
```

or

```python
>>> for key in rev.keys():
...     print(key, '->', rev[key])
...
1 -> one
2 -> two
3 -> three
>>>
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Can you do reassignment in a dictionary?

1. First check what `rev` is right now (remember `rev` is the name of our dictionary).
  
  Type:

```python
>>> rev
```

2. Try to reassign the second value (in the *key value pair*) so that it no longer reads "two" but instead reads "apple-sauce".

3. Now display `rev` again to see if it has changed.


::::::::::: solution

You should see the following output:
  `{1: 'one', 2: 'two', 3: 'three'}`

```python
rev[2] = "apple-sauce"
```

```python
{1: 'one', 2: 'apple-sauce', 3: 'three'}
```

:::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Dictionaries as of python 3.7 are now in inserstion order be default. Anything you see talking about 
dictionaries beying unordered can be safely ignored as long as you are using python 3.7 or later.

## Functions

While functions work just fine in all three modes, let's open JupyterLab so we can use **cell** mode moving forward with the rest of the workshop for the sake of convenience.  Let's do that from our terminal where in order to save time in the next episode, we will launch JupyterLab with the workshop directory containing our data files as the working directory.

*Opening JupyterLab*

```bash
# navigate to or start your terminal from the worskhop directory
$ jupyter lab
# you may see various status messages as it spins up jupyter server and launches your default browser
```

Defining a section of code as a function in Python is done using the `def` keyword. For example a function that takes two arguments and returns their sum
can be defined as:

```python
def add_function(a, b):
    result = a + b
    return result

z = add_function(20, 22)
print(z)
42
```

Key points about functions are:

- definition starts with `def`
- function body is indented
- `return` keyword precedes returned value


