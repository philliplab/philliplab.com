---
title: "Comprehending List Comprehensions"
date: 2020-08-06
description: "Understanding nested for loops in list comprehensions"
type: "post"
tags: [Python]
draft: true
---

Python list comprehensions provide a syntax for nested `for` loops. 

### The wrong way

One might expect the syntax to look like:

```
[i+5 for i in [2*j for j in range(10)]]
```

However, this comprehension is similar to two `for` loops that are executed sequentially:

```
result = []
for j in range(10):
    result[j] = j*2
for i in range(len(result)):
    result[i] += 5
```

### A right way

Nested `for` loops are useful for unpacking a list of tuples, for example:

```
dat = [(i, 2*i) for i in range(5)]
res = []
for i in dat:
    for j in i:
        res.append(j)
```

This is identical to the comprehension:

```
res = [j for i in dat for j in i]
```

Note that there are no inner set of square brackets. When two `for` keywords follow each other inside a list comprehension with no intervening square brackets, they are equivalent to nesting the loop represented by the second `for` keyword under the loop represented by the first `for` keyword.

### More information

[Python Docs](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)

[Geeks for Geeks](https://www.geeksforgeeks.org/python-convert-list-of-tuples-into-list/)

