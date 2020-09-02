---
title: "Parsing station data from GHCND"
date: 2020-08-16
description: "Reading weather data of a single station from the FWF into a Pandas DataFrame"
type: "post"
tags: [Python]
draft: false
---

The first step of any analysis project is obtaining the data and figuring out how to convert it into an analysable format. This post describes the first data source for the series on [Climate Insights]({{< ref "p010-weather-data-overview" >}}) and consists of two sections:
1. **Data Source and Format** -  Description of the souce of the data and the format it is stored in.
3. **Code** - Describe the Python code that reads the data and converts it into a appropriately shaped DataFrame.
 
### Data Source

The Global Historical Climatology Network provides an FTP server with many different datasets. These group the data in different ways and store them in different formats. For this post, the focus is on the data from a single station that is stored in fixed width format (FWF).

[Fixed Width Format Files](https://www.softinterface.com/Convert-XLS/Features/Fixed-Width-Text-File-Definition.htm): In FWF files, each row is represented by the same number of characters and the location of each column is the same in each row. Thus each data entry (cell) is padded with spaces to ensure that it is at the right location. This format has decreased in popularity in recent years, but is still in use in many long running projects. Fixed-width files can be accessed efficiently without loading it into memory by navigating to the byte on the hard drive where you know the cell you are interested in is located.

**GHCND File Format Details:** As shown in the figure below, the specific station files used by the GHCND store values in a value column and attached 3 different flags to each value. Each such value has a number of extra descriptors:

- Station Name: At which station was the observation recorded?
- Date: Date of the observation
- Metric: Does the value reflect a measurement of Temperature (TMAX, TMIN), Precipitation (PRCP), and so forth

The station, metric, year and month descriptions define the rows, while the day flag/value descriptors define the columns. This means that each row consists of 31 values and the 93 flags that correspond to those values, with the 1st value and its flags being for the 1st day of the month, the 2nd value and its flags for the 2nd day of the month and so forth.

{{< figure src="/ghcnd_fwf_annotated.png" title="Format of the GHCND FWF files" >}}

### Code

The code to load and parse a single station file is located in the `load_station` function for the `load_data.py` [script](https://github.com/philliplab/wda). The function provides parameters for specifying the station and folder containing the data.

The main parts of `load_station` are:

- Constructing the column widths and headings arguments for `pd.read_fwf`.
- Transforming the data into long format with a one dimensional column index.

#### Constructing the arguments for `pd.read_fwf`

List comprehensions provide a very convenient way to produce the specifications of the column names and column widths. First create a list with 31 repeats of the repeated column names or widths. Append this onto the names and widths of the non-repeated columns:

```
repeated_cols = [('v_'+str(i+1), 
                  'mf_'+str(i+1), 
                  'qf_'+str(i+1), 
                  'sf_'+str(i+1)) for i in range(31)]
repeated_col_widths = [(5,1,1,1) for i in range(31)]

col_names = ['station', 'year', 'month', 'metric'] + \
            [item for t in repeated_cols for item in t]
widths = [11, 4, 2, 4] + [j for k in repeated_col_widths for j in k]
```

#### Transformation into long format

Pandas' MultiIndex concept provides an extremely powerful tool for manipulating the shape of the data. The columns of this data has a 2-level index:

- The day of the month, and
- The value, or type of flag

However, the data cannot be treated as such until the other description columns are first removed from the 'main' data. These columns are the Station ID, Year, Month and Metric columns. These description columns can easily be moved into the index of the DataFrame:

```
dat.set_index(['station', 'year', 'month', 'metric'], inplace = True)
```

Once this is performed, the structure of the column index can be specified using a [list comprehension that mimics nested for loops]({{< ref "p003-comprehending-list-comprehensions" >}}):

```
days_index = [k for j in [(i, i, i, i) for i in range(31)] for k in j]
repeated_cols_index = ['value', 'mflag', 'qflag', 'sflag']*31
dat.columns = pd.MultiIndex.from_arrays([days_index, repeated_cols_index])
```

Finally, the levels of the column index can be stacked into the row index to produce a dataset in long format:

```
dat = dat.stack([0])
dat.index.rename('day', len(dat.index.names)-1, inplace = True)
```

Producing:

                                      mflag qflag sflag  value
    station     year month metric day
    USW00026617 1900 8     TMAX   12    NaN   NaN     6   11.1
                                  13    NaN   NaN     6   12.8
                                  14    NaN   NaN     6   14.4
                                  15    NaN   NaN     6   11.7
                                  16    NaN   NaN     6   14.4
    ...                                 ...   ...   ...    ...
                2020 8     TMIN   3     NaN   NaN     D    6.1
                           PRCP   0     NaN   NaN     D    3.0
                                  1     NaN   NaN     D    0.0
                                  2     NaN   NaN     D    0.0
                                  3     NaN   NaN     D    0.0
                                        
    [195444 rows x 4 columns]                                 
