---
title: "Climate Insights"
date: 2020-09-02
description: "Climate change in the data"
type: "post"
tags: [Python]
draft: true
---

Climate change is widely believed to be one of the most significant challenges humanity faces in the 22nd century. Despite a [simple explanation of the physics](https://www.youtube.com/watch?v=SN5-DnOHQmE) of the problem and [endless](https://en.wikipedia.org/wiki/Keeling_Curve) [reports](https://www.ipcc.ch/report/ar5/syr/) and [figures](https://climate.nasa.gov/resources/graphics-and-multimedia/?page=0&per_page=25&order=pub_date+desc&search=&condition_1=1%3Ais_in_resource_list) providing evidence, it remains controversial in some circles. Due to the scale of the interventions required, governments must be involved and hence the issue has become political and polarizing.

There are many factors that drive skepticism of climate change:
- It is hard to predict specific problems that wll result from climate change.
- It is hard to predict specific timelines on which events occur.
- All politically feasible interventions are extremely expensive.
- Most useful metrics are complex aggregations - making it hard to explain convincingly.

This series of posts start by reporting very simple measurements and metrics. This allows the creation of easy to explain visualizations. The simple metrics can also be very local, increasing the relevance and interest. Once basic concepts have been explained, more complicated ideas can be built systematically allowing the audience to come along for the journey.

### GHCND Data

The first [dataset](ftp://ftp.ncdc.noaa.gov/pub/data/ghcn/daily/readme.txt) under consideration is provided by the [Global Historical Climatology Network](https://www.ncdc.noaa.gov/data-access/land-based-station-data/land-based-datasets/global-historical-climatology-network-ghcn) (GHCND). It consists of high quality weather data for many weather stations around the globe over a long span of time. By inspecting this dataset one can answer **questions** such as:

- Is there a trend in temperature for my city?
- How unusual is today's temperature in my city?
- How many observed temperatures in the last x days where unusual?

The first step is loading the data, which is discussed [here]({{< ref "p005-parsing-noaa-ghcnd.md" >}}).

