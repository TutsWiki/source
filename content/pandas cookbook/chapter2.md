---
date: 2017-05-11
linktitle: Chapter 2
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter3
prev: /pandas-cookbook/chapter1
title: Chapter 2 - Selecting and finding desired data
weight: 15
url: /pandas-cookbook/chapter2
description: Select data from a pandas dataframe, take slices and get columns
keywords:
  - pandas
  - csv
  - read csv pandas
  - read column pandas
  - select column
  - plot column
---

```python
# The usual preamble
%matplotlib inline
import pandas as pd
import matplotlib.pyplot as plt

# Make the graphs a bit prettier, and bigger
pd.set_option('display.mpl_style', 'default')

# This is necessary to show lots of columns in pandas 0.12. 
# Not necessary in pandas 0.13.
pd.set_option('display.width', 5000) 
pd.set_option('display.max_columns', 60)

plt.rcParams['figure.figsize'] = (15, 5)
```

We're going to use a new dataset here, to demonstrate how to deal with larger datasets. This is a subset of the of 311 service requests from [NYC Open Data](https://nycopendata.socrata.com/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9). Download the file [311-service-requests.csv](/311-service-requests.csv).

```python
complaints = pd.read_csv('311-service-requests.csv')
```

Depending on your pandas version, you might see an error like `"DtypeWarning: Columns (8) have mixed types"`. This means that it's encountered a problem reading in our data. In this case it almost certainly means that it has columns where some of the entries are strings and some are integers.

For now we're going to ignore it and hope we don't run into a problem, but in the long run we'd need to investigate this warning.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 2.1 What's even in it? (the summary)
When you print a large dataframe, it will only show you the first few rows.
If you don't see this, don't panic! The default behavior for large dataframes changed between pandas 0.12 and 0.13. Previous to 0.13 it would show you a summary of the dataframe. This includes all the columns, and how many non-null values there are in each column.

