---
date: 2017-05-18
linktitle: Chapter 7
menu:
  main:
    parent: pandas
next: /pandas-cookbook/chapter8
prev: /pandas-cookbook/chapter6
title: Chapter 7 - Cleanup messy data
weight: 10
url: /pandas-cookbook/chapter7
description: Cleanup messy data using Pandas
keywords:
  - pandas
  - cleanup
  - messy data
---

```python
# The usual preamble
import pandas as pd

# Make the graphs a bit prettier, and bigger
pd.set_option('display.mpl_style', 'default')
figsize(15, 5)

# Always display all the columns
pd.set_option('display.line_width', 5000) 
pd.set_option('display.max_columns', 60) 
```

One of the main problems with messy data is: how do you know if it's messy or not?

We're going to use the NYC 311 service request dataset again here, since it's big and a bit unwieldy.

```python
requests = pd.read_csv('311-service-requests.csv')
```

## 7.1 How do we know if it's messy?

We're going to look at a few columns here. I know already that there are some problems with the zip code, so let's look at that first.

To get a sense for whether a column has problems, I usually use [.unique()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.unique.html) to look at all its values. If it's a numeric column, I'll instead plot a histogram to get a sense of the distribution.

When we look at the unique values in "Incident Zip", it quickly becomes clear that this is a mess.

Some of the problems:

 - Some have been parsed as strings, and some as floats
 - There are nans
 - Some of the zip codes are 29616-0759 or 83
 - There are some N/A values that pandas didn't recognize, like 'N/A' and 'NO CLUE'
 
What we can do:

 - Normalize 'N/A' and 'NO CLUE' into regular nan values
 - Look at what's up with the 83, and decide what to do
 - Make everything strings

```python
requests['Incident Zip'].unique()
```

Output:

