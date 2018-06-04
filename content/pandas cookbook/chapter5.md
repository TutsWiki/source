---
date: 2017-05-18
linktitle: Chapter 5
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter6
prev: /pandas-cookbook/chapter4
title: Chapter 5 - Web scraping with Pandas
weight: 30
url: /pandas-cookbook/chapter5
description: Web scraping with Pandas
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
import numpy as np

pd.set_option('display.mpl_style', 'default')
plt.rcParams['figure.figsize'] = (15, 3)
plt.rcParams['font.family'] = 'sans-serif'
```

## Summary

By the end of this chapter, we're going to have downloaded all of Canada's weather data for 2012, and saved it to a CSV.

We'll do this by downloading it one month at a time, and then combining all the months together.

Here's the temperature every hour for 2012! Download [weather_2012.csv](https://tutswiki.com/weather_2012.csv) and try yourself.

```python
weather_2012_final = pd.read_csv('weather_2012.csv', index_col='Date/Time')
weather_2012_final['Temp (C)'].plot(figsize=(15, 6))
```

Output:

<div>
    <img src="/img/2012_temperature_chart.png" alt="Plot temperature data for 2012" />
</div>

## 5.1 Downloading one month of weather data

When playing with the cycling data, I wanted temperature and precipitation data to find out if people like biking when it's raining. So I went to the site for [Canadian historical weather data](http://climate.weather.gc.ca/index_e.html#access), and figured out how to get it automatically.

Here we're going to get the data for March 2012, and clean it up

Here's an URL template you can use to get data in Montreal.

```python
url_template = "http://climate.weather.gc.ca/climateData/bulkdata_e.html?format=csv&stationID=5415&Year={year}&Month={month}&timeframe=1&submit=Download+Data"
```

To get the data for March 2013, we need to format it with month=3, year=2012.

```python
url = url_template.format(month=3, year=2012)
weather_mar2012 = pd.read_csv(url, skiprows=15, index_col='Date/Time', parse_dates=True, encoding='latin1', header=True)
```

This is super great! We can just use the same read_csv function as before, and just give it a URL as a filename. Awesome.

There are 16 rows of metadata at the top of this CSV, but pandas knows CSVs are weird, so there's a skiprows options. We parse the dates again, and set 'Date/Time' to be the index column. Here's the resulting dataframe.

```python
weather_mar2012
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Month</th>
      <th>Day</th>
      <th>Time</th>
      <th>Data Quality</th>
      <th>Temp (Â°C)</th>
      <th>Temp Flag</th>
      <th>Dew Point Temp (Â°C)</th>
      <th>Dew Point Temp Flag</th>
      <th>Rel Hum (%)</th>
      <th>...</th>
      <th>Wind Spd Flag</th>
      <th>Visibility (km)</th>
      <th>Visibility Flag</th>
      <th>Stn Press (kPa)</th>
      <th>Stn Press Flag</th>
      <th>Hmdx</th>
      <th>Hmdx Flag</th>
      <th>Wind Chill</th>
      <th>Wind Chill Flag</th>
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
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2012-03-01 00:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 00:00</td>
      <td>  </td>
      <td>-5.5</td>
      <td>NaN</td>
      <td>-9.7</td>
      <td>NaN</td>
      <td> 72</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.0</td>
      <td>NaN</td>
      <td> 100.97</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 01:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 01:00</td>
      <td>  </td>
      <td>-5.7</td>
      <td>NaN</td>
      <td>-8.7</td>
      <td>NaN</td>
      <td> 79</td>
      <td>...</td>
      <td> NaN</td>
      <td>  2.4</td>
      <td>NaN</td>
      <td> 100.87</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 02:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 02:00</td>
      <td>  </td>
      <td>-5.4</td>
      <td>NaN</td>
      <td>-8.3</td>
      <td>NaN</td>
      <td> 80</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.8</td>
      <td>NaN</td>
      <td> 100.80</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 03:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 03:00</td>
      <td>  </td>
      <td>-4.7</td>
      <td>NaN</td>
      <td>-7.7</td>
      <td>NaN</td>
      <td> 79</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.0</td>
      <td>NaN</td>
      <td> 100.69</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 04:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 04:00</td>
      <td>  </td>
      <td>-5.4</td>
      <td>NaN</td>
      <td>-7.8</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.6</td>
      <td>NaN</td>
      <td> 100.62</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-14</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 05:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 05:00</td>
      <td>  </td>
      <td>-5.3</td>
      <td>NaN</td>
      <td>-7.9</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  2.4</td>
      <td>NaN</td>
      <td> 100.58</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-14</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 06:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 06:00</td>
      <td>  </td>
      <td>-5.2</td>
      <td>NaN</td>
      <td>-7.8</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.0</td>
      <td>NaN</td>
      <td> 100.57</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-14</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 07:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 07:00</td>
      <td>  </td>
      <td>-4.9</td>
      <td>NaN</td>
      <td>-7.4</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.6</td>
      <td>NaN</td>
      <td> 100.59</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 08:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 08:00</td>
      <td>  </td>
      <td>-5.0</td>
      <td>NaN</td>
      <td>-7.5</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.2</td>
      <td>NaN</td>
      <td> 100.59</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 09:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 09:00</td>
      <td>  </td>
      <td>-4.9</td>
      <td>NaN</td>
      <td>-7.5</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.6</td>
      <td>NaN</td>
      <td> 100.60</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 10:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 10:00</td>
      <td>  </td>
      <td>-4.7</td>
      <td>NaN</td>
      <td>-7.3</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.2</td>
      <td>NaN</td>
      <td> 100.62</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 11:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 11:00</td>
      <td>  </td>
      <td>-4.4</td>
      <td>NaN</td>
      <td>-6.8</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.0</td>
      <td>NaN</td>
      <td> 100.66</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 12:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 12:00</td>
      <td>  </td>
      <td>-4.3</td>
      <td>NaN</td>
      <td>-6.8</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.2</td>
      <td>NaN</td>
      <td> 100.66</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 13:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 13:00</td>
      <td>  </td>
      <td>-4.3</td>
      <td>NaN</td>
      <td>-6.9</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.2</td>
      <td>NaN</td>
      <td> 100.65</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 14:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 14:00</td>
      <td>  </td>
      <td>-3.9</td>
      <td>NaN</td>
      <td>-6.6</td>
      <td>NaN</td>
      <td> 81</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.2</td>
      <td>NaN</td>
      <td> 100.67</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-11</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 15:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 15:00</td>
      <td>  </td>
      <td>-3.3</td>
      <td>NaN</td>
      <td>-6.2</td>
      <td>NaN</td>
      <td> 80</td>
      <td>...</td>
      <td> NaN</td>
      <td>  1.6</td>
      <td>NaN</td>
      <td> 100.71</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-10</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 16:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 16:00</td>
      <td>  </td>
      <td>-2.7</td>
      <td>NaN</td>
      <td>-5.7</td>
      <td>NaN</td>
      <td> 80</td>
      <td>...</td>
      <td> NaN</td>
      <td>  2.4</td>
      <td>NaN</td>
      <td> 100.74</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -8</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 17:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 17:00</td>
      <td>  </td>
      <td>-2.9</td>
      <td>NaN</td>
      <td>-5.9</td>
      <td>NaN</td>
      <td> 80</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.0</td>
      <td>NaN</td>
      <td> 100.80</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -9</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 18:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 18:00</td>
      <td>  </td>
      <td>-3.0</td>
      <td>NaN</td>
      <td>-6.0</td>
      <td>NaN</td>
      <td> 80</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.0</td>
      <td>NaN</td>
      <td> 100.87</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -9</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 19:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 19:00</td>
      <td>  </td>
      <td>-3.6</td>
      <td>NaN</td>
      <td>-6.4</td>
      <td>NaN</td>
      <td> 81</td>
      <td>...</td>
      <td> NaN</td>
      <td>  3.2</td>
      <td>NaN</td>
      <td> 100.93</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -9</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 20:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 20:00</td>
      <td>  </td>
      <td>-3.7</td>
      <td>NaN</td>
      <td>-6.4</td>
      <td>NaN</td>
      <td> 81</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.8</td>
      <td>NaN</td>
      <td> 100.95</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-10</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 21:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 21:00</td>
      <td>  </td>
      <td>-3.9</td>
      <td>NaN</td>
      <td>-6.7</td>
      <td>NaN</td>
      <td> 81</td>
      <td>...</td>
      <td> NaN</td>
      <td>  6.4</td>
      <td>NaN</td>
      <td> 100.98</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-10</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 22:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 22:00</td>
      <td>  </td>
      <td>-4.3</td>
      <td>NaN</td>
      <td>-6.9</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  2.4</td>
      <td>NaN</td>
      <td> 101.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-11</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 23:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  1</td>
      <td> 23:00</td>
      <td>  </td>
      <td>-4.3</td>
      <td>NaN</td>
      <td>-7.1</td>
      <td>NaN</td>
      <td> 81</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.8</td>
      <td>NaN</td>
      <td> 101.04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-11</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-02 00:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  2</td>
      <td> 00:00</td>
      <td>  </td>
      <td>-4.8</td>
      <td>NaN</td>
      <td>-7.3</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  3.2</td>
      <td>NaN</td>
      <td> 101.04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-02 01:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  2</td>
      <td> 01:00</td>
      <td>  </td>
      <td>-5.3</td>
      <td>NaN</td>
      <td>-7.9</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.8</td>
      <td>NaN</td>
      <td> 101.09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-02 02:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  2</td>
      <td> 02:00</td>
      <td>  </td>
      <td>-5.2</td>
      <td>NaN</td>
      <td>-7.8</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  6.4</td>
      <td>NaN</td>
      <td> 101.11</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-02 03:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  2</td>
      <td> 03:00</td>
      <td>  </td>
      <td>-5.5</td>
      <td>NaN</td>
      <td>-7.9</td>
      <td>NaN</td>
      <td> 83</td>
      <td>...</td>
      <td> NaN</td>
      <td>  4.8</td>
      <td>NaN</td>
      <td> 101.15</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-02 04:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  2</td>
      <td> 04:00</td>
      <td>  </td>
      <td>-5.6</td>
      <td>NaN</td>
      <td>-8.2</td>
      <td>NaN</td>
      <td> 82</td>
      <td>...</td>
      <td> NaN</td>
      <td>  6.4</td>
      <td>NaN</td>
      <td> 101.15</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-13</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-03-02 05:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td>  2</td>
      <td> 05:00</td>
      <td>  </td>
      <td>-5.5</td>
      <td>NaN</td>
      <td>-8.3</td>
      <td>NaN</td>
      <td> 81</td>
      <td>...</td>
      <td> NaN</td>
      <td> 12.9</td>
      <td>NaN</td>
      <td> 101.15</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12</td>
      <td> NaN</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2012-03-30 18:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 30</td>
      <td> 18:00</td>
      <td>  </td>
      <td> 3.9</td>
      <td>NaN</td>
      <td>-7.9</td>
      <td>NaN</td>
      <td> 42</td>
      <td>...</td>
      <td> NaN</td>
      <td> 24.1</td>
      <td>NaN</td>
      <td> 101.26</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-30 19:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 30</td>
      <td> 19:00</td>
      <td>  </td>
      <td> 3.1</td>
      <td>NaN</td>
      <td>-6.7</td>
      <td>NaN</td>
      <td> 49</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.29</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-30 20:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 30</td>
      <td> 20:00</td>
      <td>  </td>
      <td> 3.0</td>
      <td>NaN</td>
      <td>-8.4</td>
      <td>NaN</td>
      <td> 43</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.30</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-30 21:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 30</td>
      <td> 21:00</td>
      <td>  </td>
      <td> 1.7</td>
      <td>NaN</td>
      <td>-9.0</td>
      <td>NaN</td>
      <td> 45</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.32</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-30 22:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 30</td>
      <td> 22:00</td>
      <td>  </td>
      <td> 0.4</td>
      <td>NaN</td>
      <td>-8.1</td>
      <td>NaN</td>
      <td> 53</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.30</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-30 23:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 30</td>
      <td> 23:00</td>
      <td>  </td>
      <td> 1.4</td>
      <td>NaN</td>
      <td>-7.7</td>
      <td>NaN</td>
      <td> 51</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 00:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 00:00</td>
      <td>  </td>
      <td> 1.5</td>
      <td>NaN</td>
      <td>-8.6</td>
      <td>NaN</td>
      <td> 47</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.33</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 01:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 01:00</td>
      <td>  </td>
      <td> 1.3</td>
      <td>NaN</td>
      <td>-9.6</td>
      <td>NaN</td>
      <td> 44</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.31</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 02:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 02:00</td>
      <td>  </td>
      <td> 1.3</td>
      <td>NaN</td>
      <td>-9.7</td>
      <td>NaN</td>
      <td> 44</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.29</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 03:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 03:00</td>
      <td>  </td>
      <td> 0.7</td>
      <td>NaN</td>
      <td>-8.8</td>
      <td>NaN</td>
      <td> 49</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.30</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 04:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 04:00</td>
      <td>  </td>
      <td>-0.9</td>
      <td>NaN</td>
      <td>-8.5</td>
      <td>NaN</td>
      <td> 56</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.32</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -5</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 05:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 05:00</td>
      <td>  </td>
      <td>-0.6</td>
      <td>NaN</td>
      <td>-9.2</td>
      <td>NaN</td>
      <td> 52</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 101.30</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -5</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 06:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 06:00</td>
      <td>  </td>
      <td>-0.5</td>
      <td>NaN</td>
      <td>-9.2</td>
      <td>NaN</td>
      <td> 52</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.32</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -5</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 07:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 07:00</td>
      <td>  </td>
      <td>-0.3</td>
      <td>NaN</td>
      <td>-9.2</td>
      <td>NaN</td>
      <td> 51</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.32</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> -5</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 08:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 08:00</td>
      <td>  </td>
      <td> 0.7</td>
      <td>NaN</td>
      <td>-8.5</td>
      <td>NaN</td>
      <td> 50</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.33</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 09:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 09:00</td>
      <td>  </td>
      <td> 1.5</td>
      <td>NaN</td>
      <td>-7.8</td>
      <td>NaN</td>
      <td> 50</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-03-31 10:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 10:00</td>
      <td>  </td>
      <td> 2.9</td>
      <td>NaN</td>
      <td>-8.1</td>
      <td>NaN</td>
      <td> 44</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.30</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 11:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 11:00</td>
      <td>  </td>
      <td> 4.6</td>
      <td>NaN</td>
      <td>-9.7</td>
      <td>NaN</td>
      <td> 35</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.24</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 12:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 12:00</td>
      <td>  </td>
      <td> 6.4</td>
      <td>NaN</td>
      <td>-7.1</td>
      <td>NaN</td>
      <td> 37</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 13:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 13:00</td>
      <td>  </td>
      <td> 6.5</td>
      <td>NaN</td>
      <td>-9.7</td>
      <td>NaN</td>
      <td> 30</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.08</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 14:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 14:00</td>
      <td>  </td>
      <td> 7.7</td>
      <td>NaN</td>
      <td>-8.5</td>
      <td>NaN</td>
      <td> 31</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 101.01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 15:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 15:00</td>
      <td>  </td>
      <td> 7.7</td>
      <td>NaN</td>
      <td>-8.6</td>
      <td>NaN</td>
      <td> 30</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 100.94</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 16:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 16:00</td>
      <td>  </td>
      <td> 8.4</td>
      <td>NaN</td>
      <td>-7.7</td>
      <td>NaN</td>
      <td> 31</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 100.89</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 17:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 17:00</td>
      <td>  </td>
      <td> 7.9</td>
      <td>NaN</td>
      <td>-8.1</td>
      <td>NaN</td>
      <td> 31</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 100.88</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 18:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 18:00</td>
      <td>  </td>
      <td> 7.0</td>
      <td>NaN</td>
      <td>-8.2</td>
      <td>NaN</td>
      <td> 33</td>
      <td>...</td>
      <td> NaN</td>
      <td> 48.3</td>
      <td>NaN</td>
      <td> 100.87</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 19:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 19:00</td>
      <td>  </td>
      <td> 5.9</td>
      <td>NaN</td>
      <td>-8.0</td>
      <td>NaN</td>
      <td> 36</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 100.88</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 20:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 20:00</td>
      <td>  </td>
      <td> 4.4</td>
      <td>NaN</td>
      <td>-7.2</td>
      <td>NaN</td>
      <td> 43</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 100.85</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 21:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 21:00</td>
      <td>  </td>
      <td> 2.6</td>
      <td>NaN</td>
      <td>-6.3</td>
      <td>NaN</td>
      <td> 52</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 100.86</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 22:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 22:00</td>
      <td>  </td>
      <td> 2.7</td>
      <td>NaN</td>
      <td>-6.7</td>
      <td>NaN</td>
      <td> 50</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 100.82</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-03-31 23:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 31</td>
      <td> 23:00</td>
      <td>  </td>
      <td> 1.5</td>
      <td>NaN</td>
      <td>-6.9</td>
      <td>NaN</td>
      <td> 54</td>
      <td>...</td>
      <td> NaN</td>
      <td> 25.0</td>
      <td>NaN</td>
      <td> 100.79</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td>         Clear</td>
    </tr>
  </tbody>
