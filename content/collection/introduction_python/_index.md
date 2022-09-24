---
author: Cong Wang
date: "2022-09-25"
description: |
  It is a simple introduction about how to use Python. This in from the summer course I participited in Barcelona School of Economics.
excerpt: null
layout: single-series
publishDate: "2022-09-25"
show_author_byline: true
show_post_date: true
show_post_thumbnail: true
subtitle: Introduction of Python
title: Introduction of Python
weight: 2
---

## 1. Preliminaries

### 1.1 Getting started

There are different styles to writing code, but try to stick consistently to a (sensible) style guide: it is essential that the code not only works but is also understandable by everyone. The "official" guide for `python` can be founde here:

* Style guide: https://www.python.org/dev/peps/pep-0008/

Rules to code properly are fairly common sense, just like those of natural language. You can make them your own (within reason), the important part is to be **consistent**.

### 1.2 Printing

It can be useful to *print* what we are running on the console, this can be done with the built-in `print` command. You might have noticed that Colab notebooks automatically display the value of the last expression in a cell when you execute it, so you don't need to print that.


```python
# Colab automatically prints "y" to "out", 
# but we need to manually print "x" if we want to see it
x = 15 / 2
print(x)
y = x > 2
y
```
    7.5
    True



We may want to use placeholders to print strings. There are a plenty of ways to do that.


```python
a = 15
b = 2
c = 2
f'{a} divided by {b} is {a/b}, and it is {(a/b) > c} that this is greater than {c}'
```

    '15 divided by 2 is 7.5, and it is True that this is greater than 2'



We will go into what these objects are shortly.

---
## 2. Basic data types

Here we highlight the most important native operations you can apply to basic objects in `python`. We will go to a lower level of detail of the structure of these processes in the `R` session later in the afternoon.

### 2.1 Numerical values


```python
# Assignment
a = 10  # 10

# Increment/Decrement
a += 1  # 11 (a = a + 1)
a -= 1  # 10 (a = a - 1)

# Operations
b = a + 1  # 11
c = a - 1  # 9

d = a * 2  # 20 
e = a / 2  # 5 
f = a % 3  # 1 (remainder)
g = a ** 2  # 100 (exponentiation)

# Operations with other variables
d = a + b  # 21
```

### 2.2 String values

You can concatenate strings together with the `+` operator:


```python
"Adding" + " " + "strings" + " " + "is" + " " + "pasting"
```

    'Adding strings is pasting'



The built-in function `len` can be used to find the length of a string:


```python
len("four")
```

    4



### 2.3 Logical values

We can evaluate the relationships between different types of data in `python`. The output of such comparisons/operations are boolean variables. Some examples:


```python
# Comparing numbers
x = (1 >= 2)  # greater or equal
y = (1 == 2)  # equal
w = (1 != 2)  # different

# Parentheses are not required, but they help readability
x = (1 <= 2) and (1 > 0)  # both statements are true
y = (1 >  2) or  (1 < 3)  # at least one statement is true

print(x)
print(y)

```

    True
    True
    


```python
# It is good practice to use "is" instead of == for checking for NoneType:
x = None
y = x is None
z = x is not None
print(x)
print(y)
print(z)
```

    None
    True
    False
    

### 2.4 Lists

Lists are one of the simplest multi-value objects. They are created with square brackets:


```python
a_list = ["This", "is", "a", "list", "of", "strings"]
```

Here you can see we created a list of strings. We can also create a list of integers:


```python
num_list = [1, 5, 10]
```

Lists in `python` need not be homogenous, you can mix object types:


```python
mix_list = ['a', 'b', 1, 2, 3, True, None]
```

Sometimes you want to access individual elements from a list. You can do this using square brackets together with the index of the element:


```python
mix_list[0]  # first element
```

    'a'



Notice that the first element is indexed at `0`, the second element at `1`, and so on. You can also access a contiguous range of elements:


```python
mix_list[1:3]  # second item (index 1) and third item (index 2) only!
```

    ['b', 1]



You can also use negative indices to access items from the end. For example, the last item:


```python
num_list[-1]
```

    10



You can concatenate multiple lists together with the operator `+`:


```python
num_list + [40, 50, 60]
```

    [1, 5, 10, 40, 50, 60]



And you can check for membership with the operator `in`:


```python
"abc" in ["abc", "def", "ghi"]
```

    True



### 2.5 Tuples

Tuples are created with parenthesis:


```python
x = ("foo", 1)
```

But can also be created without any perenthesis, implied by the comma:


```python
x = "foo", 1
```

Elements in the tuple are also accessed via the index (like lists).

Lists can be used most places that a tuple is used, so it can be confusing what the difference is between the two. Besides technicalities, the following rules can help you decide when to use a tuple and when to use a list:

* `list`: many elements (potentially), unknown number, relatively homogenous, mutable.
* `tuple`: few elements, fixed number, completely heterogeneous, immutable (fixed).

The name comes from here: double, triple, quadruple... This hints that they should be of fixed length.
Since their length is fixed, we often use them with destructuring:


```python
name,num = x
```

Now the variable `name` contains the value `"foo"` and the variable `num` contains the value `1`.

### 2.6 Dictionaries

Dictionaries are another basic type in `python`. They are *associative* data structures. Like a standard dictionary, python dictionaries associate a `KEY` with a `VALUE` and are created with the `{`, `}` operators:



```python
player = {"name": "Jane", "score": 10000}
```

You can access the value via the *key*, and set it in a similar way:



```python
print(player["name"])
player["name"] = "Jane Smith"
```

Each key can only have one value. In the above example, we have overwritten the original `"name"`-key with a new value.


```python
# Create a list whose elements are of type dictionary
# Each element on the list is a player, and each player has three attributes.
players = [{"name": "John", "score": 100, "likes": ["R"]},
           {"name": "Jane", "score": 10000, "likes": ["python"]},
           {"name": "Stephen", "score": 55, "likes": ["julia"]}]
print(players[0])

# We can fetch elements of the dictionary
print(players[0]['name'])
print(players[0].get('name'))
```

### 2.7 Properties of an object: instances, attributes and methods

Objects in `python` have *classes*. For example:


```python
new_list = [1, 2, 3]  # Create a list
type(new_list)  # This function tells you the type of object
```

`new_list` is an ***instance*** of the class `list`. Instances can have ***attributes***. These attributes are just properties that are attached to the instance. They are accessed with dot notation:


```python
# I define my own class: custom types
class invented_class: 
    name = "John"
    score = 100
    def show(self): 
        print (self.name) 
        print (self.score) 
 
# Create an object of this new class
new_obj = invented_class()
print(type(new_obj))
print(new_obj.score)
```

Here `new_obj` is an instance of the class `invented_class`, and it has attributes `name` and `score`.

If the attribute happens to be a function, we call it a ***method***. Methods are functions that have a special purpose: they interact with the instance itself in some way.

---
## 3. Iterables and control flow

### 3.1 Iterate over elements of an object (`for`-loops)

You may want to perform an operation for each element in an *iterable* object, and (possibly) store the result of such operation. We do that by *looping* over this object operating on every of its elements sequentially.


```python
vector = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
squared = []
for num in vector:
    squared_num = num ** 2
    squared = squared + [squared_num]

squared
```

    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]



The structure of the loop is critical and always the same: 

```
## NOT RUN
for ELEMENT in EXISTING_OBJECT:  # Use "for", "in" and ":"
    operation_on(ELEMENT)  # Indent 4 spaces (MANDATORY)

# Loop ENDS when indentation is over
## END NOT RUN
```

`for`-loops are applicable to every instance of an iterable object class: `list`, `tuple`, `dictionary`.

### 3.2 Operations on lists

There are three main operations we perform on a list:
1. Aggregate (*reduce* operation)
2. Apply a function to each element (*map* operation)
3. Select elements (*filter* operation)

We illustrate these with some examples.


```python
# 1. AGGREGATION
# Summing the numbers in a list: 
nums = [30, 1, 4, 3, 10.5, 100]
total = 0 
for num in nums:
    total += num
    
print(total)
```

    148.5
    


```python
# 2. APPLY A FUNCTION 
# Squaring each number in a list
nums = [30, 1, 4, 3, 10.5, 100]

# This is called a "list comprehension"
# and is the python way to apply a function to 
# every element in a list
squared_nums = [num ** 2 for num in nums]
squared_nums
```

    [900, 1, 16, 9, 110.25, 10000]




```python
# 3. FILTER
# Remove all values less than 18:
ages = [0, 3, 21, 45, 10, 97]
adults = [a for a in ages if a > 17]
adults
```

    [21, 45, 97]



### 3.3 Control flow

Sometimes you may want to operate only if the object satisfies some relevant criteria, for example make a decision based on some condition holds. In these cases we use `if`-statement operations.


```python
gender = "male"
age = 20 

# Start of if-statement
if gender == "female":
    if age > 18:
        print("woman")
    else: 
        print("girl")
elif gender == "male":
    if age > 18:
        print("man")
    else: 
        print("boy")
else:
    print("other")

```

    man
    

Again, the structure of an `if`-statement is always the same, so it is essential that we use it correctly

```
## NOT RUN
if BOOLEAN:  # Use: "if", ":" and supply a boolean condition
    ACTION1a  # Indent with 4 spaces
    ACTION1b
elif BOOLEAN:  # Second layer to "if" (else-if)
    ACTION2
else:  # Rest of scenarios (make sure all contingencies are covered)
    ACTION3

## END NOT RUN
```

Like in `for`-loops, statements are closed by indentation.

---
## 4. Input/output

`python` treats external files as any other regular data type, and so they have attributes, etc. There are two actions to perform on these files, read and write.

In short, the built-in function `open` creates a `python` file object, which serves as a link to a file residing on your machine. After calling `open`, you can transfer strings of data to and from the associated external file by calling the returned file object’s methods.

Then, `with` is a file context manager which allows us to wrap file-processing code in a logic layer that ensures that the file will be closed automatically on exit.


```python
with open('simple_example.txt', 'w') as file:
    for i in range(10):
        file.write(f'I have written {i+1} lines \n')
```


```python
# Let's read the file into a variable:
with open('simple_example.txt') as file:
    content = file.read()

print(content)
```

    I have written 1 lines 
    I have written 2 lines 
    I have written 3 lines 
    I have written 4 lines 
    I have written 5 lines 
    I have written 6 lines 
    I have written 7 lines 
    I have written 8 lines 
    I have written 9 lines 
    I have written 10 lines 
    
    


```python
# The file object is actually an iterator, so we can do our usual tricks:
with open('simple_example.txt') as file:
    for line in file: 
        print(line)
```

    I have written 1 lines 
    
    I have written 2 lines 
    
    I have written 3 lines 
    
    I have written 4 lines 
    
    I have written 5 lines 
    
    I have written 6 lines 
    
    I have written 7 lines 
    
    I have written 8 lines 
    
    I have written 9 lines 
    
    I have written 10 lines 
    
    


```python
# We can also read it into a list by converiting 
# the iterator into a list directly:
with open('simple_example.txt') as f:
    content = list(f)

content[0]
```

    'I have written 1 lines \n'



Similarly, you can write things on this outside file by using `file.write()`.


```python
# 'r' is to read, 'w' to write:
with open('simple_example.txt', 'r') as file_in, open('simple_example2.txt', 'w') as file_out:
    for line in file_in:
        file_out.write(line)

```

If your file is located on a website, instead of a local path, then we need to proceed differently. We use the `urllib` package. The first task is to access the data contained on a website with an URL command. The main difference with respect to local files is how to read the file, and the structure of the imported object, which is no longer an iterable. If we want to split the loaded content into lines we need to proceed manually.


```python
# We load the data
import urllib.request

url_path = "https://raw.githubusercontent.com/barcelonagse-datascience/academic_files/master/data/textfile.txt"
file_conn = urllib.request.urlopen(url_path)  # This opens the connection to the URL

raw_txt = file_conn.read().decode() # decode is used to convert to string format
raw_txt
```




    'Why Do People Use Python?\n\nBecause there are many programming languages available today, this is the usual first question of newcomers. Given that there are roughly 1 million Python users out there at the moment, there really is no way to answer this question with complete accuracy; the choice of development tools is sometimes based on unique constraints or personal preference.\n\nBut after teaching Python to roughly 225 groups and over 3,000 students during the last 12 years, some common themes have emerged. The primary factors cited by Python users seem to be these:\n\nSoftware quality\nFor many, Python’s focus on readability, coherence, and software quality in general sets it apart from other tools in the scripting world. Python code is designed to be readable, and hence reusable and maintainable—much more so than traditional scripting languages. The uniformity of Python code makes it easy to understand, even if you did not write it. In addition, Python has deep support for more advanced software reuse mechanisms, such as object-oriented programming (OOP).\n\nDeveloper productivity\nPython boosts developer productivity many times beyond compiled or statically\ntyped languages such as C, C++, and Java. Python code is typically one-third to\none-fifth the size of equivalent C++ or Java code. That means there is less to type, less to debug, and less to maintain after the fact. Python programs also run immediately, without the lengthy compile and link steps required by some other tools, further boosting programmer speed.\n\nProgram portability\nMost Python programs run unchanged on all major computer platforms. Porting\nPython code between Linux and Windows, for example, is usually just a matter of\ncopying a script’s code between machines. Moreover, Python offers multiple options for coding portable graphical user interfaces, database access programs, web-based systems, and more. Even operating system interfaces, including program\nlaunches and directory processing, are as portable in Python as they can possibly be.\n\nSupport libraries\nPython comes with a large collection of prebuilt and portable functionality, known as the standard library. This library supports an array of application-level programming tasks, from text pattern matching to network scripting. In addition, Python can be extended with both homegrown libraries and a vast collection of third-party application support software. Python’s third-party domain offers tools for website construction, numeric programming, serial port access, game development, and much more. The NumPy extension, for instance, has been described as a free and more powerful equivalent to the Matlab numeric programming system.\n\nComponent integration\nPython scripts can easily communicate with other parts of an application, using a variety of integration mechanisms. Such integrations allow Python to be used as a product customization and extension tool. Today, Python code can invoke C and C++ libraries, can be called from C and C++ programs, can integrate with Java and .NET components, can communicate over frameworks such as COM, can\ninterface with devices over serial ports, and can interact over networks with interfaces like SOAP, XML-RPC, and CORBA. It is not a standalone tool.\n\nEnjoyment\nBecause of Python’s ease of use and built-in toolset, it can make the act of programming more pleasure than chore. Although this may be an intangible benefit, its effect on productivity is an important asset.\n\nOf these factors, the first two (quality and productivity) are probably the most compelling benefits to most Python users.\n'




```python
# Let's recover lines using split and the target line break
#split_txt = raw_txt.split('\n\n')
split_txt = raw_txt.split('\n')
split_txt[0:10]
```

    ['Why Do People Use Python?',
     '',
     'Because there are many programming languages available today, this is the usual first question of newcomers. Given that there are roughly 1 million Python users out there at the moment, there really is no way to answer this question with complete accuracy; the choice of development tools is sometimes based on unique constraints or personal preference.',
     '',
     'But after teaching Python to roughly 225 groups and over 3,000 students during the last 12 years, some common themes have emerged. The primary factors cited by Python users seem to be these:',
     '',
     'Software quality',
     'For many, Python’s focus on readability, coherence, and software quality in general sets it apart from other tools in the scripting world. Python code is designed to be readable, and hence reusable and maintainable—much more so than traditional scripting languages. The uniformity of Python code makes it easy to understand, even if you did not write it. In addition, Python has deep support for more advanced software reuse mechanisms, such as object-oriented programming (OOP).',
     '',
     'Developer productivity']



---
## 5. Functions

### 5.1 Coding functions

Functions (may) use an input to produce an output through a set of operations.

```
## NOT RUN
def function_name(input):  # Use "def", parenthesis and ":"
    operations
    return output  # End with "return"

## END NOT RUN
```

For example, here is a function that takes a number, and returns its square:


```python
def squared(x):
    return x**2

squared(7)
```

    49



Here is a more general function that takes a number and a power and returns the number to that power:


```python
def power(x, n):
    return x ** n

power(4, 3)
```

    64



Here is a function that returns the minimum and the sum of a list of numbers:


```python
def min_sum_fun(x):
    minx = x[0] if len(x) > 0 else None
    sumx = 0.0  
    for val in x: 
        if val < minx:
            minx = val
        sumx += val 
    return minx, sumx  # Notice multiple outputs (technically a tuple)

```

Now we can call that function:


```python
m, s = min_sum_fun([1, 5, 0.3, -1])  # Destructuring
w = (m, s) = min_sum_fun([1, 5, 0.3, -1])  # You can assign directly to a tuple

print(m)
print(s)
print(w)
print(type(w))
```

    -1
    5.3
    (-1, 5.3)
    <class 'tuple'>
    

### 5.2 Map, reduce and filter

`map` is a function used to apply a certain (set of) operation(s) sequentiallt to every element in a list. This is similar to a `for`-loop but through a simpler syntax.


```python
items = [1, 4, 9, 16, 25]
sqroot = map(lambda x: x**(1/2), items)  # map(function, iterable_object)
sqroot = list(sqroot)  # Put into a list
print(sqroot)
```

    [1.0, 2.0, 3.0, 4.0, 5.0]
    

Note that here `map` is using a so-called `lambda` function: this is an anonymous function (it has not been defined) which takes argument `x`, where `x` is every individual element in the `list`, and applies the function after the semi-colon to each of these elements. Note, however, that the function applied by `map` need not be a `lambda` function.

Also, the iterable element need not even be a list of objects, it can be a list of functions:


```python
def sqroot(x):
    return x**(1/2)

def squared(x):
    return x**2

functions = [sqroot, squared]  # List of functions
for i in [1, 4, 9, 16, 25]:
    results = list(map(lambda x: x(i), functions))
    print(results)

```

    [1.0, 1]
    [2.0, 16]
    [3.0, 81]
    [4.0, 256]
    [5.0, 625]
    

`reduce` is a similar function, in this case employed to perform rolling computations to sequential pairs of values in a list. In other words, the computation of an operation on an element of the list is connected to the computation of the previous element, and this accumulates (*reducing* the list to a single result).

Say you want to compute the factorial of a number:


```python
from functools import reduce

# This gives a list with the integers needed to compute the factorial
def factorial_elements(x):
    return [i + 1 for i in range(x)]

# Factorial of 5 (5!)
factorial5 = reduce(lambda x, y: x * y, factorial_elements(5))  # Needs two arguments
factorial5
```

    120



One final useful function is `filter`. It returns the list of elements of an original list that satisfy a logical condition (i.e. applies a function that returns a logical value). A simple example:


```python
num_list = [-1, 2, -3, 4, -5, 6]
pos_vals = filter(lambda x: x > 0, num_list)  # Same syntax as map/reducte
pos_vals = list(pos_vals)  # Needs to be coerced to a list too
print(pos_vals)
```

    [2, 4, 6]
    

---
## 6. Modules and imports

*Modules* are `python` files, recognised in the computer as `<filename>.py`. Data and methods defined in the module can become part of the namespace by using `import`.


```python
x = sin(5)  # The "sin" function does not exist
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [28], in <cell line: 1>()
    ----> 1 x = sin(5)
    

    NameError: name 'sin' is not defined



```python
from math import sin  # You need to "import" it
x = sin(5)
print(x)
```

There are some other useful tricks to import data and methods:


```python
from math import sin  # Imports a single function
from math import sin as sinus  # Nickname, useful when you import something with a long name
print(sinus(3))

import math  # Imports the module in the namespace, methods can then be accessed e.g.
print(math.sin(3))
```

Let us discuss here a couple of fundamental imports in `python`:

### 6.1 `numpy` (Numerical Python)

*   User's manual: https://numpy.org/doc/stable/

This is Python's stack for scientific computing. The fundamental new data type is that of a **`numpy` `array`**, Python's matrix-type object, which is used in the majority of its data analysis, statistics and machine learning modules.

These arrays contain data all of the same type (dtype), numerical of many types or boolean:


```python
# Import module 
import numpy as np  # usually imported with name "np"

# Creating an array
A = np.array([[1, 2, 3, 4, 5, 6], 
              [42, 53, 43 ,62, 7, 4], 
              [-3, -1, -4 ,-8, -52, -4], 
              [10, 0, 4 , 1, 0, 1]])