```bash
array(['11432', '11378', '10032', '10023', '10027', '11372', '11419',
       '11417', '10011', '11225', '11218', '10003', '10029', '10466',
       '11219', '10025', '10310', '11236', nan, '10033', '11216', '10016',
       '10305', '10312', '10026', '10309', '10036', '11433', '11235',
       '11213', '11379', '11101', '10014', '11231', '11234', '10457',
       '10459', '10465', '11207', '10002', '10034', '11233', '10453',
       '10456', '10469', '11374', '11221', '11421', '11215', '10007',
       '10019', '11205', '11418', '11369', '11249', '10005', '10009',
       '11211', '11412', '10458', '11229', '10065', '10030', '11222',
       '10024', '10013', '11420', '11365', '10012', '11214', '11212',
       '10022', '11232', '11040', '11226', '10281', '11102', '11208',
       '10001', '10472', '11414', '11223', '10040', '11220', '11373',
       '11203', '11691', '11356', '10017', '10452', '10280', '11217',
       '10031', '11201', '11358', '10128', '11423', '10039', '10010',
       '11209', '10021', '10037', '11413', '11375', '11238', '10473',
       '11103', '11354', '11361', '11106', '11385', '10463', '10467',
       '11204', '11237', '11377', '11364', '11434', '11435', '11210',
       '11228', '11368', '11694', '10464', '11415', '10314', '10301',
       '10018', '10038', '11105', '11230', '10468', '11104', '10471',
       '11416', '10075', '11422', '11355', '10028', '10462', '10306',
       '10461', '11224', '11429', '10035', '11366', '11362', '11206',
       '10460', '10304', '11360', '11411', '10455', '10475', '10069',
       '10303', '10308', '10302', '11357', '10470', '11367', '11370',
       '10454', '10451', '11436', '11426', '10153', '11004', '11428',
       '11427', '11001', '11363', '10004', '10474', '11430', '10000',
       '10307', '11239', '10119', '10006', '10048', '11697', '11692',
       '11693', '10573', '00083', 'N/A', '11559', '10020', '77056',
       '11776', '70711', '10282', '11109', '10044', '02061', '77092-2016',
       '14225', '55164-0737', '19711', '07306', '000000', 'NO CLUE',
       '90010', '11747', '23541', '11788', '07604', 11203.0, 11217.0,
       11418.0, 11385.0, 10461.0, 11236.0, 11223.0, 11205.0, 11218.0,
       11207.0, 11234.0, 11215.0, 11420.0, 10463.0, 11213.0, 10014.0,
       10011.0, 11421.0, 10029.0, 11433.0, 11691.0, 11358.0, 11368.0,
       11435.0, 11105.0, 11101.0, 11419.0, 11355.0, 11377.0, 11210.0,
       10040.0, 11208.0, 11228.0, 10022.0, 11412.0, 11209.0, 11211.0,
       10018.0, 11106.0, 11411.0, 11369.0, 11237.0, 11230.0, 11364.0,
       10472.0, 10304.0, 10075.0, 11249.0, 10032.0, 10016.0, 10308.0,
       10306.0, 11225.0, 10006.0, 10009.0, 10033.0, 11104.0, 11204.0,
       11415.0, 11103.0, 10025.0, 10473.0, 10469.0, 10466.0, 11231.0,
       11226.0, 10455.0, 10019.0, 11220.0, 10459.0, 10002.0, 10039.0,
       10026.0, 10456.0, 10468.0, 11222.0, 11214.0, 10470.0, 11373.0,
       11367.0, 10302.0, 11235.0, 10128.0, 10467.0, 10458.0, 10475.0,
       10474.0, 10453.0, 10462.0, 10301.0, 10065.0, 11221.0, 10031.0,
       10460.0, 11233.0, 10457.0, 10027.0, 10003.0, 10038.0, 11212.0,
       11206.0, 11434.0, 11361.0, 10036.0, 10005.0, 10024.0, 10035.0,
       10030.0, 11694.0, 10454.0, 11238.0, 10464.0, 10452.0, 10037.0,
       11219.0, 11216.0, 10028.0, 10451.0, 11229.0, 11422.0, 10010.0,
       10023.0, 11692.0, 11374.0, 11416.0, 11429.0, 10314.0, 11375.0,
       11354.0, 11378.0, 10303.0, 10034.0, 11423.0, 11372.0, 11379.0,
       10007.0, 11201.0, 10001.0, 10310.0, 10012.0, 10309.0, 11232.0,
       11224.0, 10305.0, 11693.0, 10021.0, 11432.0, 11356.0, 11436.0,
       10312.0, 11413.0, 11102.0, 10013.0, 10471.0, 11417.0, 11365.0,
       11004.0, 11366.0, 11362.0, 11370.0, 11357.0, 10112.0, 10017.0,
       10307.0, 10465.0, 11426.0, 10280.0, 11430.0, 11109.0, 11414.0,
       11788.0, 11563.0, 11580.0, 11427.0, 11428.0, 10000.0, 7087.0,
       10282.0, 11360.0, 10020.0, 83.0, 10004.0, 11363.0, 11042.0, 11040.0,
       7093.0, 10119.0, 11501.0, 92123.0, 11697.0, 0.0, 11575.0, 11239.0,
       7109.0, 11797.0, 10069.0, '10803', '11716', '11722', '11549-3650',
       '10162', '92123', '23502', '11518', '07020', '08807', '11577',
       '07114', '11003', '07201', '11563', '61702', '10103', '29616-0759',
       '35209-3114', '11520', '11735', '10129', '11005', '41042', '11590',
       6901.0, 7208.0, 10048.0, 11530.0, 13221.0, 10954.0, 11001.0,
       11735.0, 10103.0, 10044.0, 7114.0, 11111.0, 10107.0], dtype=object)
```

## 7.2 Fixing the nan values and string/float confusion

We can pass a `na_values` option to [pd.read_csv](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) to clean this up a little bit. We can also specify that the type of Incident Zip is a string, not a float.

