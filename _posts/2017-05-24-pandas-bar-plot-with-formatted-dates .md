---
layout: post
title:  "Pandas & Matplotlib: personalize the date format in a bar chart"
date:   2017-05-24 00:00:00
subtitle: A simple way to plot a bar chart with formatted dates on the x-axis with Pandas and Matplotlib
categories: programming
img:  # Add image post (optional)
tags: [pandas, matplotlib, visualization] # add tag
---

Yesterday, in the office, one of my colleague stumbled upon a problem that seemed really simple at first. He wanted to change the format of the dates on the x-axis in a simple bar chart with data read from a csv file. A really simple problem right? Well it happend that we spent quite some time in finding a simple and clean solution! We looked at several answers on Google and Stackoverflow, but nothing seemed to work. Finally I was able to came up with a solution that I will briefly explain here. (or you can look directly at this [[notebook]][notebook])

For this purpose I downloaded the timeseries of the Game of Thrones Wikipedia page views during Season 7 from [[here]][wiki_tool].
At first I simply plotted a line chart using this code:



```python
#import libraries
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
%matplotlib inline

#read data from csv
data = pd.read_csv('data.csv', usecols=['date','count'], parse_dates=['date'])
#set date as index
data.set_index('date',inplace=True)

#plot data
fig, ax = plt.subplots(figsize=(15,7))
data.plot(ax=ax)

#set ticks every week
ax.xaxis.set_major_locator(mdates.WeekdayLocator())
#set major ticks format
ax.xaxis.set_major_formatter(mdates.DateFormatter('%b %d'))
```


{% include image.html
   img="/assets/images/post/2017_05_24_line_plot.png"
   caption="Fig 1. Here you can see the number of page views of the Game of Thrones Wikipedia page. We can clearly see the peaks in the proximity of each episodes!"
%}

As you can see everything seems fine, the labels on the x-axis are well formatted with a label every week. What if we want to plot a bar chart instead? We can try to use the option **kind='bar'** in the pandas *plot()* function

```python
data.plot(kind='bar', ax=ax)
```

When we run the code again, we have the following error:

>**ValueError: DateFormatter found a value of x=0, which is an illegal date.  This usually occurs because you have not informed the axis that it is plotting dates, e.g., with ax.xaxis_date()**

and adding **ax.xaxis_date()** as suggested does not solve the problem! I tried to make the code work with the pandas *plot()* function but I couldn't find a solution. So after spending some time looking around, I decided to give up and started to use the matplotlib *bar()* function

```python
ax.bar(data.index, data['count'])
```
This is what we have

{% include image.html
   img="/assets/images/post/2017_05_24_wrong_bar_plot.png"
   caption="Fig 2. We need to fix the date format!"
%}


The date labels formatted in this way are ugly! So let's use the matplotlib **DateFormatter** to make them prettier...

```python
#set ticks every week
ax.xaxis.set_major_locator(mdates.WeekdayLocator())
#set major ticks format
ax.xaxis.set_major_formatter(mdates.DateFormatter('%b %d'))
```

{% include image.html
   img="/assets/images/post/2017_05_24_bar_plot.png"
   caption="Fig 3. Finally!"
%}

... and that's it! I hope this would help! Here you can find the code and the data that generated the plot in Fig 3. [[link]][notebook]

NOTE: If you are interseted in a short and clear way to understand the python visualization world with pandas and matplotlib here there is a great [resource][matplotlib].


[notebook]: https://github.com/scentellegher/code_snippets/blob/master/bar_chart_formatted_dates/Bar_chart_with_formatted_dates.ipynb
[wiki_tool]: https://tools.wmflabs.org/pageviews/?project=en.wikipedia.org&platform=all-access&agent=user&range=latest-20&pages=Cat|Dog
[matplotlib]: http://pbpython.com/effective-matplotlib.html