</table>
<p>744 rows × 24 columns</p>
</div>
</div>

Let's plot it!

```python
weather_mar2012[u"Temp (\xc2\xb0C)"].plot(figsize=(15, 5))
```

Output:

<div>
<img src="/img/march2017_weather_data.png" alt="Plot which borough has the most noise complaints" />
</div>

Notice how it goes up to 25° C in the middle there? That was a big deal. It was March, and people were wearing shorts outside.

And I was out of town and I missed it. Still sad, humans.

I had to write '\xb0' for that degree character °. Let's fix up the columns. We're going to just print them out, copy, and fix them up by hand.

```python
weather_mar2012.columns = [
    u'Year', u'Month', u'Day', u'Time', u'Data Quality', u'Temp (C)', 
    u'Temp Flag', u'Dew Point Temp (C)', u'Dew Point Temp Flag', 
    u'Rel Hum (%)', u'Rel Hum Flag', u'Wind Dir (10s deg)', u'Wind Dir Flag', 
    u'Wind Spd (km/h)', u'Wind Spd Flag', u'Visibility (km)', u'Visibility Flag',
    u'Stn Press (kPa)', u'Stn Press Flag', u'Hmdx', u'Hmdx Flag', u'Wind Chill', 
    u'Wind Chill Flag', u'Weather']
```

