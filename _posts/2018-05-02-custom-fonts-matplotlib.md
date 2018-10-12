---
layout: post
title:  "Add custom fonts to Matplotlib"
date:   2018-05-02 00:00:00
subtitle: How to easily install a custom font in Matplotlib
categories: visualization
---

This is step by step guide to add a custom font to Matplotlib. I have tested this procedure on both Linux and OS X machines.
Suppose you are running a Jupyter notebook and you want to change the font of a plot. Here is what you need to do! 

1. First, we need to install our custom font. Keep in mind that Matplotlib expects a font in *True Type* format (**.ttf**). For example, if we want to add the Helvetica font, we need to check if we have the font in .ttf format installed on our system otherwise we need to download it and install it.
2. Next, we need to update the font cache from the command line with the following command:
```ruby
sudo fc-cache -fv
```
3. Clear the matplotlib font cache:
```python
rm -fr ~/.cache/matplotlib
```
4. Restart your Jupyter notebook server

That's it! Now we need to instruct Matplotlib to use our custom font. Suppose that we want to plot a bar chart and change the labels and ticks font. In this case, just for visual purposes, we will use the Comic Sans font.

```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline

df = pd.DataFrame({'perc': pd.Series([45, 35, 10, 5, 3, 2], index=['A', 'B', 'C','D','E','F'])})

# specify the custom font to use
plt.rcParams['font.family'] = 'sans-serif'
plt.rcParams['font.sans-serif'] = 'Comic Sans MS'

fig, ax = plt.subplots(figsize=(7,4))
df.iloc[::-1].plot(kind='barh', legend = False, ax=ax)
ax.set_xlabel('Percentage',fontsize=15)
ax.set_ylabel('Type',fontsize=15)
```

Now, let's try to plot it and see the actual result!

{% include image.html
   img="/assets/images/post/2018_05_02_changed_font.png"
   caption="Fig 1. Font changed!"
   width=550
   height="auto"
%}

In case something went wrong and you receive an error like this

**UserWarning: findfont: Font family ['sans-serif'] not found. Falling back to DejaVu Sans (prop.get_family(), self.defaultFamily[fontext]))**

you need to check whether matplotlib is "seeing" your custom font. In order to do so, we can run in our notebook the following two lines of code
```python
import matplotlib.font_manager
matplotlib.font_manager.findSystemFonts(fontpaths=None, fontext='ttf')
```

If your font is not in the list, you need to copy your custom font in the system library
* **~/Library/Fonts/** in OS X
* **/usr/share/fonts/truetype** in Linux

and run again the commands from step 2 to 4.

You can see the complete code in this [[notebook]][notebook]


[notebook]: https://github.com/scentellegher/code_snippets/blob/master/matplotlib_font/Matplotlib_custom_font.ipynb