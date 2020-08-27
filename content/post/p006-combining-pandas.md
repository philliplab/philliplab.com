---
title: "Combining Pandas"
date: 2020-08-26
description: "Combining datasets in Pandas"
type: "post"
tags: [Python]
draft: true
---

Merge or concat? Or join? Or append? Or concatenate? From the Pandas namespace or as a method on a DataFrame? At first glance the various options for combining data in Pandas seems very confusing.

Picking **which function** to use is fairly simple:

- If you want to perform SQL-style joins on two DataFrames, use `pd.merge`.
  - Both `df.merge` and `df.join` are convenience functions that just calls `pd.merge` under the hood.
- If you want to do anything else, use `pd.concat`.
  - `pd.append` is a convenience function that just calls `pd.concat` under the hood.
  - `np.concatenate` is the numpy equivalent of `pd.concat`, ignore it.

**So, `pd.merge` for SQL-style joins and `pd.concat` for everything else.**

Figuring out **which arguments** to pass to the function are more involved. All DataFrame combination tasks can be sub-divided according to the following criteria:

- **Two or more:** Are you combining two DataFrames or more than two DataFrames?
- **Align on rows or columns:** Do you want to align the DataFrames on rows and place them side-by-side? Or align the DataFrames on columns and place them on top of each other?then concatenate the rows (add columns to the DataFrame)? Or align the DataFrames on columns and then concatenate the columns (adding rows to the DataFrame)?
- **Index or data values:** Do you want to perform the alignment using data in the DataFrame or the labels in the indices? Or a mixture?

### Two or more?

`pd.merge` only allows you to specify two input DataFrames via the left and right arguments. `pd.concat` provides a parameter that may receive an arbitrary number of DataFrames.

If you want to combine more than two DataFrames, you will either need to:

- Use multiple `pd.merge` calls, or
- Use `pd.concat`.

### Align on rows or columns?

There are two ways to combine DataFrames:

- **Align on rows:** You can align them so that their rows are in the same order and then 'transfer' the columns of the one into the other. The effect is that you make the rows longer by placing them side-by-side. This is demonstrated in the figure titled 'Aligning by Rows'.
- **Align on columns:** Alternatively, you can align them so that their columns are in the same order and then 'transfer' the rows on the one into the other. The effect is that you make the columns longer by placing the one DataFrame 'on top of' the other. This is demonstrated in the figure titled 'Aligning by Columns'.

#### Align on Rows

{{< figure src="/join_inner_only.svg" title="Aligning by Rows" >}}

One of the fundamental operations that RDBMS and SQL is used for is performing joins such as the one demonstrated in the figure above. The purpose of the `pd.merge` function is to bring these powerful operations to Pandas. Hence, you should use `pd.merge` when you want to combine DataFrames in this manner.

{{< figure src="/union.svg" title="Aligning by Columns" >}}


### More Information

[Union vs Join](https://stackoverflow.com/questions/905379/what-is-the-difference-between-join-and-union#:~:text=In%20a%20union%2C%20columns%20aren,tables%20into%20a%20single%20results.&text=Whereas%20a%20join%20is%20used,is%20used%20to%20combine%20rows.)


