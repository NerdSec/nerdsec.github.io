---
layout: post
title: "Python Fundamentals"
categories: ["Beginner's Guide"]
tags: Python
---
Python is one of the most flexible and versatile programming languages. This post is to give you a crash course in python. It is is no way a comprehensive overview of Python, but rather a quick-starter that will help you in following some of the other content on this blog.

This course will take you through the basic topics in the following order:

* [Data Types](#datatypes)
    * Numbers
    * Variables
    * Strings
    * Printing
    * Lists
    * Dictionaries
    * Booleans
    * Tuples
    * Sets
* [Comparison Operators](#comparison-operators)
* [Condition Statements](#condition-statements)
    * ifelse
    * for Loops
    * while Loops
    * range()
* [List comprehension](#list-comprehension)
* [Functions](#functions)
* [Lambda expressions](#lambda-expressions)
* [Map and filter](#map-and-filter)
* [Methods](#methods)

## Data Types

### Numbers
Python supports adding numbers quite naturally. It is extremely natural and can be done in multiple ways. Let's go though some basics.

```python
#Addition
1 + 1
#Output
2

#Multiplication
1 * 3
#Output:
3

#Division
1 / 2
#Output
0.5

#Raised To
2 ** 4
#Output
16

#Modulo Operation
4 % 2
#Output
0

#Modulo Operation
5 % 2
#Output
1
```

You can also combine multiple requests, and perform them at once as follows:

```python
(2 + 3) * (5 + 5)
#Output
50
```


### Variables

Python supports natural variable assignment. No python code needs to be terminated with a semi-colon or colon. The creators of python believed in making it more fluid and natural for it's users to understand. Assignment is usually done in a `=` sign, as follows:

```python
# Can not start with number or special characters
name_of_var = 2
x = 2
y = 3
z = x + y
```

Operations can be performed and the output can directly be assigned to variables.
>One important thing to note in the above example is the fact that the variables were initialized without specifying a data type.

```python
z
#Output
5
```

### Strings

Assignment of words is done using strings.
```python
'single quotes'
```

```python
"double quotes"
```

```python
" wrap lot's of other quotes"
```
All the mentioned lines will result in a string being created. You can assign these lines using the `=` operator, as mentioned earlier.

### Printing

Suppose if you need to print the variable and view the output on 'stdout'. This can be achived using the *print* command.

```python
x = 'hello'
```


```python
print(x)
#Output
hello
```


```python
num = 12
name = 'Sam'
print('My number is: {one}, and my name is: {two}'.format(one=num,two=name))

#Output
My number is: 12, and my name is: Sam
```


```python
print('My number is: {}, and my name is: {}'.format(num,name))
#Output
My number is: 12, and my name is: Sam
```

### Lists

Python has a specific datatype called list. This is very useful to deal with sequential data or a relative dataset.

```python
[1,2,3]
#Output
[1, 2, 3]
```

Lists can also be nested as follows:

```python
['hi',1,[1,2]]
#Output
['hi', 1, [1, 2]]
```

```python
my_list = ['a','b','c']
my_list.append('d')
my_list

#Output
['a', 'b', 'c', 'd']
```

```python
my_list[0]
#Output
'a'
```



```python
my_list[1]
#Output
'b'
```



```python
my_list[1:]
#Output
['b', 'c', 'd']

my_list[:1]
#Output
['a']

my_list[0] = 'NEW'
my_list
#Output
['NEW', 'b', 'c', 'd']
```


```python
nest = [1,2,3,[4,5,['target']]]
nest[3]
#Output
[4, 5, ['target']]
```

> Nested lists can have more than 1 pointers used together

```python
nest[3][2]
#Output
['target']
```



```python
nest[3][2][0]
#Output
'target'
```


### Dictionaries

Another useful datatype in Python is the Dictionary type. Usually, this is used to deal with *Key-Value pairs*.

```python
d = {'key1':'item1','key2':'item2'}
d
#Output
{'key1': 'item1', 'key2': 'item2'}


d['key1']
#Output
'item1'
```


### Booleans

Like any bool type, it can either be set to *True* or *False*.

```python
x=True
print(x)
#Output
True
```

### Tuples

Tuples, are very similar to lists. However, a fundamental difference in tuples is that
> they are immutable.

```python
t = (1,2,3)
t[0]
#Output
1


t[0] = 'NEW'
#Output
    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-44-97e4e33b36c2> in <module>()
    ----> 1 t[0] = 'NEW'


    TypeError: 'tuple' object does not support item assignment

```

> Values once assigned to a tuple cannot be altered.

### Sets

A set is a unique data set. It does not hold duplicate values.

```python
{1,2,3}
#Output
{1, 2, 3}

{1,2,3,1,2,1,2,3,3,3,3,2,2,2,1,1,2}
#Output
{1, 2, 3}
```

## Comparison Operators


```python
1 > 2
#Output
False

1 < 2
#Output
True

1 >= 1
#Output
True

1 <= 4
#Output
True

1 == 1
#Output
True

'hi' == 'bye'
#Output
False
```


## Logic Operators


```python
(1 > 2) and (2 < 3)
#Output
False

(1 > 2) or (2 < 3)
#Output
True


(1 == 2) or (2 == 3) or (4 == 4)
#Output
True
```

## Condition Operators

Condition statements in python help in deciding the workflow of the program. They enable us to take decisions and process the data the way we want it to. Let's see the basic conditions supported by python.

> Python is very sensitive to indentation. Always ensure that your code is indented properly, so that the python binary understands and interprets the code blocks correctly.:octocat:

### if,elif, else Statements


```python
if 1 < 2:
    print('Yep!')
```

    Yep!



```python
if 1 < 2:
    print('yep!')
```

    yep!



```python
if 1 < 2:
    print('first')
else:
    print('last')
```

    first



```python
if 1 > 2:
    print('first')
else:
    print('last')
```

    last



```python
if 1 == 2:
    print('first')
elif 3 == 3:
    print('middle')
else:
    print('Last')
```

    middle


### for Loops


```python
seq = [1,2,3,4,5]
```


```python
for item in seq:
    print(item)
```

    1
    2
    3
    4
    5



```python
for item in seq:
    print('Yep')
```

    Yep
    Yep
    Yep
    Yep
    Yep



```python
for jelly in seq:
    print(jelly+jelly)
```

    2
    4
    6
    8
    10


### while Loops


```python
i = 1
while i < 5:
    print('i is: {}'.format(i))
    i = i+1
```

    i is: 1
    i is: 2
    i is: 3
    i is: 4


### range()


```python
range(5)
```




    range(0, 5)




```python
for i in range(5):
    print(i)
```

    0
    1
    2
    3
    4



```python
list(range(5))
```




    [0, 1, 2, 3, 4]



## List comprehension


```python
x = [1,2,3,4]
```


```python
out = []
for item in x:
    out.append(item**2)
print(out)
```

    [1, 4, 9, 16]



```python
[item**2 for item in x]
```




    [1, 4, 9, 16]



## Functions


```python
def my_func(param1='default'):
    """
    Docstring goes here.
    """
    print(param1)
```


```python
my_func
```




    <function __main__.my_func>




```python
my_func()
```

    default



```python
my_func('new param')
```

    new param



```python
my_func(param1='new param')
```

    new param



```python
def square(x):
    return x**2
```


```python
out = square(2)
```


```python
print(out)
```

    4


## Lambda expressions


```python
def times2(var):
    return var*2
```


```python
times2(2)
```




    4




```python
lambda var: var*2
```




    <function __main__.<lambda>>



## Map and filter


```python
seq = [1,2,3,4,5]
```


```python
map(times2,seq)
```




    <map at 0x105316748>




```python
list(map(times2,seq))
```




    [2, 4, 6, 8, 10]




```python
list(map(lambda var: var*2,seq))
```




    [2, 4, 6, 8, 10]




```python
filter(lambda item: item%2 == 0,seq)
```




    <filter at 0x105316ac8>




```python
list(filter(lambda item: item%2 == 0,seq))
```




    [2, 4]



## Methods


```python
st = 'hello my name is Sam'
```


```python
st.lower()
```




    'hello my name is sam'




```python
st.upper()
```




    'HELLO MY NAME IS SAM'




```python
st.split()
```




    ['hello', 'my', 'name', 'is', 'Sam']




```python
tweet = 'Go Sports! #Sports'
```


```python
tweet.split('#')
```




    ['Go Sports! ', 'Sports']




```python
tweet.split('#')[1]
```




    'Sports'




```python
d
```




    {'key1': 'item1', 'key2': 'item2'}




```python
d.keys()
```




    dict_keys(['key2', 'key1'])




```python
d.items()
```




    dict_items([('key2', 'item2'), ('key1', 'item1')])




```python
lst = [1,2,3]
```


```python
lst.pop()
```




    3




```python
lst
```




    [1, 2]




```python
'x' in [1,2,3]
```




    False




```python
'x' in ['x','y','z']
```




    True



# Great Job!
