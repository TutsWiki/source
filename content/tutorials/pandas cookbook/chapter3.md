---
date: 2017-05-12
linktitle: Chapter 3
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter4
prev: /pandas-cookbook/chapter2
title: Chapter 3 - Filter dataframes
weight: 10
url: /pandas-cookbook/chapter3
description: Here we get into serious slicing and dicing and learn how to filter dataframes in complicated ways, really fast.
keywords:
  - pandas
  - slide
  - dice
  - filter dataframe
  - numpy
  - matplotlib
---

```python
# The usual preamble
%matplotlib inline
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# Make the graphs a bit prettier, and bigger
pd.set_option('display.mpl_style', 'default')
plt.rcParams['figure.figsize'] = (15, 5)


# This is necessary to show lots of columns in pandas 0.12. 
# Not necessary in pandas 0.13.
pd.set_option('display.width', 5000) 
pd.set_option('display.max_columns', 60)
```

Let's continue with our NYC 311 service requests example.

```python
complaints = pd.read_csv('311-service-requests.csv')
```

## 3.1 Selecting only noise complaints

I'd like to know which borough has the most noise complaints. First, we'll take a look at the data to see what it looks like:

```python
print complaints[:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unique Key</th>
      <th>Created Date</th>
      <th>Closed Date</th>
      <th>Agency</th>
      <th>Agency Name</th>
      <th>Complaint Type</th>
      <th>Descriptor</th>
      <th>Location Type</th>
      <th>Incident Zip</th>
      <th>Incident Address</th>
      <th>Street Name</th>
      <th>Cross Street 1</th>
      <th>Cross Street 2</th>
      <th>Intersection Street 1</th>
      <th>Intersection Street 2</th>
      <th>Address Type</th>
      <th>City</th>
      <th>Landmark</th>
      <th>Facility Type</th>
      <th>Status</th>
      <th>Due Date</th>
      <th>Resolution Action Updated Date</th>
      <th>Community Board</th>
      <th>Borough</th>
      <th>X Coordinate (State Plane)</th>
      <th>Y Coordinate (State Plane)</th>
      <th>Park Facility Name</th>
      <th>Park Borough</th>
      <th>School Name</th>
      <th>School Number</th>
      <th>School Region</th>
      <th>School Code</th>
      <th>School Phone Number</th>
      <th>School Address</th>
      <th>School City</th>
      <th>School State</th>
      <th>School Zip</th>
      <th>School Not Found</th>
      <th>School or Citywide Complaint</th>
      <th>Vehicle Type</th>
      <th>Taxi Company Borough</th>
      <th>Taxi Pick Up Location</th>
      <th>Bridge Highway Name</th>
      <th>Bridge Highway Direction</th>
      <th>Road Ramp</th>
      <th>Bridge Highway Segment</th>
      <th>Garage Lot Name</th>
      <th>Ferry Direction</th>
      <th>Ferry Terminal Name</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td> 26589651</td>
      <td> 10/31/2013 02:08:41 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>         New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td>                 Loud Talking</td>
      <td>     Street/Sidewalk</td>
      <td> 11432</td>
      <td> 90-03 169 STREET</td>
      <td>      169 STREET</td>
      <td>       90 AVENUE</td>
      <td>                        91 AVENUE</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   ADDRESS</td>
      <td>  JAMAICA</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 10:08:41 AM</td>
      <td> 10/31/2013 02:35:17 AM</td>
      <td>    12 QUEENS</td>
      <td>    QUEENS</td>
      <td> 1042027</td>
      <td> 197389</td>
      <td> Unspecified</td>
      <td>    QUEENS</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.708275</td>
      <td>-73.791604</td>
      <td>  (40.70827532593202, -73.79160395779721)</td>
    </tr>
    <tr>
      <th>1</th>
      <td> 26593698</td>
      <td> 10/31/2013 02:01:04 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>         New York City Police Department</td>
      <td>         Illegal Parking</td>
      <td> Commercial Overnight Parking</td>
      <td>     Street/Sidewalk</td>
      <td> 11378</td>
      <td>        58 AVENUE</td>
      <td>       58 AVENUE</td>
      <td>        58 PLACE</td>
      <td>                        59 STREET</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> BLOCKFACE</td>
      <td>  MASPETH</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 10:01:04 AM</td>
      <td>                    NaN</td>
      <td>    05 QUEENS</td>
      <td>    QUEENS</td>
      <td> 1009349</td>
      <td> 201984</td>
      <td> Unspecified</td>
      <td>    QUEENS</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.721041</td>
      <td>-73.909453</td>
      <td> (40.721040535628305, -73.90945306791765)</td>
    </tr>
    <tr>
      <th>2</th>
      <td> 26594139</td>
      <td> 10/31/2013 02:00:24 AM</td>
      <td> 10/31/2013 02:40:32 AM</td>
      <td>  NYPD</td>
      <td>         New York City Police Department</td>
      <td>      Noise - Commercial</td>
      <td>             Loud Music/Party</td>
      <td> Club/Bar/Restaurant</td>
      <td> 10032</td>
      <td>    4060 BROADWAY</td>
      <td>        BROADWAY</td>
      <td> WEST 171 STREET</td>
      <td>                  WEST 172 STREET</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   ADDRESS</td>
      <td> NEW YORK</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 10:00:24 AM</td>
      <td> 10/31/2013 02:39:42 AM</td>
      <td> 12 MANHATTAN</td>
      <td> MANHATTAN</td>
      <td> 1001088</td>
      <td> 246531</td>
      <td> Unspecified</td>
      <td> MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.843330</td>
      <td>-73.939144</td>
      <td>  (40.84332975466513, -73.93914371913482)</td>
    </tr>
    <tr>
      <th>3</th>
      <td> 26595721</td>
      <td> 10/31/2013 01:56:23 AM</td>
      <td> 10/31/2013 02:21:48 AM</td>
      <td>  NYPD</td>
      <td>         New York City Police Department</td>
      <td>         Noise - Vehicle</td>
      <td>               Car/Truck Horn</td>
      <td>     Street/Sidewalk</td>
      <td> 10023</td>
      <td>   WEST 72 STREET</td>
      <td>  WEST 72 STREET</td>
      <td> COLUMBUS AVENUE</td>
      <td>                 AMSTERDAM AVENUE</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> BLOCKFACE</td>
      <td> NEW YORK</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:56:23 AM</td>
      <td> 10/31/2013 02:21:10 AM</td>
      <td> 07 MANHATTAN</td>
      <td> MANHATTAN</td>
      <td>  989730</td>
      <td> 222727</td>
      <td> Unspecified</td>
      <td> MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.778009</td>
      <td>-73.980213</td>
      <td>   (40.7780087446372, -73.98021349023975)</td>
    </tr>
    <tr>
      <th>4</th>
      <td> 26590930</td>
      <td> 10/31/2013 01:53:44 AM</td>
      <td>                    NaN</td>
      <td> DOHMH</td>
      <td> Department of Health and Mental Hygiene</td>
      <td>                  Rodent</td>
      <td> Condition Attracting Rodents</td>
      <td>          Vacant Lot</td>
      <td> 10027</td>
      <td>  WEST 124 STREET</td>
      <td> WEST 124 STREET</td>
      <td>    LENOX AVENUE</td>
      <td> ADAM CLAYTON POWELL JR BOULEVARD</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> BLOCKFACE</td>
      <td> NEW YORK</td>
      <td> NaN</td>
      <td>      NaN</td>
      <td>  Pending</td>
      <td> 11/30/2013 01:53:44 AM</td>
      <td> 10/31/2013 01:59:54 AM</td>
      <td> 10 MANHATTAN</td>
      <td> MANHATTAN</td>
      <td>  998815</td>
      <td> 233545</td>
      <td> Unspecified</td>
      <td> MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.807691</td>
      <td>-73.947387</td>
      <td>  (40.80769092704951, -73.94738703491433)</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 52 columns</p>
</div>
</div>

To get the noise complaints, we need to find the rows where the "Complaint Type" column is "Noise - Street/Sidewalk". I'll show you how to do that, and then explain what's going on.

```python
noise_complaints = complaints[complaints['Complaint Type'] == "Noise - Street/Sidewalk"]
print noise_complaints[:3]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unique Key</th>
      <th>Created Date</th>
      <th>Closed Date</th>
      <th>Agency</th>
      <th>Agency Name</th>
      <th>Complaint Type</th>
      <th>Descriptor</th>
      <th>Location Type</th>
      <th>Incident Zip</th>
      <th>Incident Address</th>
      <th>Street Name</th>
      <th>Cross Street 1</th>
      <th>Cross Street 2</th>
      <th>Intersection Street 1</th>
      <th>Intersection Street 2</th>
      <th>Address Type</th>
      <th>City</th>
      <th>Landmark</th>
      <th>Facility Type</th>
      <th>Status</th>
      <th>Due Date</th>
      <th>Resolution Action Updated Date</th>
      <th>Community Board</th>
      <th>Borough</th>
      <th>X Coordinate (State Plane)</th>
      <th>Y Coordinate (State Plane)</th>
      <th>Park Facility Name</th>
      <th>Park Borough</th>
      <th>School Name</th>
      <th>School Number</th>
      <th>School Region</th>
      <th>School Code</th>
      <th>School Phone Number</th>
      <th>School Address</th>
      <th>School City</th>
      <th>School State</th>
      <th>School Zip</th>
      <th>School Not Found</th>
      <th>School or Citywide Complaint</th>
      <th>Vehicle Type</th>
      <th>Taxi Company Borough</th>
      <th>Taxi Pick Up Location</th>
      <th>Bridge Highway Name</th>
      <th>Bridge Highway Direction</th>
      <th>Road Ramp</th>
      <th>Bridge Highway Segment</th>
      <th>Garage Lot Name</th>
      <th>Ferry Direction</th>
      <th>Ferry Terminal Name</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0 </th>
      <td> 26589651</td>
      <td> 10/31/2013 02:08:41 AM</td>
      <td>                    NaN</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td>     Loud Talking</td>
      <td> Street/Sidewalk</td>
      <td> 11432</td>
      <td>    90-03 169 STREET</td>
      <td>      169 STREET</td>
      <td>        90 AVENUE</td>
      <td>    91 AVENUE</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> ADDRESS</td>
      <td>       JAMAICA</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 10:08:41 AM</td>
      <td> 10/31/2013 02:35:17 AM</td>
      <td>        12 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1042027</td>
      <td> 197389</td>
      <td> Unspecified</td>
      <td>        QUEENS</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.708275</td>
      <td>-73.791604</td>
      <td> (40.70827532593202, -73.79160395779721)</td>
    </tr>
    <tr>
      <th>16</th>
      <td> 26594086</td>
      <td> 10/31/2013 12:54:03 AM</td>
      <td> 10/31/2013 02:16:39 AM</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td> Loud Music/Party</td>
      <td> Street/Sidewalk</td>
      <td> 10310</td>
      <td> 173 CAMPBELL AVENUE</td>
      <td> CAMPBELL AVENUE</td>
      <td> HENDERSON AVENUE</td>
      <td> WINEGAR LANE</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> ADDRESS</td>
      <td> STATEN ISLAND</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:54:03 AM</td>
      <td> 10/31/2013 02:07:14 AM</td>
      <td> 01 STATEN ISLAND</td>
      <td> STATEN ISLAND</td>
      <td>  952013</td>
      <td> 171076</td>
      <td> Unspecified</td>
      <td> STATEN ISLAND</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.636182</td>
      <td>-74.116150</td>
      <td>  (40.63618202176914, -74.1161500428337)</td>
    </tr>
    <tr>
      <th>25</th>
      <td> 26591573</td>
      <td> 10/31/2013 12:35:18 AM</td>
      <td> 10/31/2013 02:41:35 AM</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td>     Loud Talking</td>
      <td> Street/Sidewalk</td>
      <td> 10312</td>
      <td>   24 PRINCETON LANE</td>
      <td>  PRINCETON LANE</td>
      <td>    HAMPTON GREEN</td>
      <td>     DEAD END</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> ADDRESS</td>
      <td> STATEN ISLAND</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:35:18 AM</td>
      <td> 10/31/2013 01:45:17 AM</td>
      <td> 03 STATEN ISLAND</td>
      <td> STATEN ISLAND</td>
      <td>  929577</td>
      <td> 140964</td>
      <td> Unspecified</td>
      <td> STATEN ISLAND</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.553421</td>
      <td>-74.196743</td>
      <td> (40.55342078716953, -74.19674315017886)</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 52 columns</p>
