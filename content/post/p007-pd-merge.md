---
title: "JOINs in Pandas with pd.merge"
date: 2020-08-31
description: "Combining DataFrames by placing them side-by-side (SQL JOIN)"
type: "post"
tags: [Python]
draft: true
---

This is the second of four posts on combining DataFrames in Pandas.

The [first post]({{< ref "p006-combining-pandas.md" >}}) explained which function to choose. When joining DataFrames by placing them side-by-side you use the `pd.merge` function - the topic of this post.

The next post will discuss `pd.concat` and the fourth and final post will examine some alternative approaches.

### Overview

The purpose of `pd.merge` is to implement SQL-like JOIN functionality in Pandas. These operations align the rows of the two DataFrames and then `transfer` the columns between them resulting in a DataFrame that is wider than any of the input DataFrames.

As of 2020-08-31, `pd.merge` has 13 parameters and some interact in non-trivial ways. It can be overwhelming to keep track of them without a mental model of their purpose.

The most important parameters are:
- Inputs: The input parameters, and
- What to align on: The parameters specifying which index levels / data columns to use to align the DataFrames.
- How to handle mismatches: The `how` parameter that controls how mismatches between the two DataFrames should be resolved.

### Input DataFrames

`pd.merge` have a `left` and a `right` parameter which are used to pass in the two DataFrames to join.

The `df.merge` and `df.join` methods essentially calls `pd.merge` with the `left` parameter set to the DataFrame on which the method was invoked. 

**HERE**
Actually, this is not a good split - think about rather:
- do the index level names and/or column names match between the DFs?
- do you want to join on the entire index and only the index? Can you left_index and right_index if the names mismatch? #TODO run some tests here
- do the names mismatch between the DFs?
- now you are stuck with left_on and right_on

### What to align on

When aligning DataFrames so that their rows are in the same order, there are three options:

- **Index:** You can use the index labels (row names), or
- **Data Values:** You can use the data values in some of the columns, or
- **Mixture:** Some mixture of the index labels and data values.

Each of these requires the use of a different subset of the parameters controlling the alignment.

#### Index

Some of the levels of the index: 

- If the index level names match between the DFs, use `on` and specify the levels.
- If the index level names do not match, then use `left_on` and `right_on` to specify the names of the levels.

The entire index (all of the levels in the index):

- You can use `left_index` = True and `right_index` = True.
- Just as above, you can also use the `on` argument and specify all the levels of the indexes.

Thus you can use any of the five parameter

#### Data Values

- If the column names match between the DFs, use the on argument and specify the column names.
- If the column names do not match between the DFs, use the left_on and right_on arguments to specify the names of the levels.

#### Mixture

Using the on argument or the left_on and right_on arguments allows you to specify a mixture index levels and column names. The DFs will be aligned so that both the labels in the specified indexes AND the data values in the specified columns are aligned.

If you want to use the whole index of one dataframe (say the left one) and a mixture of index levels and columns from the other dataframe (the right one), then you would use the left_index = True option together with a list of level names and column names passed to the right_on argument.

 