You'll notice in the summary above that there are a few columns which are are either entirely empty or only have a few values in them. Let's get rid of all of those with dropna.

The argument `axis=1` to `dropna` means "drop columns", not rows", and `how='any'` means "drop the column if any value is null".

This is much better now -- we only have columns with real data.

```python
weather_mar2012 = weather_mar2012.dropna(axis=1, how='any')
weather_mar2012[:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Month</th>
      <th>Day</th>
      <th>Time</th>
      <th>Data Quality</th>
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
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2012-03-01 00:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 1</td>
      <td> 00:00</td>
      <td>  </td>
      <td>-5.5</td>
      <td>-9.7</td>
      <td> 72</td>
      <td> 24</td>
      <td> 4.0</td>
      <td> 100.97</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 01:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 1</td>
      <td> 01:00</td>
      <td>  </td>
      <td>-5.7</td>
      <td>-8.7</td>
      <td> 79</td>
      <td> 26</td>
      <td> 2.4</td>
      <td> 100.87</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 02:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 1</td>
      <td> 02:00</td>
      <td>  </td>
      <td>-5.4</td>
      <td>-8.3</td>
      <td> 80</td>
      <td> 28</td>
      <td> 4.8</td>
      <td> 100.80</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 03:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 1</td>
      <td> 03:00</td>
      <td>  </td>
      <td>-4.7</td>
      <td>-7.7</td>
      <td> 79</td>
      <td> 28</td>
      <td> 4.0</td>
      <td> 100.69</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 04:00:00</th>
      <td> 2012</td>
      <td> 3</td>
      <td> 1</td>
      <td> 04:00</td>
      <td>  </td>
      <td>-5.4</td>
      <td>-7.8</td>
      <td> 83</td>
      <td> 35</td>
      <td> 1.6</td>
      <td> 100.62</td>
      <td> Snow</td>
    </tr>
  </tbody>
</table>
</div>
</div>

The Year/Month/Day/Time columns are redundant, though, and the Data Quality column doesn't look too useful. Let's get rid of those.

The `axis=1` argument means "Drop columns", like before. The default for operations like `dropna` and `drop` is always to operate on rows.

```python
weather_mar2012 = weather_mar2012.drop(['Year', 'Month', 'Day', 'Time', 'Data Quality'], axis=1)
weather_mar2012[:5]
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
      <th>2012-03-01 00:00:00</th>
      <td>-5.5</td>
      <td>-9.7</td>
      <td> 72</td>
      <td> 24</td>
      <td> 4.0</td>
      <td> 100.97</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 01:00:00</th>
      <td>-5.7</td>
      <td>-8.7</td>
      <td> 79</td>
      <td> 26</td>
      <td> 2.4</td>
      <td> 100.87</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 02:00:00</th>
      <td>-5.4</td>
      <td>-8.3</td>
      <td> 80</td>
      <td> 28</td>
      <td> 4.8</td>
      <td> 100.80</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 03:00:00</th>
      <td>-4.7</td>
      <td>-7.7</td>
      <td> 79</td>
      <td> 28</td>
      <td> 4.0</td>
      <td> 100.69</td>
      <td> Snow</td>
    </tr>
    <tr>
      <th>2012-03-01 04:00:00</th>
      <td>-5.4</td>
      <td>-7.8</td>
      <td> 83</td>
      <td> 35</td>
      <td> 1.6</td>
      <td> 100.62</td>
      <td> Snow</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Awesome! We now only have the relevant columns, and it's much more manageable.