```python
complaints
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
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                     Loud Talking</td>
      <td>               Street/Sidewalk</td>
      <td> 11432</td>
      <td>          90-03 169 STREET</td>
      <td>          169 STREET</td>
      <td>         90 AVENUE</td>
      <td>                        91 AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>             JAMAICA</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 10:08:41 AM</td>
      <td> 10/31/2013 02:35:17 AM</td>
      <td>            12 QUEENS</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
      <th>1 </th>
      <td> 26593698</td>
      <td> 10/31/2013 02:01:04 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>          Illegal Parking</td>
      <td>     Commercial Overnight Parking</td>
      <td>               Street/Sidewalk</td>
      <td> 11378</td>
      <td>                 58 AVENUE</td>
      <td>           58 AVENUE</td>
      <td>          58 PLACE</td>
      <td>                        59 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>             MASPETH</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 10:01:04 AM</td>
      <td>                    NaN</td>
      <td>            05 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1009349</td>
      <td> 201984</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
      <th>2 </th>
      <td> 26594139</td>
      <td> 10/31/2013 02:00:24 AM</td>
      <td> 10/31/2013 02:40:32 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10032</td>
      <td>             4060 BROADWAY</td>
      <td>            BROADWAY</td>
      <td>   WEST 171 STREET</td>
      <td>                  WEST 172 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 10:00:24 AM</td>
      <td> 10/31/2013 02:39:42 AM</td>
      <td>         12 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td> 1001088</td>
      <td> 246531</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
      <th>3 </th>
      <td> 26595721</td>
      <td> 10/31/2013 01:56:23 AM</td>
      <td> 10/31/2013 02:21:48 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>          Noise - Vehicle</td>
      <td>                   Car/Truck Horn</td>
      <td>               Street/Sidewalk</td>
      <td> 10023</td>
      <td>            WEST 72 STREET</td>
      <td>      WEST 72 STREET</td>
      <td>   COLUMBUS AVENUE</td>
      <td>                 AMSTERDAM AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:56:23 AM</td>
      <td> 10/31/2013 02:21:10 AM</td>
      <td>         07 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  989730</td>
      <td> 222727</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
      <th>4 </th>
      <td> 26590930</td>
      <td> 10/31/2013 01:53:44 AM</td>
      <td>                    NaN</td>
      <td> DOHMH</td>
      <td>           Department of Health and Mental Hygiene</td>
      <td>                   Rodent</td>
      <td>     Condition Attracting Rodents</td>
      <td>                    Vacant Lot</td>
      <td> 10027</td>
      <td>           WEST 124 STREET</td>
      <td>     WEST 124 STREET</td>
      <td>      LENOX AVENUE</td>
      <td> ADAM CLAYTON POWELL JR BOULEVARD</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>  Pending</td>
      <td> 11/30/2013 01:53:44 AM</td>
      <td> 10/31/2013 01:59:54 AM</td>
      <td>         10 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  998815</td>
      <td> 233545</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
    <tr>
      <th>5 </th>
      <td> 26592370</td>
      <td> 10/31/2013 01:46:52 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Banging/Pounding</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11372</td>
      <td>                 37 AVENUE</td>
      <td>           37 AVENUE</td>
      <td>         84 STREET</td>
      <td>                        85 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>     JACKSON HEIGHTS</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 09:46:52 AM</td>
      <td>                    NaN</td>
      <td>            03 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1016948</td>
      <td> 212540</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.749989</td>
      <td>-73.881988</td>
      <td>   (40.7499893014072, -73.88198770727831)</td>
    </tr>
    <tr>
      <th>6 </th>
      <td> 26595682</td>
      <td> 10/31/2013 01:46:40 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>         Blocked Driveway</td>
      <td>                        No Access</td>
      <td>               Street/Sidewalk</td>
      <td> 11419</td>
      <td>         107-50 109 STREET</td>
      <td>          109 STREET</td>
      <td>        107 AVENUE</td>
      <td>                       109 AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td> SOUTH RICHMOND HILL</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 09:46:40 AM</td>
      <td> 10/31/2013 01:59:51 AM</td>
      <td>            10 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1030919</td>
      <td> 187622</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.681533</td>
      <td>-73.831737</td>
      <td>  (40.68153278675525, -73.83173699701601)</td>
    </tr>
    <tr>
      <th>7 </th>
      <td> 26595195</td>
      <td> 10/31/2013 01:44:19 AM</td>
      <td> 10/31/2013 01:58:49 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11417</td>
      <td> 137-09 CROSSBAY BOULEVARD</td>
      <td>  CROSSBAY BOULEVARD</td>
      <td>     PITKIN AVENUE</td>
      <td>                 VAN WICKLEN ROAD</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>          OZONE PARK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:44:19 AM</td>
      <td> 10/31/2013 01:58:49 AM</td>
      <td>            10 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1027776</td>
      <td> 184076</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.671816</td>
      <td>-73.843092</td>
      <td>  (40.67181584567338, -73.84309181950769)</td>
    </tr>
    <tr>
      <th>8 </th>
      <td> 26590540</td>
      <td> 10/31/2013 01:44:14 AM</td>
      <td> 10/31/2013 02:28:04 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                     Loud Talking</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10011</td>
      <td>        258 WEST 15 STREET</td>
      <td>      WEST 15 STREET</td>
      <td>          7 AVENUE</td>
      <td>                         8 AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:44:14 AM</td>
      <td> 10/31/2013 02:00:56 AM</td>
      <td>         04 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  984031</td>
      <td> 208847</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.739913</td>
      <td>-74.000790</td>
      <td>  (40.73991339303542, -74.00079028612932)</td>
    </tr>
    <tr>
      <th>9 </th>
      <td> 26594392</td>
      <td> 10/31/2013 01:34:41 AM</td>
      <td> 10/31/2013 02:23:51 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11225</td>
      <td>       835 NOSTRAND AVENUE</td>
      <td>     NOSTRAND AVENUE</td>
      <td>      UNION STREET</td>
      <td>                 PRESIDENT STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:34:41 AM</td>
      <td> 10/31/2013 01:48:26 AM</td>
      <td>          09 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  997941</td>
      <td> 182725</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.668204</td>
      <td>-73.950648</td>
      <td>  (40.66820406598287, -73.95064760056546)</td>
    </tr>
    <tr>
      <th>10</th>
      <td> 26595176</td>
      <td> 10/31/2013 01:25:12 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td> Noise - House of Worship</td>
      <td>                 Loud Music/Party</td>
      <td>              House of Worship</td>
      <td> 11218</td>
      <td>            3775 18 AVENUE</td>
      <td>           18 AVENUE</td>
      <td>     EAST 9 STREET</td>
      <td>                    EAST 8 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 09:25:12 AM</td>
      <td>                    NaN</td>
      <td>          14 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  992726</td>
      <td> 170399</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.634378</td>
      <td>-73.969462</td>
      <td>  (40.63437840816299, -73.96946177104543)</td>
    </tr>
    <tr>
      <th>11</th>
      <td> 26591982</td>
      <td> 10/31/2013 01:24:14 AM</td>
      <td> 10/31/2013 01:54:39 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10003</td>
      <td>              187 2 AVENUE</td>
      <td>            2 AVENUE</td>
      <td>    EAST 11 STREET</td>
      <td>                   EAST 12 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:24:14 AM</td>
      <td> 10/31/2013 01:54:39 AM</td>
      <td>         03 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  988110</td>
      <td> 205533</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.730816</td>
      <td>-73.986073</td>
      <td>  (40.73081644089586, -73.98607265739876)</td>
    </tr>
    <tr>
      <th>12</th>
      <td> 26594169</td>
      <td> 10/31/2013 01:20:57 AM</td>
      <td> 10/31/2013 02:12:31 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>          Illegal Parking</td>
      <td>   Double Parked Blocking Vehicle</td>
      <td>               Street/Sidewalk</td>
      <td> 10029</td>
      <td>         65 EAST 99 STREET</td>
      <td>      EAST 99 STREET</td>
      <td>    MADISON AVENUE</td>
      <td>                      PARK AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:20:57 AM</td>
      <td> 10/31/2013 01:42:05 AM</td>
      <td>         11 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  997470</td>
      <td> 226725</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.788974</td>
      <td>-73.952259</td>
      <td>  (40.78897400211689, -73.95225898702977)</td>
    </tr>
    <tr>
      <th>13</th>
      <td> 26594391</td>
      <td> 10/31/2013 01:20:13 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>          Noise - Vehicle</td>
      <td>                    Engine Idling</td>
      <td>               Street/Sidewalk</td>
      <td> 10466</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>    STRANG AVENUE</td>
      <td>  AMUNDSON AVENUE</td>
      <td> INTERSECTION</td>
      <td>               BRONX</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 09:20:13 AM</td>
      <td>                    NaN</td>
      <td>             12 BRONX</td>
      <td>         BRONX</td>
      <td> 1029467</td>
      <td> 264124</td>
      <td> Unspecified</td>
      <td>         BRONX</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.891517</td>
      <td>-73.836457</td>
      <td>  (40.89151738488846, -73.83645714593568)</td>
    </tr>
    <tr>
      <th>14</th>
      <td> 26590917</td>
      <td> 10/31/2013 01:19:54 AM</td>
      <td>                    NaN</td>
      <td> DOHMH</td>
      <td>           Department of Health and Mental Hygiene</td>
      <td>                   Rodent</td>
      <td>                     Rat Sighting</td>
      <td> 1-2 Family Mixed Use Building</td>
      <td> 11219</td>
      <td>                 63 STREET</td>
      <td>           63 STREET</td>
      <td>         13 AVENUE</td>
      <td>                        14 AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>  Pending</td>
      <td> 11/30/2013 01:19:54 AM</td>
      <td> 10/31/2013 01:29:26 AM</td>
      <td>          10 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  984467</td>
      <td> 167519</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.626477</td>
      <td>-73.999218</td>
      <td>   (40.6264774690411, -73.99921826202639)</td>
    </tr>
    <tr>
      <th>15</th>
      <td> 26591458</td>
      <td> 10/31/2013 01:14:02 AM</td>
      <td> 10/31/2013 01:30:34 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td> Noise - House of Worship</td>
      <td>                 Loud Music/Party</td>
      <td>              House of Worship</td>
      <td> 10025</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td> WEST   99 STREET</td>
      <td>         BROADWAY</td>
      <td> INTERSECTION</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 09:14:02 AM</td>
      <td> 10/31/2013 01:30:34 AM</td>
      <td>         07 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  992454</td>
      <td> 229500</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.796597</td>
      <td>-73.970370</td>
      <td>   (40.7965967075252, -73.97036973473399)</td>
    </tr>
    <tr>
      <th>16</th>
      <td> 26594086</td>
      <td> 10/31/2013 12:54:03 AM</td>
      <td> 10/31/2013 02:16:39 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                 Loud Music/Party</td>
      <td>               Street/Sidewalk</td>
      <td> 10310</td>
      <td>       173 CAMPBELL AVENUE</td>
      <td>     CAMPBELL AVENUE</td>
      <td>  HENDERSON AVENUE</td>
      <td>                     WINEGAR LANE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>       STATEN ISLAND</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:54:03 AM</td>
      <td> 10/31/2013 02:07:14 AM</td>
      <td>     01 STATEN ISLAND</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.636182</td>
      <td>-74.116150</td>
      <td>   (40.63618202176914, -74.1161500428337)</td>
    </tr>
    <tr>
      <th>17</th>
      <td> 26595117</td>
      <td> 10/31/2013 12:52:46 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>          Illegal Parking</td>
      <td>    Posted Parking Sign Violation</td>
      <td>               Street/Sidewalk</td>
      <td> 11236</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td> ROCKAWAY PARKWAY</td>
      <td>  SKIDMORE AVENUE</td>
      <td> INTERSECTION</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 08:52:46 AM</td>
      <td>                    NaN</td>
      <td>          18 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1015289</td>
      <td> 169710</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.632437</td>
      <td>-73.888173</td>
      <td>  (40.63243692394328, -73.88817263437012)</td>
    </tr>
    <tr>
      <th>18</th>
      <td> 26590389</td>
      <td> 10/31/2013 12:51:00 AM</td>
      <td>                    NaN</td>
      <td>   DOT</td>
      <td>                      Department of Transportation</td>
      <td>   Street Light Condition</td>
      <td>                 Street Light Out</td>
      <td>                           NaN</td>
      <td>   NaN</td>
      <td>               226 42 ST E</td>
      <td>             42 ST E</td>
      <td>        CHURCH AVE</td>
      <td>                       SNYDER AVE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>                 NaN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td> Unspecified BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>     NaN</td>
      <td>    NaN</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>       NaN</td>
      <td>       NaN</td>
      <td>                                      NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td> 26594210</td>
      <td> 10/31/2013 12:46:27 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10033</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td> WEST  184 STREET</td>
      <td>         BROADWAY</td>
      <td> INTERSECTION</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 08:46:27 AM</td>
      <td> 10/31/2013 01:32:41 AM</td>
      <td>         12 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td> 1002294</td>
      <td> 249712</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.852058</td>
      <td>-73.934776</td>
      <td>  (40.85205827756883, -73.93477640780834)</td>
    </tr>
    <tr>
      <th>20</th>
      <td> 26592932</td>
      <td> 10/31/2013 12:43:47 AM</td>
      <td> 10/31/2013 12:56:20 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td> Noise - House of Worship</td>
      <td>                 Loud Music/Party</td>
      <td>              House of Worship</td>
      <td> 11216</td>
      <td>            778 PARK PLACE</td>
      <td>          PARK PLACE</td>
      <td>     ROGERS AVENUE</td>
      <td>                  NOSTRAND AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:43:47 AM</td>
      <td> 10/31/2013 12:56:20 AM</td>
      <td>          08 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  997608</td>
      <td> 184656</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.673505</td>
      <td>-73.951844</td>
      <td>  (40.67350473678714, -73.95184414979961)</td>
    </tr>
    <tr>
      <th>21</th>
      <td> 26594152</td>
      <td> 10/31/2013 12:41:17 AM</td>
      <td> 10/31/2013 01:04:37 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Banging/Pounding</td>
      <td>              Store/Commercial</td>
      <td> 10016</td>
      <td>             155 E 34TH ST</td>
      <td>           E 34TH ST</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      LATLONG</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:41:17 AM</td>
      <td> 10/31/2013 01:04:38 AM</td>
      <td>         06 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  990133</td>
      <td> 211136</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.746194</td>
      <td>-73.978769</td>
      <td>  (40.74619417253121, -73.97876853124392)</td>
    </tr>
    <tr>
      <th>22</th>
      <td> 26589678</td>
      <td> 10/31/2013 12:39:55 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>          Noise - Vehicle</td>
      <td>                  Car/Truck Music</td>
      <td>               Street/Sidewalk</td>
      <td> 11419</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>       112 STREET</td>
      <td>  ATLANTIC AVENUE</td>
      <td> INTERSECTION</td>
      <td> SOUTH RICHMOND HILL</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 08:39:55 AM</td>
      <td>                    NaN</td>
      <td>            09 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1030314</td>
      <td> 191578</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.692394</td>
      <td>-73.833891</td>
      <td>   (40.69239424979043, -73.8338912453996)</td>
    </tr>
    <tr>
      <th>23</th>
      <td> 26592304</td>
      <td> 10/31/2013 12:38:00 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11216</td>
      <td>       371 TOMPKINS AVENUE</td>
      <td>     TOMPKINS AVENUE</td>
      <td>    MADISON STREET</td>
      <td>                    PUTNAM AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 08:38:00 AM</td>
      <td> 10/31/2013 01:16:53 AM</td>
      <td>          03 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  999720</td>
      <td> 188825</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.684944</td>
      <td>-73.944221</td>
      <td>   (40.6849442562592, -73.94422078036632)</td>
    </tr>
    <tr>
      <th>24</th>
      <td> 26591892</td>
      <td> 10/31/2013 12:37:16 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>         Blocked Driveway</td>
      <td>                   Partial Access</td>
      <td>               Street/Sidewalk</td>
      <td> 10305</td>
      <td>           1496 BAY STREET</td>
      <td>          BAY STREET</td>
      <td>      LYMAN AVENUE</td>
      <td>                      SCHOOL ROAD</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>       STATEN ISLAND</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 08:37:16 AM</td>
      <td> 10/31/2013 12:52:10 AM</td>
      <td>     01 STATEN ISLAND</td>
      <td> STATEN ISLAND</td>
      <td>  967283</td>
      <td> 160518</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.607245</td>
      <td>-74.061106</td>
      <td>  (40.60724493456944, -74.06110566015863)</td>
    </tr>
    <tr>
      <th>25</th>
      <td> 26591573</td>
      <td> 10/31/2013 12:35:18 AM</td>
      <td> 10/31/2013 02:41:35 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                     Loud Talking</td>
      <td>               Street/Sidewalk</td>
      <td> 10312</td>
      <td>         24 PRINCETON LANE</td>
      <td>      PRINCETON LANE</td>
      <td>     HAMPTON GREEN</td>
      <td>                         DEAD END</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>       STATEN ISLAND</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:35:18 AM</td>
      <td> 10/31/2013 01:45:17 AM</td>
      <td>     03 STATEN ISLAND</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.553421</td>
      <td>-74.196743</td>
      <td>  (40.55342078716953, -74.19674315017886)</td>
    </tr>
    <tr>
      <th>26</th>
      <td> 26590509</td>
      <td> 10/31/2013 12:33:00 AM</td>
      <td>                    NaN</td>
      <td>   DOT</td>
      <td>                      Department of Transportation</td>
      <td>   Street Light Condition</td>
      <td>                 Street Light Out</td>
      <td>                           NaN</td>
      <td>   NaN</td>
      <td>                   38 ST E</td>
      <td>             38 ST E</td>
      <td>        CHURCH AVE</td>
      <td>                      LINDEN BLVD</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>                 NaN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td> Unspecified BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>     NaN</td>
      <td>    NaN</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>       NaN</td>
      <td>       NaN</td>
      <td>                                      NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td> 26591379</td>
      <td> 10/31/2013 12:32:44 AM</td>
      <td>                    NaN</td>
      <td> DOHMH</td>
      <td>           Department of Health and Mental Hygiene</td>
      <td>     Harboring Bees/Wasps</td>
      <td>      Bees/Wasps - Not a beekeper</td>
      <td>  3+ Family Mixed Use Building</td>
      <td> 10025</td>
      <td>       501 WEST 110 STREET</td>
      <td>     WEST 110 STREET</td>
      <td>  AMSTERDAM AVENUE</td>
      <td>                         BROADWAY</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td> 11/30/2013 12:32:44 AM</td>
      <td>                    NaN</td>
      <td>         09 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  994143</td>
      <td> 231888</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.803149</td>
      <td>-73.964266</td>
      <td>  (40.80314938553783, -73.96426608076082)</td>
    </tr>
    <tr>
      <th>28</th>
      <td> 26594085</td>
      <td> 10/31/2013 12:32:08 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                     Loud Talking</td>
      <td>               Street/Sidewalk</td>
      <td> 10026</td>
      <td>       121 WEST 116 STREET</td>
      <td>     WEST 116 STREET</td>
      <td>      LENOX AVENUE</td>
      <td>                         7 AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td> Assigned</td>
      <td> 10/31/2013 08:32:08 AM</td>
      <td> 10/31/2013 02:00:57 AM</td>
      <td>         10 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  997947</td>
      <td> 231613</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.802390</td>
      <td>-73.950526</td>
      <td>  (40.80238950799943, -73.95052644123253)</td>
    </tr>
    <tr>
      <th>29</th>
      <td> 26589201</td>
      <td> 10/31/2013 12:32:00 AM</td>
      <td>                    NaN</td>
      <td>   DOT</td>
      <td>                      Department of Transportation</td>
      <td>   Street Light Condition</td>
      <td>                 Street Light Out</td>
      <td>                           NaN</td>
      <td> 10309</td>
      <td>        295 BAYVIEW AVENUE</td>
      <td>      BAYVIEW AVENUE</td>
      <td>       VAIL AVENUE</td>
      <td>                     BAYVIEW LANE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>       STATEN ISLAND</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td>     03 STATEN ISLAND</td>
      <td> STATEN ISLAND</td>
      <td>  927687</td>
      <td> 127837</td>
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
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.517378</td>
      <td>-74.203435</td>
      <td> (40.517377871705676, -74.20343466779575)</td>
    </tr>
    <tr>
      <th>30</th>
      <td> 26591641</td>
      <td> 10/31/2013 12:31:17 AM</td>
      <td> 10/31/2013 02:41:36 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>         Blocked Driveway</td>
      <td>                        No Access</td>
      <td>               Street/Sidewalk</td>
      <td> 10312</td>
      <td>         24 PRINCETON LANE</td>
      <td>      PRINCETON LANE</td>
      <td>     HAMPTON GREEN</td>
      <td>                         DEAD END</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>       STATEN ISLAND</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:31:17 AM</td>
      <td> 10/31/2013 01:43:09 AM</td>
      <td>     03 STATEN ISLAND</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.553421</td>
      <td>-74.196743</td>
      <td>  (40.55342078716953, -74.19674315017886)</td>
    </tr>
    <tr>
      <th>31</th>
      <td> 26595564</td>
      <td> 10/31/2013 12:30:36 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                 Loud Music/Party</td>
      <td>               Street/Sidewalk</td>
      <td> 11236</td>
      <td>                  AVENUE J</td>
      <td>            AVENUE J</td>
      <td>    EAST 80 STREET</td>
      <td>                   EAST 81 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 08:30:36 AM</td>
      <td>                    NaN</td>
      <td>          18 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1008937</td>
      <td> 170310</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
      <th>32</th>
      <td> 26591378</td>
      <td> 10/31/2013 12:30:31 AM</td>
      <td>                    NaN</td>
      <td>   TLC</td>
      <td>                     Taxi and Limousine Commission</td>
      <td>           Taxi Complaint</td>
      <td>                 Driver Complaint</td>
      <td>                           NaN</td>
      <td> 10036</td>
      <td>             645 10 AVENUE</td>
      <td>           10 AVENUE</td>
      <td>    WEST 45 STREET</td>
      <td>                   WEST 46 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td>         04 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  985965</td>
      <td> 216868</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Other</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.761929</td>
      <td>-73.993809</td>
      <td> (40.761928847500016, -73.99380918401052)</td>
    </tr>
    <tr>
      <th>33</th>
      <td> 26593872</td>
      <td> 10/31/2013 12:29:47 AM</td>
      <td> 10/31/2013 12:38:29 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td> Noise - House of Worship</td>
      <td>                 Banging/Pounding</td>
      <td>              House of Worship</td>
      <td> 10025</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td> WEST   99 STREET</td>
      <td> AMSTERDAM AVENUE</td>
      <td> INTERSECTION</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:29:47 AM</td>
      <td> 10/31/2013 12:38:29 AM</td>
      <td>         07 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  992846</td>
      <td> 229279</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.795990</td>
      <td>-73.968954</td>
      <td> (40.795989749917204, -73.96895423714467)</td>
    </tr>
    <tr>
      <th>34</th>
      <td> 26591420</td>
      <td> 10/31/2013 12:28:30 AM</td>
      <td> 10/31/2013 02:06:11 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>      Homeless Encampment</td>
      <td>                              NaN</td>
      <td>    Residential Building/House</td>
      <td> 10025</td>
      <td>             2754 BROADWAY</td>
      <td>            BROADWAY</td>
      <td>   WEST 105 STREET</td>
      <td>                  WEST 106 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:28:30 AM</td>
      <td> 10/31/2013 02:06:11 AM</td>
      <td>         07 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  993139</td>
      <td> 231139</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.801095</td>
      <td>-73.967894</td>
      <td>   (40.8010946529914, -73.96789356094007)</td>
    </tr>
    <tr>
      <th>35</th>
      <td> 26592976</td>
      <td> 10/31/2013 12:23:24 AM</td>
      <td> 10/31/2013 01:05:41 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>         Blocked Driveway</td>
      <td>                        No Access</td>
      <td>               Street/Sidewalk</td>
      <td> 11433</td>
      <td>           173-41 103 ROAD</td>
      <td>            103 ROAD</td>
      <td>        173 STREET</td>
      <td>                       177 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>             JAMAICA</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:23:24 AM</td>
      <td> 10/31/2013 01:05:41 AM</td>
      <td>            12 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1044124</td>
      <td> 195866</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.704081</td>
      <td>-73.784054</td>
      <td>  (40.70408112125158, -73.78405385422116)</td>
    </tr>
    <tr>
      <th>36</th>
      <td> 26590262</td>
      <td> 10/31/2013 12:23:00 AM</td>
      <td>                    NaN</td>
      <td>   DOT</td>
      <td>                      Department of Transportation</td>
      <td> Traffic Signal Condition</td>
      <td>                       Controller</td>
      <td>                           NaN</td>
      <td> 11235</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>  SHORE BOULEVARD</td>
      <td>       CASS PLACE</td>
      <td> INTERSECTION</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td>          15 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  997073</td>
      <td> 151225</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.581744</td>
      <td>-73.953836</td>
      <td>   (40.5817444882428, -73.95383634845487)</td>
    </tr>
    <tr>
      <th>37</th>
      <td> 26589606</td>
      <td> 10/31/2013 12:20:44 AM</td>
      <td> 10/31/2013 02:10:24 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11216</td>
      <td>       826 ST JOHN'S PLACE</td>
      <td>     ST JOHN'S PLACE</td>
      <td>     ROGERS AVENUE</td>
      <td>                  NOSTRAND AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:20:44 AM</td>
      <td> 10/31/2013 02:10:24 AM</td>
      <td>          08 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  997865</td>
      <td> 183985</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.671663</td>
      <td>-73.950919</td>
      <td> (40.671662601079895, -73.95091901534035)</td>
    </tr>
    <tr>
      <th>38</th>
      <td> 26592083</td>
      <td> 10/31/2013 12:20:00 AM</td>
      <td>                    NaN</td>
      <td>   DOT</td>
      <td>                      Department of Transportation</td>
      <td> Traffic Signal Condition</td>
      <td>                       Controller</td>
      <td>                           NaN</td>
      <td> 11213</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>   BUFFALO AVENUE</td>
      <td>       PARK PLACE</td>
      <td> INTERSECTION</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td>          08 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1004987</td>
      <td> 184136</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.672063</td>
      <td>-73.925244</td>
      <td>  (40.67206324438088, -73.92524432147842)</td>
    </tr>
    <tr>
      <th>39</th>
      <td> 26593840</td>
      <td> 10/31/2013 12:19:48 AM</td>
      <td>                    NaN</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>         Blocked Driveway</td>
      <td>                        No Access</td>
      <td>               Street/Sidewalk</td>
      <td> 11379</td>
      <td>             78-41 68 ROAD</td>
      <td>             68 ROAD</td>
      <td>         78 STREET</td>
      <td>                        79 STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>      MIDDLE VILLAGE</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>     Open</td>
      <td> 10/31/2013 08:19:48 AM</td>
      <td>                    NaN</td>
      <td>            05 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1019062</td>
      <td> 198120</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.710402</td>
      <td>-73.874433</td>
      <td>   (40.71040190143904, -73.8744325577748)</td>
    </tr>
    <tr>
      <th>40</th>
      <td> 26589646</td>
      <td> 10/31/2013 12:18:05 AM</td>
      <td> 10/31/2013 01:26:15 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11101</td>
      <td>     34-19 STEINWAY STREET</td>
      <td>     STEINWAY STREET</td>
      <td>         34 AVENUE</td>
      <td>                        35 AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>    LONG ISLAND CITY</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:18:05 AM</td>
      <td> 10/31/2013 01:26:15 AM</td>
      <td>            01 QUEENS</td>
      <td>        QUEENS</td>
      <td> 1006080</td>
      <td> 214807</td>
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
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.756245</td>
      <td>-73.921205</td>
      <td>  (40.75624514774764, -73.92120466494264)</td>
    </tr>
    <tr>
      <th>41</th>
      <td> 26593296</td>
      <td> 10/31/2013 12:16:25 AM</td>
      <td>                    NaN</td>
      <td> DOHMH</td>
      <td>           Department of Health and Mental Hygiene</td>
      <td>       Food Establishment</td>
      <td>          Rodents/Insects/Garbage</td>
      <td>    Restaurant/Bar/Deli/Bakery</td>
      <td> 10014</td>
      <td>     12 CHRISTOPHER STREET</td>
      <td>  CHRISTOPHER STREET</td>
      <td>  GREENWICH AVENUE</td>
      <td>                       GAY STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td> 12/07/2013 12:16:25 AM</td>
      <td>                    NaN</td>
      <td>         02 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  984181</td>
      <td> 206685</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.733979</td>
      <td>-74.000249</td>
      <td>   (40.73397924003587, -74.0002489720853)</td>
    </tr>
    <tr>
      <th>42</th>
      <td> 26590480</td>
      <td> 10/31/2013 12:15:06 AM</td>
      <td> 10/31/2013 03:00:20 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>              Store/Commercial</td>
      <td> 11231</td>
      <td>       325 COLUMBIA STREET</td>
      <td>     COLUMBIA STREET</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      LATLONG</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:15:06 AM</td>
      <td> 10/31/2013 02:58:55 AM</td>
      <td>          06 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  982995</td>
      <td> 187440</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.681156</td>
      <td>-74.004525</td>
      <td>  (40.68115617695543, -74.00452481832494)</td>
    </tr>
    <tr>
      <th>43</th>
      <td> 26589626</td>
      <td> 10/31/2013 12:14:42 AM</td>
      <td> 10/31/2013 01:39:00 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 11234</td>
      <td>      2192 FLATBUSH AVENUE</td>
      <td>     FLATBUSH AVENUE</td>
      <td>    EAST 46 STREET</td>
      <td>                         AVENUE O</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:14:42 AM</td>
      <td> 10/31/2013 01:39:00 AM</td>
      <td>          18 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1003628</td>
      <td> 163910</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.616550</td>
      <td>-73.930202</td>
      <td>  (40.61655032892211, -73.93020153359745)</td>
    </tr>
    <tr>
      <th>44</th>
      <td> 26592898</td>
      <td> 10/31/2013 12:12:08 AM</td>
      <td> 10/31/2013 01:13:45 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>             Noise - Park</td>
      <td>                     Loud Talking</td>
      <td>               Park/Playground</td>
      <td> 10457</td>
      <td>        CROTONA PARK NORTH</td>
      <td>  CROTONA PARK NORTH</td>
      <td>    CLINTON AVENUE</td>
      <td>                  PROSPECT AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>    BLOCKFACE</td>
      <td>               BRONX</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:12:08 AM</td>
      <td> 10/31/2013 01:13:45 AM</td>
      <td>             06 BRONX</td>
      <td>         BRONX</td>
      <td> 1013947</td>
      <td> 245819</td>
      <td> Unspecified</td>
      <td>         BRONX</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.841342</td>
      <td>-73.892672</td>
      <td> (40.841341641554614, -73.89267161957397)</td>
    </tr>
    <tr>
      <th>45</th>
      <td> 26590446</td>
      <td> 10/31/2013 12:11:58 AM</td>
      <td> 10/31/2013 01:54:38 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                 Loud Music/Party</td>
      <td>               Street/Sidewalk</td>
      <td> 10459</td>
      <td>       819 EAST 167 STREET</td>
      <td>     EAST 167 STREET</td>
      <td>      UNION AVENUE</td>
      <td>                  PROSPECT AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>               BRONX</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:11:58 AM</td>
      <td> 10/31/2013 01:54:38 AM</td>
      <td>             03 BRONX</td>
      <td>         BRONX</td>
      <td> 1011935</td>
      <td> 240454</td>
      <td> Unspecified</td>
      <td>         BRONX</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.826623</td>
      <td>-73.899965</td>
      <td>  (40.826622810177874, -73.8999653556452)</td>
    </tr>
    <tr>
      <th>46</th>
      <td> 26595546</td>
      <td> 10/31/2013 12:09:07 AM</td>
      <td> 10/31/2013 12:53:12 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10465</td>
      <td>  4005 EAST TREMONT AVENUE</td>
      <td> EAST TREMONT AVENUE</td>
      <td>    SAMPSON AVENUE</td>
      <td>                     GERBER PLACE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>               BRONX</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:09:07 AM</td>
      <td> 10/31/2013 12:53:12 AM</td>
      <td>             10 BRONX</td>
      <td>         BRONX</td>
      <td> 1034640</td>
      <td> 238172</td>
      <td> Unspecified</td>
      <td>         BRONX</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.820259</td>
      <td>-73.817942</td>
      <td> (40.820259030934515, -73.81794234356029)</td>
    </tr>
    <tr>
      <th>47</th>
      <td> 26595944</td>
      <td> 10/31/2013 12:08:47 AM</td>
      <td>                    NaN</td>
      <td>   TLC</td>
      <td>                     Taxi and Limousine Commission</td>
      <td>           Taxi Complaint</td>
      <td>                 Driver Complaint</td>
      <td>                           NaN</td>
      <td> 10036</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td> WEST   46 STREET</td>
      <td>        10 AVENUE</td>
      <td> INTERSECTION</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td> 11/14/2013 12:08:47 AM</td>
      <td>                    NaN</td>
      <td>         04 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  986020</td>
      <td> 216961</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Other</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.762184</td>
      <td>-73.993611</td>
      <td>  (40.76218409774632, -73.99361062023596)</td>
    </tr>
    <tr>
      <th>48</th>
      <td> 26595084</td>
      <td> 10/31/2013 12:07:45 AM</td>
      <td> 10/31/2013 01:43:11 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10014</td>
      <td>     12 CHRISTOPHER STREET</td>
      <td>  CHRISTOPHER STREET</td>
      <td>  GREENWICH AVENUE</td>
      <td>                       GAY STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:07:45 AM</td>
      <td> 10/31/2013 01:43:11 AM</td>
      <td>         02 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  984181</td>
      <td> 206685</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.733979</td>
      <td>-74.000249</td>
      <td>   (40.73397924003587, -74.0002489720853)</td>
    </tr>
    <tr>
      <th>49</th>
      <td> 26595553</td>
      <td> 10/31/2013 12:05:10 AM</td>
      <td> 10/31/2013 02:43:43 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>  Noise - Street/Sidewalk</td>
      <td>                     Loud Talking</td>
      <td>               Street/Sidewalk</td>
      <td> 11225</td>
      <td>        25 LEFFERTS AVENUE</td>
      <td>     LEFFERTS AVENUE</td>
      <td> WASHINGTON AVENUE</td>
      <td>                   BEDFORD AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:05:10 AM</td>
      <td> 10/31/2013 01:29:29 AM</td>
      <td>          09 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  995366</td>
      <td> 180388</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
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
      <th>50</th>
      <td> 26594087</td>
      <td> 10/31/2013 12:04:50 AM</td>
      <td> 10/31/2013 01:09:38 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>              Store/Commercial</td>
      <td> 10011</td>
      <td>      258 WEST 15TH STREET</td>
      <td>    WEST 15TH STREET</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      LATLONG</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:04:50 AM</td>
      <td> 10/31/2013 01:09:38 AM</td>
      <td>         04 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  983789</td>
      <td> 208891</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.740034</td>
      <td>-74.001664</td>
      <td>  (40.74003415280169, -74.00166357336052)</td>
    </tr>
    <tr>
      <th>51</th>
      <td> 26595572</td>
      <td> 10/31/2013 12:03:27 AM</td>
      <td>                    NaN</td>
      <td>   DOT</td>
      <td>                      Department of Transportation</td>
      <td>        Broken Muni Meter</td>
      <td>                       No Receipt</td>
      <td>                        Street</td>
      <td> 10003</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td> EAST   10 STREET</td>
      <td>         2 AVENUE</td>
      <td> INTERSECTION</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td> 11/21/2013 12:03:27 AM</td>
      <td>                    NaN</td>
      <td>         03 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  987906</td>
      <td> 205154</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.729776</td>
      <td>-73.986809</td>
      <td>  (40.72977626532991, -73.98680891976119)</td>
    </tr>
    <tr>
      <th>52</th>
      <td> 26590848</td>
      <td> 10/31/2013 12:02:01 AM</td>
      <td> 10/31/2013 01:02:28 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>         Blocked Driveway</td>
      <td>                        No Access</td>
      <td>               Street/Sidewalk</td>
      <td> 11207</td>
      <td>          422 WYONA STREET</td>
      <td>        WYONA STREET</td>
      <td>     SUTTER AVENUE</td>
      <td>                     BLAKE AVENUE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:02:01 AM</td>
      <td> 10/31/2013 01:02:28 AM</td>
      <td>          05 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1014188</td>
      <td> 182855</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.668521</td>
      <td>-73.892081</td>
      <td>  (40.66852085465471, -73.89208096944678)</td>
    </tr>
    <tr>
      <th>53</th>
      <td> 26590413</td>
      <td> 10/31/2013 12:01:47 AM</td>
      <td> 10/31/2013 12:39:31 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>           Club/Bar/Restaurant</td>
      <td> 10002</td>
      <td>      121 RIVINGTON STREET</td>
      <td>    RIVINGTON STREET</td>
      <td>      ESSEX STREET</td>
      <td>                   NORFOLK STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:01:47 AM</td>
      <td> 10/31/2013 12:39:31 AM</td>
      <td>         03 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td>  987766</td>
      <td> 201503</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.719755</td>
      <td>-73.987316</td>
      <td>  (40.71975521311322, -73.98731595609806)</td>
    </tr>
    <tr>
      <th>54</th>
      <td> 26591287</td>
      <td> 10/31/2013 12:01:45 AM</td>
      <td> 10/31/2013 12:02:37 AM</td>
      <td>   HRA</td>
      <td>                      HRA Benefit Card Replacement</td>
      <td> Benefit Card Replacement</td>
      <td>                         Medicaid</td>
      <td>            NYC Street Address</td>
      <td>   NaN</td>
      <td>                       NaN</td>
      <td>                 NaN</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>          NaN</td>
      <td>                 NaN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>   Closed</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td>        0 Unspecified</td>
      <td>   Unspecified</td>
      <td>     NaN</td>
      <td>    NaN</td>
      <td> Unspecified</td>
      <td>   Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>       NaN</td>
      <td>       NaN</td>
      <td>                                      NaN</td>
    </tr>
    <tr>
      <th>55</th>
      <td> 26595001</td>
      <td> 10/31/2013 12:01:34 AM</td>
      <td> 10/31/2013 01:32:43 AM</td>
      <td>  NYPD</td>
      <td>                   New York City Police Department</td>
      <td>       Noise - Commercial</td>
      <td>                 Loud Music/Party</td>
      <td>              Store/Commercial</td>
      <td> 10034</td>
      <td>     524 WEST 207TH STREET</td>
      <td>   WEST 207TH STREET</td>
      <td>               NaN</td>
      <td>                              NaN</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      LATLONG</td>
      <td>            NEW YORK</td>
      <td> NaN</td>
      <td>    Precinct</td>
      <td>   Closed</td>
      <td> 10/31/2013 08:01:34 AM</td>
      <td> 10/31/2013 01:32:43 AM</td>
      <td>         01 MANHATTAN</td>
      <td>     MANHATTAN</td>
      <td> 1006481</td>
      <td> 254514</td>
      <td> Unspecified</td>
      <td>     MANHATTAN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td>   N</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.865229</td>
      <td>-73.919626</td>
      <td>  (40.86522877164924, -73.91962575276823)</td>
    </tr>
    <tr>
      <th>56</th>
      <td> 26591162</td>
      <td> 10/31/2013 12:01:00 AM</td>
      <td>                    NaN</td>
      <td>  DSNY</td>
      <td>                              BCC - Brooklyn South</td>
      <td>     Sanitation Condition</td>
      <td> 15 Street Cond/Dump-Out/Drop-Off</td>
      <td>                        Street</td>
      <td> 11231</td>
      <td>       135 COLUMBIA STREET</td>
      <td>     COLUMBIA STREET</td>
      <td>       KANE STREET</td>
      <td>                    IRVING STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td> DSNY Garage</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td>                    NaN</td>
      <td>          06 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td>  983797</td>
      <td> 189648</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.687217</td>
      <td>-74.001633</td>
      <td>  (40.68721671044577, -74.00163340956126)</td>
    </tr>
    <tr>
      <th>57</th>
      <td> 26593459</td>
      <td> 10/31/2013 12:00:00 AM</td>
      <td>                    NaN</td>
      <td>   HPD</td>
      <td> Department of Housing Preservation and Develop...</td>
      <td>                 ELECTRIC</td>
      <td>                  ELECTRIC-SUPPLY</td>
      <td>          RESIDENTIAL BUILDING</td>
      <td> 11233</td>
      <td>         199 HOWARD AVENUE</td>
      <td>       HOWARD AVENUE</td>
      <td> BAINBRIDGE STREET</td>
      <td>                  CHAUNCEY STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td> 10/31/2013 12:00:00 AM</td>
      <td>          03 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1006513</td>
      <td> 187623</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.681631</td>
      <td>-73.919732</td>
      <td>  (40.68163056217252, -73.91973166452124)</td>
    </tr>
    <tr>
      <th>58</th>
      <td> 26592634</td>
      <td> 10/31/2013 12:00:00 AM</td>
      <td>                    NaN</td>
      <td>   HPD</td>
      <td> Department of Housing Preservation and Develop...</td>
      <td>                 PLUMBING</td>
      <td>                       BASIN/SINK</td>
      <td>          RESIDENTIAL BUILDING</td>
      <td> 11233</td>
      <td>         199 HOWARD AVENUE</td>
      <td>       HOWARD AVENUE</td>
      <td> BAINBRIDGE STREET</td>
      <td>                  CHAUNCEY STREET</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>            BROOKLYN</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td> 10/31/2013 12:00:00 AM</td>
      <td>          03 BROOKLYN</td>
      <td>      BROOKLYN</td>
      <td> 1006513</td>
      <td> 187623</td>
      <td> Unspecified</td>
      <td>      BROOKLYN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.681631</td>
      <td>-73.919732</td>
      <td>  (40.68163056217252, -73.91973166452124)</td>
    </tr>
    <tr>
      <th>59</th>
      <td> 26591688</td>
      <td> 10/31/2013 12:00:00 AM</td>
      <td>                    NaN</td>
      <td>   HPD</td>
      <td> Department of Housing Preservation and Develop...</td>
      <td>                  HEATING</td>
      <td>                             HEAT</td>
      <td>          RESIDENTIAL BUILDING</td>
      <td> 10453</td>
      <td>       150 WEST 179 STREET</td>
      <td>     WEST 179 STREET</td>
      <td>    ANDREWS AVENUE</td>
      <td>                     LORING PLACE</td>
      <td>              NaN</td>
      <td>              NaN</td>
      <td>      ADDRESS</td>
      <td>               BRONX</td>
      <td> NaN</td>
      <td>         NaN</td>
      <td>     Open</td>
      <td>                    NaN</td>
      <td> 10/31/2013 12:00:00 AM</td>
      <td>             05 BRONX</td>
      <td>         BRONX</td>
      <td> 1008161</td>
      <td> 250940</td>
      <td> Unspecified</td>
      <td>         BRONX</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
      <td> NaN</td>
      <td>NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> 40.855415</td>
      <td>-73.913565</td>
      <td> (40.855414830918306, -73.91356461276855)</td>
    </tr>
    <tr>
      <th></th>
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
  </tbody>
</table>
<p>111069 rows × 52 columns</p>
</div>
</div>

## 2.2 Selecting columns and rows

To select a column, we index with the name of the column, like this:

```python
complaints['Complaint Type']
```

Output:

```bash