The **only two functions you need** are `pd.merge` and `pd.concat`:
- `df.join` and `df.merge` are just convenience functions for calling **`pd.merge`**.
- `df.append` is just a convenience function for calling **`pd.concat`**.
- `np.concatenate` is for Numpy arrays - it is a decoy - **ignore it**. [(Unless you want to do esoteric things)](https://stackoverflow.com/a/15582359)

All the decisions that has to be made about combining DataFrames are determined by the answers to the following three questions:

- **Two or more:** Are you combining two DataFrames or more than two DataFrames?
- **Align on rows or columns:** Do you want to align the DataFrames on rows and place them side-by-side? Or align the DataFrames on columns and place them on top of each other?
- **Index or data values:** Do you want to perform the alignment using data in the DataFrame or the labels in the indices? Or a mixture?

### The fundamental question

There are two ways to combine DataFrames: Horizontally or Vertically.

#### Horizontal

To combine two DataFrames horizontally, align them so that their rows are in the same order and then 'transfer' the columns of the one into the other. This is demonstrated in the figure titled 'Aligning by Rows' below.

{{< figure src="/join_inner_only.svg" title="Aligning by Rows" >}}

The effect of this type of combination is that you make the rows longer by placing them side-by-side - i.e. result is wider than either of the input DataFrames. 

Horizontal combinations of DataFrames are analogous to SQL JOINs and the purpose of **`pd.merge`** is to bring these powerful operations to Pandas.

If you want to combine DataFrames by placing them side-by-side, use **`pd.merge`**.

#### Vertical

To combine two DataFrames vertically, align them so that their columns are in the same order and then 'transfer' the rows on the one into the other. This is demonstrated in the figure titled 'Aligning by Columns' below. 

{{< figure src="/union.svg" title="Aligning by Columns" >}}

Such combinations make the columns longer by placing the one DataFrame 'on top of' the other - i.e. the result is taller than either of the input DataFrames. 

These combinations are analogous to UNIONS in SQL and are performed with the **`pd.concat`** function.

If you want to combine DataFrames by placing them on top of each other, use **`pd.concat`**.

### Potential confusion

What if you want to align them by index instead of rows or columns?

Pandas terminology can be a little confusing here. The index contains the 'row names'. Thus aligning the indexes of two DataFrames will place their rows in the same order.

### Conclusion

If you want to JOIN DataFrames by placing them side-by-side, use **`pd.merge`**.

If you want the UNION of DataFrames, obtained by placing them on top of each other, use **`pd.concat`**.

### More information

[Union vs Join](https://stackoverflow.com/questions/905379/what-is-the-difference-between-join-and-union#:~:text=In%20a%20union%2C%20columns%20aren,tables%20into%20a%20single%20results.&text=Whereas%20a%20join%20is%20used,is%20used%20to%20combine%20rows.)

### Index or data values?

When aligning DataFrames so that their rows are in the same order, there are three options:

- **Index:** You can use the index labels (row names), or
- **Data Values:** You can use the data values in some of the columns, or
- **Mixture:** Some mixture of the index labels and data values.

Note that when aligning the columns, you can only use the column names - so you do not have to deal with this complexity.

When aligning on rows, `pd.merge` is the weapon of choice so this decision affects which arguments of `pd.merge` you will use. There are multiple ways to achieve any of these three effects:

#### Index

Some of the levels of the index: 

- If the index level names match between the DFs, use on and specify the levels.
- If the index level names do not match, then use left_on and right_on to specify the names of the levels.

The entire index (all of the levels in the index):

- You can use left_index = True and right_index = True
- Just as above, you can also use the on argument and specify all the levels of the indexes

#### Data Values

- If the column names match between the DFs, use the on argument and specify the column names.
- If the column names do not match between the DFs, use the left_on and right_on arguments to specify the names of the levels.

#### Mixture

Using the on argument or the left_on and right_on arguments allows you to specify a mixture index levels and column names. The DFs will be aligned so that both the labels in the specified indexes AND the data values in the specified columns are aligned.

If you want to use the whole index of one dataframe (say the left one) and a mixture of index levels and columns from the other dataframe (the right one), then you would use the left_index = True option together with a list of level names and column names passed to the right_on argument.

### Conclusion

If you want to combine DataFrames by aligning their columns and the placing them on top of each other, use pd.concat. 

- In this case you are restricted to only using the column names for aligning the DataFrames.

If you want to combine DataFrames by aligning their rows and then placing them side-by-side, use pd.merge.

- If you want to combine more than two DataFrames, you need to use multiple calls to pd.merge.
- You can align based on index labels, data values in columns or a mixture. This is controlled by using some subset of the on, right_on, left_on, right_index and left_index arguments.

Finally, both pd.merge and pd.concat can perform all the operations describe in this post:
- For pd.concate, you can specify axis = 1 and then use the join argument and the index (row names) to combine the DFs by placing them side-by-side. (It is OK to do this)
- For pd.merge, you can perform a full outer join specifying the entire dataset and the columns to join on to place the DFs on top of each other. (Don't do this)






























### More Information