## 5.2 Plotting the temperature by hour of day

This one's just for fun -- we've already done this before, using groupby and aggregate! We will learn whether or not it gets colder at night. Well, obviously. But let's do it anyway.

```python
temperatures = weather_mar2012[[u'Temp (C)']].copy()
print(temperatures.head)
temperatures.loc[:,'Hour'] = weather_mar2012.index.hour
temperatures.groupby('Hour').aggregate(np.median).plot()
```

Output:

```bash
Date/Time                    
2012-03-01 00:00:00      -5.5
2012-03-01 01:00:00      -5.7
2012-03-01 02:00:00      -5.4
2012-03-01 03:00:00      -4.7
2012-03-01 04:00:00      -5.4
2012-03-01 05:00:00      -5.3
2012-03-01 06:00:00      -5.2
2012-03-01 07:00:00      -4.9
2012-03-01 08:00:00      -5.0
2012-03-01 09:00:00      -4.9
2012-03-01 10:00:00      -4.7
2012-03-01 11:00:00      -4.4
2012-03-01 12:00:00      -4.3
2012-03-01 13:00:00      -4.3
2012-03-01 14:00:00      -3.9
2012-03-01 15:00:00      -3.3
2012-03-01 16:00:00      -2.7
2012-03-01 17:00:00      -2.9
2012-03-01 18:00:00      -3.0
2012-03-01 19:00:00      -3.6
2012-03-01 20:00:00      -3.7
2012-03-01 21:00:00      -3.9
2012-03-01 22:00:00      -4.3
2012-03-01 23:00:00      -4.3
2012-03-02 00:00:00      -4.8
2012-03-02 01:00:00      -5.3
2012-03-02 02:00:00      -5.2
2012-03-02 03:00:00      -5.5
2012-03-02 04:00:00      -5.6
2012-03-02 05:00:00      -5.5
...                       ...
2012-03-30 18:00:00       3.9
2012-03-30 19:00:00       3.1
2012-03-30 20:00:00       3.0
2012-03-30 21:00:00       1.7
2012-03-30 22:00:00       0.4
2012-03-30 23:00:00       1.4
2012-03-31 00:00:00       1.5
2012-03-31 01:00:00       1.3
2012-03-31 02:00:00       1.3
2012-03-31 03:00:00       0.7
2012-03-31 04:00:00      -0.9
2012-03-31 05:00:00      -0.6
2012-03-31 06:00:00      -0.5
2012-03-31 07:00:00      -0.3
2012-03-31 08:00:00       0.7
2012-03-31 09:00:00       1.5
2012-03-31 10:00:00       2.9
2012-03-31 11:00:00       4.6
2012-03-31 12:00:00       6.4
2012-03-31 13:00:00       6.5
2012-03-31 14:00:00       7.7
2012-03-31 15:00:00       7.7
2012-03-31 16:00:00       8.4
2012-03-31 17:00:00       7.9
2012-03-31 18:00:00       7.0
2012-03-31 19:00:00       5.9
2012-03-31 20:00:00       4.4
2012-03-31 21:00:00       2.6
2012-03-31 22:00:00       2.7
2012-03-31 23:00:00       1.5

[744 rows x 1 columns]>
```

