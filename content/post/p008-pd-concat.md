---
title: "UNIONs in Pandas with pd.concat"
date: 2020-09-02
description: "Combining DataFrames by placing them on top of each other (SQL UNION)"
type: "post"
tags: [Python]
draft: true
---

This is the third of four posts on combining DataFrames in Pandas.

The [first post]({{< ref "p006-combining-pandas.md" >}}) explained which function to choose. When joining DataFrames by placing them side-by-side you use the `pd.merge` function - the topic of this post.

The next post will discuss `pd.concat` and the fourth and final post will examine some alternative approaches.




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

