---
title: "Indexing in Python"
date: 2020-08-07
description: "A systematic overview of the indexing strategies of base python, NumPy and Pandas"
type: "post"
tags: [Python]
draft: true
---

There are numerous ways to index (extract subsets of data) in Python, NumPy and Pandas, producing to some confusion when starting to learn about NumPy and Pandas. This post aims to give a highly simplified strucure for thinking about indexing:

- Standard python provides convenient indexing for one dimensional datasets.
- NumPy extends this syntax to higher dimensions.
- Pandas allows you to attach custom labels to your data and index using these labels.

Understanding this structure should hopefully allow you to learn about the rich sets of index-related features without getting too lost.

### Standard Python

Standard Python, provides a basic notation for indexing sequence objects (such as lists). This notation takes the form `x[start:stop:step]` where `x` is a sequence object (like a list). 

- `start` is the index of the first element to extract (start = 0 selects the first element of the sequence object). Default is zero.
- `stop` is the index of the last element to extract. Default is the last element in the sequence objects.
- `step` is what size steps to take between start and stop. 1 (the default) will select every element. 2 will select every second element and so forth.

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

Note:
- The `start:stop:step` notation creates a slice objects.
- There are alternative indexing strategies, such as indexing with a tuple of integers instead of slice objects - which has different syntax.

### Pandas

While NumPy extends the standard Python indexing to multiple dimensions, Pandas adds a useful feature to indexing. It enables the specification of custom labels for rows and column, and the use of these labels when selecting data from the dataset.

In general, indexing in NumPy requires that you know the location inside the dataset of the data that interests you. You need to know the row and column numbers that correspond to the subset of that data you want to extract. Pandas adds a layer of convenience allowing you to, for example, label each row with a date and then extract all rows with data from the year 2005:

```
timedeltas = pd.to_timedelta([150*i for i in range(10)], unit = 'D'))
dat = pd.Series(np.random.random(10), 
                index = pd.to_datetime('2003') + timedeltas)
print(dat.loc['2005'])
```

Output:

    2005-01-20    0.883133
    2005-06-19    0.554248
    2005-11-16    0.140964
    dtype: float64

__Pay attention to the `.loc` suffix__

By using `.iloc` you can access Pandas data objects using NumPy indexing syntax.

Pandas supports many other indexing strategies, which will be discussed later.

### Summary

This post outlines a very simple structure relating indexing strategies in Pandas, NumPy and standard python to each other. In order to create such a simplified overview, it ignored many of the features, a few notable ommissions below:

- You can use comparison operators to generate Boolean arrays and index with these arrays.
- In Pandas, you can index without using the .loc or .iloc suffixes - the behaviour of this syntax is complicated.
- In Pandas, you can have a hierarchy of labels. (MultiIndex)
- Fancy Indexing: Indexing with arrays of integers specifying locations.

Hopefully, future posts will explore these topics.

### More Information

[Data School](https://www.youtube.com/watch?v=tcRGa2soc-c)

[NumPy Indexing](https://numpy.org/doc/stable/reference/arrays.indexing.html)

[PDSH: NumPy Basics](https://jakevdp.github.io/PythonDataScienceHandbook/02.02-the-basics-of-numpy-arrays.html)

[PDSH: Pandas Indexing](https://jakevdp.github.io/PythonDataScienceHandbook/03.02-data-indexing-and-selection.html)