</div>
</div>

If you look at noise_complaints, you'll see that this worked, and it only contains complaints with the right complaint type. But how does this work? Let's deconstruct it into two pieces

```python
complaints['Complaint Type'] == "Noise - Street/Sidewalk"
```

Output:

```bash
0      True
1     False
2     False
3     False
4     False
5     False
6     False
7     False
8     False
9     False
10    False
11    False
12    False
13    False
14    False
...
111054     True
111055    False
111056    False
111057    False
111058    False
111059     True
111060    False
111061    False
111062    False
111063    False
111064    False
111065    False
111066     True
111067    False
111068    False
Name: Complaint Type, Length: 111069, dtype: bool
```

This is a big array of Trues and Falses, one for each row in our dataframe. When we index our dataframe with this array, we get just the rows where our boolean array evaluated to True. It's important to note that for row filtering by a boolean array the length of our dataframe's index must be the same length as the boolean array used for filtering.

You can also combine more than one condition with the & operator like this:

```python
is_noise = complaints['Complaint Type'] == "Noise - Street/Sidewalk"
in_brooklyn = complaints['Borough'] == "BROOKLYN"
print complaints[is_noise & in_brooklyn][:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unique Key</th>
      <th>Created Date</th>
      <th>Closed Date</th>
      <th>Agency</th>
      <th>Agency Name</th>
      <th>Complaint Type</th>
      <th>Descriptor</th>
      <th>Location Type</th>
      <th>Incident Zip</th>
      <th>Incident Address</th>
      <th>Street Name</th>
      <th>Cross Street 1</th>
      <th>Cross Street 2</th>
      <th>Intersection Street 1</th>
      <th>Intersection Street 2</th>
      <th>Address Type</th>
      <th>City</th>
      <th>Landmark</th>
      <th>Facility Type</th>
      <th>Status</th>
      <th>Due Date</th>
      <th>Resolution Action Updated Date</th>
      <th>Community Board</th>
      <th>Borough</th>
      <th>X Coordinate (State Plane)</th>
      <th>Y Coordinate (State Plane)</th>
      <th>Park Facility Name</th>
      <th>Park Borough</th>
      <th>School Name</th>
      <th>School Number</th>
      <th>School Region</th>
      <th>School Code</th>
      <th>School Phone Number</th>
      <th>School Address</th>
      <th>School City</th>
      <th>School State</th>
      <th>School Zip</th>
      <th>School Not Found</th>
      <th>School or Citywide Complaint</th>
      <th>Vehicle Type</th>
      <th>Taxi Company Borough</th>
      <th>Taxi Pick Up Location</th>
      <th>Bridge Highway Name</th>
      <th>Bridge Highway Direction</th>
      <th>Road Ramp</th>
      <th>Bridge Highway Segment</th>
      <th>Garage Lot Name</th>
      <th>Ferry Direction</th>
      <th>Ferry Terminal Name</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31 </th>
      <td> 26595564</td>
      <td> 10/31/2013 12:30:36 AM</td>
      <td>                    NaN</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td> Loud Music/Party</td>
      <td> Street/Sidewalk</td>
      <td> 11236</td>
      <td>           AVENUE J</td>
      <td>        AVENUE J</td>
      <td>    EAST 80 STREET</td>
      <td> EAST 81 STREET</td>
      <td>           NaN</td>
      <td>           NaN</td>
      <td>    BLOCKFACE</td>
      <td> BROOKLYN</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td>   Open</td>
      <td> 10/31/2013 08:30:36 AM</td>
      <td>                    NaN</td>
      <td> 18 BROOKLYN</td>
      <td> BROOKLYN</td>
      <td> 1008937</td>
      <td> 170310</td>
      <td> Unspecified</td>
      <td> BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.634104</td>
      <td>-73.911055</td>
      <td> (40.634103775951736, -73.91105541883589)</td>
    </tr>
    <tr>
      <th>49 </th>
      <td> 26595553</td>
      <td> 10/31/2013 12:05:10 AM</td>
      <td> 10/31/2013 02:43:43 AM</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td>     Loud Talking</td>
      <td> Street/Sidewalk</td>
      <td> 11225</td>
      <td> 25 LEFFERTS AVENUE</td>
      <td> LEFFERTS AVENUE</td>
      <td> WASHINGTON AVENUE</td>
      <td> BEDFORD AVENUE</td>
      <td>           NaN</td>
      <td>           NaN</td>
      <td>      ADDRESS</td>
      <td> BROOKLYN</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td> Closed</td>
      <td> 10/31/2013 08:05:10 AM</td>
      <td> 10/31/2013 01:29:29 AM</td>
      <td> 09 BROOKLYN</td>
      <td> BROOKLYN</td>
      <td>  995366</td>
      <td> 180388</td>
      <td> Unspecified</td>
      <td> BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.661793</td>
      <td>-73.959934</td>
      <td>   (40.6617931276793, -73.95993363978067)</td>
    </tr>
    <tr>
      <th>109</th>
      <td> 26594653</td>
      <td> 10/30/2013 11:26:32 PM</td>
      <td> 10/31/2013 12:18:54 AM</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td> Loud Music/Party</td>
      <td> Street/Sidewalk</td>
      <td> 11222</td>
      <td>                NaN</td>
      <td>             NaN</td>
      <td>               NaN</td>
      <td>            NaN</td>
      <td> DOBBIN STREET</td>
      <td> NORMAN STREET</td>
      <td> INTERSECTION</td>
      <td> BROOKLYN</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td> Closed</td>
      <td> 10/31/2013 07:26:32 AM</td>
      <td> 10/31/2013 12:18:54 AM</td>
      <td> 01 BROOKLYN</td>
      <td> BROOKLYN</td>
      <td>  996925</td>
      <td> 203271</td>
      <td> Unspecified</td>
      <td> BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.724600</td>
      <td>-73.954271</td>
      <td> (40.724599563793525, -73.95427134534344)</td>
    </tr>
    <tr>
      <th>236</th>
      <td> 26591992</td>
      <td> 10/30/2013 10:02:58 PM</td>
      <td> 10/30/2013 10:23:20 PM</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td>     Loud Talking</td>
      <td> Street/Sidewalk</td>
      <td> 11218</td>
      <td>      DITMAS AVENUE</td>
      <td>   DITMAS AVENUE</td>
      <td>               NaN</td>
      <td>            NaN</td>
      <td>           NaN</td>
      <td>           NaN</td>
      <td>      LATLONG</td>
      <td> BROOKLYN</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td> Closed</td>
      <td> 10/31/2013 06:02:58 AM</td>
      <td> 10/30/2013 10:23:20 PM</td>
      <td> 01 BROOKLYN</td>
      <td> BROOKLYN</td>
      <td>  991895</td>
      <td> 171051</td>
      <td> Unspecified</td>
      <td> BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.636169</td>
      <td>-73.972455</td>
      <td>  (40.63616876563881, -73.97245504682485)</td>
    </tr>
    <tr>
      <th>370</th>
      <td> 26594167</td>
      <td> 10/30/2013 08:38:25 PM</td>
      <td> 10/30/2013 10:26:28 PM</td>
      <td> NYPD</td>
      <td> New York City Police Department</td>
      <td> Noise - Street/Sidewalk</td>
      <td> Loud Music/Party</td>
      <td> Street/Sidewalk</td>
      <td> 11218</td>
      <td>   126 BEVERLY ROAD</td>
      <td>    BEVERLY ROAD</td>
      <td>     CHURCH AVENUE</td>
      <td>  EAST 2 STREET</td>
      <td>           NaN</td>
      <td>           NaN</td>
      <td>      ADDRESS</td>
      <td> BROOKLYN</td>
      <td> NaN</td>
      <td> Precinct</td>
      <td> Closed</td>
      <td> 10/31/2013 04:38:25 AM</td>
      <td> 10/30/2013 10:26:28 PM</td>
      <td> 12 BROOKLYN</td>
      <td> BROOKLYN</td>
      <td>  990144</td>
      <td> 173511</td>
      <td> Unspecified</td>
      <td> BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.642922</td>
      <td>-73.978762</td>
      <td>   (40.6429222774404, -73.97876175474585)</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 52 columns</p>
</div>
</div>

Or if we just wanted a few columns:

```python
print complaints[is_noise & in_brooklyn][['Complaint Type', 'Borough', 'Created Date', 'Descriptor']][:10]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Complaint Type</th>
      <th>Borough</th>
      <th>Created Date</th>
      <th>Descriptor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31  </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/31/2013 12:30:36 AM</td>
      <td> Loud Music/Party</td>
    </tr>
    <tr>
      <th>49  </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/31/2013 12:05:10 AM</td>
      <td>     Loud Talking</td>
    </tr>
    <tr>
      <th>109 </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/30/2013 11:26:32 PM</td>
      <td> Loud Music/Party</td>
    </tr>
    <tr>
      <th>236 </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/30/2013 10:02:58 PM</td>
      <td>     Loud Talking</td>
    </tr>
    <tr>
      <th>370 </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/30/2013 08:38:25 PM</td>
      <td> Loud Music/Party</td>
    </tr>
    <tr>
      <th>378 </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/30/2013 08:32:13 PM</td>
      <td>     Loud Talking</td>
    </tr>
    <tr>
      <th>656 </th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/30/2013 06:07:39 PM</td>
      <td> Loud Music/Party</td>
    </tr>
    <tr>
      <th>1251</th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/30/2013 03:04:51 PM</td>
      <td>     Loud Talking</td>
    </tr>
    <tr>
      <th>5416</th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/29/2013 10:07:02 PM</td>
      <td>     Loud Talking</td>
    </tr>
    <tr>
      <th>5584</th>
      <td> Noise - Street/Sidewalk</td>
      <td> BROOKLYN</td>
      <td> 10/29/2013 08:15:59 PM</td>
      <td> Loud Music/Party</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 4 columns</p>
</div>
</div>

## 3.2 A digression about numpy arrays

