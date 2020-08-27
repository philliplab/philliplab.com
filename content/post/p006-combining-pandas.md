---
title: "Combining Pandas"
date: 2020-08-26
description: "Combining datasets in Pandas"
type: "post"
tags: [Python]
draft: true
---

Merge or concat? Or join? Or append? Or concatenate? From the Pandas namespace or as a method on a dataframe? At first glance the various options for combining data in Pandas seems very confusing.

Picking **which function** to use is fairly simple:

- If you want to perform sql-style joins on two DataFrames, use `pd.merge`.
  - Both `df.merge` and `df.join` are convenience functions that just calls `pd.merge` under the hood.
- If you want to do anything else, use `pd.concat`.
  - `pd.append` is a convenience function that just calls `pd.concat` under the hood.
  - `np.concatenate` is the numpy equivalent of `pd.concat`, ignore it.

**So, `pd.merge` for sql-style joins and `pd.concat` for everything else.**

Figuring out **which arguments** to pass to the function are more involved. All DataFrame combination tasks can be sub-divided according to the following criteria:

- **Two or more:** Are you combining two dataframes or more than two dataframes?
- **Align on rows or columns:** Do you want to align the dataframes on rows and then concatenate the rows (add columns to the DataFrame)? Or align the dataframes on columns and then concatenate the columns (adding rows to the DataFrame)?
- **Index or data values:** Do you want to perform the alignment using data in the DataFrame or the labels in the indices? or a mixture?

### Two or more?

`pd.merge` only allows you to specify two input DataFrames via the left and right arguments. `pd.concat` provides a parameter that may receive an arbitrary number of DataFrames.

If you want to combine more than two DataFrames, you will either need to:

- Use multiple `pd.merge` calls, or
- Use `pd.concat`.

### Align on rows or columns?



{{< figure src="/join_inner_only.svg" title="Aligning by Rows" >}}

{{< figure src="/union.svg" title="Aligning by Columns" >}}


### More

Union vs Join https://stackoverflow.com/questions/905379/what-is-the-difference-between-join-and-union#:~:text=In%20a%20union%2C%20columns%20aren,tables%20into%20a%20single%20results.&text=Whereas%20a%20join%20is%20used,is%20used%20to%20combine%20rows.