0      Noise - Street/Sidewalk
1              Illegal Parking
2           Noise - Commercial
3              Noise - Vehicle
4                       Rodent
5           Noise - Commercial
6             Blocked Driveway
7           Noise - Commercial
8           Noise - Commercial
9           Noise - Commercial
10    Noise - House of Worship
11          Noise - Commercial
12             Illegal Parking
13             Noise - Vehicle
14                      Rodent
...
111054    Noise - Street/Sidewalk
111055         Noise - Commercial
111056      Street Sign - Missing
111057                      Noise
111058         Noise - Commercial
111059    Noise - Street/Sidewalk
111060                      Noise
111061         Noise - Commercial
111062               Water System
111063               Water System
111064    Maintenance or Facility
111065            Illegal Parking
111066    Noise - Street/Sidewalk
111067         Noise - Commercial
111068           Blocked Driveway
Name: Complaint Type, Length: 111069, dtype: object
```

To get the first 5 rows of a dataframe, we can use a slice: df[:5].
This is a great way to get a sense for what kind of information is in the dataframe -- take a minute to look at the contents and get a feel for this dataset.

```python
complaints[:5]
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

We can combine these to get the first 5 rows of a column:

```python
complaints['Complaint Type'][:5]
```

Output:

```bash
0    Noise - Street/Sidewalk
1            Illegal Parking
2         Noise - Commercial
3            Noise - Vehicle
4                     Rodent
Name: Complaint Type, dtype: object
```

