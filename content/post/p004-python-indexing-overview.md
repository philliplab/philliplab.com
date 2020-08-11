---
title: "Indexing in Python"
date: 2020-08-07
description: "A systematic overview of the indexing strategies of base python, NumPy and Pandas"
type: "post"
tags: [Python]
draft: true
---

There are numerous ways to index in Pandas. This can lead to some confusion when starting to learn Pandas (and NumPy). This post aims to give a high level overview of the different indexing strategies in python, NumPy and Pandas. Future posts will explore the details of each.

### Standard Python

In normal Python, there is a convenient notation for indexing sequence objects. This notation takes the form x[start:stop:step] where x is a sequence object (like a list). This allows you to easily perform operations like: 

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

The standard Python indexing allows you to easily select a row, and a few columns from a row: 

```
dat[2]
dat[2][1:]
```

It is not a convenient for selecting columns:

```
[j[2] for j in dat]
```

### NumPy

NumPy introduces the array, and ways of indexing the array what is convenient for data analysis. While everything discussed here extends elegantly into higher dimensions, I will restrict this post to two dimensional data.

```
import numpy as np
dat = np.random.randint(low = 0, high = 9, size = (10,4))
```

This creates an array with 10 rows and 4 columns of random integers between 0 and 9.

There are many convenient ways to index this array, including a straight forward extension of the Python indexing described above:

```
dat[1::2, 1:3]
```

The `1::2` part of the index selects out every second row (row 0 = the first row) starting at the second row. The `1:3` part selects out the second and 3rd columns (column 0 = the first column) of the dataset.

Thus NumPy allows you to specify indexing details for the different dimensions by separating them with a comma inside the square barckets.

Warning: The indexing is much more flexible and powerful than just this simple example. So if you experience strange behaviour, your syntax probably match one of the other indexing strategies available for NumPy.

### Pandas



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
