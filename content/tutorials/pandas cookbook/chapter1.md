---
date: 2017-05-10
linktitle: Chapter 1
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter2
title: Chapter 1 - Reading from a CSV
weight: 10
url: /pandas-cookbook/chapter1
---
## 1.1 Reading data from a CSV file

You can read data from a CSV file using the [read_csv](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) function. By default, it assumes that the fields are comma-separated.

We're going to be looking some cyclist data from Montréal. Here's the [original page](http://donnees.ville.montreal.qc.ca/dataset/velos-comptage) (in French). We're using the data from 2012. Download the [bikes.csv](/bikes.csv) file to try out the below examples.

This dataset is a list of how many people were on 7 different bike paths in Montreal, each day.

```python
import pandas as pd

broken_df = pd.read_csv('bikes.csv')
# Look at the first 3 rows
print broken_df[:3]
```

Output:

```bash
  Date;Berri 1;Br?beuf (donn?es non disponibles);C?te-Sainte-Catherine;Maisonneuve 1;Maisonneuve 2;du Parc;Pierre-Dupuy;Rachel1;St-Urbain (donn?es non disponibles)
0                   01/01/2012;35;;0;38;51;26;10;16;                                                                                                               
1                   02/01/2012;83;;1;68;153;53;6;43;                                                                                                               
2                 03/01/2012;135;;2;104;248;89;3;58;   
```

You'll notice that this is totally broken! read_csv has a bunch of options that will let us fix that, though. Here we'll

- Change the column separator to a ;
- Set the encoding to '_latin1_' (the default is '_utf8_')
- Parse the dates in the 'Date' column
- Tell it that our dates have the date first instead of the month first
- Set the index to be the 'Date' column

```python
fixed_df = pd.read_csv('bikes.csv', sep=';', encoding='latin1', parse_dates=['Date'], dayfirst=True, index_col='Date')
print fixed_df[:3]
```

Output:


| Date       | Berri 1 | Brébeuf  (données  non  disponibles) | Côte- Sainte- Catherine | Maisonneuve 1 | Maisonneuve 2 | du Parc | Pierre- Dupuy | Rachel1 | St-Urbain (données  non  disponibles) |
|------------|---------|--------------------------------------|-------------------------|---------------|---------------|---------|---------------|---------|---------------------------------------|
| 2012-01-01 | 35      | NaN                                  | 0                       | 38            | 51            | 26      | 10            | 16      | NaN                                   |
| 2012-01-02 | 83      | NaN                                  | 1                       | 68            | 153           | 53      | 6             | 43      | NaN                                   |
| 2012-01-03 | 135     | NaN                                  | 2                       | 104           | 248           | 89      | 3             | 58      | NaN                                   |
