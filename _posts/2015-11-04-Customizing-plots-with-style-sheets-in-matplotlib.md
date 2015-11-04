---
title: Matplotlib Style Sheets
author: juliano
---

Hi all, how are you?

[Matplotlib](http://matplotlib.org/) is a [python](http://python.org/) plotting library and a very useful tool for researchers. You can use it to generate all desired graphs using only one command in order to make results evaluation quicker and easier.

Since version 1.4, Matplotlib provides support to customize graphs through the **style** package.  You can check the default styles through the command:

{% highlight python %}
from matplotlib import pyplot as plt
print (plt.style.available)

[ 'seaborn-whitegrid', 
  'seaborn-white', 
  'grayscale', 
  'seaborn-muted', 
  'seaborn-colorblind', 
  'seaborn-paper', 
  'dark_background', 
  'ggplot', 'classic', 
  'seaborn-darkgrid', 
  'seaborn-ticks', 
  'seaborn-bright', 
  'seaborn-notebook', 
  'seaborn-pastel', 
  'seaborn-talk', 
  'seaborn-poster', 
  'bmh', 
  'seaborn-dark-palette', 
  'fivethirtyeight', 
  'seaborn-dark',  
  'seaborn-deep']
{% endhighlight %}

As I searched for a "palette" of these styles and I did not found, I decided to test them and post the result here. It might be useful and help any of you decide which style use in your plots.

![bmh](/assets/matplotlib/name_bmh.png)
![seaborn-talk](/assets/matplotlib/name_seaborn-talk.png)
![seaborn-darkgrid](/assets/matplotlib/name_seaborn-darkgrid.png)
![seaborn-notebook](/assets/matplotlib/name_seaborn-notebook.png)
![seaborn-dark-palette](/assets/matplotlib/name_seaborn-dark-palette.png)
![seaborn-dark](/assets/matplotlib/name_seaborn-dark.png)
![seaborn-muted](/assets/matplotlib/name_seaborn-muted.png)
![ggplot](/assets/matplotlib/name_ggplot.png)
![seaborn-white](/assets/matplotlib/name_seaborn-white.png)
![classic](/assets/matplotlib/name_classic.png)
![seaborn-pastel](/assets/matplotlib/name_seaborn-pastel.png)
![seaborn-poster](/assets/matplotlib/name_seaborn-poster.png)
![grayscale](/assets/matplotlib/name_grayscale.png)
![dark_background](/assets/matplotlib/name_dark_background.png)
![fivethirtyeight](/assets/matplotlib/name_fivethirtyeight.png)
![seaborn-paper](/assets/matplotlib/name_seaborn-paper.png)
![seaborn-colorblind](/assets/matplotlib/name_seaborn-colorblind.png)
![seaborn-deep](/assets/matplotlib/name_seaborn-deep.png)
![seaborn-bright](/assets/matplotlib/name_seaborn-bright.png)
![seaborn-ticks](/assets/matplotlib/name_seaborn-ticks.png)
![seaborn-whitegrid](/assets/matplotlib/name_seaborn-whitegrid.png)


Thanks for your attention!
