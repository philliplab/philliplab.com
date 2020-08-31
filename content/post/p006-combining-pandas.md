---
title: "Combining Pandas"
date: 2020-08-31
description: "Combining datasets in Pandas"
type: "post"
tags: [Python]
draft: false
---

When I combine DataFrames, I always fear that the Panda police might arrest me for doing it wrong.

**Merge** or **concat**? Or **join**? Or **append**? Or **concatenate**? From the Pandas namespace or as a method on a DataFrame? Once that is decided, you still have to decide which set of arguments to use.

The next few posts aim to provide a framework for thinking about combining DataFrames and how to decide which approach to use.

This first post will focus on **choosing which function** to use. The second post will explore `pd.merge` and its swarm of parameters. Next `pd.concat` will be described in the third post. The final post of the series will demonstrate alternative strategies.

### From six to two

The **only two functions you need** are `pd.merge` and `pd.concat`:
- `df.merge` is just convenience functions for calling **`pd.merge`**.
- `df.append` is just a convenience function for calling **`pd.concat`**.
- `df.join` is another convenience function. It usually calls **`pd.merge`**, but can also call **`pd.concat`**.
- `np.concatenate` is for Numpy arrays - it is a decoy - **ignore it**. [(Unless you want to do esoteric things)](https://stackoverflow.com/a/15582359)

### The fundamental question

There are two ways to combine DataFrames: **Horizontally or Vertically**. This determines the choice between `pd.merge` and `pd.concat`.

#### Horizontal

To combine two DataFrames horizontally, align them so that their rows are in the same order and then 'transfer' the columns of the one into the other. This is demonstrated in the figure titled 'Aligning by Rows' below.

{{< figure src="/join_inner_only.svg" title="Aligning by Rows" >}}

The effect of this type of combination is that you make the rows longer by placing them side-by-side - i.e. result is wider than either of the input DataFrames. 

Horizontal combinations of DataFrames are **analogous to SQL JOINs** and the purpose of **`pd.merge`** is to bring these powerful operations to Pandas.

If you want to combine DataFrames by placing them side-by-side, use **`pd.merge`**.

#### Vertical

To combine two DataFrames vertically, align them so that their columns are in the same order and then 'transfer' the rows on the one into the other. This is demonstrated in the figure titled 'Aligning by Columns' below. 

{{< figure src="/union.svg" title="Aligning by Columns" >}}

Such combinations make the columns longer by placing the one DataFrame 'on top of' the other - i.e. the result is taller than either of the input DataFrames. 

These combinations are **analogous to UNIONS** in SQL and are performed with the **`pd.concat`** function.

If you want to combine DataFrames by placing them on top of each other, use **`pd.concat`**.

### Potential confusion

What if you want to align the DataFrames **by index** instead of rows or columns?

Pandas terminology can be a little confusing here. The index contains the 'row names'. Thus aligning the indexes of two DataFrames will place their rows in the same order.

### Conclusion

If you want to JOIN DataFrames by placing them side-by-side, use **`pd.merge`**.

If you want the UNION of DataFrames, obtained by placing them on top of each other, use **`pd.concat`**.

### More information

[Union vs Join](https://stackoverflow.com/questions/905379/what-is-the-difference-between-join-and-union#:~:text=In%20a%20union%2C%20columns%20aren,tables%20into%20a%20single%20results.&text=Whereas%20a%20join%20is%20used,is%20used%20to%20combine%20rows.)