```python
na_values = ['NO CLUE', 'N/A', '0']
requests = pd.read_csv('../data/311-service-requests.csv', na_values=na_values, dtype={'Incident Zip': str})
requests['Incident Zip'].unique()
```

Output:

```bash
array(['11432', '11378', '10032', '10023', '10027', '11372', '11419',
       '11417', '10011', '11225', '11218', '10003', '10029', '10466',
       '11219', '10025', '10310', '11236', nan, '10033', '11216', '10016',
       '10305', '10312', '10026', '10309', '10036', '11433', '11235',
       '11213', '11379', '11101', '10014', '11231', '11234', '10457',
       '10459', '10465', '11207', '10002', '10034', '11233', '10453',
       '10456', '10469', '11374', '11221', '11421', '11215', '10007',
       '10019', '11205', '11418', '11369', '11249', '10005', '10009',
       '11211', '11412', '10458', '11229', '10065', '10030', '11222',
       '10024', '10013', '11420', '11365', '10012', '11214', '11212',
       '10022', '11232', '11040', '11226', '10281', '11102', '11208',
       '10001', '10472', '11414', '11223', '10040', '11220', '11373',
       '11203', '11691', '11356', '10017', '10452', '10280', '11217',
       '10031', '11201', '11358', '10128', '11423', '10039', '10010',
       '11209', '10021', '10037', '11413', '11375', '11238', '10473',
       '11103', '11354', '11361', '11106', '11385', '10463', '10467',
       '11204', '11237', '11377', '11364', '11434', '11435', '11210',
       '11228', '11368', '11694', '10464', '11415', '10314', '10301',
       '10018', '10038', '11105', '11230', '10468', '11104', '10471',
       '11416', '10075', '11422', '11355', '10028', '10462', '10306',
       '10461', '11224', '11429', '10035', '11366', '11362', '11206',
       '10460', '10304', '11360', '11411', '10455', '10475', '10069',
       '10303', '10308', '10302', '11357', '10470', '11367', '11370',
       '10454', '10451', '11436', '11426', '10153', '11004', '11428',
       '11427', '11001', '11363', '10004', '10474', '11430', '10000',
       '10307', '11239', '10119', '10006', '10048', '11697', '11692',
       '11693', '10573', '00083', '11559', '10020', '77056', '11776',
       '70711', '10282', '11109', '10044', '02061', '77092-2016', '14225',
       '55164-0737', '19711', '07306', '000000', '90010', '11747', '23541',
       '11788', '07604', '10112', '11563', '11580', '07087', '11042',
       '07093', '11501', '92123', '00000', '11575', '07109', '11797',
       '10803', '11716', '11722', '11549-3650', '10162', '23502', '11518',
       '07020', '08807', '11577', '07114', '11003', '07201', '61702',
       '10103', '29616-0759', '35209-3114', '11520', '11735', '10129',
       '11005', '41042', '11590', '06901', '07208', '11530', '13221',
       '10954', '11111', '10107'], dtype=object)
```

## 7.3 What's up with the dashes?

```python
rows_with_dashes = requests['Incident Zip'].str.contains('-').fillna(False)
len(requests[rows_with_dashes])
```

Output:

```bash
5
```

