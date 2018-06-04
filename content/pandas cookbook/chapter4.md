---
date: 2017-05-15
linktitle: Chapter 4
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter5
prev: /pandas-cookbook/chapter3
title: Chapter 4 - Groupby and Aggregate
weight: 25
url: /pandas-cookbook/chapter4
description: Find out on which weekday people bike the most with groupby and aggregate
keywords:
  - pandas
  - Groupby
  - Aggregate
  - numpy
  - matplotlib
---

```python
%matplotlib inline

import pandas as pd
import matplotlib.pyplot as plt

pd.set_option('display.mpl_style', 'default') # Make the graphs a bit prettier
plt.rcParams['figure.figsize'] = (15, 5)
plt.rcParams['font.family'] = 'sans-serif'

# This is necessary to show lots of columns in pandas 0.12. 
# Not necessary in pandas 0.13.
pd.set_option('display.width', 5000) 
pd.set_option('display.max_columns', 60)
```


Okay! We're going back to our bike path dataset here. I live in Montreal, and I was curious about whether we're more of a commuter city or a biking-for-fun city -- do people bike more on weekends, or on weekdays?

## 4.1 Adding a 'weekday' column to our dataframe

First, we need to load up the data. We've done this before.

```python
bikes = pd.read_csv('bikes.csv', sep=';', encoding='latin1', parse_dates=['Date'], dayfirst=True, index_col='Date')
bikes['Berri 1'].plot()
```

Output:
<div>
<img src="/img/plot_single_column.png" alt="Plotting CSV column with Matplotlib" />
</div>

Next up, we're just going to look at the Berri bike path. Berri is a street in Montreal, with a pretty important bike path. I use it mostly on my way to the library now, but I used to take it to work sometimes when I worked in Old Montreal.
So we're going to create a dataframe with just the Berri bikepath in it

```python
berri_bikes = bikes[['Berri 1']].copy()
berri_bikes[:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Berri 1</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>  35</td>
    </tr>
    <tr>
      <th>2012-01-02</th>
      <td>  83</td>
    </tr>
    <tr>
      <th>2012-01-03</th>
      <td> 135</td>
    </tr>
    <tr>
      <th>2012-01-04</th>
      <td> 144</td>
    </tr>
    <tr>
      <th>2012-01-05</th>
      <td> 197</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Next, we need to add a 'weekday' column. Firstly, we can get the weekday from the index. We haven't talked about indexes yet, but the index is what's on the left on the above dataframe, under 'Date'. It's basically all the days of the year.

```python
berri_bikes.index
```

Output:

```bash
<class 'pandas.tseries.index.DatetimeIndex'>
[2012-01-01, ..., 2012-11-05]
Length: 310, Freq: None, Timezone: None
```

You can see that actually some of the days are missing -- only 310 days of the year are actually there. Who knows why.
Pandas has a bunch of really great time series functionality, so if we wanted to get the day of the month for each row, we could do it like this:

```python
berri_bikes.index.day
```

Output:

```bash
array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17,
       18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31,  1,  2,  3,
        4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
       21, 22, 23, 24, 25, 26, 27, 28, 29,  1,  2,  3,  4,  5,  6,  7,  8,
        9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25,
       26, 27, 28, 29, 30, 31,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11,
       12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28,
       29, 30,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15,
       16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31,  1,
        2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17, 18,
       19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30,  1,  2,  3,  4,  5,
        6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
       23, 24, 25, 26, 27, 28, 29, 30, 31,  1,  2,  3,  4,  5,  6,  7,  8,
        9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25,
       26, 27, 28, 29, 30, 31,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11,
       12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28,
       29, 30,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15,
       16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31,  1,
        2,  3,  4,  5], dtype=int32)
```

We actually want the weekday, though:

```python
berri_bikes.index.weekday
```

Output:

```bash
array([6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0,
       1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2,
       3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4,
       5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6,
       0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1,
       2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3,
       4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5,
       6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0,
       1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2,
       3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4,
       5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6,
       0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1,
       2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0, 1, 2, 3,
       4, 5, 6, 0, 1, 2, 3, 4, 5, 6, 0], dtype=int32)
