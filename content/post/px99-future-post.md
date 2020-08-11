---
title: "Future Post"
date: 2020-08-07
description: "Work in progress"
type: "post"
tags: [Python]
draft: true
---

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