<div>
    <img src="/img/lowest_temp_plot.png" alt="Plot dataframe as per weekdays" />
</div>

So it looks like the time with the highest median temperature is 2pm. Neat.

## 5.3 Getting the whole year of data

Okay, so what if we want the data for the whole year? Ideally the API would just let us download that, but I couldn't figure out a way to do that.

First, let's put our work from above into a function that gets the weather for a given month.

I noticed that there's an irritating bug where when I ask for January, it gives me data for the previous year, so we'll fix that too. [no, really. You can check =)]

```python
def download_weather_month(year, month):
    if month == 1:
        year += 1
    url = url_template.format(year=year, month=month)
    weather_data = pd.read_csv(url, skiprows=15, index_col='Date/Time', parse_dates=True, header=True)
    weather_data = weather_data.dropna(axis=1)
    weather_data.columns = [col.replace('\xb0', '') for col in weather_data.columns]
    weather_data = weather_data.drop(['Year', 'Day', 'Month', 'Time', 'Data Quality'], axis=1)
    return weather_data
```

We can test that this function does the right thing:

```python
download_weather_month(2012, 1)[:5]
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
      <th>2013-01-01 00:00:00</th>
      <td>-1.0</td>
      <td> -1.7</td>
      <td> 95</td>
      <td> 35</td>
      <td>  6.4</td>
      <td>  99.89</td>
      <td>         Snow</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td>-2.0</td>
      <td> -5.1</td>
      <td> 79</td>
      <td> 35</td>
      <td> 16.1</td>
      <td>  99.93</td>
      <td> Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td>-2.7</td>
      <td> -6.0</td>
      <td> 78</td>
      <td> 28</td>
      <td> 19.3</td>
      <td> 100.08</td>
      <td>         Snow</td>
    </tr>
    <tr>
      <th>2013-01-01 03:00:00</th>
      <td>-5.6</td>
      <td>-11.7</td>
      <td> 62</td>
      <td> 30</td>
      <td> 25.0</td>
      <td> 100.21</td>
      <td>        Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 04:00:00</th>
      <td>-7.7</td>
      <td>-12.6</td>
      <td> 68</td>
      <td> 35</td>
      <td> 19.3</td>
      <td> 100.32</td>
      <td> Mainly Clear</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Now we can get all the months at once. This will take a little while to run.

```python
data_by_month = [download_weather_month(2012, i) for i in range(1, 13)]
```

Once we have this, it's easy to concatenate all the dataframes together into one big dataframe using [pd.concat](http://pandas.pydata.org/pandas-docs/version/0.20/generated/pandas.concat.html). And now we have the whole year's data!

```python
weather_2012 = pd.concat(data_by_month)
weather_2012
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
      <th>2013-01-01 00:00:00</th>
      <td> -1.0</td>
      <td> -1.7</td>
      <td> 95</td>
      <td> 35</td>
      <td>  6.4</td>
      <td>  99.89</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2013-01-01 01:00:00</th>
      <td> -2.0</td>
      <td> -5.1</td>
      <td> 79</td>
      <td> 35</td>
      <td> 16.1</td>
      <td>  99.93</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 02:00:00</th>
      <td> -2.7</td>
      <td> -6.0</td>
      <td> 78</td>
      <td> 28</td>
      <td> 19.3</td>
      <td> 100.08</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2013-01-01 03:00:00</th>
      <td> -5.6</td>
      <td>-11.7</td>
      <td> 62</td>
      <td> 30</td>
      <td> 25.0</td>
      <td> 100.21</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 04:00:00</th>
      <td> -7.7</td>
      <td>-12.6</td>
      <td> 68</td>
      <td> 35</td>
      <td> 19.3</td>
      <td> 100.32</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 05:00:00</th>
      <td> -9.7</td>
      <td>-14.8</td>
      <td> 66</td>
      <td> 33</td>
      <td> 25.0</td>
      <td> 100.47</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 06:00:00</th>
      <td>-11.1</td>
      <td>-17.0</td>
      <td> 62</td>
      <td> 30</td>
      <td> 25.0</td>
      <td> 100.65</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 07:00:00</th>
      <td>-12.2</td>
      <td>-17.2</td>
      <td> 66</td>
      <td> 20</td>
      <td> 25.0</td>
      <td> 100.78</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 08:00:00</th>
      <td>-13.0</td>
      <td>-17.7</td>
      <td> 68</td>
      <td> 13</td>
      <td> 24.1</td>
      <td> 100.87</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 09:00:00</th>
      <td>-13.0</td>
      <td>-17.3</td>
      <td> 70</td>
      <td> 20</td>
      <td> 24.1</td>
      <td> 100.86</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 10:00:00</th>
      <td>-12.6</td>
      <td>-17.8</td>
      <td> 65</td>
      <td> 19</td>
      <td> 24.1</td>
      <td> 100.90</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 11:00:00</th>
      <td>-12.2</td>
      <td>-17.6</td>
      <td> 64</td>
      <td> 22</td>
      <td> 24.1</td>
      <td> 100.88</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 12:00:00</th>
      <td>-11.8</td>
      <td>-17.2</td>
      <td> 64</td>
      <td> 26</td>
      <td> 24.1</td>
      <td> 100.87</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 13:00:00</th>
      <td>-11.3</td>
      <td>-17.4</td>
      <td> 61</td>
      <td> 26</td>
      <td> 24.1</td>
      <td> 100.83</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 14:00:00</th>
      <td>-11.3</td>
      <td>-17.4</td>
      <td> 61</td>
      <td> 28</td>
      <td> 24.1</td>
      <td> 100.82</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 15:00:00</th>
      <td>-11.4</td>
      <td>-17.6</td>
      <td> 60</td>
      <td> 30</td>
      <td> 24.1</td>
      <td> 100.85</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 16:00:00</th>
      <td>-12.0</td>
      <td>-18.0</td>
      <td> 61</td>
      <td> 22</td>
      <td> 24.1</td>
      <td> 100.81</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 17:00:00</th>
      <td>-13.0</td>
      <td>-18.4</td>
      <td> 64</td>
      <td> 19</td>
      <td> 25.0</td>
      <td> 100.90</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 18:00:00</th>
      <td>-13.4</td>
      <td>-18.4</td>
      <td> 66</td>
      <td> 24</td>
      <td> 25.0</td>
      <td> 100.96</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 19:00:00</th>
      <td>-14.1</td>
      <td>-18.7</td>
      <td> 68</td>
      <td> 20</td>
      <td> 25.0</td>
      <td> 101.02</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 20:00:00</th>
      <td>-14.3</td>
      <td>-19.0</td>
      <td> 67</td>
      <td> 15</td>
      <td> 25.0</td>
      <td> 101.04</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 21:00:00</th>
      <td>-14.8</td>
      <td>-19.5</td>
      <td> 67</td>
      <td> 15</td>
      <td> 25.0</td>
      <td> 100.98</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2013-01-01 22:00:00</th>
      <td>-16.3</td>
      <td>-20.2</td>
      <td> 72</td>
      <td>  7</td>
      <td> 25.0</td>
      <td> 100.98</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2013-01-01 23:00:00</th>
      <td>-15.4</td>
      <td>-19.8</td>
      <td> 69</td>
      <td> 11</td>
      <td> 25.0</td>
      <td> 100.99</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2013-01-02 00:00:00</th>
      <td>-14.0</td>
      <td>-18.4</td>
      <td> 69</td>
      <td> 11</td>
      <td> 19.3</td>
      <td> 100.96</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2013-01-02 01:00:00</th>
      <td>-14.1</td>
      <td>-18.3</td>
      <td> 70</td>
      <td> 11</td>
      <td> 25.0</td>
      <td> 100.91</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2013-01-02 02:00:00</th>
      <td>-14.3</td>
      <td>-18.3</td>
      <td> 72</td>
      <td> 13</td>
      <td> 25.0</td>
      <td> 100.94</td>
      <td>  Snow Showers</td>
    </tr>
    <tr>
      <th>2013-01-02 03:00:00</th>
      <td>-14.7</td>
      <td>-18.0</td>
      <td> 76</td>
      <td>  9</td>
      <td> 19.3</td>
      <td> 100.91</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2013-01-02 04:00:00</th>
      <td>-14.2</td>
      <td>-17.1</td>
      <td> 79</td>
      <td>  6</td>
      <td>  9.7</td>
      <td> 100.83</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2013-01-02 05:00:00</th>
      <td>-14.3</td>
      <td>-17.0</td>
      <td> 80</td>
      <td>  0</td>
      <td>  6.4</td>
      <td> 100.81</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2012-12-30 18:00:00</th>
      <td>-12.6</td>
      <td>-16.0</td>
      <td> 76</td>
      <td> 24</td>
      <td> 25.0</td>
      <td> 101.36</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-12-30 19:00:00</th>
      <td>-13.4</td>
      <td>-16.5</td>
      <td> 77</td>
      <td> 26</td>
      <td> 25.0</td>
      <td> 101.47</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-12-30 20:00:00</th>
      <td>-13.8</td>
      <td>-16.5</td>
      <td> 80</td>
      <td> 24</td>
      <td> 25.0</td>
      <td> 101.52</td>
      <td>         Clear</td>
    </tr>
    <tr>
      <th>2012-12-30 21:00:00</th>
      <td>-13.8</td>
      <td>-16.5</td>
      <td> 80</td>
      <td> 20</td>
      <td> 25.0</td>
      <td> 101.50</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-12-30 22:00:00</th>
      <td>-13.7</td>
      <td>-16.3</td>
      <td> 81</td>
      <td> 19</td>
      <td> 25.0</td>
      <td> 101.54</td>
      <td>  Mainly Clear</td>
    </tr>
    <tr>
      <th>2012-12-30 23:00:00</th>
      <td>-12.1</td>
      <td>-15.1</td>
      <td> 78</td>
      <td> 28</td>
      <td> 25.0</td>
      <td> 101.52</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 00:00:00</th>
      <td>-11.1</td>
      <td>-14.4</td>
      <td> 77</td>
      <td> 26</td>
      <td> 25.0</td>
      <td> 101.51</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 01:00:00</th>
      <td>-10.7</td>
      <td>-14.0</td>
      <td> 77</td>
      <td> 15</td>
      <td> 25.0</td>
      <td> 101.50</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 02:00:00</th>
      <td>-10.1</td>
      <td>-13.4</td>
      <td> 77</td>
      <td>  9</td>
      <td> 25.0</td>
      <td> 101.45</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 03:00:00</th>
      <td>-11.8</td>
      <td>-14.4</td>
      <td> 81</td>
      <td>  6</td>
      <td> 25.0</td>
      <td> 101.42</td>
      <td> Mostly Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 04:00:00</th>
      <td>-10.5</td>
      <td>-12.8</td>
      <td> 83</td>
      <td> 11</td>
      <td> 25.0</td>
      <td> 101.34</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 05:00:00</th>
      <td>-10.2</td>
      <td>-12.4</td>
      <td> 84</td>
      <td>  6</td>
      <td> 25.0</td>
      <td> 101.28</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 06:00:00</th>
      <td> -9.7</td>
      <td>-11.7</td>
      <td> 85</td>
      <td>  4</td>
      <td> 25.0</td>
      <td> 101.23</td>
      <td>        Cloudy</td>
    </tr>
    <tr>
      <th>2012-12-31 07:00:00</th>
      <td> -9.3</td>
      <td>-11.3</td>
      <td> 85</td>
      <td>  0</td>
      <td> 19.3</td>
      <td> 101.19</td>
      <td>  Snow Showers</td>
    </tr>
    <tr>
      <th>2012-12-31 08:00:00</th>
      <td> -8.6</td>
      <td>-10.3</td>
      <td> 87</td>
      <td>  4</td>
      <td>  3.2</td>
      <td> 101.14</td>
      <td>  Snow Showers</td>
    </tr>
    <tr>
      <th>2012-12-31 09:00:00</th>
      <td> -8.1</td>
      <td> -9.6</td>
      <td> 89</td>
      <td>  4</td>
      <td>  2.4</td>
      <td> 101.09</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 10:00:00</th>
      <td> -7.4</td>
      <td> -8.9</td>
      <td> 89</td>
      <td>  4</td>
      <td>  6.4</td>
      <td> 101.05</td>
      <td>      Snow,Fog</td>
    </tr>
    <tr>
      <th>2012-12-31 11:00:00</th>
      <td> -6.7</td>
      <td> -7.9</td>
      <td> 91</td>
      <td>  9</td>
      <td>  9.7</td>
      <td> 100.93</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 12:00:00</th>
      <td> -5.8</td>
      <td> -7.5</td>
      <td> 88</td>
      <td>  4</td>
      <td> 12.9</td>
      <td> 100.78</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 13:00:00</th>
      <td> -4.6</td>
      <td> -6.6</td>
      <td> 86</td>
      <td>  4</td>
      <td> 12.9</td>
      <td> 100.63</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 14:00:00</th>
      <td> -3.4</td>
      <td> -5.7</td>
      <td> 84</td>
      <td>  6</td>
      <td> 11.3</td>
      <td> 100.57</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 15:00:00</th>
      <td> -2.3</td>
      <td> -4.6</td>
      <td> 84</td>
      <td>  9</td>
      <td>  9.7</td>
      <td> 100.47</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 16:00:00</th>
      <td> -1.4</td>
      <td> -4.0</td>
      <td> 82</td>
      <td> 13</td>
      <td> 12.9</td>
      <td> 100.40</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 17:00:00</th>
      <td> -1.1</td>
      <td> -3.3</td>
      <td> 85</td>
      <td> 19</td>
      <td>  9.7</td>
      <td> 100.30</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 18:00:00</th>
      <td> -1.3</td>
      <td> -3.1</td>
      <td> 88</td>
      <td> 17</td>
      <td>  9.7</td>
      <td> 100.19</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 19:00:00</th>
      <td>  0.1</td>
      <td> -2.7</td>
      <td> 81</td>
      <td> 30</td>
      <td>  9.7</td>
      <td> 100.13</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 20:00:00</th>
      <td>  0.2</td>
      <td> -2.4</td>
      <td> 83</td>
      <td> 24</td>
      <td>  9.7</td>
      <td> 100.03</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 21:00:00</th>
      <td> -0.5</td>
      <td> -1.5</td>
      <td> 93</td>
      <td> 28</td>
      <td>  4.8</td>
      <td>  99.95</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 22:00:00</th>
      <td> -0.2</td>
      <td> -1.8</td>
      <td> 89</td>
      <td> 28</td>
      <td>  9.7</td>
      <td>  99.91</td>
      <td>          Snow</td>
    </tr>
    <tr>
      <th>2012-12-31 23:00:00</th>
      <td>  0.0</td>
      <td> -2.1</td>
      <td> 86</td>
      <td> 30</td>
      <td> 11.3</td>
      <td>  99.89</td>
      <td>          Snow</td>
    </tr>
  </tbody>
</table>
<p>8784 rows × 7 columns</p>
</div>
</div>

## 5.4 Saving to a CSV

It's slow and unnecessary to download the data every time, so let's save our dataframe for later use!

```python
weather_2012.to_csv('weather_2012.csv')
```

And we're done!
