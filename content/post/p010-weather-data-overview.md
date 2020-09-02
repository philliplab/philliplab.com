---
title: "Climate Insights"
date: 2020-09-02
description: "Climate change in the data"
type: "post"
tags: [Python]
draft: true
---

Climate change is widely believed to be one of the most significant challenges we face in the 22nd century. Despite a very simple explanation of the physics of the problem and endless reports and figures providing evidence for the phenomena, it remains controversial in some circles. Due to the scale of the interventions required, governments must be involved and hence the issue has become political and polarizing.

There are many factors that drive skepticism of climate change:
- It is hard to predict specific problems that will result from climate change
- It is hard to predict the timelines on which such changes will occur
- All politically feasible interventions are extremely expensive
- Most useful metrics are very hard to measure and complex aggregations - making it hard to explain convincingly

This series of posts will aim to start by reporting very simple measurements and metrics. This allows the creation of easy to explain visualizations. The simple metrics can also be very local increasing the relevance and interest. Once basic concepts have been explained, more complicated ideas can be built systematically allowing the audience to come along for the journey - hopefully reducing skepticism.

The first dataset under consideration is that provided by the Global Historical Climatology Network (GHCND). This consists of high quality weather data for many weather stations around the globe over a long span of time. By inspecting these dataset one can answer **questions** such as:

- Is there an upwards trend in temperature for a given city?
- How unusual is today's temperature in my city?
- How many temperatures recorded in the last x days where unusual?

The first step is loading the data, which is discussed [here]({{< ref "p005-parsing-noaa-ghcnd.md" >}}).