```python
requests[rows_with_dashes]
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
      <th>29136</th>
      <td> 26550551</td>
      <td> 10/24/2013 06:16:34 PM</td>
      <td>                    NaN</td>
      <td> DCA</td>
      <td> Department of Consumer Affairs</td>
      <td> Consumer Complaint</td>
      <td> False Advertising</td>
      <td>    NaN</td>
      <td> 77092-2016</td>
      <td>  2700 EAST SELTICE WAY</td>
      <td>   EAST SELTICE WAY</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>    HOUSTON</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Assigned</td>
      <td> 11/13/2013 11:15:20 AM</td>
      <td> 10/29/2013 11:16:16 AM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td>                NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
    <tr>
      <th>30939</th>
      <td> 26548831</td>
      <td> 10/24/2013 09:35:10 AM</td>
      <td>                    NaN</td>
      <td> DCA</td>
      <td> Department of Consumer Affairs</td>
      <td> Consumer Complaint</td>
      <td>        Harassment</td>
      <td>    NaN</td>
      <td> 55164-0737</td>
      <td>         P.O. BOX 64437</td>
      <td>              64437</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   ST. PAUL</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Assigned</td>
      <td> 11/13/2013 02:30:21 PM</td>
      <td> 10/29/2013 02:31:06 PM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td>                NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
    <tr>
      <th>70539</th>
      <td> 26488417</td>
      <td> 10/15/2013 03:40:33 PM</td>
      <td>                    NaN</td>
      <td> TLC</td>
      <td>  Taxi and Limousine Commission</td>
      <td>     Taxi Complaint</td>
      <td>  Driver Complaint</td>
      <td> Street</td>
      <td> 11549-3650</td>
      <td> 365 HOFSTRA UNIVERSITY</td>
      <td> HOFSTRA UNIVERSITY</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   HEMSTEAD</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Assigned</td>
      <td> 11/30/2013 01:20:33 PM</td>
      <td> 10/16/2013 01:21:39 PM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td> La Guardia Airport</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
    <tr>
      <th>85821</th>
      <td> 26468296</td>
      <td> 10/10/2013 12:36:43 PM</td>
      <td> 10/26/2013 01:07:07 AM</td>
      <td> DCA</td>
      <td> Department of Consumer Affairs</td>
      <td> Consumer Complaint</td>
      <td>     Debt Not Owed</td>
      <td>    NaN</td>
      <td> 29616-0759</td>
      <td>           PO BOX 25759</td>
      <td>          BOX 25759</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> GREENVILLE</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   Closed</td>
      <td> 10/26/2013 09:20:28 AM</td>
      <td> 10/26/2013 01:07:07 AM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td>                NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
    <tr>
      <th>89304</th>
      <td> 26461137</td>
      <td> 10/09/2013 05:23:46 PM</td>
      <td> 10/25/2013 01:06:41 AM</td>
      <td> DCA</td>
      <td> Department of Consumer Affairs</td>
      <td> Consumer Complaint</td>
      <td>        Harassment</td>
      <td>    NaN</td>
      <td> 35209-3114</td>
      <td>        600 BEACON PKWY</td>
      <td>        BEACON PKWY</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> BIRMINGHAM</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>   Closed</td>
      <td> 10/25/2013 02:43:42 PM</td>
      <td> 10/25/2013 01:06:41 AM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td>                NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
  </tbody>
</table>
</div>
</div>

I thought these were missing data and originally deleted them like this:

```python
requests['Incident Zip'][rows_with_dashes] = np.nan
```

But then my friend pointed out that 9-digit zip codes are normal. Let's look at all the zip codes with more than 5 digits, make sure they're okay, and then truncate them.

```python
long_zip_codes = requests['Incident Zip'].str.len() > 5
requests['Incident Zip'][long_zip_codes].unique()
```

Output:

```bash
array(['77092-2016', '55164-0737', '000000', '11549-3650', '29616-0759',
       '35209-3114'], dtype=object)