```

We can access the elements in an array using multi-index notation (counting starts from 0, slicing `a:b` is inclusive:exclusive, negative indices, etc.)


```python
print(A)
# Print from row 3 onwards, columns 3 and 5
print(A[2:, [2, 4]])
```


```python
# Attributes
A.shape  # dimension
A.min()  # minimum (you can similarly use max, sum, etc.)
A.diagonal()  # diagonal
B = A.transpose()  # transposing
C = A.reshape(6, 4)  # rearrange values to change dimension (CAREFUL!)
print(A.shape)
print(B.shape)
print(C.shape)
A.dot(B)  # dot-product (with array B)

```

#### 6.1.1. `array` operations

Mathematical symbols take on mathematical meanings in `numpy`, and so the `+` operator between two `np.array` just tries to add them together elementwise (it works differently to `list`-type objects). To concatenate you need a specific function.



```python
# Numpy array addition: 
a, b = np.array([1, 2, 3]), np.array([4, 5, 6])
print(a + b)  # Addition
print(np.concatenate([a,b]))  # Concatenation
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [29], in <cell line: 2>()
          1 # Numpy array addition: 
    ----> 2 a, b = np.array([1, 2, 3]), np.array([4, 5, 6])
          3 print(a + b)  # Addition
          4 print(np.concatenate([a,b]))
    

    NameError: name 'np' is not defined


### 6.2 `pandas` (Panel Data Structures)

This is the module in Python for doing rectangular-data management, analysis and plotting. It provides tools to read and write external data.



```python
import pandas as pd  # Usually imported with name "pd"

file_path = "https://raw.githubusercontent.com/barcelonagse-datascience/academic_files/master/data/tips.csv"
tips = pd.read_csv(file_path)  # read_csv allows importing .CSV files
print(tips.head(5))  # head gives first rows (arg = number of rows)

# Data formats in pandas
print(type(tips))
print(type(tips['tip']))
```

       total_bill   tip     sex smoker  day    time  size
    0       16.99  1.01  Female     No  Sun  Dinner     2
    1       10.34  1.66    Male     No  Sun  Dinner     3
    2       21.01  3.50    Male     No  Sun  Dinner     3
    3       23.68  3.31    Male     No  Sun  Dinner     2
    4       24.59  3.61  Female     No  Sun  Dinner     4
    <class 'pandas.core.frame.DataFrame'>
    <class 'pandas.core.series.Series'>
    

There are the two basic data formats in `pandas`: `Series` and `DataFrame`. A `Series` is the equivalent to a vector in linear algebra. A `DataFrame` is the equivalent to a rectangular data structure. These types are equipped with several attributes useful for data management and analysis. In the tips example, the variable object `tips` is a `DataFrame`, while any individual column would be a `Series`.

---
## 7. Structured data operations (`pandas`)

### 7.1 `Series`

They are accessed via their index, which can be either a number or a string. They are typically obtained by reading a dataset from an external file or when operating on a `DataFrame`, but they can be defined manually:


```python
# Here no indices are specified (there are defaults)
a_series = pd.Series([1, 15, -5, None, 4, 123, 0, 78, 0, 1, -4])
a_series
```

    0       1.0
    1      15.0
    2      -5.0
    3       NaN
    4       4.0
    5     123.0
    6       0.0
    7      78.0
    8       0.0
    9       1.0
    10     -4.0
    dtype: float64




```python
# Accessing a certain value via the index
a_series[0]
```

    1.0




```python
# .values returns a numpy.ndarray of the values
a_series.values
a_series.index, a_series.index.values
```

    (RangeIndex(start=0, stop=11, step=1),
     array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10], dtype=int64))




```python
# You can overwrite the index directly: 
a_series.index = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k"]
a_series
```

    a      1.0
    b     15.0
    c     -5.0
    d      NaN
    e      4.0
    f    123.0
    g      0.0
    h     78.0
    i      0.0
    j      1.0
    k     -4.0
    dtype: float64



Accessing values via the index can be very useful, but sometimes you want to access the values as though they were a Python list. In other words "I want the first value!", without having to know the name of the label. This can be achieved with `.iloc`:



```python
a_series.iloc[0], a_series["a"], a_series.iloc[-1]

```

    (1.0, 1.0, -4.0)




```python
# This just resets the index to be as we found it originally
a_series = a_series.reset_index(drop = True)
x = a_series.sort_values()

x[0], x.iloc[0] 
# the indices remain, so x indexed by 0 is 1
# the ordering changes, so the first element of x is -5.0, the minimum
```

    (1.0, -5.0)



As usual one should explore the attributes of any python object one ends up working with. We have already accessed the `.index` and `.value`. Some other that are worth highlighting:

*   `.map`
*   `.corr`
*   `.describe`
*   `.hist`
*   `.plot`
*   `.size`
*   `.value_counts`
*   `.sort_values`

For example:


```python
tips.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>244.000000</td>
      <td>244.000000</td>
      <td>244.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>19.785943</td>
      <td>2.998279</td>
      <td>2.569672</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.902412</td>
      <td>1.383638</td>
      <td>0.951100</td>
    </tr>
    <tr>
      <th>min</th>
      <td>3.070000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>13.347500</td>
      <td>2.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>17.795000</td>
      <td>2.900000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>24.127500</td>
      <td>3.562500</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>50.810000</td>
      <td>10.000000</td>
      <td>6.000000</td>
    </tr>
  </tbody>
</table>
</div>



### 7.1.1 Operations with `Series`

`Series` are `numpy` arrays behind the scenes, so we can compute element-wise functions on one series or several series at the same time. The result is another series with data type depending on the type of operations performed.



```python
series1 = pd.Series([1, 3, 5, 7])
series2 = pd.Series([0, 10, -1, 6])

series3 = 2 * series1 + abs(series2)
series4 = series1 > series2 

print(series3)
print(series4)
# Take a look at the different Series objects!
```

    0     2
    1    16
    2    11
    3    20
    dtype: int64
    0     True
    1    False
    2     True
    3     True
    dtype: bool
    

What goes on in the previous examples is more subtle than it looks. How does Python know which elements from each series to join in the required operation together? What happens is that the indices happened to be the same. So when we ask something like

`series3 = series1 + series2`,

Python looks for entries in each series with the same index and then does an elementwise summation that it stores in a like-wise index in Series 3.

Consider instead the following example:



```python
series1 = pd.Series([1, 10], index=["A", "B"])
series2 = pd.Series([4, -1], index=["C", "D"])
series3 = series1 + series2
print(series3)
```

    A   NaN
    B   NaN
    C   NaN
    D   NaN
    dtype: float64
    

This aspect makes it very easy to work with series that we have sorted or manipulated otherwise; there is always the address to access a value. This helps prevent accidentally combining values we didn't mean to combine!


```python
# accesing by list of index labels
a_series.index = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K"]
x = a_series[["A", "K"]]
```


```python
a_series
```

    A      1.0
    B     15.0
    C     -5.0
    D      NaN
    E      4.0
    F    123.0
    G      0.0
    H     78.0
    I      0.0
    J      1.0
    K     -4.0
    dtype: float64




```python
x
```

    A    1.0
    K   -4.0
    dtype: float64




```python
# Getting a boolean-valued series by checking a condition
choose = (a_series == 0.0)
choose
```

    A    False
    B    False
    C    False
    D    False
    E    False
    F    False
    G     True
    H    False
    I     True
    J    False
    K    False
    dtype: bool




```python
# Notice the index of x is a SUBSET of the index of "a_series"
# This can be useful when needing to relate values back to the original "a_series"!
x = a_series[choose]
print(x)
# or the complement
a_series[~choose]
```

    G    0.0
    I    0.0
    dtype: float64
    

    A      1.0
    B     15.0
    C     -5.0
    D      NaN
    E      4.0
    F    123.0
    H     78.0
    J      1.0
    K     -4.0
    dtype: float64



We often use boolean masks to filter data in Pandas. We also get special boolean algebra operators to use in `numpy`/`pandas`, distinct from the and/or/not you will use in regular Python: `&` (AND), `|` (OR), `~` (NOT).


### 7.1.2 Missing values

A series object in `pandas` can help us deal with missing data. We already see very naturally how data management can quickly lead to missing data.


```python
series3 = series1 + series2
print(series3)
```

    A   NaN
    B   NaN
    C   NaN
    D   NaN
    dtype: float64
    

What happened there is that in the operation labels could not be matched, so `pandas` tried to sum a numeric value with missing value, the result of which is a missing value.
The way to manually specify in `pandas` that a value is missing is to use `None`, as below:


```python
temp = pd.Series([1, None, 2])
print(temp)
```

    0    1.0
    1    NaN
    2    2.0
    dtype: float64
    

`pandas` coerces `None` values to `NaN` ("Not a Number") values. We can create boolean masks on the basis of such values. The way to identify NaN or None values in a Series is to use either of the equivalent two attributes: `.isna()` and `.isnull()` (existing the opposite `.notna()` and `.notnull()`).


```python
temp.isna()
temp.isnull()
```

    0    False
    1     True
    2    False
    dtype: bool




```python
temp.notna()
temp.notnull()
```

    0     True
    1    False
    2     True
    dtype: bool



### 7.2 `DataFrame`

This is `pandas` model for rectangular data. Operationally similar to a dictionary of `Series`; each column of the `DataFrame` is a `Series` object, and comes with all the attributes/methods of a `Series`. An implication is that within each column the data type is common; across columns this can change.


```python
# Recall our "tips" dataset
tips.head(5)  # The first method of our DataFrame object
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# the other important attribute: name of rows and columns
print(tips.shape)
print(tips.index)
print(tips.columns)
```

    (244, 7)
    RangeIndex(start=0, stop=244, step=1)
    Index(['total_bill', 'tip', 'sex', 'smoker', 'day', 'time', 'size'], dtype='object')
    

To access the columns of a `DataFrame` there are two standard ways.


```python
tips.tip  # As if it were a method (NOT recommended)
tips["size"]  # As if it were a named index (RECOMMENDED)
```

    0      2
    1      3
    2      3
    3      2
    4      4
          ..
    239    3
    240    2
    241    2
    242    2
    243    2
    Name: size, Length: 244, dtype: int64



The latter is recommended because, for example, the column `size` cannot be accessed via the first option (it would be confused with the method `.size`). The result of this operation is an object of type `Series`.

We can access various columns at a time, supplying a list of columns, obtaining a dataframe with the same index as the original and columns the chosen subset.


```python
tips[["tip", "size", "sex"]].head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tip</th>
      <th>size</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.01</td>
      <td>2</td>
      <td>Female</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.66</td>
      <td>3</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.50</td>
      <td>3</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.31</td>
      <td>2</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.61</td>
      <td>4</td>
      <td>Female</td>
    </tr>
  </tbody>
</table>
</div>



Similarly, you can access rows instead of columns with the same philosophy.

*   Using a list of index labels: `tips.loc[ [index1, index2, ...] ]`
*   Using a list of integer index location (i-loc): `tips.iloc[ [integer1, integer2, ...] ]`




```python
# Accessing rows AND columns!
# Example of 2-dimension loc
tips.loc[[1, 3], ['sex', 'smoker']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>smoker</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>No</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Accessing rows AND columns!
# Example of 2-dimensional iloc
tips.iloc[[1, 3], 2:5]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
    </tr>
  </tbody>
</table>
</div>



Note that certain operations are exchangeable: the 3rd element of column "sex" can be obtained with either of the following ways:


```python
tips.sex[2]  # Access col as series, then the 3rd element of that
tips.loc[2, "sex"]  # Access the entry in DF by giving the index labels of row and col
tips.loc[2]["sex"]  # Accessing the whole row as a series, then using the column name as index label
```




    'Male'



As with `Series`, we can use a boolean-valued series to index a `DataFrame` provided the share the same index labels. The simplest instance of this is to use series produced as boolean masks of columns of the dataframe. The output of this *filtering* operation is a dataframe with subset of rows corresponding to the `True` values in the boolean mask.


```python
tips[tips['sex'] == "Male"].head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25.29</td>
      <td>4.71</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>8.77</td>
      <td>2.00</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
tips[(tips['sex'] == "Male") & (tips['day'] == "Sun")].head(5)  # Multiple booleans ("&", "|", "~")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>25.29</td>
      <td>4.71</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>8.77</td>
      <td>2.00</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



A `DataFrame` comes with several attributes for computing column-wise statistics and summaries. We highlight some of them:

