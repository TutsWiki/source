---
date: 2017-05-18
linktitle: Chapter 8
menu:
  main:
    parent: pandas
prev: /pandas-cookbook/chapter7
title: Chapter 8 - Parsing Unix timestamps
weight: 45
url: /pandas-cookbook/chapter8
description: Parsing Unix timestamps
keywords:
  - pandas
  - parsing time
  - timestamp
  - unix timestamp
  - datetime
---

## 8.1 Parsing Unix timestamps

It's not obvious how to deal with Unix timestamps in pandas -- it took me quite a while to figure this out. The file we're using here is a popularity-contest file I found on my system at [/var/log/popularity-contest](/popularity-contest).

Here's an [explanation of how this file works](http://popcon.ubuntu.com/README).

I'm going to hope that nothing in it is sensitive :)

```python
import pandas as pd
# Read it, and remove the last row
popcon = pd.read_csv('popularity-contest', sep=' ', )[:-1]
popcon.columns = ['atime', 'ctime', 'package-name', 'mru-program', 'tag']
popcon[:5]
```

The colums are the access time, created time, package name, recently used program, and a tag

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>atime</th>
      <th>ctime</th>
      <th>package-name</th>
      <th>mru-program</th>
      <th>tag</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td> 1387295797</td>
      <td> 1367633260</td>
      <td>    perl-base</td>
      <td>                                /usr/bin/perl</td>
      <td>            NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td> 1387295796</td>
      <td> 1354370480</td>
      <td>        login</td>
      <td>                                      /bin/su</td>
      <td>            NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td> 1387295743</td>
      <td> 1354341275</td>
      <td>   libtalloc2</td>
      <td> /usr/lib/x86_64-linux-gnu/libtalloc.so.2.0.7</td>
      <td>            NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td> 1387295743</td>
      <td> 1387224204</td>
      <td> libwbclient0</td>
      <td>   /usr/lib/x86_64-linux-gnu/libwbclient.so.0</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>4</th>
      <td> 1387295742</td>
      <td> 1354341253</td>
      <td>  libselinux1</td>
      <td>        /lib/x86_64-linux-gnu/libselinux.so.1</td>
      <td>            NaN</td>
    </tr>
  </tbody>
</table>
</div>
</div>

The magical part about parsing timestamps in pandas is that numpy datetimes are already stored as Unix timestamps. So all we need to do is tell pandas that these integers are actually datetimes -- it doesn't need to do any conversion at all.

We need to convert these to ints to start:

```python
popcon['atime'] = popcon['atime'].astype(int)
popcon['ctime'] = popcon['ctime'].astype(int)
```

Every numpy array and pandas series has a dtype -- this is usually int64, float64, or object. Some of the time types available are datetime64[s], datetime64[ms], and datetime64[us]. There are also timedelta types, similarly.