```

Those all look okay to truncate to me.

```python
requests['Incident Zip'] = requests['Incident Zip'].str.slice(0, 5)
```

Done.

Earlier I thought 00083 was a broken zip code, but turns out Central Park's zip code 00083! Shows what I know. I'm still concerned about the 00000 zip codes, though: let's look at that.

```python
requests[requests['Incident Zip'] == '00000']
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
      <th>42600</th>
      <td> 26529313</td>
      <td> 10/22/2013 02:51:06 PM</td>
      <td> NaN</td>
      <td> TLC</td>
      <td> Taxi and Limousine Commission</td>
      <td> Taxi Complaint</td>
      <td> Driver Complaint</td>
      <td>    NaN</td>
      <td> 00000</td>
      <td>          EWR EWR</td>
      <td>            EWR</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NEWARK</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Assigned</td>
      <td> 12/07/2013 09:53:51 AM</td>
      <td> 10/23/2013 09:54:43 AM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td> Other</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
    <tr>
      <th>60843</th>
      <td> 26507389</td>
      <td> 10/17/2013 05:48:44 PM</td>
      <td> NaN</td>
      <td> TLC</td>
      <td> Taxi and Limousine Commission</td>
      <td> Taxi Complaint</td>
      <td> Driver Complaint</td>
      <td> Street</td>
      <td> 00000</td>
      <td> 1 NEWARK AIRPORT</td>
      <td> NEWARK AIRPORT</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NEWARK</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> Assigned</td>
      <td> 12/02/2013 11:59:46 AM</td>
      <td> 10/18/2013 12:01:08 PM</td>
      <td> 0 Unspecified</td>
      <td> Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> Unspecified</td>
      <td> Unspecified</td>
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
      <td> Other</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td> NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td> NaN</td>
    </tr>
  </tbody>
</table>
</div>
</div>

This looks bad to me. Let's set these to nan.

```python
zero_zips = requests['Incident Zip'] == '00000'
requests['Incident Zip'][zero_zips] = np.nan
```

Great. Let's see where we are now:

```python
unique_zips = requests['Incident Zip'].unique()
unique_zips.sort()
unique_zips
```

Output:

```bash
array([nan, '00083', '02061', '06901', '07020', '07087', '07093', '07109',
       '07114', '07201', '07208', '07306', '07604', '08807', '10000',
       '10001', '10002', '10003', '10004', '10005', '10006', '10007',
       '10009', '10010', '10011', '10012', '10013', '10014', '10016',
       '10017', '10018', '10019', '10020', '10021', '10022', '10023',
       '10024', '10025', '10026', '10027', '10028', '10029', '10030',
       '10031', '10032', '10033', '10034', '10035', '10036', '10037',
       '10038', '10039', '10040', '10044', '10048', '10065', '10069',
       '10075', '10103', '10107', '10112', '10119', '10128', '10129',
       '10153', '10162', '10280', '10281', '10282', '10301', '10302',
       '10303', '10304', '10305', '10306', '10307', '10308', '10309',
       '10310', '10312', '10314', '10451', '10452', '10453', '10454',
       '10455', '10456', '10457', '10458', '10459', '10460', '10461',
       '10462', '10463', '10464', '10465', '10466', '10467', '10468',
       '10469', '10470', '10471', '10472', '10473', '10474', '10475',
       '10573', '10803', '10954', '11001', '11003', '11004', '11005',
       '11040', '11042', '11101', '11102', '11103', '11104', '11105',
       '11106', '11109', '11111', '11201', '11203', '11204', '11205',
       '11206', '11207', '11208', '11209', '11210', '11211', '11212',
       '11213', '11214', '11215', '11216', '11217', '11218', '11219',
       '11220', '11221', '11222', '11223', '11224', '11225', '11226',
       '11228', '11229', '11230', '11231', '11232', '11233', '11234',
       '11235', '11236', '11237', '11238', '11239', '11249', '11354',
       '11355', '11356', '11357', '11358', '11360', '11361', '11362',
       '11363', '11364', '11365', '11366', '11367', '11368', '11369',
       '11370', '11372', '11373', '11374', '11375', '11377', '11378',
       '11379', '11385', '11411', '11412', '11413', '11414', '11415',
       '11416', '11417', '11418', '11419', '11420', '11421', '11422',
       '11423', '11426', '11427', '11428', '11429', '11430', '11432',
       '11433', '11434', '11435', '11436', '11501', '11518', '11520',
       '11530', '11549', '11559', '11563', '11575', '11577', '11580',
       '11590', '11691', '11692', '11693', '11694', '11697', '11716',
       '11722', '11735', '11747', '11776', '11788', '11797', '13221',
       '14225', '19711', '23502', '23541', '29616', '35209', '41042',
       '55164', '61702', '70711', '77056', '77092', '90010', '92123'], dtype=object)
```

