---
layout: post
title:  "PySpark: great-circle distance"
date:   2021-03-23 00:00:00
subtitle: How to compute the great-circle distance between two GPS points with PySpark without using UDFs.
categories: programming
img:  # Add image post (optional)
tags: [pyspark, spatial data, programming, great-circle distance, haversine distance] # add tag
---

Suppose we want to compute the distance between millions of GPS points (lat, lon) with PySpark.  
We could write a PySpark UDF that wraps a distance function of a python library like [geopy][geopy]. The problem with UDFs or vectorized UDFs is that they suffer from high serialization and invocation overhead thus they are SLOW (they are written in python)!  
**We shouldn't use UDFs unless we have no other choice!**

Here you can find a pure PySpark implementation of the great-circle distance (in km) between two GPS points _p1_ and _p2_


```python
import pyspark.sql.functions as F

def distance(lat_p1, lon_p1, lat_p2, lon_p2):
    '''
    Calculates the great-circle distance (in km) between two 
    GPS points p1 and p2
    https://en.wikipedia.org/wiki/Great-circle_distance#Formulae
    -------------------------------------
    :param lat_p1: latitude of origin point
    :param lon_p1: longitude of origin point
    :param lat_p1: latitude of destination point
    :param lon_p1: longitude of destination point
    :returns: distance in km
    '''
    return F.acos(
        F.sin(F.toRadians(lat_p1)) * F.sin(F.toRadians(lat_p2)) + 
        F.cos(F.toRadians(lat_p1)) * F.cos(F.toRadians(lat_p2)) * 
            F.cos(F.toRadians(lon_p1) - F.toRadians(lon_p2))
    ) * F.lit(6371.0)
```


[geopy]: https://pypi.org/project/geopy/