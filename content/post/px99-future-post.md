---
title: "Future Post"
date: 2099-12-31
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



Notes about [[MultiIndexes]] from LOS.

MultiIndexes enables the addition of dimensions to Series and DataFrames by creating hierarchical index structures.

A MultiIndex consists of multiple levels (each similar to a variable describing some group the observation belong to) and each level has a set of labels associated with it.

Practically, MultiIndexes provide a convenient way to select subsets of the data based on different conditions placed specified for the various levels of the MultiIndex.

Old waffling:

Pandas allows you to add dimensions to Series and DataFrames by means of MultiIndexes.

These objects create hierarchical index structures that allows you to annotate observations with labels for multiple grouping/categorization variables. These different grouping variables are called the levels of the index. For example, an index that tracks the collection date and collection location for each observation has 2 levels with names 'collection_date' and 'collection_location'. Each level will have labels:

- collection_date: '2010-03-02', '2010-03-03', ...
- collection_location: 'Houston', 'Vancouver', ...

Hence the labels associated with the collection_date level of the MultiIndex are dates. Likewise, the labels associated with the collection_location level of the MultiIndex are city names.

Such indexes allow you to easily perform queries such as: "Get all observations for Houston", or "Get all observations after 2010-05-01 for Vancouver and Toronto".

