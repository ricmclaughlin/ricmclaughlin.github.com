---
layout: post
title: "AWS - QuickSight"
description: ""
category: posts
tags: [aws, analytics, quicksight, aws-services, aws-solutions-arch-pro, aws-spec-ml]
---
{% include JB/setup %}

# QuickSight
[QuickSight](https://aws.amazon.com/quicksight/) is a cloud-scale business intelligence (BI) service that can use to deliver easy-to-understand insights - another way of saying it makes dashboards. But these dashboards are snap-shots of an analysis - the filtering, parameters, controls, and sorts are embedded in the dashboard. There's an in-memory computation engine called SPICE if the data is _imported_ into QuickSight. The Enterprise edition has a security feature called Column-Level security.

QuickSight has it's own access control set up - uses IAM for admin rights; groups and users in the enterprise edition and just users in the standard version. Dashboards, which include access to the underlying data, can be shared with groups and users.

## Visualizations
There are tons of different visualizations supported using QuickSight:
AutoGraph - automatically selects type based on time.

For comparison and distribution:
- bar charts &amp; histograms to display the distribution of continuous numerical values in data.

Regression model:
Residual plot on a histogram where a residual is an observation in the evaluation data is the difference between the true target and the predicted target.

For changes over time:
- Line Graphs

For correlation:
- Scatter plots/box plots - Use scatter plots to visualize two or three measures across two dimensions to show how much one variable is affected by another. In ML scenarios, use to visualize predicted and true values.
- Heat maps - Use heat maps to show a measure for the intersection of two dimensions, with color-coding to easily differentiate where values fall in the range.

For Aggregation: 
- Pie charts - Use pie charts to compare values for items in a dimension. 
- Tree maps - rectangle version of a pie chart

- Radar chart - Use radar charts, which are also known as spider charts, to visualize multivariate data

Visualize outliers:
- Box plots - AKA as *Box and Whisker plot* - visualize quartiles and outliers

Classification ML analysis:
- Confusion matrix

## Machine Learning Insights
Anomaly detection
Forecasting
Auto-narratives