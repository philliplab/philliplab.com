---
title: "First Weather Data Plots"
date: 2020-09-30
description: "A closer look at the data of a single weather station"
type: "post"
tags: [Python]
draft: true
---

This post details a first look at the data from a single weather station. The goal is to gain basic familiarity with the structure of the data and to see if there are apparent issues or patterns.



The following will be examined:
- The distribution of the response variables
- The number and location of missing data points
- Pattern accross years
- Patterns within years
- Patterns within months
- Autocorrelation






Observations per year for early years

	count
metric	TMAX	TMIN
year		
1948	168	168
1949	365	365
1950	151	151
1959	333	333
1960	365	365




Auto correlations

	lag	cor
0	1	0.917
1	2	0.869
2	3	0.850
3	4	0.839
4	5	0.833
5	6	0.830
6	7	0.827
7	8	0.824
8	9	0.821
9	10	0.817

