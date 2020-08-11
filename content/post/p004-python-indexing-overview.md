---
title: "Indexing in Python"
date: 2020-08-07
description: "A systematic overview of the indexing strategies of base python, NumPy and Pandas"
type: "post"
tags: [Python]
draft: true
---

There are numerous ways to index in Pandas. This can lead to some confusion when starting to learn Pandas (and NumPy). This post aims to give a highly simplified strucure for thinking about indexing in Python, NumPy and Pandas:

- Standard python provides convenient indexing for one dimensional datasets.
- NumPy extends this syntax to higher dimensions.
- Pandas allows you to attach custom labels to your data and index using these labels.

Understanding this structure should hopefully allow you to learn about the rich sets of index-related features without getting too lost.

### Standard Python

In normal Python, there is a convenient notation for indexing sequence objects (such as lists). This notation takes the form x[start:stop:step] where x is a sequence object (like a list). 

- start is the index of the first element to extract (start = 0 selects the first element of the sequence object). Default is zero.
- stop is the index of the last element to extract. Default is the last element in the sequence objects.
- step is what size steps to take between start and stop. 1 (the default) will select every element. 2 will select every second element and so forth.

This allows you to easily perform operations like: 

"Retrieve every 2nd element starting from the 10th"

```
dat = list(range(30))
print (dat[10::2])
```

In data analysis one often works with 2-d dataset and want to select blocks of the dataset.

```
dat = [[chr(i+90), i+10, i*2] for i in range(5)]
```

Produces a dataset like: (without the headers)

| Letter | Number1 | Number2 |
| -      | ---     | --      |
| P      | 10      | 0       |
| Q      | 11      | 2       |
| R      | 12      | 4       |
| S      | 13      | 6       |
| T      | 14      | 8       |

The standard Python indexing allows you to easily select a row, or a few columns from a row: 

```
dat[2]
dat[2][1:]
```

It is not as convenient for selecting columns:

```
[j[2] for j in dat]
```

Note:

- The `start:stop:step` notation creates a slice objects.
- There are alternative indexing strategies, such as indexing with a tuple of integers. This will be discussed in future posts.

### NumPy

NumPy introduces the array, and ways of indexing the array that is more convenient for data analysis. While everything discussed here extends elegantly into higher dimensions, I will restrict this post to two dimensional data.

```
import numpy as np
dat = np.random.randint(low = 0, high = 9, size = (10,4))
```

This creates an array with 10 rows and 4 columns of random integers between 0 and 9.

There are many convenient ways to index this array, including a straight forward extension of the Python indexing described above:

```
dat[1::2, 1:3]
```

The `1::2` part of the index selects out every second row starting at the second row (row 0 = the first row). The `1:3` part selects out the second and 3rd columns (column 0 = the first column) of the dataset.

Thus NumPy allows you to specify indexing details for the different dimensions by separating them with a comma inside the square brackets.

Note: Just as for standard python, the indexing is much more flexible and powerful than just this simple example. So if you experience strange behaviour, your syntax probably match one of the other indexing strategies available for NumPy.

### Pandas

While NumPy extends the standard Python indexing to multiple dimensions, Pandas adds a useful feature to indexing. It enables the specification of custom labels for rows and column, and the use of these labels when selecting data from the dataset.

In general, indexing in NumPy requires that you know the location inside the dataset of the data that interests you. You need to know the row and column numbers that correspond to the subset of that data you want to extract. Pandas adds a layer of convenience allowing you to, for example, label each row with a date and then extract all rows with data from the year 2005:

```
timedeltas = pd.to_timedelta([150*i for i in range(10)], unit = 'D'))
dat = pd.Series(np.random.random(10), 
                index = pd.to_datetime('2003') + timedeltas)
print(dat)
```

Output:

    2003-01-01    0.284773
    2003-05-31    0.645078
    2003-10-28    0.747747
    2004-03-26    0.722569
    2004-08-23    0.639060
    2005-01-20    0.883133
    2005-06-19    0.554248
    2005-11-16    0.140964
    2006-04-15    0.896328
    2006-09-12    0.614332
    dtype: float64

```
dat.loc['2005']
```

*Pay attention to the `.loc` suffix*

By using `.iloc' you can access Pandas data objects using NumPy indexing syntax.

Pandas supports many other indexing strategies, which will be discussed later.

### Summary



### Pandas

The following needs to be teased apart:

- Standard Python vs NumPy Indexing vs Pandas Indexing
- Using the labels in the index(es) (.loc)
- Using integer locations (.iloc)
- Using Boolean arrays or comparison operations (also .loc)

### Standard Python vs NumPy vs Pandas Indexing

### Using the labels in the index(ex)

### Using Integer locations

### Using Boolean arrays or comparison operations

#### More details related to .loc Pandas indexing - move to different post.

For now, I believe the best ways to index are:

- Always use .loc[] or .query()
- Really, always use .loc.
- It seems like you can access MultiIndex levels (for rows) with .query as if they are normal columns.
- : confuses .loc, so use slice(None) instead (1 exception, listed below).
- The format for .loc is:
  - 1 element that is a tuple for a Series object and 2 elements that are each tuples for a DataFrame
  - Each element of the tuple corresponds with one level of the MultiIndex. In case of DataFrames, the elements of the first [0] tuple correspond with the row MultiIndex and the elements of the second [1] tuple corresponds with the column MultiIndex.
  - For DataFrames, the second tuple can be replaced by : to select all columns.
  - To specify a single value from an index level, set the corresponding element for the corresponding tuple equal to that value.
  - To specify a set of values from an index level, set the corresponding element for the corresponding tuple equal to a list that contains all the values of interest.
  - To select ALL values of an index level, set the corresponding element in the tuple equal to slice(None).
- Are you sure that you are using .loc?
