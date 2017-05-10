---
date: 2017-05-10
linktitle: Chapter 1
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter2
title: Pandas Cookbook
weight: 10
url: /pandas-cookbook/chapter1
---
## 1.1 Reading data from a CSV file

You can read data from a CSV file using the [read_csv](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) function. By default, it assumes that the fields are comma-separated.

We're going to be looking some cyclist data from Montréal. Here's the [original page](http://donnees.ville.montreal.qc.ca/dataset/velos-comptage) (in French), but it's already included in this repository. We're using the data from 2012.

This dataset is a list of how many people were on 7 different bike paths in Montreal, each day.

```python
broken_df = pd.read_csv('../data/bikes.csv')
# Look at the first 3 rows
broken_df[:3]
```