and it doesn't matter which direction we do it in:

```python
complaints['Complaint Type'][:5]
```

Output:

```bash
0    Noise - Street/Sidewalk
1            Illegal Parking
2         Noise - Commercial
3            Noise - Vehicle
4                     Rodent
Name: Complaint Type, dtype: object
```

## 2.3 Selecting multiple columns

What if we just want to know the complaint type and the borough, but not the rest of the information? Pandas makes it really easy to select a subset of the columns: just index with list of columns you want.

```python
complaints[['Complaint Type', 'Borough']]
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0 </th>
      <td>  Noise - Street/Sidewalk</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>1 </th>
      <td>          Illegal Parking</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>2 </th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>3 </th>
      <td>          Noise - Vehicle</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>4 </th>
      <td>                   Rodent</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>5 </th>
      <td>       Noise - Commercial</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>6 </th>
      <td>         Blocked Driveway</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>7 </th>
      <td>       Noise - Commercial</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>8 </th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>9 </th>
      <td>       Noise - Commercial</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>10</th>
      <td> Noise - House of Worship</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>          Illegal Parking</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>          Noise - Vehicle</td>
      <td>         BRONX</td>
    </tr>
    <tr>
      <th>14</th>
      <td>                   Rodent</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>15</th>
      <td> Noise - House of Worship</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>  Noise - Street/Sidewalk</td>
      <td> STATEN ISLAND</td>
    </tr>
    <tr>
      <th>17</th>
      <td>          Illegal Parking</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>   Street Light Condition</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>20</th>
      <td> Noise - House of Worship</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>          Noise - Vehicle</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>23</th>
      <td>       Noise - Commercial</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>         Blocked Driveway</td>
      <td> STATEN ISLAND</td>
    </tr>
    <tr>
      <th>25</th>
      <td>  Noise - Street/Sidewalk</td>
      <td> STATEN ISLAND</td>
    </tr>
    <tr>
      <th>26</th>
      <td>   Street Light Condition</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>     Harboring Bees/Wasps</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>  Noise - Street/Sidewalk</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>   Street Light Condition</td>
      <td> STATEN ISLAND</td>
    </tr>
    <tr>
      <th>30</th>
      <td>         Blocked Driveway</td>
      <td> STATEN ISLAND</td>
    </tr>
    <tr>
      <th>31</th>
      <td>  Noise - Street/Sidewalk</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>32</th>
      <td>           Taxi Complaint</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>33</th>
      <td> Noise - House of Worship</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>34</th>
      <td>      Homeless Encampment</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>35</th>
      <td>         Blocked Driveway</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>36</th>
      <td> Traffic Signal Condition</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>37</th>
      <td>       Noise - Commercial</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>38</th>
      <td> Traffic Signal Condition</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>39</th>
      <td>         Blocked Driveway</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>40</th>
      <td>       Noise - Commercial</td>
      <td>        QUEENS</td>
    </tr>
    <tr>
      <th>41</th>
      <td>       Food Establishment</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>42</th>
      <td>       Noise - Commercial</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>43</th>
      <td>       Noise - Commercial</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>44</th>
      <td>             Noise - Park</td>
      <td>         BRONX</td>
    </tr>
    <tr>
      <th>45</th>
      <td>  Noise - Street/Sidewalk</td>
      <td>         BRONX</td>
    </tr>
    <tr>
      <th>46</th>
      <td>       Noise - Commercial</td>
      <td>         BRONX</td>
    </tr>
    <tr>
      <th>47</th>
      <td>           Taxi Complaint</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>48</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>49</th>
      <td>  Noise - Street/Sidewalk</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>50</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>51</th>
      <td>        Broken Muni Meter</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>52</th>
      <td>         Blocked Driveway</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>53</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>54</th>
      <td> Benefit Card Replacement</td>
      <td>   Unspecified</td>
    </tr>
    <tr>
      <th>55</th>
      <td>       Noise - Commercial</td>
      <td>     MANHATTAN</td>
    </tr>
    <tr>
      <th>56</th>
      <td>     Sanitation Condition</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>57</th>
      <td>                 ELECTRIC</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>58</th>
      <td>                 PLUMBING</td>
      <td>      BROOKLYN</td>
    </tr>
    <tr>
      <th>59</th>
      <td>                  HEATING</td>
      <td>         BRONX</td>
    </tr>
    <tr>
      <th></th>
      <td>...</td>
      <td>...</td>
    </tr>
  </tbody>
</table>
<p>111069 rows × 2 columns</p>
</div>
</div>

That showed us a summary, and then we can look at the first 10 rows:

```python
complaints[['Complaint Type', 'Borough']][:10]
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td> Noise - Street/Sidewalk</td>
      <td>    QUEENS</td>
    </tr>
    <tr>
      <th>1</th>
      <td>         Illegal Parking</td>
      <td>    QUEENS</td>
    </tr>
    <tr>
      <th>2</th>
      <td>      Noise - Commercial</td>
      <td> MANHATTAN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>         Noise - Vehicle</td>
      <td> MANHATTAN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>                  Rodent</td>
      <td> MANHATTAN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>      Noise - Commercial</td>
      <td>    QUEENS</td>
    </tr>
    <tr>
      <th>6</th>
      <td>        Blocked Driveway</td>
      <td>    QUEENS</td>
    </tr>
    <tr>
      <th>7</th>
      <td>      Noise - Commercial</td>
      <td>    QUEENS</td>
    </tr>
    <tr>
      <th>8</th>
      <td>      Noise - Commercial</td>
      <td> MANHATTAN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>      Noise - Commercial</td>
      <td>  BROOKLYN</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 2 columns</p>