```

These are the days of the week, where 0 is Monday. I found out that 0 was Monday by checking on a calendar.

Now that we know how to get the weekday, we can add it as a column in our dataframe like this:

```python
berri_bikes.loc[:,'weekday'] = berri_bikes.index.weekday
berri_bikes[:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Berri 1</th>
      <th>weekday</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01</th>
      <td>  35</td>
      <td> 6</td>
    </tr>
    <tr>
      <th>2012-01-02</th>
      <td>  83</td>
      <td> 0</td>
    </tr>
    <tr>
      <th>2012-01-03</th>
      <td> 135</td>
      <td> 1</td>
    </tr>
    <tr>
      <th>2012-01-04</th>
      <td> 144</td>
      <td> 2</td>
    </tr>
    <tr>
      <th>2012-01-05</th>
      <td> 197</td>
      <td> 3</td>
    </tr>
  </tbody>
</table>
</div>
</div>

## 4.2 Adding up the cyclists by weekday

This turns out to be really easy!
Dataframes have a [.groupby()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html) method that is similar to [SQL groupby](https://docs.microsoft.com/en-us/sql/t-sql/queries/select-group-by-transact-sql), if you're familiar with that. I'm not going to explain more about it right now -- if you want to to know more, [the documentation](http://pandas.pydata.org/pandas-docs/stable/groupby.html) is really good.

In this case, `berri_bikes.groupby('weekday').aggregate(sum)` means 

> "Group the rows by weekday and then add up all the values with the same weekday."

```python
weekday_counts = berri_bikes.groupby('weekday').aggregate(sum)
weekday_counts
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Berri 1</th>
    </tr>
    <tr>
      <th>weekday</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td> 134298</td>
    </tr>
    <tr>
      <th>1</th>
      <td> 135305</td>
    </tr>
    <tr>
      <th>2</th>
      <td> 152972</td>
    </tr>
    <tr>
      <th>3</th>
      <td> 160131</td>
    </tr>
    <tr>
      <th>4</th>
      <td> 141771</td>
    </tr>
    <tr>
      <th>5</th>
      <td> 101578</td>
    </tr>
    <tr>
      <th>6</th>
      <td>  99310</td>
    </tr>
  </tbody>
</table>
</div>
</div>

It's hard to remember what 0, 1, 2, 3, 4, 5, 6 mean, so we can fix it up and graph it:

```python
weekday_counts.index = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
weekday_counts
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Berri 1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Monday</th>
      <td> 134298</td>
    </tr>
    <tr>
      <th>Tuesday</th>
      <td> 135305</td>
    </tr>
    <tr>
      <th>Wednesday</th>
      <td> 152972</td>
    </tr>
    <tr>
      <th>Thursday</th>
      <td> 160131</td>
    </tr>
    <tr>
      <th>Friday</th>
      <td> 141771</td>
    </tr>
    <tr>
      <th>Saturday</th>
      <td> 101578</td>
    </tr>
    <tr>
      <th>Sunday</th>
      <td>  99310</td>
    </tr>
  </tbody>
</table>
</div>
</div>

```python
weekday_counts.plot(kind='bar')
```

Output:

<div>
    <img src="/img/cycle_weeday_plot.png" alt="Plot dataframe as per weekdays" />
</div>

So it looks like Montrealers are commuter cyclists -- they bike much more during the week. Neat!

## 4.3 Putting it together

Let's put all that together, to prove how easy it is. 6 lines of magical pandas!
If you want to play around, try changing sum to max, numpy.median, or any other function you like.

```python
bikes = pd.read_csv('../data/bikes.csv', 
                    sep=';', encoding='latin1', 
                    parse_dates=['Date'], dayfirst=True, 
                    index_col='Date')
# Add the weekday column
berri_bikes = bikes[['Berri 1']].copy()
berri_bikes.loc[:,'weekday'] = berri_bikes.index.weekday

# Add up the number of cyclists by weekday, and plot!
weekday_counts = berri_bikes.groupby('weekday').aggregate(sum)
weekday_counts.index = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
weekday_counts.plot(kind='bar')
```

Output:

<div>
    <img src="/img/cycle_weeday_plot.png" alt="Plot dataframe as per weekdays" />
</div>