Amazing! This is much cleaner. There's something a bit weird here, though -- I looked up 77056 on Google maps, and that's in Texas.

Let's take a closer look:

```python
zips = requests['Incident Zip']
# Let's say the zips starting with '0' and '1' are okay, for now. (this isn't actually true -- 13221 is in Syracuse, and why?)
is_close = zips.str.startswith('0') | zips.str.startswith('1')
# There are a bunch of NaNs, but we're not interested in them right now, so we'll say they're True
is_far = ~(is_close.fillna(True).astype(bool))
zips[is_far]
```

Output:

```bash
12102    77056
13450    70711
29136    77092
30939    55164
44008    90010
47048    23541
57636    92123
71001    92123
71834    23502
80573    61702
85821    29616
89304    35209
94201    41042
Name: Incident Zip, dtype: object
```

```python
requests[is_far][['Incident Zip', 'Descriptor', 'City']].sort('Incident Zip')
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Incident Zip</th>
      <th>Descriptor</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>71834</th>
      <td> 23502</td>
      <td>        Harassment</td>
      <td>     NORFOLK</td>
    </tr>
    <tr>
      <th>47048</th>
      <td> 23541</td>
      <td>        Harassment</td>
      <td>     NORFOLK</td>
    </tr>
    <tr>
      <th>85821</th>
      <td> 29616</td>
      <td>     Debt Not Owed</td>
      <td>  GREENVILLE</td>
    </tr>
    <tr>
      <th>89304</th>
      <td> 35209</td>
      <td>        Harassment</td>
      <td>  BIRMINGHAM</td>
    </tr>
    <tr>
      <th>94201</th>
      <td> 41042</td>
      <td>        Harassment</td>
      <td>    FLORENCE</td>
    </tr>
    <tr>
      <th>30939</th>
      <td> 55164</td>
      <td>        Harassment</td>
      <td>    ST. PAUL</td>
    </tr>
    <tr>
      <th>80573</th>
      <td> 61702</td>
      <td>   Billing Dispute</td>
      <td>  BLOOMIGTON</td>
    </tr>
    <tr>
      <th>13450</th>
      <td> 70711</td>
      <td>  Contract Dispute</td>
      <td>     CLIFTON</td>
    </tr>
    <tr>
      <th>12102</th>
      <td> 77056</td>
      <td>     Debt Not Owed</td>
      <td>     HOUSTON</td>
    </tr>
    <tr>
      <th>29136</th>
      <td> 77092</td>
      <td> False Advertising</td>
      <td>     HOUSTON</td>
    </tr>
    <tr>
      <th>44008</th>
      <td> 90010</td>
      <td>   Billing Dispute</td>
      <td> LOS ANGELES</td>
    </tr>
    <tr>
      <th>57636</th>
      <td> 92123</td>
      <td>        Harassment</td>
      <td>   SAN DIEGO</td>
    </tr>
    <tr>
      <th>71001</th>
      <td> 92123</td>
      <td>   Billing Dispute</td>
      <td>   SAN DIEGO</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Okay, there really are requests coming from LA and Houston! Good to know. Filtering by zip code is probably a bad way to handle this -- we should really be looking at the city instead.

```python
requests['City'].str.upper().value_counts()
```

Output:

```bash
BROOKLYN            31662
NEW YORK            22664
BRONX               18438
STATEN ISLAND        4766
JAMAICA              2246
FLUSHING             1803
ASTORIA              1568
RIDGEWOOD            1073
CORONA                707
OZONE PARK            693
LONG ISLAND CITY      678
FAR ROCKAWAY          652
ELMHURST              647
WOODSIDE              609
EAST ELMHURST         562
...
MELVILLE                  1
PORT JEFFERSON STATION    1
NORWELL                   1
EAST ROCKAWAY             1
BIRMINGHAM                1
ROSLYN                    1
LOS ANGELES               1
MINEOLA                   1
JERSEY CITY               1
ST. PAUL                  1
CLIFTON                   1
COL.ANVURES               1
EDGEWATER                 1
ROSELYN                   1
CENTRAL ISLIP             1
Length: 100, dtype: int64
```

It looks like these are legitimate complaints, so we'll just leave them alone.

## 7.4 Putting it together

Here's what we ended up doing to clean up our zip codes, all together:

```python
na_values = ['NO CLUE', 'N/A', '0']
requests = pd.read_csv('311-service-requests.csv', 
                       na_values=na_values, 
                       dtype={'Incident Zip': str})

