---
date: 2020-08-05
linktitle: How to convert a Python script to module
title: How to convert a Python script to module
weight: 10
url: /convert-python-script-to-module
description: How can we import the functions (or classes) from a script without having the script start doing something?
keywords:
  - python
  - name
  - main
  - script
  - module
  - import
tags: [python]  
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="How to convert a Python script to module" />
<meta name=”twitter:description” content="How can we import the functions (or classes) from a file without having the script start doing something?" />

It's common to create small scripts which we want to combine into a larger script. We don't want to copy and paste the code. We want to leave the working code in one file and use it in multiple places. Often we want to combine elements from multiple files to create more sophisticated scripts.

The problem we have is that when we import a script it actually starts running. This is generally not what we expect when we import a script so that we can reuse it.
How can we import the functions (or classes) from a file without having the script start doing something?

## Getting ready

Let's say that we have a handy implementation of the haversine distance function called `haversine()`, and it's in a file named `script.py`.

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

Initially, the file might look like this:

```python
import csv 
import pathlib 
from math import radians, sin, cos, sqrt, asin 
from functools import partial 

MI= 3959 
NM= 3440 
KM= 6373 

def haversine( lat_1: float, lon_1: float, 
    lat_2: float, lon_2: float, *, R: float ) -> float: 
    ... and more ... 

nm_haversine = partial(haversine, R=NM) 

source_path = pathlib.Path("waypoints.csv") 
with source_path.open() as source_file: 
    reader= csv.DictReader(source_file) 
    start = next(reader) 
    for point in reader: 
        d = nm_haversine( 
            float(start['lat']), float(start['lon']), 
            float(point['lat']), float(point['lon']) 
        ) 
        print(start, point, d) 
        start= point 
```

We've omitted the body of the `haversine()` function, showing only ... and more..., since the full source is available on [github](https://gist.github.com/chankeypathak/f51642a66792e8a0fca52446d3697349). We've focused on the context in which the function is in a Python script that also opens a file, `wapypoints.csv`, and does some processing on that file.

How can we import this module without it printing a display of distances between waypoints in our `waypoints.csv` file?

## How to do it...

Python scripts can be simple to write. Indeed, it's often too simple to create a working script. Here's how we transform a simple script into a reusable library:

1) Identify the statements that do the work of the script: we'll distinguish between definition and action. Statements such as `import`, `def`, and `class` are clearly definitional—they support the work but they don't do the work. Almost all other statements take action. In our example, we have four assignment statements that are more definition than action. The distinction is entirely one of intent. All statements, by definition, take an action. These actions, though, are more like the action of the def statement than they are like the action of the `with` statement later in the script. Here are the generally definitional statements:

    ```python
    MI= 3959 
    NM= 3440 
    KM= 6373 
    
    def haversine( lat_1: float, lon_1: float, 
    	lat_2: float, lon_2: float, *, R: float ) -> float: 
    	... and more ... 
    
    nm_haversine = partial(haversine, R=NM) 
    ```

The rest of the statements clearly take an action toward producing the printed results.

2) Wrap the actions into a function:

    ```python
    def analyze(): 
    	source_path = pathlib.Path("waypoints.csv") 
    	with source_path.open() as source_file: 
    		reader= csv.DictReader(source_file) 
    		start = next(reader) 
    		for point in reader: 
    			d = nm_haversine( 
    				float(start['lat']), float(start['lon']), 
    				float(point['lat']), float(point['lon']) 
    				) 
    			print(start, point, d) 
    			start= point 
    ```

3) Where possible, extract literals and make them into parameters. This is often a simple movement of the literal to a parameter with a default value. From this:

    ```python
    def analyze(): 
    	source_path = pathlib.Path("waypoints.csv") 
    ```

To this:

    ```python
    def analyze(source_name="waypoints.csv"): 
        source_path = pathlib.Path(source_name) 
    ```

4) Include the following as the only high-level action statements in the script file:

    ```python        
    if __name__ == "__main__": 
        analyze() 
    ```

We've packaged the action of the script as a function. The top-level action script is now wrapped in an if statement so that it isn't executed during import.

## How it works...

The most important rule for Python is that an `import` of a module is essentially the same as running the module as a script. The statements in the file are executed in order from top to bottom.

When we import a file, we're generally interested in executing the `def` and `class` statements. We might be interested in some assignment statements.

When Python runs a script, it sets a number of built-in special variables. One of these is `__name__`. This variable has two different values, depending on the context in which the file is being executed:

- The top-level script, executed from the command line: In this case, the value of the built-in special name of `__name__` is set to `__main__`.

- A file being executed because of an import statement: In this case, the value of `__name__` is the name of the module being created.
 
The standard name of `__main__` may seem a little odd at first. Why not use the filename in all cases? This special name is assigned because a Python script can be read from one of many sources. It can be a file. Python can also be read from the `stdin` pipeline, or it can be provided on the Python command line using the `-c` option.

When a file is being imported, however, the value of `__name__` is set to the name of the module. It will not be `__main__`. In our example, the value `__name__` during import processing will be `script`.

## There's more...

We can now build useful work around a reusable library. We might make several files that look like this:

File `trip_1.py`:

```python
from script import analyze 
analyze('trip_1.csv') 
```
	
Or perhaps something even more complex:

File `all_trips.py`:

```python
from script import analyze 
for trip in 'trip_1.csv', 'trip_2.csv': 
    analyze(trip) 
```		
		
The goal is to decompose a practical solution into two collections of features:

- The definition of classes and functions
- A very small action-oriented script that uses the definitions to do useful work

To get to this goal, we'll often start with a script that conflates both sets of features. This kind of script can be viewed as a **spike solution**. Our spike solution should evolve towards a more refined solution as soon as we're sure that it works.

A *spike* or *piton* is a piece of removable mountain-climbing gear that doesn't get us any higher on the route, but it enables us to climb safely.

### Also see
- [How to run a Python module as script?](https://tutswiki.com/run-module-as-script-python)
- [PEP 338 -- Executing modules as scripts](https://www.python.org/dev/peps/pep-0338/)
- [What is if __name__ == "__main__" in Python?](/if-name-main-in-python/)
- [What is the difference between a module and a script in Python?](https://stackoverflow.com/questions/2996110/what-is-the-difference-between-a-module-and-a-script-in-python)