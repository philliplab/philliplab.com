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

### What to align on: The simple answer

You can always perform a pd.merge with the left_on and right_on arguments specifying the names of all the columns/index levels you want to consider in the join.

If all the index levels and column names you want to use in the join are the same in both dataframes, you can use the on argument.

A good baseline recommendation for the use of pd.merge is thus:

Are the column names and index levels you want to join on the same in both DFs?
- If yes: Use on
- If no: Use left_on and right_on

This rule will allow you to use all the functionality of pd.merge.

### What to align on: The complicated answer

In addition to on, left_on and right_on discussed above pd.merge also have the left_index and right_index parameters for controlling the join. These provide the option of saving some typing when the join is restricted to all the levels of the index of at least one of the DataFrames, but adds much complexity to deciding which parameters to use. The list below maps out a decision tree that yield an optimized choice of parameters to use for almost all cases:

- **Only join names match:** Do all the column & index levels you want to join on have the same names in both DataFrames? Additionally, are these the ONLY columns & index levels that have the same names in both DataFrames? Then the defaults will work and you need **no arguments**.
- **Both whole indexes and the levels match:** Do you want to join on the entire index and only on the index for both DataFrames? Additionally, are the names of the indexes identical? Then you can pass True to both the **left_index and right_index parameters**.
- **Join names match:** Do the index level names and/or column names that you want to join on match between the DFs? Pass a list of all the columns and/or index levels you want to join on to the **on parameter**.
- **One whole index:** Do you want to join on the entire index (all levels) of one of the DataFrames and some subset of the columns & index levels of the other? In this case you can use either **left_index = True and pass a list of column/level names to right_on** or use **right_index = True and pass a list of column/level names to left_on**. Note that the names do not have to match. pd.merge will assume that the list of column/level names will match the order of levels of the full index you want to join on.
- **Everything else:** You have to use left_on and right_on. Pass a list of the columns & index levels you want to use from the left DataFrame to left_on and treat right_on similarly.

There are a couple of cases not covered by the list above. They were excluded to keep the list length reasonable and because you can always fall back to using left_on and right_on. This is the most explicit option and covers all the cases, so there is little cost to not knowing the 100% most optional set of parameters to use for all the cases.

### How to handle mismatches

The final important parameter of pd.merge specifies how to handle mismatches between the two DataFrames in the contents of the columns & index levels involved in the join. The `how` parameter allows you to if you want to do an inner join, left join, right join or an outer join. This is very well described in the Wikipedia article on [joins](https://en.wikipedia.org/wiki/Join_(SQL))

### The remaining parameters

Thusfar 8 parameters were discussed. Five remain. Their use is straight-forward:

- suffixes: Add suffixes to the **columns & index levels** to indicate if they were from the left or right DataFrames?
- indicator: Add column detailing the which DataFrame each **row** is from?
- sort: Sort the resulting DataFrame based on the join columns & index levels?
- copy: Optimize the operation to prevent unnecessary copying?
- validate: Run additional checks.

### Conclusion

You can access all the functionality of the pd.merge function with a very simple set of rules.

Since joining DataFrames is such a fundamental and often used operation in data analysis, pd.merge adds extra options which reduce the number and length of arguments you have to pass. This comes at the cost of producing a complex decision tree for finding the optimal combination of paramters. Additionally, it allows you to perform the same thing.

### More information

[pd.merge documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)



### OLD 

All the decisions that has to be made about combining DataFrames are determined by the answers to the following three questions:

- **Two or more:** Are you combining two DataFrames or more than two DataFrames?
- **Align on rows or columns:** Do you want to align the DataFrames on rows and place them side-by-side? Or align the DataFrames on columns and place them on top of each other?
- **Index or data values:** Do you want to perform the alignment using data in the DataFrame or the labels in the indices? Or a mixture?

- If you want to combine more than two DataFrames, you need to use multiple calls to pd.merge.
- You can align based on index labels, data values in columns or a mixture. This is controlled by using some subset of the on, right_on, left_on, right_index and left_index arguments.

Finally, both pd.merge and pd.concat can perform all the operations describe in this post:
- For pd.concate, you can specify axis = 1 and then use the join argument and the index (row names) to combine the DFs by placing them side-by-side. (It is OK to do this)
- For pd.merge, you can perform a full outer join specifying the entire dataset and the columns to join on to place the DFs on top of each other. (Don't do this)