We can use the [pd.to_datetime](http://pandas.pydata.org/pandas-docs/version/0.20/generated/pandas.to_datetime.html) function to convert our integer timestamps into datetimes. This is a constant-time operation -- we're not actually changing any of the data, just how pandas thinks about it.

```python
popcon['atime'] = pd.to_datetime(popcon['atime'], unit='s')
popcon['ctime'] = pd.to_datetime(popcon['ctime'], unit='s')
popcon['atime'].dtype
```

Output:

```bash
dtype('<M8[ns]')
```

If we look at the dtype now, it's <M8[ns]. As far as I can tell M8 is secret code for datetime64.

So now we can look at our atime and ctime as dates!

```python
popcon[:5]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>atime</th>
      <th>ctime</th>
      <th>package-name</th>
      <th>mru-program</th>
      <th>tag</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-12-17 15:56:37</td>
      <td>2013-05-04 02:07:40</td>
      <td>    perl-base</td>
      <td>                                /usr/bin/perl</td>
      <td>            NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-12-17 15:56:36</td>
      <td>2012-12-01 14:01:20</td>
      <td>        login</td>
      <td>                                      /bin/su</td>
      <td>            NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-12-17 15:55:43</td>
      <td>2012-12-01 05:54:35</td>
      <td>   libtalloc2</td>
      <td> /usr/lib/x86_64-linux-gnu/libtalloc.so.2.0.7</td>
      <td>            NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-12-17 15:55:43</td>
      <td>2013-12-16 20:03:24</td>
      <td> libwbclient0</td>
      <td>   /usr/lib/x86_64-linux-gnu/libwbclient.so.0</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-12-17 15:55:42</td>
      <td>2012-12-01 05:54:13</td>
      <td>  libselinux1</td>
      <td>        /lib/x86_64-linux-gnu/libselinux.so.1</td>
      <td>            NaN</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Now suppose we want to look at all packages that aren't libraries.

First, I want to get rid of everything with timestamp 0. Notice how we can just use a string in this comparison, even though it's actually a timestamp on the inside? That is because pandas is amazing.

```python
popcon = popcon[popcon['atime'] > '1970-01-01']
```

Now we can use pandas' magical string abilities to just look at rows where the package name doesn't contain 'lib'.

```python
nonlibraries = popcon[~popcon['package-name'].str.contains('lib')]
nonlibraries.sort('ctime', ascending=False)[:10]
```

Output:

<div class="output_html rendered_html output_subarea output_execute_result">
<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>atime</th>
      <th>ctime</th>
      <th>package-name</th>
      <th>mru-program</th>
      <th>tag</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>57 </th>
      <td>2013-12-17 04:55:39</td>
      <td>2013-12-17 04:55:42</td>
      <td>                       ddd</td>
      <td>                                      /usr/bin/ddd</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>450</th>
      <td>2013-12-16 20:03:20</td>
      <td>2013-12-16 20:05:13</td>
      <td>                    nodejs</td>
      <td>                                      /usr/bin/npm</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>454</th>
      <td>2013-12-16 20:03:20</td>
      <td>2013-12-16 20:05:04</td>
      <td> switchboard-plug-keyboard</td>
      <td>      /usr/lib/plugs/pantheon/keyboard/options.txt</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>445</th>
      <td>2013-12-16 20:03:20</td>
      <td>2013-12-16 20:05:04</td>
      <td>     thunderbird-locale-en</td>
      <td> /usr/lib/thunderbird-addons/extensions/langpac...</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>396</th>
      <td>2013-12-16 20:08:27</td>
      <td>2013-12-16 20:05:03</td>
      <td>           software-center</td>
      <td>                  /usr/sbin/update-software-center</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>449</th>
      <td>2013-12-16 20:03:20</td>
      <td>2013-12-16 20:05:00</td>
      <td>          samba-common-bin</td>
      <td>                               /usr/bin/net.samba3</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>397</th>
      <td>2013-12-16 20:08:25</td>
      <td>2013-12-16 20:04:59</td>
      <td>     postgresql-client-9.1</td>
      <td>                  /usr/lib/postgresql/9.1/bin/psql</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>398</th>
      <td>2013-12-16 20:08:23</td>
      <td>2013-12-16 20:04:58</td>
      <td>            postgresql-9.1</td>
      <td>            /usr/lib/postgresql/9.1/bin/postmaster</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>452</th>
      <td>2013-12-16 20:03:20</td>
      <td>2013-12-16 20:04:55</td>
      <td>                  php5-dev</td>
      <td>                 /usr/include/php5/main/snprintf.h</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
    <tr>
      <th>440</th>
      <td>2013-12-16 20:03:20</td>
      <td>2013-12-16 20:04:54</td>
      <td>                  php-pear</td>
      <td>                       /usr/share/php/XML/Util.php</td>
      <td> &lt;RECENT-CTIME&gt;</td>
    </tr>
  </tbody>
</table>
</div>
</div>

Okay, cool, it says that I I installed ddd recently. And postgresql! I remember installing those things. Neat.

The whole message here is that if you have a timestamp in seconds or milliseconds or nanoseconds, then you can just "cast" it to a `datetime64[the-right-thing]` and pandas/numpy will take care of the rest.

## Where from here?

This was an attempt to provide concise cookbook with real life examples. We suggest you to have a look at the [official cookbook](http://pandas.pydata.org/pandas-docs/version/0.15.2/cookbook.html#cookbook) also.