*   `.boxplot` (check out the `by = ` option)
*   `.corrwith` and `.corr` (within and across `DataFrame`'s)
*   `.dot`
*   `.mean/median/max/quantile/sum`, etc.
*   `.sample`
*   `.sort_values`
*   `.unique`


### 7.2.1 `GroupBy`

This `DataFrame` method groups the `DataFrame` according to the values of a column, treating them as categorical values. It returns a groupby object.


```python
# Group tips DataFrame by size of table
by_size = tips.groupby("size")
print(by_size)

# If we coerce it to a list, we see something interesting: 
# it's basically a list of tuples
# The first element is the "category" variable, the second
# is a datafame. 
list(by_size)
```

    <pandas.core.groupby.generic.DataFrameGroupBy object at 0x00000213FDE3FFD0>
    




    [(1,
           total_bill   tip     sex smoker   day    time  size
      67         3.07  1.00  Female    Yes   Sat  Dinner     1
      82        10.07  1.83  Female     No  Thur   Lunch     1
      111        7.25  1.00  Female     No   Sat  Dinner     1
      222        8.58  1.92    Male    Yes   Fri   Lunch     1),
     (2,
           total_bill   tip     sex smoker   day    time  size
      0         16.99  1.01  Female     No   Sun  Dinner     2
      3         23.68  3.31    Male     No   Sun  Dinner     2
      6          8.77  2.00    Male     No   Sun  Dinner     2
      8         15.04  1.96    Male     No   Sun  Dinner     2
      9         14.78  3.23    Male     No   Sun  Dinner     2
      ..          ...   ...     ...    ...   ...     ...   ...
      237       32.83  1.17    Male    Yes   Sat  Dinner     2
      240       27.18  2.00  Female    Yes   Sat  Dinner     2
      241       22.67  2.00    Male    Yes   Sat  Dinner     2
      242       17.82  1.75    Male     No   Sat  Dinner     2
      243       18.78  3.00  Female     No  Thur  Dinner     2
      
      [156 rows x 7 columns]),
     (3,
           total_bill    tip     sex smoker   day    time  size
      1         10.34   1.66    Male     No   Sun  Dinner     3
      2         21.01   3.50    Male     No   Sun  Dinner     3
      16        10.33   1.67  Female     No   Sun  Dinner     3
      17        16.29   3.71    Male     No   Sun  Dinner     3
      18        16.97   3.50  Female     No   Sun  Dinner     3
      19        20.65   3.35    Male     No   Sat  Dinner     3
      35        24.06   3.60    Male     No   Sat  Dinner     3
      36        16.31   2.00    Male     No   Sat  Dinner     3
      37        16.93   3.07  Female     No   Sat  Dinner     3
      38        18.69   2.31    Male     No   Sat  Dinner     3
      39        31.27   5.00    Male     No   Sat  Dinner     3
      40        16.04   2.24    Male     No   Sat  Dinner     3
      48        28.55   2.05    Male     No   Sun  Dinner     3
      64        17.59   2.64    Male     No   Sat  Dinner     3
      65        20.08   3.15    Male     No   Sat  Dinner     3
      71        17.07   3.00  Female     No   Sat  Dinner     3
      102       44.30   2.50  Female    Yes   Sat  Dinner     3
      112       38.07   4.00    Male     No   Sun  Dinner     3
      114       25.71   4.00  Female     No   Sun  Dinner     3
      129       22.82   2.18    Male     No  Thur   Lunch     3
      146       18.64   1.36  Female     No  Thur   Lunch     3
      152       17.26   2.74    Male     No   Sun  Dinner     3
      162       16.21   2.00  Female     No   Sun  Dinner     3
      165       24.52   3.48    Male     No   Sun  Dinner     3
      170       50.81  10.00    Male    Yes   Sat  Dinner     3
      182       45.35   3.50    Male    Yes   Sun  Dinner     3
      186       20.90   3.50  Female    Yes   Sun  Dinner     3
      188       18.15   3.50  Female    Yes   Sun  Dinner     3
      189       23.10   4.00    Male    Yes   Sun  Dinner     3
      200       18.71   4.00    Male    Yes  Thur   Lunch     3
      205       16.47   3.23  Female    Yes  Thur   Lunch     3
      206       26.59   3.41    Male    Yes   Sat  Dinner     3
      210       30.06   2.00    Male    Yes   Sat  Dinner     3
      214       28.17   6.50  Female    Yes   Sat  Dinner     3
      223       15.98   3.00  Female     No   Fri   Lunch     3
      231       15.69   3.00    Male    Yes   Sat  Dinner     3
      238       35.83   4.67  Female     No   Sat  Dinner     3
      239       29.03   5.92    Male     No   Sat  Dinner     3),
     (4,
           total_bill   tip     sex smoker   day    time  size
      4         24.59  3.61  Female     No   Sun  Dinner     4
      5         25.29  4.71    Male     No   Sun  Dinner     4
      7         26.88  3.12    Male     No   Sun  Dinner     4
      11        35.26  5.00  Female     No   Sun  Dinner     4
      13        18.43  3.00    Male     No   Sun  Dinner     4
      23        39.42  7.58    Male     No   Sat  Dinner     4
      25        17.81  2.34    Male     No   Sat  Dinner     4
      31        18.35  2.50    Male     No   Sat  Dinner     4
      33        20.69  2.45  Female     No   Sat  Dinner     4
      44        30.40  5.60    Male     No   Sun  Dinner     4
      47        32.40  6.00    Male     No   Sun  Dinner     4
      52        34.81  5.20  Female     No   Sun  Dinner     4
      54        25.56  4.34    Male     No   Sun  Dinner     4
      56        38.01  3.00    Male    Yes   Sat  Dinner     4
      59        48.27  6.73    Male     No   Sat  Dinner     4
      63        18.29  3.76    Male    Yes   Sat  Dinner     4
      77        27.20  4.00    Male     No  Thur   Lunch     4
      85        34.83  5.17  Female     No  Thur   Lunch     4
      95        40.17  4.73    Male    Yes   Fri  Dinner     4
      116       29.93  5.07    Male     No   Sun  Dinner     4
      119       24.08  2.92  Female     No  Thur   Lunch     4
      153       24.55  2.00    Male     No   Sun  Dinner     4
      154       19.77  2.00    Male     No   Sun  Dinner     4
      157       25.00  3.75  Female     No   Sun  Dinner     4
      159       16.49  2.00    Male     No   Sun  Dinner     4
      160       21.50  3.50    Male     No   Sun  Dinner     4
      167       31.71  4.50    Male     No   Sun  Dinner     4
      180       34.65  3.68    Male    Yes   Sun  Dinner     4
      183       23.17  6.50    Male    Yes   Sun  Dinner     4
      197       43.11  5.00  Female    Yes  Thur   Lunch     4
      204       20.53  4.00    Male    Yes  Thur   Lunch     4
      207       38.73  3.00    Male    Yes   Sat  Dinner     4
      211       25.89  5.16    Male    Yes   Sat  Dinner     4
      212       48.33  9.00    Male     No   Sat  Dinner     4
      219       30.14  3.09  Female    Yes   Sat  Dinner     4
      227       20.45  3.00    Male     No   Sat  Dinner     4
      230       24.01  2.00    Male    Yes   Sat  Dinner     4),
     (5,
           total_bill   tip     sex smoker   day    time  size
      142       41.19  5.00    Male     No  Thur   Lunch     5
      155       29.85  5.14  Female     No   Sun  Dinner     5
      185       20.69  5.00    Male     No   Sun  Dinner     5
      187       30.46  2.00    Male    Yes   Sun  Dinner     5
      216       28.15  3.00    Male    Yes   Sat  Dinner     5),
     (6,
           total_bill  tip     sex smoker   day    time  size
      125       29.80  4.2  Female     No  Thur   Lunch     6
      141       34.30  6.7    Male     No  Thur   Lunch     6
      143       27.05  5.0  Female     No  Thur   Lunch     6
      156       48.17  5.0    Male     No   Sun  Dinner     6)]




```python
list(tips.groupby("sex"))
```

    [('Female',
           total_bill   tip     sex smoker   day    time  size
      0         16.99  1.01  Female     No   Sun  Dinner     2
      4         24.59  3.61  Female     No   Sun  Dinner     4
      11        35.26  5.00  Female     No   Sun  Dinner     4
      14        14.83  3.02  Female     No   Sun  Dinner     2
      16        10.33  1.67  Female     No   Sun  Dinner     3
      ..          ...   ...     ...    ...   ...     ...   ...
      226       10.09  2.00  Female    Yes   Fri   Lunch     2
      229       22.12  2.88  Female    Yes   Sat  Dinner     2
      238       35.83  4.67  Female     No   Sat  Dinner     3
      240       27.18  2.00  Female    Yes   Sat  Dinner     2
      243       18.78  3.00  Female     No  Thur  Dinner     2
      
      [87 rows x 7 columns]),
     ('Male',
           total_bill   tip   sex smoker  day    time  size
      1         10.34  1.66  Male     No  Sun  Dinner     3
      2         21.01  3.50  Male     No  Sun  Dinner     3
      3         23.68  3.31  Male     No  Sun  Dinner     2
      5         25.29  4.71  Male     No  Sun  Dinner     4
      6          8.77  2.00  Male     No  Sun  Dinner     2
      ..          ...   ...   ...    ...  ...     ...   ...
      236       12.60  1.00  Male    Yes  Sat  Dinner     2
      237       32.83  1.17  Male    Yes  Sat  Dinner     2
      239       29.03  5.92  Male     No  Sat  Dinner     3
      241       22.67  2.00  Male    Yes  Sat  Dinner     2
      242       17.82  1.75  Male     No  Sat  Dinner     2
      
      [157 rows x 7 columns])]




```python
# We can iterate through the groupby just like we would a list of tuples!
for sex, data in tips.groupby("sex"):
    print(sex)
    print(data.mean())

```

    Female
    total_bill    18.056897
    tip            2.833448
    size           2.459770
    dtype: float64
    Male
    total_bill    20.744076
    tip            3.089618
    size           2.630573
    dtype: float64
    

    C:\Users\wangc\AppData\Local\Temp\ipykernel_17768\969912475.py:4: FutureWarning: Dropping of nuisance columns in DataFrame reductions (with 'numeric_only=None') is deprecated; in a future version this will raise TypeError.  Select only valid columns before calling the reduction.
      print(data.mean())
    

We `groupby` to perform *some* operation on each group, to *map* over the groups, applying a function to each element Very often this function is itself an aggregation (*reduction*). We want to somehow aggregate each group into a value or set of values that *describe* it.

To apply functions to each element of a `groupby`, we use `.apply`:



```python
# Get the maximum bill by gender: 
def max_bill(df):
    return df['total_bill'].max()

tips.groupby("sex").apply(max_bill)
```

    sex
    Female    44.30
    Male      50.81
    dtype: float64



Many aggregation functions that exist on Series and `DataFrames` (`mean`, `max`, `min`, etc.) can be called directly via the groupby object:


```python
print(tips.groupby("sex").max())
print(tips.groupby("sex").mean())
```

            total_bill   tip smoker   day   time  size
    sex                                               
    Female       44.30   6.5    Yes  Thur  Lunch     6
    Male         50.81  10.0    Yes  Thur  Lunch     6
            total_bill       tip      size
    sex                                   
    Female   18.056897  2.833448  2.459770
    Male     20.744076  3.089618  2.630573
    

We can actually `groupby` more than one column:


```python
tips.groupby(["sex", "day"])['tip'].mean()
```

    sex     day 
    Female  Fri     2.781111
            Sat     2.801786
            Sun     3.367222
            Thur    2.575625
    Male    Fri     2.693000
            Sat     3.083898
            Sun     3.220345
            Thur    2.980333
    Name: tip, dtype: float64



### 7.2.2 Combining `DataFrame`'s

There are many ways to combine various `DataFrame`s into a new one, extending in many ways what we already saw for operations on `Series`. The main ways of doing this are:

*   **Concatenate**: paste row-column-wise and taking action on `NaN`s (this works more on the rectangular structure of the data)
*   **Merge**: combine `DataFrame`s using a common piece of information, e.g. an identifier column (this works more as a database operation)




```python
# Concatenate
df1 = pd.DataFrame({"A": pd.Series([1, 2, 3]), "B": pd.Series([4, 5, 6])})
df2 = pd.DataFrame({"A": pd.Series([4]), "C": pd.Series([7])})
pd.concat([df1, df2], axis = 0)
# axis: 0 for pasting below, 1 for pasting on the side
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>NaN</td>
      <td>7.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1 = pd.DataFrame({"A": pd.Series([1, 2, 3]), "B": pd.Series([4, 5, 6])})
df2 = pd.DataFrame({"A": pd.Series([4]), "C": pd.Series([7])})
pd.concat([df1, df2], axis = 1, join = "inner")  # "inner" join
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Concatenation is mostly used when the rows or columns are shared. 
# For example, you might have data with the same columns and want to concatenate them on axis 0:
# But note: what happened to the index? 
# We might want to reset it. 
df1 = pd.DataFrame({"A": pd.Series([1, 2, 3]), "B": pd.Series([4, 5, 6])})
df2 = pd.DataFrame({"A": pd.Series([4]), "B": pd.Series([7])})
df3 = pd.concat([df1, df2], axis = 0)
df3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6</td>
    </tr>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Similarly, you might have data with the same rows and different columns:
df1 = pd.DataFrame({"A": pd.Series([1, 2, 3]), "B": pd.Series([4, 5, 6])})
df2 = pd.DataFrame({"C": pd.Series([4]), "D": pd.Series([7])})
pd.concat([df1, df2], axis = 1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4</td>
      <td>4.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# But note what happens if the rows do not align, and you concatenate on axis 1 (by rows):
df1 = pd.DataFrame({"A": pd.Series([1, 2, 3]), "B": pd.Series([4, 5, 6])})
df2 = pd.DataFrame({"C": pd.Series([7]), "D": pd.Series([10])})
pd.concat([df1, df2], axis = 1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Let's go with merging now: `merge` is commonly used when your two `DataFrame`s must be connected and they do not share an index or columns. With merge we will connect two `DataFrame`s on some common piece of information, e.g. a common column. There are four types of `join` operations:

*   `inner`-join: **intersection** of *keys*
*   `outer`-join: **union** of *keys*
*   `left`-join: use *keys* from **left only**
*   `right`-join: use *keys* from **right only**


```python
# Merging
df1 = pd.DataFrame({"A": pd.Series([1, 2, 3]), "B": pd.Series([4, 5, 6])})
df2 = pd.DataFrame({"A": pd.Series([3, 4]), "C": pd.Series([7, 8])})

# "on" defines on what piece of information the DataFrame's will merge
pd.merge(df1, df2, on = 'A', how = 'right')  # if column names differ, use "left_on" and "right_on"
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>6.0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>NaN</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(df1, df2, on = 'A', how = 'left')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6</td>
      <td>7.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(df1, df2, on = 'A', how = 'outer')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>NaN</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(df1, df2, on='A', how = 'inner')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>6</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>



---
## 8. String manipulation

### 8.1 Simple operations

You may find yourself working with text data, a.k.a. strings. 

Strings are actually iterables, just like lists. They can be subset analogously:


```python
x = "one python string"
x[4:10]
```

    'python'




```python
# You can also turn a string into a list of strings via the "split" method:
x = "one python string"
y = x.split(" ")
y == ["one", "python", "string"]
```

    True




```python
# The reverse is also possible via the "join" method:
space = " "
z = space.join(y)
z == x
```

    True



You can also make everything lower (or upper) case, replace certain substrings with other substrings, and check for the existence of a substring with `in`:


```python
z = "My Python String"
z.lower()
```

    'my python string'




```python
z.upper()
```

    'MY PYTHON STRING'




```python
w = z.replace("Python", "R")
print(w)
"Python" in w
```

    My R String
    
    False



There are many more easy-to-use, built-in tools for working with text data in Python. You can read more here:

*   https://docs.python.org/3/library/stdtypes.html#string-method

### 8.2 Regular expressions

A *regular expression* is a special sequence of characters that helps you match or find other strings or sets of strings, using a specialized syntax held in a pattern. Regular expressions are used in different languages, we'll also review them in the `R` session. Python uses the module `re` to deal with them.

Let's pretend we have an `html` text. Find **all** words tagged as bold (`<b>word</b>`) in `html`, turn them to italics (`<i>word</i>`) and add the word "freaking" before them.


```python
import re

a_text = 'Why am I taking this <b>course</b>? I want to go back to the <b>holidays</b>.'

# Prefix 'r' is an indicator of regular expression
the_regex = r'<b>([a-z\s]+)</b>'
replacement = r'<i>freaking \1</i>'

# Function to replace a regular expression with another
re.sub(the_regex, replacement, a_text)
```

    'Why am I taking this <i>freaking course</i>? I want to go back to the <i>freaking holidays</i>.'



Some helpful functions to manouvering with regular expressions with `re`:

*  `re.search(pattern, string)`: scan through `string` looking for locations where the regular expression `pattern` produces a match, and return a corresponding match object.
*  `re.match(pattern, string)`: if zero or more characters at the beginning of `string` match the regular expression `pattern`, return a corresponding match object.
*  `re.split(pattern, string)`: split `string` by the occurrences of `pattern`
*  `re.sub(pattern, repl, string)`: return the `string `obtained by replacing the leftmost non-overlapping occurrences of `pattern` in string by the replacement `repl`
*  `re.findall()`: search for *all* occurrences that match a given pattern

Regular expressions are relatively complex and there is a long list of combinations we can make, which are well outside the scope of this introduction. We limit to mentioning some special characters to form regular expressions:

*  `^`: Start of string
*  `$`: End of string
*  `.`: One character, no matter which (except line break)
*  `*`: match 0 or more repetitions of the preceding RE
*  `+`: match 1 or more repetitions of the preceding RE
*  `?`: match 0 or 1 repetition of the preceding RE
*  `{m,n}`: Causes the resulting RE to match from m to n repetitions of the preceding RE. The comma and the n are optional depending on the case
*  `|`: OR. E.g. A|B means match RE A or B
*  `(...)`: Matches whatever regular expression is inside the parentheses
*  `[]`: Defines a subset of characters to match
*  `\`: Either escapes special characters (permitting you to match characters like '*', '?', and so forth), or signals a special sequence (e.g. \s means a white space).
*  `\w`: Matches a word (letters only)
*  `\W`: Matches a word (letters and characters, equivalent to `[^a-zA-Z0-9_]`)

---
## 9. Some advanced concepts


### 9.1 Copy vs. assignment

Look at the following example:


```python
a = [1, 2, 5]
b = a
b[2] = 10
```


```python
print(a)
print(b)
```

    [1, 2, 10]
    [1, 2, 10]
    

What happens is that really `a` and `b` point to the same place in the memory and share the same data. The way to create an object that will *copy* the data in a but not *share* the data with a is to use the method `copy()`.


```python
a = [1, 2, 5]
b = a.copy()
b[2] = 10
```


```python
print(a)
print(b)
```

    [1, 2, 5]
    [1, 2, 10]
    

This goes a bit deeper into the concept how `python` uses memory in your computer, but it is important to have a grasp on it in order to avoid potential problems when memory becomes an issue.


```python
a = [1, [2, 3], 5, 'abc'] 
b = a.copy()
b[1].append(100)
```


```python
print(a)
print(b)
```

    [1, [2, 3, 100], 5, 'abc']
    [1, [2, 3, 100], 5, 'abc']
    

Things become a little trickier when you deal with lists of lists, for this reason there is also the `deepcopy`. 


```python
a = [1, [2, 3], 5, 'abc'] 
b = a.copy()
b[1].append(100)
```


```python
print(a)
print(b)
```

    [1, [2, 3, 100], 5, 'abc']
    [1, [2, 3, 100], 5, 'abc']
    

(What happened here is that both `a` and `b` still point to some deeper list `[2, 3]`)


```python
# Try now 
from copy import deepcopy
b = deepcopy(a)
b[1].pop()  # Removes (last) item on the list
print(a)
print(b)
```

    [1, [2, 3, 100], 5, 'abc']
    [1, [2, 3], 5, 'abc']
    

### 9.2 Error handling

Sometimes there problems come up in the code that make it not function properlu. When that happens, if there is no error raised, the problem could be anywhere and that makes it very hard to fix.

Setting up informative *errors* if something is not working well, on the other hand, tells us where things go wrong and point us to what we need to fix.


```python
# Errors in Python are called Exceptions. 
e = Exception()
type(e)
```

    Exception




```python
# Exceptions are raised with the "raise" keyword: 
raise Exception('Oops!')
```


    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    Input In [90], in <cell line: 2>()
          1 # Exceptions are raised with the "raise" keyword: 
    ----> 2 raise Exception('Oops!')
    

    Exception: Oops!


Every *part* of your program, for example each function, must be in charge of things going as expected inside its body. If something goes wrong, it should tell us what happened.

One way to do this is to check for possible problems before they occur:


```python
def age_a_person(person):
    if not hasattr(person, 'age'):
       raise Exception(f'The person must have an age attribute! Given: {person}')
    return person.age + 1

age_a_person('notaperson')
```

    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    Input In [91], in <cell line: 6>()
          3        raise Exception(f'The person must have an age attribute! Given: {person}')
          4     return person.age + 1
    ----> 6 age_a_person('notaperson')
    

    Input In [91], in age_a_person(person)
          1 def age_a_person(person):
          2     if not hasattr(person, 'age'):
    ----> 3        raise Exception(f'The person must have an age attribute! Given: {person}')
          4     return person.age + 1
    

    Exception: The person must have an age attribute! Given: notaperson


Python encourages that one should first try, and *catch* any expected errors that occur, handling them then. By *catching an error* we mean the following:



```python
def age_a_person(person):
    try:
        return person.age + 1
    except AttributeError as e:
        raise Exception(f'The person must have an age attribute! Given: {person}') from e

age_a_person('notaperson')
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Input In [92], in age_a_person(person)
          2 try:
    ----> 3     return person.age + 1
          4 except AttributeError as e:
    

    AttributeError: 'str' object has no attribute 'age'

    
    The above exception was the direct cause of the following exception:
    

    Exception                                 Traceback (most recent call last)

    Input In [92], in <cell line: 7>()
          4     except AttributeError as e:
          5         raise Exception(f'The person must have an age attribute! Given: {person}') from e
    ----> 7 age_a_person('notaperson')
    

    Input In [92], in age_a_person(person)
          3     return person.age + 1
          4 except AttributeError as e:
    ----> 5     raise Exception(f'The person must have an age attribute! Given: {person}') from e
    

    Exception: The person must have an age attribute! Given: notaperson


### 9.3 Default values in functions

In `python` we get to assign default values to inputs of functions. For example:


```python
# New function "f"
def f(a = 1, b = 2):
    return a + b

```


```python
# Function "f" can be validly be called in the following ways
print(f())
print(f(10))
print(f(b = 4))
print(f(10, 4))
print(f(a = 10, b = 4))
print(f(b = 4, a = 10))
```

    3
    12
    5
    14
    14
    14
    


```python
# But NOT like this
f(a = 10, 4)
```


      Input In [95]
        f(a = 10, 4)
                   ^
    SyntaxError: positional argument follows keyword argument
    


---
## 10. Particular topics

### 10.1 Date and time objects

We introduce how to deal with time and date type of objects in Python. To do this, we need a module denominated `datetime`, which it also involves the class `datetime`.


```python
from datetime import datetime  # This is a module and a class

# Current date
current_time = datetime.now()
print(current_time)
print(type(current_time))
```

    2022-09-24 14:04:55.241435
    <class 'datetime.datetime'>
    

`current_time` is of class `datetime`, but this is just one of five distinct time-object classes:

*   `datetime`: allows to work with times and dates together (month, day, year, hour, second, microsecond).
*   `date`: works with dates only (month, day, year), independent of time. 
*   `time`: works with time only (hour, minute, second, microsecond), independent of date. 
*   `timedelta`: a duration of time used for measuring distance between to time points.
*   `tzinfo`: to deal with time zones.

The most common scenario with time data is translating from and to regular character objects, which is the most frequent format that we will encounter when importing times and dates. We use the following two functions:


```python
today_date = '2022-01-04'

# Create date object in given time format yyyy-mm-dd
today_date = datetime.strptime(today_date, '%Y-%m-%d')

print(today_date)
print(type(today_date))
```

    2022-01-04 00:00:00
    <class 'datetime.datetime'>
    

Note we used the *pattern* `%Y-%m-%d` to indicate the year-month-day format we want to give the date object. A full list of imaginable patterns can be found in the library's [documentation](https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior).


```python
print('* Month:', today_date.month)  # Get month from date
print('* Year:', today_date.year)  # Get month from year
print('* Day of month:', today_date.day)  # ...
print('* Day of Week (number):', today_date.weekday())  # Recall indexing
```

    * Month: 1
    * Year: 2022
    * Day of month: 4
    * Day of Week (number): 1
    


```python
print('* Hour: ', current_time.hour)
print('* Minute: ', current_time.minute)
print(current_time.isocalendar())  # Returns (year, # week, # day)
```

    * Hour:  14
    * Minute:  4
    datetime.IsoCalendarDate(year=2022, week=38, weekday=6)
    

As mentioned, you can convert from time to string with the function `strftime()`.


```python
today_str = datetime.strftime(today_date, format = '%Y-%m-%d')
type(today_str)
```

    str



Recall we mentioned that to measure time spans, or to operate dates or times (add/subtract) we could use the `timedelta` type of object. Mind you, these objects need not be anchored on a specific date and they can be a generic time frame.


```python
from datetime import timedelta

# timedelta objects
three_weeks = timedelta(weeks = 3)
one_year = timedelta(days = 365)

print(three_weeks)
print(type(three_weeks))
print(three_weeks.days)
print(one_year.days)
```

    21 days, 0:00:00
    <class 'datetime.timedelta'>
    21
    365
    

Let us now operate on these objects.


```python
from datetime import datetime, timedelta

# Current time
now = datetime.now()
print("Today's date: ", str(now))

# Add three weeks to current date
now_in_3weeks = now + three_weeks
print('Date after three weeks: ', now_in_3weeks)

# Subtract one year from current date
one_year_ago = now - one_year
print('Date one year ago: ', one_year_ago)
print(type(one_year_ago))
```

    Today's date:  2022-09-24 14:04:57.393466
    Date after three weeks:  2022-10-15 14:04:57.393466
    Date one year ago:  2021-09-24 14:04:57.393466
    <class 'datetime.datetime'>
    


```python
from datetime import date

# Create two dates
date1 = date(2011, 5, 28)
date2 = date(2015, 6, 6)
# create two dates with year, month, day, hour, minute, and second
date1b = datetime(2011, 5, 28, 23, 1, 0)
date2b = datetime(2015, 6, 6, 22, 52, 10)

# Difference between two dates
date_diff = date2 - date1
date_diffb = date2b - date1b
print("Time difference (days): ", date_diff.days)
print("Time difference: ", date_diffb)
print(type(date_diff))
```

    Time difference (days):  1470
    Time difference:  1469 days, 23:51:10
    <class 'datetime.timedelta'>
    


```python
# To work with time zones:
from pytz import timezone

# Create timezone US/Eastern
est = timezone('US/Eastern')

# Re-set date to local time
loc_time = est.localize(datetime(2015, 6, 6, 22, 52, 10))
print(loc_time)
```

    2015-06-06 22:52:10-04:00
    

You can also work with time objects using `pandas`. You can convert text strings into `pandas` `Datetime` objects using:

*  `to_datetime()`: to convert string dates/times to `datetime` objects.
*  `to_timedelta()`: find differences in times.



```python
import pandas as pd
import numpy as np

# String to datetime
good_date = pd.to_datetime("6th of June, 2015")
print(good_date)

# Create date series to_timedelta() (add numpy)
date_series = good_date + pd.to_timedelta(np.arange(12), 'D')
print(date_series)

# Create date series using date_range() function
date_series = pd.date_range('06/06/2015', periods = 12, freq = 'D')
print(date_series)
```

    2015-06-06 00:00:00
    DatetimeIndex(['2015-06-06', '2015-06-07', '2015-06-08', '2015-06-09',
                   '2015-06-10', '2015-06-11', '2015-06-12', '2015-06-13',
                   '2015-06-14', '2015-06-15', '2015-06-16', '2015-06-17'],
                  dtype='datetime64[ns]', freq=None)
    DatetimeIndex(['2015-06-06', '2015-06-07', '2015-06-08', '2015-06-09',
                   '2015-06-10', '2015-06-11', '2015-06-12', '2015-06-13',
                   '2015-06-14', '2015-06-15', '2015-06-16', '2015-06-17'],
                  dtype='datetime64[ns]', freq='D')
    


```python
# Create a DataFrame with date as a column
data = pd.DataFrame()
data['date'] = date_series
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-06-06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-06-07</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-06-08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-06-09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-06-10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Extract year, month, day, hour, and minute; and assign to new columns
data['year'] = data['date'].dt.year
data['month'] = data['date'].dt.month
data['day'] = data['date'].dt.day
data['hour'] = data['date'].dt.hour
data['minute'] = data['date'].dt.minute
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>hour</th>
      <th>minute</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-06-06</td>
      <td>2015</td>
      <td>6</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-06-07</td>
      <td>2015</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-06-08</td>
      <td>2015</td>
      <td>6</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-06-09</td>
      <td>2015</td>
      <td>6</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-06-10</td>
      <td>2015</td>
      <td>6</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 10.2 Web scraping and `html` parsing

Web *scraping* deals with the retrieval of information featured in some web page. It basically consists of reading the content in a URL, and posteriorly *parsing* it to extract the relevant information that you are looking for. This is an extensive topic in itself so we will just introduce the plain basics to get you started.

Before anything, note that most information in a HTML source is of little use to us and is used to render and format the webpage itself. Therefore it would be wise to familiarise yourself with HTML tags as and when needed. We must consider some design aspects of creating a *spider* that will *crawl* the target webpage and get us the desired content.

1.   Identify tags that contain useful information
2.   Add randomised waiting periods between every access to website
3.   Make use of logs to monitor progress
4.   Regularly write collected data to an external file
5.   **Always** respect `robots.txt` file defined by websites

We use as an example Wikipedia. The target will be to  retrieve the summary of the featured article in Wikipedia along with all the URLs in it.


```python
from bs4 import BeautifulSoup
#import urllib  # If you're using Python 2.x
import urllib.request  # If you're using Python 3

url = 'https://en.wikipedia.org/wiki/Main_Page'
#html = urllib.urlopen(url).read()  # Python 2.x
with urllib.request.urlopen(url) as url_content:
    html = url_content.read()

# BeautifulSoup() is a formatting module to interpret html/xml
soup = BeautifulSoup(html)
print(soup)
```

    <!DOCTYPE html>
    
    <html class="client-nojs" dir="ltr" lang="en">
    <head>
    <meta charset="utf-8"/>
    <title>Wikipedia, the free encyclopedia</title>
    <script>document.documentElement.className="client-js";RLCONF={"wgBreakFrames":false,"wgSeparatorTransformTable":["",""],"wgDigitTransformTable":["",""],"wgDefaultDateFormat":"dmy","wgMonthNames":["","January","February","March","April","May","June","July","August","September","October","November","December"],"wgRequestId":"ee933970-ac45-4871-ae1f-7d07362e511f","wgCSPNonce":false,"wgCanonicalNamespace":"","wgCanonicalSpecialPageName":false,"wgNamespaceNumber":0,"wgPageName":"Main_Page","wgTitle":"Main Page","wgCurRevisionId":1108085777,"wgRevisionId":1108085777,"wgArticleId":15580374,"wgIsArticle":true,"wgIsRedirect":false,"wgAction":"view","wgUserName":null,"wgUserGroups":["*"],"wgCategories":[],"wgPageContentLanguage":"en","wgPageContentModel":"wikitext","wgRelevantPageName":"Main_Page","wgRelevantArticleId":15580374,"wgIsProbablyEditable":false,"wgRelevantPageIsProbablyEditable":false,"wgRestrictionEdit":["sysop"],"wgRestrictionMove":["sysop"],"wgIsMainPage":true,"wgFlaggedRevsParams":{
    "tags":{"status":{"levels":1}}},"wgVisualEditor":{"pageLanguageCode":"en","pageLanguageDir":"ltr","pageVariantFallbacks":"en"},"wgMFDisplayWikibaseDescriptions":{"search":true,"nearby":true,"watchlist":true,"tagline":false},"wgWMESchemaEditAttemptStepOversample":false,"wgWMEPageLength":3000,"wgNoticeProject":"wikipedia","wgVector2022PreviewPages":[],"wgMediaViewerOnClick":true,"wgMediaViewerEnabledByDefault":true,"wgPopupsFlags":10,"wgULSCurrentAutonym":"English","wgEditSubmitButtonLabelPublish":true,"wgCentralAuthMobileDomain":false,"wgULSPosition":"interlanguage","wgULSisCompactLinksEnabled":true,"wgWikibaseItemId":"Q5296","GEHomepageSuggestedEditsEnableTopics":true,"wgGETopicsMatchModeEnabled":false,"wgGEStructuredTaskRejectionReasonTextInputEnabled":false};RLSTATE={"ext.globalCssJs.user.styles":"ready","site.styles":"ready","user.styles":"ready","ext.globalCssJs.user":"ready","user":"ready","user.options":"loading","skins.vector.styles.legacy":"ready",
    "ext.visualEditor.desktopArticleTarget.noscript":"ready","ext.wikimediaBadges":"ready","ext.uls.interlanguage":"ready"};RLPAGEMODULES=["site","mediawiki.page.ready","skins.vector.legacy.js","mmv.head","mmv.bootstrap.autostart","ext.visualEditor.desktopArticleTarget.init","ext.visualEditor.targetLoader","ext.eventLogging","ext.wikimediaEvents","ext.navigationTiming","ext.cx.eventlogging.campaigns","ext.centralNotice.geoIP","ext.centralNotice.startUp","ext.gadget.ReferenceTooltips","ext.gadget.charinsert","ext.gadget.extra-toolbar-buttons","ext.gadget.switcher","ext.centralauth.centralautologin","ext.popups","ext.uls.interface","ext.growthExperiments.SuggestedEditSession"];</script>
    <script>(RLQ=window.RLQ||[]).push(function(){mw.loader.implement("user.options@12s5i",function($,jQuery,require,module){mw.user.tokens.set({"patrolToken":"+\\","watchToken":"+\\","csrfToken":"+\\"});});});</script>
    <link href="/w/load.php?lang=en&amp;modules=ext.uls.interlanguage%7Cext.visualEditor.desktopArticleTarget.noscript%7Cext.wikimediaBadges%7Cskins.vector.styles.legacy&amp;only=styles&amp;skin=vector" rel="stylesheet"/>
    <script async="" src="/w/load.php?lang=en&amp;modules=startup&amp;only=scripts&amp;raw=1&amp;skin=vector"></script>
    <meta content="" name="ResourceLoaderDynamicStyles"/>
    <link href="/w/load.php?lang=en&amp;modules=site.styles&amp;only=styles&amp;skin=vector" rel="stylesheet"/>
    <meta content="MediaWiki 1.40.0-wmf.2" name="generator"/>
    <meta content="origin" name="referrer"/>
    <meta content="origin-when-crossorigin" name="referrer"/>
    <meta content="origin-when-cross-origin" name="referrer"/>
    <meta content="telephone=no" name="format-detection"/>
    <meta content="https://upload.wikimedia.org/wikipedia/commons/0/0f/Sawmill_fire_arizona_20171024_1.jpg" property="og:image"/>
    <meta content="1200" property="og:image:width"/>
    <meta content="928" property="og:image:height"/>
    <meta content="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Sawmill_fire_arizona_20171024_1.jpg/800px-Sawmill_fire_arizona_20171024_1.jpg" property="og:image"/>
    <meta content="800" property="og:image:width"/>
    <meta content="619" property="og:image:height"/>
    <meta content="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Sawmill_fire_arizona_20171024_1.jpg/640px-Sawmill_fire_arizona_20171024_1.jpg" property="og:image"/>
    <meta content="640" property="og:image:width"/>
    <meta content="495" property="og:image:height"/>
    <meta content="width=1000" name="viewport"/>
    <meta content="Wikipedia, the free encyclopedia" property="og:title"/>
    <meta content="website" property="og:type"/>
    <link href="//upload.wikimedia.org" rel="preconnect"/>
    <link href="//en.m.wikipedia.org/wiki/Main_Page" media="only screen and (max-width: 720px)" rel="alternate"/>
    <link href="/w/api.php?action=featuredfeed&amp;feed=potd&amp;feedformat=atom" rel="alternate" title="Wikipedia picture of the day feed" type="application/atom+xml"/>
    <link href="/w/api.php?action=featuredfeed&amp;feed=featured&amp;feedformat=atom" rel="alternate" title="Wikipedia featured articles feed" type="application/atom+xml"/>
    <link href="/w/api.php?action=featuredfeed&amp;feed=onthisday&amp;feedformat=atom" rel="alternate" title='Wikipedia "On this day..." feed' type="application/atom+xml"/>
    <link href="/static/apple-touch/wikipedia.png" rel="apple-touch-icon"/>
    <link href="/static/favicon/wikipedia.ico" rel="icon"/>
    <link href="/w/opensearch_desc.php" rel="search" title="Wikipedia (en)" type="application/opensearchdescription+xml"/>
    <link href="//en.wikipedia.org/w/api.php?action=rsd" rel="EditURI" type="application/rsd+xml"/>
    <link href="https://creativecommons.org/licenses/by-sa/3.0/" rel="license"/>
    <link href="https://en.wikipedia.org/wiki/Main_Page" rel="canonical"/>
    <link href="//meta.wikimedia.org" rel="dns-prefetch"/>
    <link href="//login.wikimedia.org" rel="dns-prefetch"/>
    </head>
    <body class="mediawiki ltr sitedir-ltr mw-hide-empty-elt ns-0 ns-subject page-Main_Page rootpage-Main_Page skin-vector action-view skin-vector-legacy vector-toc-not-collapsed vector-feature-language-in-header-enabled vector-feature-language-in-main-page-header-disabled vector-feature-language-alert-in-sidebar-enabled vector-feature-sticky-header-disabled vector-feature-sticky-header-edit-disabled vector-feature-table-of-contents-disabled vector-feature-visual-enhancement-next-disabled"><div class="noprint" id="mw-page-base"></div>
    <div class="noprint" id="mw-head-base"></div>
    <div class="mw-body" id="content" role="main">
    <a id="top"></a>
    <div id="siteNotice"><!-- CentralNotice --></div>
    <div class="mw-indicators">
    </div>
    <h1 class="firstHeading mw-first-heading" id="firstHeading" style="display: none"><span class="mw-page-title-main">Main Page</span></h1>
    <div class="vector-body" id="bodyContent">
    <div class="noprint" id="siteSub">From Wikipedia, the free encyclopedia</div>
    <div id="contentSub"></div>
    <div id="contentSub2"></div>
    <div id="jump-to-nav"></div>
    <a class="mw-jump-link" href="#mw-head">Jump to navigation</a>
    <a class="mw-jump-link" href="#searchInput">Jump to search</a>
    <div class="mw-body-content mw-content-ltr" dir="ltr" id="mw-content-text" lang="en"><div class="mw-parser-output"><style data-mw-deduplicate="TemplateStyles:r1094834805">.mw-parser-output .mp-box{border:1px solid #aaa;padding:0 0.5em 0.5em;margin-top:4px}.mw-parser-output .mp-h2,body.skin-timeless .mw-parser-output .mp-h2{border:1px solid #aaa;margin:0.5em 0;padding:0.2em 0.4em;font-size:120%;font-weight:bold;font-family:inherit}.mw-parser-output h2.mp-h2::after{border:none}.mw-parser-output .mp-later{font-size:85%;font-weight:normal}.mw-parser-output #mp-topbanner{background:#f9f9f9;border-color:#ddd}.mw-parser-output #mp-welcomecount{text-align:center;margin:0.4em}.mw-parser-output #mp-welcome{font-size:162%;padding:0.1em}.mw-parser-output #mp-welcome h1{font-size:inherit;font-family:inherit;display:inline;border:none}.mw-parser-output #mp-welcome h1::after{content:none}.mw-parser-output #mp-free{font-size:95%}.mw-parser-output #articlecount{font-size:85%}.mw-parser-output .mp-contains-float::after{content:"";display:block;clear:both}.mw-parser-output #mp-banner{background:#fffaf5;border-color:#f2e0ce}.mw-parser-output #mp-left{background:#f5fffa;border-color:#cef2e0}.mw-parser-output #mp-left .mp-h2{background:#cef2e0;border-color:#a3bfb1}.mw-parser-output #mp-right{background:#f5faff;border-color:#cedff2}.mw-parser-output #mp-right .mp-h2{background:#cedff2;border-color:#a3b0bf}.mw-parser-output #mp-middle{background:#fff5fa;border-color:#f2cedd}.mw-parser-output #mp-middle .mp-h2{background:#f2cedd;border-color:#bfa3af}.mw-parser-output #mp-lower{background:#faf5ff;border-color:#ddcef2}.mw-parser-output #mp-lower .mp-h2{background:#ddcef2;border-color:#afa3bf}.mw-parser-output #mp-bottom{border-color:#e2e2e2}.mw-parser-output #mp-bottom .mp-h2{background:#eee;border-color:#ddd}@media(max-width:875px){.mw-parser-output #mp-tfp table,.mw-parser-output #mp-tfp tr,.mw-parser-output #mp-tfp td,.mw-parser-output #mp-tfp tbody{display:block!important;width:100%!important;box-sizing:border-box}.mw-parser-output #mp-tfp tr:first-child td:first-child a{text-align:center;display:table;margin:0 auto}}@media(min-width:875px){.mw-parser-output #mp-upper{display:flex}.mw-parser-output #mp-left{flex:1 1 55%;margin-right:2px}.mw-parser-output #mp-right{flex:1 1 45%;margin-left:2px}}.mw-parser-output div.hlist.inline ul,.mw-parser-output div.hlist.inline li,.mw-parser-output div.hlist.inline{display:inline}</style>
    <div class="mp-box" id="mp-topbanner">
    <div id="mp-welcomecount">
    <div id="mp-welcome"><h1><span class="mw-headline" id="Welcome_to_Wikipedia">Welcome to <a href="/wiki/Wikipedia" title="Wikipedia">Wikipedia</a></span></h1>,</div>
    <div id="mp-free">the <a href="/wiki/Free_content" title="Free content">free</a> <a href="/wiki/Encyclopedia" title="Encyclopedia">encyclopedia</a> that <a href="/wiki/Help:Introduction_to_Wikipedia" title="Help:Introduction to Wikipedia">anyone can edit</a>.</div>
    <div id="articlecount"><a href="/wiki/Special:Statistics" title="Special:Statistics">6,554,229</a> articles in <a href="/wiki/English_language" title="English language">English</a></div>
    </div>
    </div>
    <div id="mp-upper">
    <div class="MainPageBG mp-box" id="mp-left">
    <h2 class="mp-h2" id="mp-tfa-h2"><span id="From_today.27s_featured_article"></span><span class="mw-headline" id="From_today's_featured_article">From today's featured article</span></h2>
    <div class="mp-contains-float" id="mp-tfa"><div id="mp-tfa-img" style="float: left; margin: 0.5em 0.9em 0.4em 0em;">
    <div class="thumbinner mp-thumb" style="background: transparent; border: none; padding: 0; max-width: 159px;">
    <a class="image" href="/wiki/File:Sawmill_fire_arizona_20171024_1.jpg" title="Extent of the Sawmill Fire"><img alt="Extent of the Sawmill Fire" data-file-height="792" data-file-width="1024" decoding="async" height="123" src="//upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Sawmill_fire_arizona_20171024_1.jpg/159px-Sawmill_fire_arizona_20171024_1.jpg" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Sawmill_fire_arizona_20171024_1.jpg/239px-Sawmill_fire_arizona_20171024_1.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Sawmill_fire_arizona_20171024_1.jpg/318px-Sawmill_fire_arizona_20171024_1.jpg 2x" width="159"/></a><div class="thumbcaption" style="padding: 0.25em 0; word-wrap: break-word;">Extent of the Sawmill Fire</div></div>
    </div>
    <p>The <b><a href="/wiki/Sawmill_Fire_(2017)" title="Sawmill Fire (2017)">Sawmill Fire</a></b> was a wildfire that burned almost 47,000 acres (19,000 ha) of the U.S. state of <a href="/wiki/Arizona" title="Arizona">Arizona</a>. It began on April 23, 2017, at a <a href="/wiki/Gender_reveal_party" title="Gender reveal party">gender reveal party</a> held in the <a href="/wiki/Coronado_National_Forest" title="Coronado National Forest">Coronado National Forest</a> with the detonation by gunshot of a target packed with blue powder and <a href="/wiki/Tannerite" title="Tannerite">tannerite</a>. Because of hot and arid conditions in the area, the fire spread rapidly and forced the closure of <a href="/wiki/Arizona_State_Route_89" title="Arizona State Route 89">Arizona State Route 89</a> and <a href="/wiki/Interstate_10" title="Interstate 10">Interstate 10</a> and the evacuation of 100 people. By April 30, the fire was fully contained and evacuation orders were lifted. No injuries or fatalities resulted from the fire, which cost $8,200,000 and required the deployment of over 800 first responders to contain and suppress. No buildings were destroyed by the fire, though the historic <a href="/wiki/Empire_Ranch" title="Empire Ranch">Empire Ranch</a> was as little as 50 feet (15 m) from the flames at times. When the source of the fire became public knowledge, it resulted in widespread ridicule of gender reveal parties in concept and practice. (<b><a href="/wiki/Sawmill_Fire_(2017)" title="Sawmill Fire (2017)">Full article...</a></b>)
    </p>
    <div class="tfa-recent" style="text-align: right;">
    Recently featured: <div class="hlist hlist-separated inline">
    <ul><li><a href="/wiki/Jason_Sendwe" title="Jason Sendwe">Jason Sendwe</a></li>
    <li><a href="/wiki/A_and_B_Loop" title="A and B Loop">A and B Loop</a></li>
    <li><a href="/wiki/Alexander_Cameron_Rutherford" title="Alexander Cameron Rutherford">Alexander Cameron Rutherford</a></li></ul>
    </div></div>
    <div class="hlist hlist-separated tfa-footer noprint" style="text-align:right;">
    <ul><li><b><a href="/wiki/Wikipedia:Today%27s_featured_article/September_2022" title="Wikipedia:Today's featured article/September 2022">Archive</a></b></li>
    <li><b><a class="extiw" href="https://lists.wikimedia.org/postorius/lists/daily-article-l.lists.wikimedia.org/" title="mail:daily-article-l">By email</a></b></li>
    <li><b><a href="/wiki/Wikipedia:Featured_articles" title="Wikipedia:Featured articles">More featured articles</a></b></li>
    <li><b><a href="/wiki/Wikipedia:About_Today%27s_featured_article" title="Wikipedia:About Today's featured article">About</a></b></li></ul>
    </div></div>
    <h2 class="mp-h2" id="mp-dyk-h2"><span class="mw-headline" id="Did_you_know_...">Did you know ...</span></h2>
    <div class="mp-contains-float" id="mp-dyk">
    <div class="dyk-img" style="float: right; margin-left: 0.5em;">
    <div class="thumbinner mp-thumb" style="background: transparent; border: none; padding: 0; max-width: 118px;">
    <a class="image" href="/wiki/File:Saint_Mary_of_the_Assumption_Church_(Lancaster,_Ohio)_-_exterior.jpg" title="Basilica of St. Mary of the Assumption"><img alt="Basilica of St. Mary of the Assumption" data-file-height="4829" data-file-width="3449" decoding="async" height="165" src="//upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Saint_Mary_of_the_Assumption_Church_%28Lancaster%2C_Ohio%29_-_exterior.jpg/118px-Saint_Mary_of_the_Assumption_Church_%28Lancaster%2C_Ohio%29_-_exterior.jpg" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Saint_Mary_of_the_Assumption_Church_%28Lancaster%2C_Ohio%29_-_exterior.jpg/177px-Saint_Mary_of_the_Assumption_Church_%28Lancaster%2C_Ohio%29_-_exterior.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Saint_Mary_of_the_Assumption_Church_%28Lancaster%2C_Ohio%29_-_exterior.jpg/236px-Saint_Mary_of_the_Assumption_Church_%28Lancaster%2C_Ohio%29_-_exterior.jpg 2x" width="118"/></a><div class="thumbcaption" style="padding: 0.25em 0; word-wrap: break-word;">Basilica of St. Mary of the Assumption</div></div>
    </div>
    <ul><li>... that after the <b><a href="/wiki/Basilica_of_St._Mary_of_the_Assumption_(Lancaster,_Ohio)" title="Basilica of St. Mary of the Assumption (Lancaster, Ohio)">Basilica of St. Mary of the Assumption</a></b> <i>(pictured)</i> was named a minor basilica by <a href="/wiki/Pope_Francis" title="Pope Francis">Pope Francis</a> in 2022, this was announced on the vigil of the <a href="/wiki/Assumption_of_Mary" title="Assumption of Mary">Assumption of Mary</a>?</li>
    <li>... that <b><a href="/wiki/Epistola_consolatoria_ad_pergentes_in_bellum" title="Epistola consolatoria ad pergentes in bellum">a Carolingian military sermon</a></b> promises soldiers victory, provided they do not engage in <a href="/wiki/Human_sexual_activity" title="Human sexual activity">sexual activity</a> or <a href="/wiki/Looting" title="Looting">looting</a>?</li>
    <li>... that the slopes near <b><a href="/wiki/Bass_Lake_(Watauga_County,_North_Carolina)" title="Bass Lake (Watauga County, North Carolina)">Bass Lake</a></b> at <a href="/wiki/Flat_Top_Manor" title="Flat Top Manor">Flat Top Manor</a> in North Carolina were covered with hundreds of apple trees?</li>
    <li>... that <b><a href="/wiki/Rockstar_Vancouver" title="Rockstar Vancouver">Rockstar Vancouver</a></b> developed most of the "Beta 5" update for <i><a href="/wiki/Counter-Strike_(video_game)" title="Counter-Strike (video game)">Counter-Strike</a></i><span style="padding-left:0.15em;">?</span></li>
    <li>... that Iraqi psychologist <b><a href="/wiki/Nuri_Ja%27far" title="Nuri Ja'far">Nuri Ja'far</a></b>, in his youth, was denied admission to the <a href="/wiki/College_of_Medicine_University_of_Baghdad" title="College of Medicine University of Baghdad">College of Medicine University of Baghdad</a> by <a href="/wiki/Harry_Sinderson" title="Harry Sinderson">Harry Sinderson</a>?</li>
    <li>... that <a href="/wiki/Billie_Eilish" title="Billie Eilish">Billie Eilish</a> and <a href="/wiki/Finneas_O%27Connell" title="Finneas O'Connell">Finneas O'Connell</a> wrote the melody for their four-time Grammy-nominated song "<b><a href="/wiki/Happier_Than_Ever_(song)" title="Happier Than Ever (song)">Happier Than Ever</a></b>" on an $80 guitar?</li>
    <li>... that South African politician <b><a href="/wiki/Speedy_Mashilo" title="Speedy Mashilo">Speedy Mashilo</a></b> was kidnapped for seven hours?</li>
    <li>... that <i><b><a href="/wiki/The_Random_Years" title="The Random Years">The Random Years</a></b></i> includes a version of <a href="/wiki/Strip_game#Poker" title="Strip game">strip poker</a> played to <i><a href="/wiki/Antiques_Roadshow_(American_TV_program)" title="Antiques Roadshow (American TV program)">Antiques Roadshow</a></i><span style="padding-left:0.15em;">?</span></li></ul>
    <div class="hlist hlist-separated dyk-footer noprint" style="margin-top: 0.5em; text-align: right;">
    <ul><li><b><a href="/wiki/Wikipedia:Recent_additions" title="Wikipedia:Recent additions">Archive</a></b></li>
    <li><b><a href="/wiki/Help:Your_first_article" title="Help:Your first article">Start a new article</a></b></li>
    <li><b><a href="/wiki/Template_talk:Did_you_know" title="Template talk:Did you know">Nominate an article</a></b></li></ul>
    </div>
    </div>
    </div>
    <div class="MainPageBG mp-box" id="mp-right">
    <h2 class="mp-h2" id="mp-itn-h2"><span class="mw-headline" id="In_the_news">In the news</span></h2>
    <div class="mp-contains-float" id="mp-itn"><style data-mw-deduplicate="TemplateStyles:r1053378754">.mw-parser-output .itn-img{float:right;margin-left:0.5em;margin-top:0.2em}</style><div class="itn-img" role="figure">
    <div class="thumbinner mp-thumb" style="background: transparent; border: none; padding: 0; max-width: 155px;">
    <a class="image" href="/wiki/File:Uprising_in_Tehran,_Keshavarz_Boulvard_September_2022_(2,_cropped_for_ITN).jpg" title="Protesters in Tehran, Iran"><img alt="Protesters in Tehran, Iran" class="thumbborder" data-file-height="550" data-file-width="660" decoding="async" height="128" src="//upload.wikimedia.org/wikipedia/commons/thumb/1/18/Uprising_in_Tehran%2C_Keshavarz_Boulvard_September_2022_%282%2C_cropped_for_ITN%29.jpg/153px-Uprising_in_Tehran%2C_Keshavarz_Boulvard_September_2022_%282%2C_cropped_for_ITN%29.jpg" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/1/18/Uprising_in_Tehran%2C_Keshavarz_Boulvard_September_2022_%282%2C_cropped_for_ITN%29.jpg/230px-Uprising_in_Tehran%2C_Keshavarz_Boulvard_September_2022_%282%2C_cropped_for_ITN%29.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/1/18/Uprising_in_Tehran%2C_Keshavarz_Boulvard_September_2022_%282%2C_cropped_for_ITN%29.jpg/306px-Uprising_in_Tehran%2C_Keshavarz_Boulvard_September_2022_%282%2C_cropped_for_ITN%29.jpg 2x" width="153"/></a><div class="thumbcaption" style="padding: 0.25em 0; word-wrap: break-word; text-align: left;">Protesters in <a href="/wiki/Tehran" title="Tehran">Tehran</a>, Iran</div></div>
    </div>
    <ul><li>Following the <b><a href="/wiki/Death_of_Mahsa_Amini" title="Death of Mahsa Amini">death of Mahsa Amini</a></b>, at least 50 people are killed during  <b><a href="/wiki/Mahsa_Amini_protests" title="Mahsa Amini protests">protests</a></b> <i>(example pictured)</i> in Iran.</li>
    <li>At least 100 people are killed in <b><a href="/wiki/2022_Kyrgyzstan%E2%80%93Tajikistan_clashes" title="2022 Kyrgyzstan–Tajikistan clashes">renewed fighting</a></b> between Kyrgyzstan and Tajikistan.</li>
    <li>In <b><a href="/wiki/2022_Swedish_general_election" title="2022 Swedish general election">the Swedish general election</a></b>, the bloc consisting of the <a href="/wiki/Sweden_Democrats" title="Sweden Democrats">Sweden Democrats</a>, <a href="/wiki/Moderate_Party" title="Moderate Party">Moderates</a>, <a href="/wiki/Christian_Democrats_(Sweden)" title="Christian Democrats (Sweden)">Christian Democrats</a> and <a href="/wiki/Liberals_(Sweden)" title="Liberals (Sweden)">Liberals</a> wins a majority of seats in the <a href="/wiki/Riksdag" title="Riksdag">Riksdag</a>.</li>
    <li>French-Swiss filmmaker <b><a href="/wiki/Jean-Luc_Godard" title="Jean-Luc Godard">Jean-Luc Godard</a></b> dies at the <span class="nowrap">age of 91</span>.</li></ul>
    <div class="itn-footer" style="margin-top: 0.5em;">
    <div><b><a href="/wiki/Portal:Current_events" title="Portal:Current events">Ongoing</a></b>: <div class="hlist hlist-separated inline">
    <ul><li><a href="/wiki/2022_Russian_invasion_of_Ukraine" title="2022 Russian invasion of Ukraine">Russian invasion of Ukraine</a></li></ul></div></div>
    <div><b><a href="/wiki/Deaths_in_2022" title="Deaths in 2022">Recent deaths</a></b>: <div class="hlist hlist-separated inline">
    <ul><li><a href="/wiki/Tom_Benner" title="Tom Benner">Tom Benner</a></li>
    <li><a href="/wiki/Greg_Lee_(basketball)" title="Greg Lee (basketball)">Greg Lee</a></li>
    <li><a href="/wiki/Maarten_Schmidt" title="Maarten Schmidt">Maarten Schmidt</a></li>
    <li><a href="/wiki/Harry_Langford" title="Harry Langford">Harry Langford</a></li>
    <li><a href="/wiki/John_Hamblin" title="John Hamblin">John Hamblin</a></li>
    <li><a href="/wiki/Cal_Browning" title="Cal Browning">Cal Browning</a></li></ul></div></div></div>
    <div class="hlist hlist-separated itn-footer noprint" style="text-align:right;">
    <ul><li><b><a href="/wiki/Wikipedia:In_the_news/Candidates" title="Wikipedia:In the news/Candidates">Nominate an article</a></b></li></ul>
    </div></div>
    <h2 class="mp-h2" id="mp-otd-h2"><span class="mw-headline" id="On_this_day">On this day</span></h2>
    <div class="mp-contains-float" id="mp-otd">
    <p><b><a href="/wiki/September_24" title="September 24">September 24</a></b>: <b><a href="/wiki/Heritage_Day_(South_Africa)" title="Heritage Day (South Africa)">Heritage Day</a></b> in South Africa
    </p>
    <div id="mp-otd-img" style="float:right;margin-left:0.5em;">
    <div class="thumbinner mp-thumb" style="background: transparent; border: none; padding: 0; max-width: 123px;">
    <a class="image" href="/wiki/File:Sir_James_Brooke_(1847)_by_Francis_Grant.jpg" title="Sir James Brooke"><img alt="Sir James Brooke" data-file-height="800" data-file-width="622" decoding="async" height="158" src="//upload.wikimedia.org/wikipedia/commons/thumb/7/73/Sir_James_Brooke_%281847%29_by_Francis_Grant.jpg/123px-Sir_James_Brooke_%281847%29_by_Francis_Grant.jpg" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/7/73/Sir_James_Brooke_%281847%29_by_Francis_Grant.jpg/185px-Sir_James_Brooke_%281847%29_by_Francis_Grant.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/7/73/Sir_James_Brooke_%281847%29_by_Francis_Grant.jpg/246px-Sir_James_Brooke_%281847%29_by_Francis_Grant.jpg 2x" width="123"/></a><div class="thumbcaption" style="padding: 0.25em 0; word-wrap: break-word;">Sir James Brooke</div></div>
    </div>
    <ul><li><a href="/wiki/1841" title="1841">1841</a> – Raja Muda Hashim, the uncle of <a href="/wiki/Omar_Ali_Saifuddin_II" title="Omar Ali Saifuddin II">Omar Ali Saifuddin II</a>, Sultan of <a href="/wiki/Bruneian_Sultanate_(1368%E2%80%931888)" title="Bruneian Sultanate (1368–1888)">Brunei</a>, conceded land to the British adventurer <a href="/wiki/James_Brooke" title="James Brooke">James Brooke</a> <i>(pictured)</i> to establish the <b><a href="/wiki/Raj_of_Sarawak" title="Raj of Sarawak">Raj of Sarawak</a></b>.</li>
    <li><a href="/wiki/1869" title="1869">1869</a> – <a href="/wiki/Jay_Gould" title="Jay Gould">Jay Gould</a>, <a href="/wiki/James_Fisk_(financier)" title="James Fisk (financier)">James Fisk</a> and other <a href="/wiki/Speculation" title="Speculation">speculators</a> plotted but failed to control the <a href="/wiki/New_York_Gold_Exchange" title="New York Gold Exchange">United States gold market</a>, <b><a href="/wiki/Black_Friday_(1869)" title="Black Friday (1869)">causing prices to plummet</a></b>.</li>
    <li><a href="/wiki/1945" title="1945">1945</a> – Dozens of Jews were injured in the <b><a href="/wiki/Topo%C4%BE%C4%8Dany_pogrom" title="Topoľčany pogrom">Topoľčany pogrom</a></b>, one of the worst episodes of <a href="/wiki/Postwar_anti-Jewish_violence_in_Slovakia" title="Postwar anti-Jewish violence in Slovakia">anti-Jewish violence in postwar Czechoslovakia</a>.</li>
    <li><a href="/wiki/1992" title="1992">1992</a> – After his neighbor identified handwriting samples placed on local billboards by police, <b><a href="/wiki/Oba_Chandler" title="Oba Chandler">Oba Chandler</a></b> was arrested three years after he committed a triple murder in the <a href="/wiki/Tampa_Bay_area" title="Tampa Bay area">Tampa Bay area</a> in Florida.</li>
    <li><a href="/wiki/2019" title="2019">2019</a> – The <a href="/wiki/Supreme_Court_of_the_United_Kingdom" title="Supreme Court of the United Kingdom">Supreme Court of the United Kingdom</a> <b><a href="/wiki/R_(Miller)_v_The_Prime_Minister_and_Cherry_v_Advocate_General_for_Scotland" title="R (Miller) v The Prime Minister and Cherry v Advocate General for Scotland">unanimously ruled</a></b> that advice given by Prime Minister <a href="/wiki/Boris_Johnson" title="Boris Johnson">Boris Johnson</a> to Queen <a href="/wiki/Elizabeth_II" title="Elizabeth II">Elizabeth II</a> that <b><a href="/wiki/2019_British_prorogation_controversy" title="2019 British prorogation controversy">Parliament should be prorogued</a></b> was unlawful.</li></ul>
    <div class="hlist hlist-separated" style="margin-top: 0.5em;"><ul><li><b><a href="/wiki/Gao_Pian" title="Gao Pian">Gao Pian</a></b>  (<abbr title="died">d.</abbr> 887)</li><li><b><a href="/wiki/Bessie_Braddock" title="Bessie Braddock">Bessie Braddock</a></b>   (<abbr title="born">b.</abbr> 1899)</li><li><b><a href="/wiki/John_Young_(astronaut)" title="John Young (astronaut)">John Young</a></b> (<abbr title="born">b.</abbr> 1930)</li></ul></div>
    <div style="margin-top:0.5em;">
    More anniversaries: <div class="hlist hlist-separated inline nowraplinks">
    <ul><li><a href="/wiki/September_23" title="September 23">September 23</a></li>
    <li><b><a href="/wiki/September_24" title="September 24">September 24</a></b></li>
    <li><a href="/wiki/September_25" title="September 25">September 25</a></li></ul>
    </div></div>
    <div class="hlist hlist-separated otd-footer noprint" style="text-align:right;">
    <ul><li><b><a href="/wiki/Wikipedia:Selected_anniversaries/September" title="Wikipedia:Selected anniversaries/September">Archive</a></b></li>
    <li><b><a class="extiw" href="https://lists.wikimedia.org/postorius/lists/daily-article-l.lists.wikimedia.org/" title="mail:daily-article-l">By email</a></b></li>
    <li><b><a href="/wiki/List_of_days_of_the_year" title="List of days of the year">List of days of the year</a></b></li></ul>
    </div></div>
    </div>
    </div>
    <div class="MainPageBG mp-box" id="mp-lower">
    <h2 class="mp-h2" id="mp-tfp-h2"><span id="Today.27s_featured_picture"></span><span class="mw-headline" id="Today's_featured_picture">Today's featured picture</span></h2>
    <div id="mp-tfp">
    <table role="presentation" style="margin:0 3px 3px; width:100%; box-sizing:border-box; text-align:left; background-color:transparent; border-collapse:collapse;">
    <tbody><tr>
    <td style="padding:0 0.9em 0 0; width:400px;"><a class="image" href="/wiki/File:Brown-headed_Honeyeater_-_Patchewollock.jpg" title="Brown-headed honeyeater"><img alt="Brown-headed honeyeater" data-file-height="2841" data-file-width="4261" decoding="async" height="267" src="//upload.wikimedia.org/wikipedia/commons/thumb/0/02/Brown-headed_Honeyeater_-_Patchewollock.jpg/400px-Brown-headed_Honeyeater_-_Patchewollock.jpg" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/0/02/Brown-headed_Honeyeater_-_Patchewollock.jpg/600px-Brown-headed_Honeyeater_-_Patchewollock.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/0/02/Brown-headed_Honeyeater_-_Patchewollock.jpg/800px-Brown-headed_Honeyeater_-_Patchewollock.jpg 2x" width="400"/></a>
    </td>
    <td style="padding:0 6px 0 0">
    <p>The <b><a href="/wiki/Brown-headed_honeyeater" title="Brown-headed honeyeater">brown-headed honeyeater</a></b> (<i>Melithreptus brevirostris</i>) is a species of <a href="/wiki/Passerine" title="Passerine">passerine</a> bird in the family Meliphagidae, the <a href="/wiki/Honeyeater" title="Honeyeater">honeyeaters</a>. <a href="/wiki/Endemism" title="Endemism">Endemic</a> to Australia, its natural <a href="/wiki/Habitat" title="Habitat">habitats</a> are temperate forests and Mediterranean-type shrubby vegetation. Insects form the bulk of its diet, and like its close relatives, the <a href="/wiki/Black-chinned_honeyeater" title="Black-chinned honeyeater">black-chinned</a> and <a href="/wiki/Strong-billed_honeyeater" title="Strong-billed honeyeater">strong-billed honeyeaters</a>, it forages by probing in the bark of tree trunks and branches. This brown-headed honeyeater was photographed perching on a branch in <a href="/wiki/Patchewollock" title="Patchewollock">Patchewollock</a>, Victoria.
    </p>
    <p style="text-align:left;"><small>Photograph credit: <a href="/wiki/User:JJ_Harrison" title="User:JJ Harrison">John Harrison</a></small></p>
    <div class="potd-recent" style="text-align:right;">
    Recently featured: <div class="hlist hlist-separated inline">
    <ul><li><a href="/wiki/Template:POTD/2022-09-23" title="Template:POTD/2022-09-23">Lichfield Cathedral</a></li>
    <li><a href="/wiki/Template:POTD/2022-09-22" title="Template:POTD/2022-09-22">Onésime Reclus</a></li>
    <li><a href="/wiki/Template:POTD/2022-09-21" title="Template:POTD/2022-09-21"><i>Pantala flavescens</i></a></li></ul>
    </div></div>
    <div class="hlist hlist-separated potd-footer noprint" style="text-align:right;">
    <ul><li><b><a href="/wiki/Wikipedia:Picture_of_the_day/Archive" title="Wikipedia:Picture of the day/Archive">Archive</a></b></li>
    <li><b><a href="/wiki/Wikipedia:Featured_pictures" title="Wikipedia:Featured pictures">More featured pictures</a></b></li></ul>
    </div>
    </td></tr></tbody></table></div>
    </div>
    <div class="mp-box" id="mp-bottom">
    <h2 class="mp-h2" id="mp-other"><span class="mw-headline" id="Other_areas_of_Wikipedia">Other areas of Wikipedia</span></h2>
    <div id="mp-other-content">
    <ul><li><b><a href="/wiki/Wikipedia:Community_portal" title="Wikipedia:Community portal">Community portal</a></b> – The central hub for editors, with resources, links, tasks, and announcements.</li>
    <li><b><a href="/wiki/Wikipedia:Village_pump" title="Wikipedia:Village pump">Village pump</a></b> – Forum for discussions about Wikipedia itself, including policies and technical issues.</li>
    <li><b><a href="/wiki/Wikipedia:News" title="Wikipedia:News">Site news</a></b> – Sources of news about Wikipedia and the broader Wikimedia movement.</li>
    <li><b><a href="/wiki/Wikipedia:Teahouse" title="Wikipedia:Teahouse">Teahouse</a></b> – Ask basic questions about using or editing Wikipedia.</li>
    <li><b><a href="/wiki/Wikipedia:Help_desk" title="Wikipedia:Help desk">Help desk</a></b> – Ask questions about using or editing Wikipedia.</li>
    <li><b><a href="/wiki/Wikipedia:Reference_desk" title="Wikipedia:Reference desk">Reference desk</a></b> – Ask research questions about encyclopedic topics.</li>
    <li><b><a href="/wiki/Wikipedia:Contents/Portals" title="Wikipedia:Contents/Portals">Content portals</a></b> – A unique way to navigate the encyclopedia.</li></ul>
    </div>
    <h2 class="mp-h2" id="mp-sister"><span id="Wikipedia.27s_sister_projects"></span><span class="mw-headline" id="Wikipedia's_sister_projects">Wikipedia's sister projects</span></h2>
    <div id="mp-sister-content"><style data-mw-deduplicate="TemplateStyles:r1007624485">.mw-parser-output #sister-projects-list{display:flex;flex-wrap:wrap}.mw-parser-output #sister-projects-list li{display:inline-block}.mw-parser-output #sister-projects-list li span{font-weight:bold}.mw-parser-output #sister-projects-list li>div{display:inline-block;vertical-align:middle;padding:6px 4px}.mw-parser-output #sister-projects-list li>div:first-child{text-align:center}@media(min-width:360px){.mw-parser-output #sister-projects-list li{width:33%;min-width:20em;white-space:nowrap;flex:1 0 25%}.mw-parser-output #sister-projects-list li>div:first-child{min-width:50px}}</style>
    <p>Wikipedia is written by volunteer editors and hosted by the <a href="/wiki/Wikimedia_Foundation" title="Wikimedia Foundation">Wikimedia Foundation</a>, a non-profit organization that also hosts a range of other volunteer <a class="extiw" href="https://wikimediafoundation.org/our-work/wikimedia-projects/" title="foundationsite:our-work/wikimedia-projects/">projects</a>:
    </p>
    <div class="plainlist">
    <ul id="sister-projects-list">
    <li>
    <div><a href="https://commons.wikimedia.org/wiki/" title="Commons"><img alt="Commons logo" data-file-height="1376" data-file-width="1024" decoding="async" height="42" src="//upload.wikimedia.org/wikipedia/en/thumb/4/4a/Commons-logo.svg/31px-Commons-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/4/4a/Commons-logo.svg/47px-Commons-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/4/4a/Commons-logo.svg/62px-Commons-logo.svg.png 2x" width="31"/></a></div>
    <div><span><a class="extiw" href="https://commons.wikimedia.org/wiki/" title="c:">Commons</a></span><br/>Free media repository</div>
    </li>
    <li>
    <div><a href="https://www.mediawiki.org/wiki/" title="MediaWiki"><img alt="MediaWiki logo" data-file-height="100" data-file-width="100" decoding="async" height="35" src="//upload.wikimedia.org/wikipedia/commons/thumb/a/a6/MediaWiki-2020-icon.svg/35px-MediaWiki-2020-icon.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/a/a6/MediaWiki-2020-icon.svg/53px-MediaWiki-2020-icon.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/a/a6/MediaWiki-2020-icon.svg/70px-MediaWiki-2020-icon.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://www.mediawiki.org/wiki/" title="mw:">MediaWiki</a></span><br/>Wiki software development</div>
    </li>
    <li>
    <div><a href="https://meta.wikimedia.org/wiki/" title="Meta-Wiki"><img alt="Meta-Wiki logo" data-file-height="900" data-file-width="900" decoding="async" height="35" src="//upload.wikimedia.org/wikipedia/commons/thumb/7/75/Wikimedia_Community_Logo.svg/35px-Wikimedia_Community_Logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/7/75/Wikimedia_Community_Logo.svg/53px-Wikimedia_Community_Logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/7/75/Wikimedia_Community_Logo.svg/70px-Wikimedia_Community_Logo.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://meta.wikimedia.org/wiki/" title="m:">Meta-Wiki</a></span><br/>Wikimedia project coordination</div>
    </li>
    <li>
    <div><a href="https://en.wikibooks.org/wiki/" title="Wikibooks"><img alt="Wikibooks logo" data-file-height="300" data-file-width="300" decoding="async" height="35" src="//upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikibooks-logo.svg/35px-Wikibooks-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikibooks-logo.svg/53px-Wikibooks-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikibooks-logo.svg/70px-Wikibooks-logo.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://en.wikibooks.org/wiki/" title="b:">Wikibooks</a></span><br/>Free textbooks and manuals</div>
    </li>
    <li>
    <div><a href="https://www.wikidata.org/wiki/" title="Wikidata"><img alt="Wikidata logo" data-file-height="590" data-file-width="1050" decoding="async" height="26" src="//upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Wikidata-logo.svg/47px-Wikidata-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Wikidata-logo.svg/71px-Wikidata-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Wikidata-logo.svg/94px-Wikidata-logo.svg.png 2x" width="47"/></a></div>
    <div><span><a class="extiw" href="https://www.wikidata.org/wiki/" title="d:">Wikidata</a></span><br/>Free knowledge base</div>
    </li>
    <li>
    <div><a href="https://en.wikinews.org/wiki/" title="Wikinews"><img alt="Wikinews logo" data-file-height="415" data-file-width="759" decoding="async" height="28" src="//upload.wikimedia.org/wikipedia/commons/thumb/2/24/Wikinews-logo.svg/51px-Wikinews-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/2/24/Wikinews-logo.svg/77px-Wikinews-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/2/24/Wikinews-logo.svg/102px-Wikinews-logo.svg.png 2x" width="51"/></a></div>
    <div><span><a class="extiw" href="https://en.wikinews.org/wiki/" title="n:">Wikinews</a></span><br/>Free-content news</div>
    </li>
    <li>
    <div><a href="https://en.wikiquote.org/wiki/" title="Wikiquote"><img alt="Wikiquote logo" data-file-height="355" data-file-width="300" decoding="async" height="41" src="//upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikiquote-logo.svg/35px-Wikiquote-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikiquote-logo.svg/53px-Wikiquote-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Wikiquote-logo.svg/70px-Wikiquote-logo.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://en.wikiquote.org/wiki/" title="q:">Wikiquote</a></span><br/>Collection of quotations</div>
    </li>
    <li>
    <div><a href="https://en.wikisource.org/wiki/" title="Wikisource"><img alt="Wikisource logo" data-file-height="430" data-file-width="410" decoding="async" height="37" src="//upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Wikisource-logo.svg/35px-Wikisource-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Wikisource-logo.svg/53px-Wikisource-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Wikisource-logo.svg/70px-Wikisource-logo.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://en.wikisource.org/wiki/" title="s:">Wikisource</a></span><br/>Free-content library</div>
    </li>
    <li>
    <div><a href="https://species.wikimedia.org/wiki/" title="Wikispecies"><img alt="Wikispecies logo" data-file-height="1103" data-file-width="941" decoding="async" height="41" src="//upload.wikimedia.org/wikipedia/commons/thumb/d/df/Wikispecies-logo.svg/35px-Wikispecies-logo.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/d/df/Wikispecies-logo.svg/53px-Wikispecies-logo.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/d/df/Wikispecies-logo.svg/70px-Wikispecies-logo.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://species.wikimedia.org/wiki/" title="species:">Wikispecies</a></span><br/>Directory of species</div>
    </li>
    <li>
    <div><a href="https://en.wikiversity.org/wiki/" title="Wikiversity"><img alt="Wikiversity logo" data-file-height="512" data-file-width="626" decoding="async" height="34" src="//upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Wikiversity_logo_2017.svg/41px-Wikiversity_logo_2017.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Wikiversity_logo_2017.svg/62px-Wikiversity_logo_2017.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/0/0b/Wikiversity_logo_2017.svg/82px-Wikiversity_logo_2017.svg.png 2x" width="41"/></a></div>
    <div><span><a class="extiw" href="https://en.wikiversity.org/wiki/" title="v:">Wikiversity</a></span><br/>Free learning tools</div>
    </li>
    <li>
    <div><a href="https://en.wikivoyage.org/wiki/" title="Wikivoyage"><img alt="Wikivoyage logo" data-file-height="193" data-file-width="193" decoding="async" height="35" src="//upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Wikivoyage-Logo-v3-icon.svg/35px-Wikivoyage-Logo-v3-icon.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Wikivoyage-Logo-v3-icon.svg/53px-Wikivoyage-Logo-v3-icon.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Wikivoyage-Logo-v3-icon.svg/70px-Wikivoyage-Logo-v3-icon.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://en.wikivoyage.org/wiki/" title="voy:">Wikivoyage</a></span><br/>Free travel guide</div>
    </li>
    <li>
    <div><a href="https://en.wiktionary.org/wiki/" title="Wiktionary"><img alt="Wiktionary logo" data-file-height="391" data-file-width="391" decoding="async" height="35" src="//upload.wikimedia.org/wikipedia/en/thumb/0/06/Wiktionary-logo-v2.svg/35px-Wiktionary-logo-v2.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/0/06/Wiktionary-logo-v2.svg/53px-Wiktionary-logo-v2.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/0/06/Wiktionary-logo-v2.svg/70px-Wiktionary-logo-v2.svg.png 2x" width="35"/></a></div>
    <div><span><a class="extiw" href="https://en.wiktionary.org/wiki/" title="wikt:">Wiktionary</a></span><br/>Dictionary and thesaurus</div>
    </li>
    </ul>
    </div></div>
    <h2 class="mp-h2" id="mp-lang"><span class="mw-headline" id="Wikipedia_languages">Wikipedia languages</span></h2>
    <div><style data-mw-deduplicate="TemplateStyles:r997272951">.mw-parser-output .wikipedia-languages-complete{font-weight:bold}.mw-parser-output .wikipedia-languages ul{margin-left:0}.mw-parser-output .wikipedia-languages ul a{white-space:nowrap}.mw-parser-output .wikipedia-languages>ul{list-style:none;text-align:center;clear:both}.mw-parser-output .wikipedia-languages-count-container{width:90%;display:flex;justify-content:center;padding-top:1em;margin:0 auto}.mw-parser-output .wikipedia-languages-prettybars{width:100%;height:1px;margin:0.5em 0;background-color:#c8ccd1;flex-shrink:1;align-self:center}.mw-parser-output .wikipedia-languages-count{padding:0 1em;white-space:nowrap}</style>
    <div class="wikipedia-languages nourlexpansion">
    <p>This Wikipedia is written in <a href="/wiki/English_language" title="English language">English</a>. Many <a class="extiw" href="https://meta.wikimedia.org/wiki/List_of_Wikipedias" title="meta:List of Wikipedias">other Wikipedias are available</a>; some of the largest are listed below.
    </p>
    <ul class="plainlinks">
    <li>
    <div class="wikipedia-languages-count-container">
    <div class="wikipedia-languages-prettybars"></div>
    <div class="wikipedia-languages-count" role="heading">1,000,000+ articles</div>
    <div class="wikipedia-languages-prettybars"></div>
    </div>
    <ul class="wikipedia-languages-langs hlist hlist-separated inline">
    <li><a class="external text" href="https://ar.wikipedia.org/wiki/"><span class="autonym" lang="ar" title="Arabic (ar:)">العربية</span></a></li>
    <li><a class="external text" href="https://de.wikipedia.org/wiki/"><span class="autonym" lang="de" title="German (de:)">Deutsch</span></a></li>
    <li><a class="external text" href="https://es.wikipedia.org/wiki/"><span class="autonym" lang="es" title="Spanish (es:)">Español</span></a></li>
    <li><a class="external text" href="https://fr.wikipedia.org/wiki/"><span class="autonym" lang="fr" title="French (fr:)">Français</span></a></li>
    <li><a class="external text" href="https://it.wikipedia.org/wiki/"><span class="autonym" lang="it" title="Italian (it:)">Italiano</span></a></li>
    <li><a class="external text" href="https://nl.wikipedia.org/wiki/"><span class="autonym" lang="nl" title="Dutch (nl:)">Nederlands</span></a></li>
    <li><a class="external text" href="https://ja.wikipedia.org/wiki/"><span class="autonym" lang="ja" title="Japanese (ja:)">日本語</span></a></li>
    <li><a class="external text" href="https://pl.wikipedia.org/wiki/"><span class="autonym" lang="pl" title="Polish (pl:)">Polski</span></a></li>
    <li><a class="external text" href="https://pt.wikipedia.org/wiki/"><span class="autonym" lang="pt" title="Portuguese (pt:)">Português</span></a></li>
    <li><a class="external text" href="https://ru.wikipedia.org/wiki/"><span class="autonym" lang="ru" title="Russian (ru:)">Русский</span></a></li>
    <li><a class="external text" href="https://sv.wikipedia.org/wiki/"><span class="autonym" lang="sv" title="Swedish (sv:)">Svenska</span></a></li>
    <li><a class="external text" href="https://uk.wikipedia.org/wiki/"><span class="autonym" lang="uk" title="Ukrainian (uk:)">Українська</span></a></li>
    <li><a class="external text" href="https://vi.wikipedia.org/wiki/"><span class="autonym" lang="vi" title="Vietnamese (vi:)">Tiếng Việt</span></a></li>
    <li><a class="external text" href="https://zh.wikipedia.org/wiki/"><span class="autonym" lang="zh" title="Chinese (zh:)">中文</span></a></li>
    </ul>
    </li>
    <li>
    <div class="wikipedia-languages-count-container">
    <div class="wikipedia-languages-prettybars"></div>
    <div class="wikipedia-languages-count" role="heading">250,000+ articles</div>
    <div class="wikipedia-languages-prettybars"></div>
    </div>
    <ul class="wikipedia-languages-langs hlist hlist-separated inline">
    <li><a class="external text" href="https://id.wikipedia.org/wiki/"><span class="autonym" lang="id" title="Indonesian (id:)">Bahasa Indonesia</span></a></li>
    <li><a class="external text" href="https://ms.wikipedia.org/wiki/"><span class="autonym" lang="ms" title="Malay (ms:)">Bahasa Melayu</span></a></li>
    <li><a class="external text" href="https://zh-min-nan.wikipedia.org/wiki/"><span class="autonym" lang="nan" title="Min Nan Chinese (nan:)">Bân-lâm-gú</span></a></li>
    <li><a class="external text" href="https://bg.wikipedia.org/wiki/"><span class="autonym" lang="bg" title="Bulgarian (bg:)">Български</span></a></li>
    <li><a class="external text" href="https://ca.wikipedia.org/wiki/"><span class="autonym" lang="ca" title="Catalan (ca:)">Català</span></a></li>
    <li><a class="external text" href="https://cs.wikipedia.org/wiki/"><span class="autonym" lang="cs" title="Czech (cs:)">Čeština</span></a></li>
    <li><a class="external text" href="https://da.wikipedia.org/wiki/"><span class="autonym" lang="da" title="Danish (da:)">Dansk</span></a></li>
    <li><a class="external text" href="https://eo.wikipedia.org/wiki/"><span class="autonym" lang="eo" title="Esperanto (eo:)">Esperanto</span></a></li>
    <li><a class="external text" href="https://eu.wikipedia.org/wiki/"><span class="autonym" lang="eu" title="Basque (eu:)">Euskara</span></a></li>
    <li><a class="external text" href="https://fa.wikipedia.org/wiki/"><span class="autonym" lang="fa" title="Persian (fa:)">فارسی</span></a>‎</li>
    <li><a class="external text" href="https://he.wikipedia.org/wiki/"><span class="autonym" lang="he" title="Hebrew (he:)">עברית</span></a></li>
    <li><a class="external text" href="https://ko.wikipedia.org/wiki/"><span class="autonym" lang="ko" title="Korean (ko:)">한국어</span></a></li>
    <li><a class="external text" href="https://hu.wikipedia.org/wiki/"><span class="autonym" lang="hu" title="Hungarian (hu:)">Magyar</span></a></li>
    <li><a class="external text" href="https://no.wikipedia.org/wiki/"><span class="autonym" lang="no" title="Norwegian (no:)">Norsk Bokmål</span></a></li>
    <li><a class="external text" href="https://ro.wikipedia.org/wiki/"><span class="autonym" lang="ro" title="Romanian (ro:)">Română</span></a></li>
    <li><a class="external text" href="https://sr.wikipedia.org/wiki/"><span class="autonym" lang="sr" title="Serbian (sr:)">Srpski</span></a></li>
    <li><a class="external text" href="https://sh.wikipedia.org/wiki/"><span class="autonym" lang="sh" title="Serbo-Croatian (sh:)">Srpskohrvatski</span></a></li>
    <li><a class="external text" href="https://fi.wikipedia.org/wiki/"><span class="autonym" lang="fi" title="Finnish (fi:)">Suomi</span></a></li>
    <li><a class="external text" href="https://tr.wikipedia.org/wiki/"><span class="autonym" lang="tr" title="Turkish (tr:)">Türkçe</span></a></li>
    </ul>
    </li>
    <li>
    <div class="wikipedia-languages-count-container">
    <div class="wikipedia-languages-prettybars"></div>
    <div class="wikipedia-languages-count" role="heading">50,000+ articles</div>
    <div class="wikipedia-languages-prettybars"></div>
    </div>
    <ul class="wikipedia-languages-langs hlist hlist-separated inline">
    <li><a class="external text" href="https://ast.wikipedia.org/wiki/"><span class="autonym" lang="ast" title="Asturian (ast:)">Asturianu</span></a></li>
    <li><a class="external text" href="https://bn.wikipedia.org/wiki/"><span class="autonym" lang="bn" title="Bangla (bn:)">বাংলা</span></a></li>
    <li><a class="external text" href="https://bs.wikipedia.org/wiki/"><span class="autonym" lang="bs" title="Bosnian (bs:)">Bosanski</span></a></li>
    <li><a class="external text" href="https://et.wikipedia.org/wiki/"><span class="autonym" lang="et" title="Estonian (et:)">Eesti</span></a></li>
    <li><a class="external text" href="https://el.wikipedia.org/wiki/"><span class="autonym" lang="el" title="Greek (el:)">Ελληνικά</span></a></li>
    <li><a class="external text" href="https://simple.wikipedia.org/wiki/"><span class="autonym" lang="simple" title="Simple English (simple:)">Simple English</span></a></li>
    <li><a class="external text" href="https://gl.wikipedia.org/wiki/"><span class="autonym" lang="gl" title="Galician (gl:)">Galego</span></a></li>
    <li><a class="external text" href="https://hr.wikipedia.org/wiki/"><span class="autonym" lang="hr" title="Croatian (hr:)">Hrvatski</span></a></li>
    <li><a class="external text" href="https://lv.wikipedia.org/wiki/"><span class="autonym" lang="lv" title="Latvian (lv:)">Latviešu</span></a></li>
    <li><a class="external text" href="https://lt.wikipedia.org/wiki/"><span class="autonym" lang="lt" title="Lithuanian (lt:)">Lietuvių</span></a></li>
    <li><a class="external text" href="https://ml.wikipedia.org/wiki/"><span class="autonym" lang="ml" title="Malayalam (ml:)">മലയാളം</span></a></li>
    <li><a class="external text" href="https://mk.wikipedia.org/wiki/"><span class="autonym" lang="mk" title="Macedonian (mk:)">Македонски</span></a></li>
    <li><a class="external text" href="https://nn.wikipedia.org/wiki/"><span class="autonym" lang="nn" title="Norwegian Nynorsk (nn:)">Norsk nynorsk</span></a></li>
    <li><a class="external text" href="https://sq.wikipedia.org/wiki/"><span class="autonym" lang="sq" title="Albanian (sq:)">Shqip</span></a></li>
    <li><a class="external text" href="https://sk.wikipedia.org/wiki/"><span class="autonym" lang="sk" title="Slovak (sk:)">Slovenčina</span></a></li>
    <li><a class="external text" href="https://sl.wikipedia.org/wiki/"><span class="autonym" lang="sl" title="Slovenian (sl:)">Slovenščina</span></a></li>
    <li><a class="external text" href="https://th.wikipedia.org/wiki/"><span class="autonym" lang="th" title="Thai (th:)">ไทย</span></a></li>
    </ul>
    </li>
    </ul>
    </div></div>
    </div>
    <!-- 
    NewPP limit report
    Parsed by mw1429
    Cached time: 20220924113114
    Cache expiry: 3600
    Reduced expiry: true
    Complications: []
    CPU time usage: 0.277 seconds
    Real time usage: 0.359 seconds
    Preprocessor visited node count: 3929/1000000
    Post‐expand include size: 114189/2097152 bytes
    Template argument size: 9535/2097152 bytes
    Highest expansion depth: 18/100
    Expensive parser function count: 13/500
    Unstrip recursion depth: 0/20
    Unstrip post‐expand size: 3979/5000000 bytes
    Lua time usage: 0.054/10.000 seconds
    Lua memory usage: 1871375/52428800 bytes
    Number of Wikibase entities loaded: 0/400
    -->
    <!--
    Transclusion expansion time report (%,ms,calls,template)
    100.00%  227.766      1 -total
     38.28%   87.187      1 Wikipedia:Main_Page/Tomorrow
     29.58%   67.370      8 Template:Main_page_image
     20.72%   47.202      8 Template:Str_number/trim
     18.44%   42.010      1 Wikipedia:Today's_featured_article/September_24,_2022
     17.63%   40.157      1 Template:Did_you_know/Queue/2
     17.01%   38.753      2 Template:Main_page_image/TFA
     15.43%   35.135      2 Template:Wikipedia_languages
     14.40%   32.808      1 Template:DYKbotdo
     13.59%   30.960      1 Template:Mbox
    -->
    <!-- Saved in parser cache with key enwiki:pcache:idhash:15580374-0!canonical and timestamp 20220924113113 and revision id 1108085777.
     -->
    </div><noscript><img alt="" height="1" src="//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1" style="border: none; position: absolute;" title="" width="1"/></noscript>
    <div class="printfooter" data-nosnippet="">Retrieved from "<a dir="ltr" href="https://en.wikipedia.org/w/index.php?title=Main_Page&amp;oldid=1108085777">https://en.wikipedia.org/w/index.php?title=Main_Page&amp;oldid=1108085777</a>"</div></div>
    <div class="catlinks catlinks-allhidden" data-mw="interface" id="catlinks"></div>
    </div>
    </div>
    <div id="mw-navigation">
    <h2>Navigation menu</h2>
    <div id="mw-head">
    <nav aria-labelledby="p-personal-label" class="vector-menu mw-portlet mw-portlet-personal vector-user-menu-legacy" id="p-personal" role="navigation">
    <h3 class="vector-menu-heading" id="p-personal-label">
    <span class="vector-menu-heading-label">Personal tools</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="mw-list-item" id="pt-anonuserpage"><span title="The user page for the IP address you are editing as">Not logged in</span></li><li class="mw-list-item" id="pt-anontalk"><a accesskey="n" href="/wiki/Special:MyTalk" title="Discussion about edits from this IP address [n]"><span>Talk</span></a></li><li class="mw-list-item" id="pt-anoncontribs"><a accesskey="y" href="/wiki/Special:MyContributions" title="A list of edits made from this IP address [y]"><span>Contributions</span></a></li><li class="mw-list-item" id="pt-createaccount"><a href="/w/index.php?title=Special:CreateAccount&amp;returnto=Main+Page" title="You are encouraged to create an account and log in; however, it is not mandatory"><span>Create account</span></a></li><li class="mw-list-item" id="pt-login"><a accesskey="o" href="/w/index.php?title=Special:UserLogin&amp;returnto=Main+Page" title="You're encouraged to log in; however, it's not mandatory. [o]"><span>Log in</span></a></li></ul>
    </div>
    </nav>
    <div id="left-navigation">
    <nav aria-labelledby="p-namespaces-label" class="vector-menu mw-portlet mw-portlet-namespaces vector-menu-tabs vector-menu-tabs-legacy" id="p-namespaces" role="navigation">
    <h3 class="vector-menu-heading" id="p-namespaces-label">
    <span class="vector-menu-heading-label">Namespaces</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="selected mw-list-item" id="ca-nstab-main"><a accesskey="c" href="/wiki/Main_Page" title="View the content page [c]"><span>Main Page</span></a></li><li class="mw-list-item" id="ca-talk"><a accesskey="t" href="/wiki/Talk:Main_Page" rel="discussion" title="Discuss improvements to the content page [t]"><span>Talk</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-variants-label" class="vector-menu mw-portlet mw-portlet-variants emptyPortlet vector-menu-dropdown" id="p-variants" role="navigation">
    <input aria-haspopup="true" aria-labelledby="p-variants-label" class="vector-menu-checkbox" data-event-name="ui.dropdown-p-variants" id="p-variants-checkbox" role="button" type="checkbox"/>
    <label aria-label="Change language variant" class="vector-menu-heading" id="p-variants-label">
    <span class="vector-menu-heading-label">English</span>
    </label>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"></ul>
    </div>
    </nav>
    </div>
    <div id="right-navigation">
    <nav aria-labelledby="p-views-label" class="vector-menu mw-portlet mw-portlet-views vector-menu-tabs vector-menu-tabs-legacy" id="p-views" role="navigation">
    <h3 class="vector-menu-heading" id="p-views-label">
    <span class="vector-menu-heading-label">Views</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="selected vector-tab-noicon mw-list-item" id="ca-view"><a href="/wiki/Main_Page"><span>Read</span></a></li><li class="vector-tab-noicon mw-list-item" id="ca-viewsource"><a accesskey="e" href="/w/index.php?title=Main_Page&amp;action=edit" title="This page is protected.
    You can view its source [e]"><span>View source</span></a></li><li class="vector-tab-noicon mw-list-item" id="ca-history"><a accesskey="h" href="/w/index.php?title=Main_Page&amp;action=history" title="Past revisions of this page [h]"><span>View history</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-cactions-label" class="vector-menu mw-portlet mw-portlet-cactions emptyPortlet vector-menu-dropdown" id="p-cactions" role="navigation" title="More options">
    <input aria-haspopup="true" aria-labelledby="p-cactions-label" class="vector-menu-checkbox" data-event-name="ui.dropdown-p-cactions" id="p-cactions-checkbox" role="button" type="checkbox"/>
    <label class="vector-menu-heading" id="p-cactions-label">
    <span class="vector-menu-heading-label">More</span>
    </label>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"></ul>
    </div>
    </nav>
    <div class="vector-search-box-vue vector-search-box-show-thumbnail vector-search-box-auto-expand-width vector-search-box" id="p-search" role="search">
    <div>
    <h3>
    <label for="searchInput">Search</label>
    </h3>
    <form action="/w/index.php" class="vector-search-box-form" id="searchform">
    <div class="vector-search-box-inner" data-search-loc="header-navigation" id="simpleSearch">
    <input accesskey="f" aria-label="Search Wikipedia" autocapitalize="sentences" class="vector-search-box-input" id="searchInput" name="search" placeholder="Search Wikipedia" title="Search Wikipedia [f]" type="search"/>
    <input name="title" type="hidden" value="Special:Search"/>
    <input class="searchButton mw-fallbackSearchButton" id="mw-searchButton" name="fulltext" title="Search Wikipedia for this text" type="submit" value="Search"/>
    <input class="searchButton" id="searchButton" name="go" title="Go to a page with this exact name if it exists" type="submit" value="Go"/>
    </div>
    </form>
    </div>
    </div>
    </div>
    </div>
    <div id="mw-panel">
    <div id="p-logo" role="banner">
    <a class="mw-wiki-logo" href="/wiki/Main_Page" title="Visit the main page"></a>
    </div>
    <nav aria-labelledby="p-navigation-label" class="vector-menu mw-portlet mw-portlet-navigation vector-menu-portal portal" id="p-navigation" role="navigation">
    <h3 class="vector-menu-heading" id="p-navigation-label">
    <span class="vector-menu-heading-label">Navigation</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="mw-list-item" id="n-mainpage-description"><a accesskey="z" href="/wiki/Main_Page" title="Visit the main page [z]"><span>Main page</span></a></li><li class="mw-list-item" id="n-contents"><a href="/wiki/Wikipedia:Contents" title="Guides to browsing Wikipedia"><span>Contents</span></a></li><li class="mw-list-item" id="n-currentevents"><a href="/wiki/Portal:Current_events" title="Articles related to current events"><span>Current events</span></a></li><li class="mw-list-item" id="n-randompage"><a accesskey="x" href="/wiki/Special:Random" title="Visit a randomly selected article [x]"><span>Random article</span></a></li><li class="mw-list-item" id="n-aboutsite"><a href="/wiki/Wikipedia:About" title="Learn about Wikipedia and how it works"><span>About Wikipedia</span></a></li><li class="mw-list-item" id="n-contactpage"><a href="//en.wikipedia.org/wiki/Wikipedia:Contact_us" title="How to contact Wikipedia"><span>Contact us</span></a></li><li class="mw-list-item" id="n-sitesupport"><a href="https://donate.wikimedia.org/wiki/Special:FundraiserRedirector?utm_source=donate&amp;utm_medium=sidebar&amp;utm_campaign=C13_en.wikipedia.org&amp;uselang=en" title="Support us by donating to the Wikimedia Foundation"><span>Donate</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-interaction-label" class="vector-menu mw-portlet mw-portlet-interaction vector-menu-portal portal" id="p-interaction" role="navigation">
    <h3 class="vector-menu-heading" id="p-interaction-label">
    <span class="vector-menu-heading-label">Contribute</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="mw-list-item" id="n-help"><a href="/wiki/Help:Contents" title="Guidance on how to use and edit Wikipedia"><span>Help</span></a></li><li class="mw-list-item" id="n-introduction"><a href="/wiki/Help:Introduction" title="Learn how to edit Wikipedia"><span>Learn to edit</span></a></li><li class="mw-list-item" id="n-portal"><a href="/wiki/Wikipedia:Community_portal" title="The hub for editors"><span>Community portal</span></a></li><li class="mw-list-item" id="n-recentchanges"><a accesskey="r" href="/wiki/Special:RecentChanges" title="A list of recent changes to Wikipedia [r]"><span>Recent changes</span></a></li><li class="mw-list-item" id="n-upload"><a href="/wiki/Wikipedia:File_Upload_Wizard" title="Add images or other media for use on Wikipedia"><span>Upload file</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-tb-label" class="vector-menu mw-portlet mw-portlet-tb vector-menu-portal portal" id="p-tb" role="navigation">
    <h3 class="vector-menu-heading" id="p-tb-label">
    <span class="vector-menu-heading-label">Tools</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="mw-list-item" id="t-whatlinkshere"><a accesskey="j" href="/wiki/Special:WhatLinksHere/Main_Page" title="List of all English Wikipedia pages containing links to this page [j]"><span>What links here</span></a></li><li class="mw-list-item" id="t-recentchangeslinked"><a accesskey="k" href="/wiki/Special:RecentChangesLinked/Main_Page" rel="nofollow" title="Recent changes in pages linked from this page [k]"><span>Related changes</span></a></li><li class="mw-list-item" id="t-upload"><a accesskey="u" href="/wiki/Wikipedia:File_Upload_Wizard" title="Upload files [u]"><span>Upload file</span></a></li><li class="mw-list-item" id="t-specialpages"><a accesskey="q" href="/wiki/Special:SpecialPages" title="A list of all special pages [q]"><span>Special pages</span></a></li><li class="mw-list-item" id="t-permalink"><a href="/w/index.php?title=Main_Page&amp;oldid=1108085777" title="Permanent link to this revision of this page"><span>Permanent link</span></a></li><li class="mw-list-item" id="t-info"><a href="/w/index.php?title=Main_Page&amp;action=info" title="More information about this page"><span>Page information</span></a></li><li class="mw-list-item" id="t-cite"><a href="/w/index.php?title=Special:CiteThisPage&amp;page=Main_Page&amp;id=1108085777&amp;wpFormIdentifier=titleform" title="Information on how to cite this page"><span>Cite this page</span></a></li><li class="mw-list-item" id="t-wikibase"><a accesskey="g" href="https://www.wikidata.org/wiki/Special:EntityPage/Q5296" title="Structured data on this page hosted by Wikidata [g]"><span>Wikidata item</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-coll-print_export-label" class="vector-menu mw-portlet mw-portlet-coll-print_export vector-menu-portal portal" id="p-coll-print_export" role="navigation">
    <h3 class="vector-menu-heading" id="p-coll-print_export-label">
    <span class="vector-menu-heading-label">Print/export</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="mw-list-item" id="coll-download-as-rl"><a href="/w/index.php?title=Special:DownloadAsPdf&amp;page=Main_Page&amp;action=show-download-screen" title="Download this page as a PDF file"><span>Download as PDF</span></a></li><li class="mw-list-item" id="t-print"><a accesskey="p" href="/w/index.php?title=Main_Page&amp;printable=yes" title="Printable version of this page [p]"><span>Printable version</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-wikibase-otherprojects-label" class="vector-menu mw-portlet mw-portlet-wikibase-otherprojects vector-menu-portal portal" id="p-wikibase-otherprojects" role="navigation">
    <h3 class="vector-menu-heading" id="p-wikibase-otherprojects-label">
    <span class="vector-menu-heading-label">In other projects</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="wb-otherproject-link wb-otherproject-commons mw-list-item"><a href="https://commons.wikimedia.org/wiki/Main_Page" hreflang="en"><span>Wikimedia Commons</span></a></li><li class="wb-otherproject-link wb-otherproject-mediawiki mw-list-item"><a href="https://www.mediawiki.org/wiki/MediaWiki" hreflang="en"><span>MediaWiki</span></a></li><li class="wb-otherproject-link wb-otherproject-meta mw-list-item"><a href="https://meta.wikimedia.org/wiki/Main_Page" hreflang="en"><span>Meta-Wiki</span></a></li><li class="wb-otherproject-link wb-otherproject-sources mw-list-item"><a href="https://wikisource.org/wiki/Main_Page" hreflang="en"><span>Multilingual Wikisource</span></a></li><li class="wb-otherproject-link wb-otherproject-species mw-list-item"><a href="https://species.wikimedia.org/wiki/Main_Page" hreflang="en"><span>Wikispecies</span></a></li><li class="wb-otherproject-link wb-otherproject-wikibooks mw-list-item"><a href="https://en.wikibooks.org/wiki/Main_Page" hreflang="en"><span>Wikibooks</span></a></li><li class="wb-otherproject-link wb-otherproject-wikidata mw-list-item"><a href="https://www.wikidata.org/wiki/Wikidata:Main_Page" hreflang="en"><span>Wikidata</span></a></li><li class="wb-otherproject-link wb-otherproject-wikimania mw-list-item"><a href="https://wikimania.wikimedia.org/wiki/2022:Wikimania" hreflang="en"><span>Wikimania</span></a></li><li class="wb-otherproject-link wb-otherproject-wikinews mw-list-item"><a href="https://en.wikinews.org/wiki/Main_Page" hreflang="en"><span>Wikinews</span></a></li><li class="wb-otherproject-link wb-otherproject-wikiquote mw-list-item"><a href="https://en.wikiquote.org/wiki/Main_Page" hreflang="en"><span>Wikiquote</span></a></li><li class="wb-otherproject-link wb-otherproject-wikisource mw-list-item"><a href="https://en.wikisource.org/wiki/Main_Page" hreflang="en"><span>Wikisource</span></a></li><li class="wb-otherproject-link wb-otherproject-wikiversity mw-list-item"><a href="https://en.wikiversity.org/wiki/Wikiversity:Main_Page" hreflang="en"><span>Wikiversity</span></a></li><li class="wb-otherproject-link wb-otherproject-wikivoyage mw-list-item"><a href="https://en.wikivoyage.org/wiki/Main_Page" hreflang="en"><span>Wikivoyage</span></a></li><li class="wb-otherproject-link wb-otherproject-wiktionary mw-list-item"><a href="https://en.wiktionary.org/wiki/Wiktionary:Main_Page" hreflang="en"><span>Wiktionary</span></a></li></ul>
    </div>
    </nav>
    <nav aria-labelledby="p-lang-label" class="vector-menu mw-portlet mw-portlet-lang vector-menu-portal portal" id="p-lang" role="navigation">
    <h3 class="vector-menu-heading" id="p-lang-label">
    <span class="vector-menu-heading-label">Languages</span>
    </h3>
    <div class="vector-menu-content">
    <ul class="vector-menu-content-list"><li class="interlanguage-link interwiki-ar mw-list-item"><a class="interlanguage-link-target" href="https://ar.wikipedia.org/wiki/" hreflang="ar" lang="ar" title="Arabic"><span>العربية</span></a></li><li class="interlanguage-link interwiki-bn mw-list-item"><a class="interlanguage-link-target" href="https://bn.wikipedia.org/wiki/" hreflang="bn" lang="bn" title="Bangla"><span>বাংলা</span></a></li><li class="interlanguage-link interwiki-bg mw-list-item"><a class="interlanguage-link-target" href="https://bg.wikipedia.org/wiki/" hreflang="bg" lang="bg" title="Bulgarian"><span>Български</span></a></li><li class="interlanguage-link interwiki-bs mw-list-item"><a class="interlanguage-link-target" href="https://bs.wikipedia.org/wiki/" hreflang="bs" lang="bs" title="Bosnian"><span>Bosanski</span></a></li><li class="interlanguage-link interwiki-ca mw-list-item"><a class="interlanguage-link-target" href="https://ca.wikipedia.org/wiki/" hreflang="ca" lang="ca" title="Catalan"><span>Català</span></a></li><li class="interlanguage-link interwiki-cs mw-list-item"><a class="interlanguage-link-target" href="https://cs.wikipedia.org/wiki/" hreflang="cs" lang="cs" title="Czech"><span>Čeština</span></a></li><li class="interlanguage-link interwiki-da mw-list-item"><a class="interlanguage-link-target" href="https://da.wikipedia.org/wiki/" hreflang="da" lang="da" title="Danish"><span>Dansk</span></a></li><li class="interlanguage-link interwiki-de mw-list-item"><a class="interlanguage-link-target" href="https://de.wikipedia.org/wiki/" hreflang="de" lang="de" title="German"><span>Deutsch</span></a></li><li class="interlanguage-link interwiki-et mw-list-item"><a class="interlanguage-link-target" href="https://et.wikipedia.org/wiki/" hreflang="et" lang="et" title="Estonian"><span>Eesti</span></a></li><li class="interlanguage-link interwiki-el mw-list-item"><a class="interlanguage-link-target" href="https://el.wikipedia.org/wiki/" hreflang="el" lang="el" title="Greek"><span>Ελληνικά</span></a></li><li class="interlanguage-link interwiki-es mw-list-item"><a class="interlanguage-link-target" href="https://es.wikipedia.org/wiki/" hreflang="es" lang="es" title="Spanish"><span>Español</span></a></li><li class="interlanguage-link interwiki-eo mw-list-item"><a class="interlanguage-link-target" href="https://eo.wikipedia.org/wiki/" hreflang="eo" lang="eo" title="Esperanto"><span>Esperanto</span></a></li><li class="interlanguage-link interwiki-eu mw-list-item"><a class="interlanguage-link-target" href="https://eu.wikipedia.org/wiki/" hreflang="eu" lang="eu" title="Basque"><span>Euskara</span></a></li><li class="interlanguage-link interwiki-fa mw-list-item"><a class="interlanguage-link-target" href="https://fa.wikipedia.org/wiki/" hreflang="fa" lang="fa" title="Persian"><span>فارسی</span></a></li><li class="interlanguage-link interwiki-fr mw-list-item"><a class="interlanguage-link-target" href="https://fr.wikipedia.org/wiki/" hreflang="fr" lang="fr" title="French"><span>Français</span></a></li><li class="interlanguage-link interwiki-gl mw-list-item"><a class="interlanguage-link-target" href="https://gl.wikipedia.org/wiki/" hreflang="gl" lang="gl" title="Galician"><span>Galego</span></a></li><li class="interlanguage-link interwiki-ko mw-list-item"><a class="interlanguage-link-target" href="https://ko.wikipedia.org/wiki/" hreflang="ko" lang="ko" title="Korean"><span>한국어</span></a></li><li class="interlanguage-link interwiki-hr mw-list-item"><a class="interlanguage-link-target" href="https://hr.wikipedia.org/wiki/" hreflang="hr" lang="hr" title="Croatian"><span>Hrvatski</span></a></li><li class="interlanguage-link interwiki-id mw-list-item"><a class="interlanguage-link-target" href="https://id.wikipedia.org/wiki/" hreflang="id" lang="id" title="Indonesian"><span>Bahasa Indonesia</span></a></li><li class="interlanguage-link interwiki-it mw-list-item"><a class="interlanguage-link-target" href="https://it.wikipedia.org/wiki/" hreflang="it" lang="it" title="Italian"><span>Italiano</span></a></li><li class="interlanguage-link interwiki-he mw-list-item"><a class="interlanguage-link-target" href="https://he.wikipedia.org/wiki/" hreflang="he" lang="he" title="Hebrew"><span>עברית</span></a></li><li class="interlanguage-link interwiki-ka mw-list-item"><a class="interlanguage-link-target" href="https://ka.wikipedia.org/wiki/" hreflang="ka" lang="ka" title="Georgian"><span>ქართული</span></a></li><li class="interlanguage-link interwiki-lv mw-list-item"><a class="interlanguage-link-target" href="https://lv.wikipedia.org/wiki/" hreflang="lv" lang="lv" title="Latvian"><span>Latviešu</span></a></li><li class="interlanguage-link interwiki-lt mw-list-item"><a class="interlanguage-link-target" href="https://lt.wikipedia.org/wiki/" hreflang="lt" lang="lt" title="Lithuanian"><span>Lietuvių</span></a></li><li class="interlanguage-link interwiki-hu mw-list-item"><a class="interlanguage-link-target" href="https://hu.wikipedia.org/wiki/" hreflang="hu" lang="hu" title="Hungarian"><span>Magyar</span></a></li><li class="interlanguage-link interwiki-mk mw-list-item"><a class="interlanguage-link-target" href="https://mk.wikipedia.org/wiki/" hreflang="mk" lang="mk" title="Macedonian"><span>Македонски</span></a></li><li class="interlanguage-link interwiki-ms mw-list-item"><a class="interlanguage-link-target" href="https://ms.wikipedia.org/wiki/" hreflang="ms" lang="ms" title="Malay"><span>Bahasa Melayu</span></a></li><li class="interlanguage-link interwiki-nl mw-list-item"><a class="interlanguage-link-target" href="https://nl.wikipedia.org/wiki/" hreflang="nl" lang="nl" title="Dutch"><span>Nederlands</span></a></li><li class="interlanguage-link interwiki-ja mw-list-item"><a class="interlanguage-link-target" href="https://ja.wikipedia.org/wiki/" hreflang="ja" lang="ja" title="Japanese"><span>日本語</span></a></li><li class="interlanguage-link interwiki-no mw-list-item"><a class="interlanguage-link-target" href="https://no.wikipedia.org/wiki/" hreflang="nb" lang="nb" title="Norwegian Bokmål"><span>Norsk bokmål</span></a></li><li class="interlanguage-link interwiki-nn mw-list-item"><a class="interlanguage-link-target" href="https://nn.wikipedia.org/wiki/" hreflang="nn" lang="nn" title="Norwegian Nynorsk"><span>Norsk nynorsk</span></a></li><li class="interlanguage-link interwiki-pl mw-list-item"><a class="interlanguage-link-target" href="https://pl.wikipedia.org/wiki/" hreflang="pl" lang="pl" title="Polish"><span>Polski</span></a></li><li class="interlanguage-link interwiki-pt mw-list-item"><a class="interlanguage-link-target" href="https://pt.wikipedia.org/wiki/" hreflang="pt" lang="pt" title="Portuguese"><span>Português</span></a></li><li class="interlanguage-link interwiki-ro mw-list-item"><a class="interlanguage-link-target" href="https://ro.wikipedia.org/wiki/" hreflang="ro" lang="ro" title="Romanian"><span>Română</span></a></li><li class="interlanguage-link interwiki-ru mw-list-item"><a class="interlanguage-link-target" href="https://ru.wikipedia.org/wiki/" hreflang="ru" lang="ru" title="Russian"><span>Русский</span></a></li><li class="interlanguage-link interwiki-simple mw-list-item"><a class="interlanguage-link-target" href="https://simple.wikipedia.org/wiki/" hreflang="en-simple" lang="en-simple" title="Simple English"><span>Simple English</span></a></li><li class="interlanguage-link interwiki-sk mw-list-item"><a class="interlanguage-link-target" href="https://sk.wikipedia.org/wiki/" hreflang="sk" lang="sk" title="Slovak"><span>Slovenčina</span></a></li><li class="interlanguage-link interwiki-sl mw-list-item"><a class="interlanguage-link-target" href="https://sl.wikipedia.org/wiki/" hreflang="sl" lang="sl" title="Slovenian"><span>Slovenščina</span></a></li><li class="interlanguage-link interwiki-sr mw-list-item"><a class="interlanguage-link-target" href="https://sr.wikipedia.org/wiki/" hreflang="sr" lang="sr" title="Serbian"><span>Српски / srpski</span></a></li><li class="interlanguage-link interwiki-sh mw-list-item"><a class="interlanguage-link-target" href="https://sh.wikipedia.org/wiki/" hreflang="sh" lang="sh" title="Serbo-Croatian"><span>Srpskohrvatski / српскохрватски</span></a></li><li class="interlanguage-link interwiki-fi mw-list-item"><a class="interlanguage-link-target" href="https://fi.wikipedia.org/wiki/" hreflang="fi" lang="fi" title="Finnish"><span>Suomi</span></a></li><li class="interlanguage-link interwiki-sv mw-list-item"><a class="interlanguage-link-target" href="https://sv.wikipedia.org/wiki/" hreflang="sv" lang="sv" title="Swedish"><span>Svenska</span></a></li><li class="interlanguage-link interwiki-th mw-list-item"><a class="interlanguage-link-target" href="https://th.wikipedia.org/wiki/" hreflang="th" lang="th" title="Thai"><span>ไทย</span></a></li><li class="interlanguage-link interwiki-tr mw-list-item"><a class="interlanguage-link-target" href="https://tr.wikipedia.org/wiki/" hreflang="tr" lang="tr" title="Turkish"><span>Türkçe</span></a></li><li class="interlanguage-link interwiki-uk mw-list-item"><a class="interlanguage-link-target" href="https://uk.wikipedia.org/wiki/" hreflang="uk" lang="uk" title="Ukrainian"><span>Українська</span></a></li><li class="interlanguage-link interwiki-vi mw-list-item"><a class="interlanguage-link-target" href="https://vi.wikipedia.org/wiki/" hreflang="vi" lang="vi" title="Vietnamese"><span>Tiếng Việt</span></a></li><li class="interlanguage-link interwiki-zh mw-list-item"><a class="interlanguage-link-target" href="https://zh.wikipedia.org/wiki/" hreflang="zh" lang="zh" title="Chinese"><span>中文</span></a></li></ul>
    </div>
    </nav>
    </div>
    </div>
    <footer class="mw-footer" id="footer" role="contentinfo">
    <ul id="footer-info">
    <li id="footer-info-lastmod"> This page was last edited on 2 September 2022, at 13:05<span class="anonymous-show"> (UTC)</span>.</li>
    <li id="footer-info-copyright">Text is available under the <a href="//en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" rel="license">Creative Commons Attribution-ShareAlike License 3.0</a><a href="//creativecommons.org/licenses/by-sa/3.0/" rel="license" style="display:none;"></a>;
    additional terms may apply.  By using this site, you agree to the <a href="//foundation.wikimedia.org/wiki/Terms_of_Use">Terms of Use</a> and <a href="//foundation.wikimedia.org/wiki/Privacy_policy">Privacy Policy</a>. Wikipedia® is a registered trademark of the <a href="//www.wikimediafoundation.org/">Wikimedia Foundation, Inc.</a>, a non-profit organization.</li>
    </ul>
    <ul id="footer-places">
    <li id="footer-places-privacy"><a href="https://foundation.wikimedia.org/wiki/Privacy_policy">Privacy policy</a></li>
    <li id="footer-places-about"><a href="/wiki/Wikipedia:About">About Wikipedia</a></li>
    <li id="footer-places-disclaimer"><a href="/wiki/Wikipedia:General_disclaimer">Disclaimers</a></li>
    <li id="footer-places-contact"><a href="//en.wikipedia.org/wiki/Wikipedia:Contact_us">Contact Wikipedia</a></li>
    <li id="footer-places-mobileview"><a class="noprint stopMobileRedirectToggle" href="//en.m.wikipedia.org/w/index.php?title=Main_Page&amp;mobileaction=toggle_view_mobile">Mobile view</a></li>
    <li id="footer-places-developers"><a href="https://developer.wikimedia.org">Developers</a></li>
    <li id="footer-places-statslink"><a href="https://stats.wikimedia.org/#/en.wikipedia.org">Statistics</a></li>
    <li id="footer-places-cookiestatement"><a href="https://foundation.wikimedia.org/wiki/Cookie_statement">Cookie statement</a></li>
    </ul>
    <ul class="noprint" id="footer-icons">
    <li id="footer-copyrightico"><a href="https://wikimediafoundation.org/"><img alt="Wikimedia Foundation" height="31" loading="lazy" src="/static/images/footer/wikimedia-button.png" srcset="/static/images/footer/wikimedia-button-1.5x.png 1.5x, /static/images/footer/wikimedia-button-2x.png 2x" width="88"/></a></li>
    <li id="footer-poweredbyico"><a href="https://www.mediawiki.org/"><img alt="Powered by MediaWiki" height="31" loading="lazy" src="/static/images/footer/poweredby_mediawiki_88x31.png" srcset="/static/images/footer/poweredby_mediawiki_132x47.png 1.5x, /static/images/footer/poweredby_mediawiki_176x62.png 2x" width="88"/></a></li>
    </ul>
    </footer>
    <script>(RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgPageParseReport":{"limitreport":{"cputime":"0.277","walltime":"0.359","ppvisitednodes":{"value":3929,"limit":1000000},"postexpandincludesize":{"value":114189,"limit":2097152},"templateargumentsize":{"value":9535,"limit":2097152},"expansiondepth":{"value":18,"limit":100},"expensivefunctioncount":{"value":13,"limit":500},"unstrip-depth":{"value":0,"limit":20},"unstrip-size":{"value":3979,"limit":5000000},"entityaccesscount":{"value":0,"limit":400},"timingprofile":["100.00%  227.766      1 -total"," 38.28%   87.187      1 Wikipedia:Main_Page/Tomorrow"," 29.58%   67.370      8 Template:Main_page_image"," 20.72%   47.202      8 Template:Str_number/trim"," 18.44%   42.010      1 Wikipedia:Today's_featured_article/September_24,_2022"," 17.63%   40.157      1 Template:Did_you_know/Queue/2"," 17.01%   38.753      2 Template:Main_page_image/TFA"," 15.43%   35.135      2 Template:Wikipedia_languages"," 14.40%   32.808      1 Template:DYKbotdo"," 13.59%   30.960      1 Template:Mbox"]},"scribunto":{"limitreport-timeusage":{"value":"0.054","limit":"10.000"},"limitreport-memusage":{"value":1871375,"limit":52428800}},"cachereport":{"origin":"mw1429","timestamp":"20220924113114","ttl":3600,"transientcontent":true}}});});</script>
    <script type="application/ld+json">{"@context":"https:\/\/schema.org","@type":"Article","name":"Main Page","url":"https:\/\/en.wikipedia.org\/wiki\/Main_Page","sameAs":"http:\/\/www.wikidata.org\/entity\/Q5296","mainEntity":"http:\/\/www.wikidata.org\/entity\/Q5296","author":{"@type":"Organization","name":"Contributors to Wikimedia projects"},"publisher":{"@type":"Organization","name":"Wikimedia Foundation, Inc.","logo":{"@type":"ImageObject","url":"https:\/\/www.wikimedia.org\/static\/images\/wmf-hor-googpub.png"}},"datePublished":"2002-01-26T15:28:12Z","dateModified":"2022-09-02T13:05:06Z","image":"https:\/\/upload.wikimedia.org\/wikipedia\/commons\/0\/0f\/Sawmill_fire_arizona_20171024_1.jpg","headline":"Wikimedia project page"}</script><script type="application/ld+json">{"@context":"https:\/\/schema.org","@type":"Article","name":"Main Page","url":"https:\/\/en.wikipedia.org\/wiki\/Main_Page","sameAs":"http:\/\/www.wikidata.org\/entity\/Q5296","mainEntity":"http:\/\/www.wikidata.org\/entity\/Q5296","author":{"@type":"Organization","name":"Contributors to Wikimedia projects"},"publisher":{"@type":"Organization","name":"Wikimedia Foundation, Inc.","logo":{"@type":"ImageObject","url":"https:\/\/www.wikimedia.org\/static\/images\/wmf-hor-googpub.png"}},"datePublished":"2002-01-26T15:28:12Z","dateModified":"2022-09-02T13:05:06Z","image":"https:\/\/upload.wikimedia.org\/wikipedia\/commons\/0\/0f\/Sawmill_fire_arizona_20171024_1.jpg","headline":"Wikimedia project page"}</script>
    <script>(RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgBackendResponseTime":131,"wgHostname":"mw1319"});});</script>
    </body>
    </html>
    


```python
print(type(soup))
print(soup.prettify()[0:1000])
```

    <class 'bs4.BeautifulSoup'>
    <!DOCTYPE html>
    <html class="client-nojs" dir="ltr" lang="en">
     <head>
      <meta charset="utf-8"/>
      <title>
       Wikipedia, the free encyclopedia
      </title>
      <script>
       document.documentElement.className="client-js";RLCONF={"wgBreakFrames":false,"wgSeparatorTransformTable":["",""],"wgDigitTransformTable":["",""],"wgDefaultDateFormat":"dmy","wgMonthNames":["","January","February","March","April","May","June","July","August","September","October","November","December"],"wgRequestId":"ee933970-ac45-4871-ae1f-7d07362e511f","wgCSPNonce":false,"wgCanonicalNamespace":"","wgCanonicalSpecialPageName":false,"wgNamespaceNumber":0,"wgPageName":"Main_Page","wgTitle":"Main Page","wgCurRevisionId":1108085777,"wgRevisionId":1108085777,"wgArticleId":15580374,"wgIsArticle":true,"wgIsRedirect":false,"wgAction":"view","wgUserName":null,"wgUserGroups":["*"],"wgCategories":[],"wgPageContentLanguage":"en","wgPageContentModel":"wikitext","wgRelevantPageName":"Main_Page","wgRelevantArticleId":15580374,"wgIsProb
    

The text is structured with HTML tags organised in a systematic way. This structure is referred to as a *HTML DOM* (Document Object Model).

We are interested grabbing the summary of article of the day, from the Wikipedia main page along with all the links in it. So we'll use `BeautifulSoup`'s functions to parse HTML tags that contain this information or lead us to it in the form of embedded URLs.

On inspection of the HTML source we identify that the article summary is enclosed in the tag table with the attribute `id = mp-upper`. The idea is as follows,

1.   Using the function in `.find_all()` in BeautifulSoup we search for all tags of the type `table`
2.   Search the returned list for all the tag that has an attribute `id = mp-upper`.
3.   Now that you have the table notice that the information needed is in the tag called `p`. Search for all tags `p`
4.  Identify the correct paragraph and extract URLs.
5.  Extract text.




```python
# 1. Look for all tags called table
tables_list = soup.find_all(name = "table")
tables_list
```

    [<table role="presentation" style="margin:0 3px 3px; width:100%; box-sizing:border-box; text-align:left; background-color:transparent; border-collapse:collapse;">
     <tbody><tr>
     <td style="padding:0 0.9em 0 0; width:400px;"><a class="image" href="/wiki/File:Brown-headed_Honeyeater_-_Patchewollock.jpg" title="Brown-headed honeyeater"><img alt="Brown-headed honeyeater" data-file-height="2841" data-file-width="4261" decoding="async" height="267" src="//upload.wikimedia.org/wikipedia/commons/thumb/0/02/Brown-headed_Honeyeater_-_Patchewollock.jpg/400px-Brown-headed_Honeyeater_-_Patchewollock.jpg" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/0/02/Brown-headed_Honeyeater_-_Patchewollock.jpg/600px-Brown-headed_Honeyeater_-_Patchewollock.jpg 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/0/02/Brown-headed_Honeyeater_-_Patchewollock.jpg/800px-Brown-headed_Honeyeater_-_Patchewollock.jpg 2x" width="400"/></a>
     </td>
     <td style="padding:0 6px 0 0">
     <p>The <b><a href="/wiki/Brown-headed_honeyeater" title="Brown-headed honeyeater">brown-headed honeyeater</a></b> (<i>Melithreptus brevirostris</i>) is a species of <a href="/wiki/Passerine" title="Passerine">passerine</a> bird in the family Meliphagidae, the <a href="/wiki/Honeyeater" title="Honeyeater">honeyeaters</a>. <a href="/wiki/Endemism" title="Endemism">Endemic</a> to Australia, its natural <a href="/wiki/Habitat" title="Habitat">habitats</a> are temperate forests and Mediterranean-type shrubby vegetation. Insects form the bulk of its diet, and like its close relatives, the <a href="/wiki/Black-chinned_honeyeater" title="Black-chinned honeyeater">black-chinned</a> and <a href="/wiki/Strong-billed_honeyeater" title="Strong-billed honeyeater">strong-billed honeyeaters</a>, it forages by probing in the bark of tree trunks and branches. This brown-headed honeyeater was photographed perching on a branch in <a href="/wiki/Patchewollock" title="Patchewollock">Patchewollock</a>, Victoria.
     </p>
     <p style="text-align:left;"><small>Photograph credit: <a href="/wiki/User:JJ_Harrison" title="User:JJ Harrison">John Harrison</a></small></p>
     <div class="potd-recent" style="text-align:right;">
     Recently featured: <div class="hlist hlist-separated inline">
     <ul><li><a href="/wiki/Template:POTD/2022-09-23" title="Template:POTD/2022-09-23">Lichfield Cathedral</a></li>
     <li><a href="/wiki/Template:POTD/2022-09-22" title="Template:POTD/2022-09-22">Onésime Reclus</a></li>
     <li><a href="/wiki/Template:POTD/2022-09-21" title="Template:POTD/2022-09-21"><i>Pantala flavescens</i></a></li></ul>
     </div></div>
     <div class="hlist hlist-separated potd-footer noprint" style="text-align:right;">
     <ul><li><b><a href="/wiki/Wikipedia:Picture_of_the_day/Archive" title="Wikipedia:Picture of the day/Archive">Archive</a></b></li>
     <li><b><a href="/wiki/Wikipedia:Featured_pictures" title="Wikipedia:Featured pictures">More featured pictures</a></b></li></ul>
     </div>
     </td></tr></tbody></table>]




```python
# 2. Search for the table of interest by matching on the key 'id'
for table in tables_list:
    try:
        if table['id'] == 'mp-upper':
            article_table = table
    except:
        None

print('object type of "article_table" is ' + str(type(article_table)) + '\n')
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [111], in <cell line: 9>()
          6     except:
          7         None
    ----> 9 print('object type of "article_table" is ' + str(type(article_table)) + '\n')
    

    NameError: name 'article_table' is not defined



```python
# 3. Search for the tag "p"
paragraph = article_table.findAll(name = 'p')[0]
paragraph
```


```python
# 4. Extract URLs
urls = [tag['href'] for tag in paragraph.findAll('a', href = True)]
for url in urls:
    print(url)

print('\n')
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [112], in <cell line: 2>()
          1 # 4. Extract URLs
    ----> 2 urls = [tag['href'] for tag in paragraph.findAll('a', href = True)]
          3 for url in urls:
          4     print(url)
    

    NameError: name 'paragraph' is not defined



```python
# 5. Build the text of the article summary by looping through all the children and concatinating the text 
text = ''
for ch in paragraph.children:
    text = text + ch.string

print(text + '\n')
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [113], in <cell line: 3>()
          1 # 5. Build the text of the article summary by looping through all the children and concatinating the text 
          2 text = ''
    ----> 3 for ch in paragraph.children:
          4     text = text + ch.string
          6 print(text + '\n')
    

    NameError: name 'paragraph' is not defined


