---
date: 2017-05-18
linktitle: Chapter 6
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter7
prev: /pandas-cookbook/chapter5
title: Chapter 6 - String Operations
weight: 10
url: /pandas-cookbook/chapter6
description: String Operations
keywords:
  - pandas
  - string
---

```python
%matplotlib inline

import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

pd.set_option('display.mpl_style', 'default')
plt.rcParams['figure.figsize'] = (15, 3)
plt.rcParams['font.family'] = 'sans-serif'
```

We saw earlier that pandas is really good at dealing with dates. It is also amazing with strings! We're going to go back to our weather data from Chapter 5, here.

```python
weather_2012 = pd.read_csv('weather_2012.csv', parse_dates=True, index_col='Date/Time')
weather_2012[:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Temp (C)</th>
      <th>Dew Point Temp (C)</th>
      <th>Rel Hum (%)</th>
      <th>Wind Spd (km/h)</th>
      <th>Visibility (km)</th>
      <th>Stn Press (kPa)</th>
      <th>Weather</th>
    </tr>
    <tr>
      <th>Date/Time</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-01-01 00:00:00</th>
      <td>-1.8</td>
      <td>-3.9</td>
      <td> 86</td>
      <td> 4</td>
      <td> 8.0</td>
      <td> 101.24</td>
      <td>                  Fog</td>
    </tr>
    <tr>
      <th>2012-01-01 01:00:00</th>
      <td>-1.8</td>
      <td>-3.7</td>
      <td> 87</td>
      <td> 4</td>
      <td> 8.0</td>
      <td> 101.24</td>
      <td>                  Fog</td>
    </tr>
    <tr>
      <th>2012-01-01 02:00:00</th>
      <td>-1.8</td>
      <td>-3.4</td>
      <td> 89</td>
      <td> 7</td>
      <td> 4.0</td>
      <td> 101.26</td>
      <td> Freezing Drizzle,Fog</td>
    </tr>
    <tr>
      <th>2012-01-01 03:00:00</th>
      <td>-1.5</td>
      <td>-3.2</td>
      <td> 88</td>
      <td> 6</td>
      <td> 4.0</td>
      <td> 101.27</td>
      <td> Freezing Drizzle,Fog</td>
    </tr>
    <tr>
      <th>2012-01-01 04:00:00</th>
      <td>-1.5</td>
      <td>-3.3</td>
      <td> 88</td>
      <td> 7</td>
      <td> 4.8</td>
      <td> 101.23</td>
      <td>                  Fog</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 7 columns</p>
</div>
</div>

## 6.1 String operations

You'll see that the 'Weather' column has a text description of the weather that was going on each hour. We'll assume it's snowing if the text description contains "Snow".

pandas provides vectorized string functions, to make it easy to operate on columns containing text. There are some great [examples](http://pandas.pydata.org/pandas-docs/stable/basics.html#vectorized-string-methods) in the documentation.

```python
weather_description = weather_2012['Weather']
is_snowing = weather_description.str.contains('Snow')
```

This gives us a binary vector, which is a bit hard to look at, so we'll plot it.

```python
# Not super useful
is_snowing[:5]
```

Output:

```bash
Date/Time
2012-01-01 00:00:00    False
2012-01-01 01:00:00    False
2012-01-01 02:00:00    False
2012-01-01 03:00:00    False
2012-01-01 04:00:00    False
Name: Weather, dtype: bool
```

```python
# More useful!
is_snowing.plot()
```

Output:

<div>
<img src="/img/snow_plot.png" alt="Plot which borough has the most noise complaints" />
</div>