</div>
</div>

## 2.4 What's the most common complaint type?

This is a really easy question to answer! There's a [.value_counts()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html) method that we can use:

```python
complaints['Complaint Type'].value_counts()
```

Output:

```bash
HEATING                     14200
GENERAL CONSTRUCTION         7471
Street Light Condition       7117
DOF Literature Request       5797
PLUMBING                     5373
PAINT - PLASTER              5149
Blocked Driveway             4590
NONCONST                     3998
Street Condition             3473
Illegal Parking              3343
Noise                        3321
Traffic Signal Condition     3145
Dirty Conditions             2653
Water System                 2636
Noise - Commercial           2578
...
Opinion for the Mayor                2
Window Guard                         2
DFTA Literature Request              2
Legal Services Provider Complaint    2
Open Flame Permit                    1
Snow                                 1
Municipal Parking Facility           1
X-Ray Machine/Equipment              1
Stalled Sites                        1
DHS Income Savings Requirement       1
Tunnel Condition                     1
Highway Sign - Damaged               1
Ferry Permit                         1
Trans Fat                            1
DWD                                  1
Length: 165, dtype: int64
```

If we just wanted the top 10 most common complaints, we can do this:

```python
complaint_counts = complaints['Complaint Type'].value_counts()
complaint_counts[:10]
```

Output:

```bash
HEATING                   14200
GENERAL CONSTRUCTION       7471
Street Light Condition     7117
DOF Literature Request     5797
PLUMBING                   5373
PAINT - PLASTER            5149
Blocked Driveway           4590
NONCONST                   3998
Street Condition           3473
Illegal Parking            3343
dtype: int64
```

But it gets better! We can plot them!

```python
complaint_counts[:10].plot(kind='bar')
```

Output:
<div>
<img src="/img/plot_complaints.png" alt="Plotting complaints with Matplotlib" />
</div>