def fix_zip_codes(zips):
    # Truncate everything to length 5 
    zips = zips.str.slice(0, 5)
    
    # Set 00000 zip codes to nan
    zero_zips = zips == '00000'
    zips[zero_zips] = np.nan
    
    return zips
    
requests['Incident Zip'] = fix_zip_codes(requests['Incident Zip'])
requests['Incident Zip'].unique()
```

Output:

```bash
array(['11432', '11378', '10032', '10023', '10027', '11372', '11419',
       '11417', '10011', '11225', '11218', '10003', '10029', '10466',
       '11219', '10025', '10310', '11236', nan, '10033', '11216', '10016',
       '10305', '10312', '10026', '10309', '10036', '11433', '11235',
       '11213', '11379', '11101', '10014', '11231', '11234', '10457',
       '10459', '10465', '11207', '10002', '10034', '11233', '10453',
       '10456', '10469', '11374', '11221', '11421', '11215', '10007',
       '10019', '11205', '11418', '11369', '11249', '10005', '10009',
       '11211', '11412', '10458', '11229', '10065', '10030', '11222',
       '10024', '10013', '11420', '11365', '10012', '11214', '11212',
       '10022', '11232', '11040', '11226', '10281', '11102', '11208',
       '10001', '10472', '11414', '11223', '10040', '11220', '11373',
       '11203', '11691', '11356', '10017', '10452', '10280', '11217',
       '10031', '11201', '11358', '10128', '11423', '10039', '10010',
       '11209', '10021', '10037', '11413', '11375', '11238', '10473',
       '11103', '11354', '11361', '11106', '11385', '10463', '10467',
       '11204', '11237', '11377', '11364', '11434', '11435', '11210',
       '11228', '11368', '11694', '10464', '11415', '10314', '10301',
       '10018', '10038', '11105', '11230', '10468', '11104', '10471',
       '11416', '10075', '11422', '11355', '10028', '10462', '10306',
       '10461', '11224', '11429', '10035', '11366', '11362', '11206',
       '10460', '10304', '11360', '11411', '10455', '10475', '10069',
       '10303', '10308', '10302', '11357', '10470', '11367', '11370',
       '10454', '10451', '11436', '11426', '10153', '11004', '11428',
       '11427', '11001', '11363', '10004', '10474', '11430', '10000',
       '10307', '11239', '10119', '10006', '10048', '11697', '11692',
       '11693', '10573', '00083', '11559', '10020', '77056', '11776',
       '70711', '10282', '11109', '10044', '02061', '77092', '14225',
       '55164', '19711', '07306', '90010', '11747', '23541', '11788',
       '07604', '10112', '11563', '11580', '07087', '11042', '07093',
       '11501', '92123', '11575', '07109', '11797', '10803', '11716',
       '11722', '11549', '10162', '23502', '11518', '07020', '08807',
       '11577', '07114', '11003', '07201', '61702', '10103', '29616',
       '35209', '11520', '11735', '10129', '11005', '41042', '11590',
       '06901', '07208', '11530', '13221', '10954', '11111', '10107'], dtype=object)
```
