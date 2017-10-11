---
layout: single
title: "How to remove borders and add legends to spatial plots in R. "
excerpt: "In this lesson we review how to remove those pesky borders from a raster plot using base plot in R. We also cover adding legends to your plot outside of the plot extent."
authors: ['Leah Wasser']
modified: '2017-10-11'
category: [courses]
class-lesson: ['how-to-hints-week8']
permalink: /courses/earth-analytics/spectral-remote-sensing-modis/refine-plots-report/
nav-title: 'Refine RGB plots'
week: 8
course: "earth-analytics"
sidebar:
  nav:
author_profile: false
comments: true
order: 2
topics:
  reproducible-science-and-programming:
  data-exploration-and-analysis: ['data-visualization']
  spatial-data-and-gis: ['raster-data']
redirect_from:
  - "/courses/earth-analytics/week-7/refine-plots-report/"
---

{% include toc title="In This Lesson" icon="file-text" %}

<div class='notice--success' markdown="1">

## <i class="fa fa-graduation-cap" aria-hidden="true"></i> Learning Objectives

After completing this tutorial, you will be able to:

* Remove borders and refine the size of plots in an output rmarkdown report.
* Adjust the aspect ratio of a plot rendered to a pdf using knitr.
* Customize the location of legends in a plot in `R`.

## <i class="fa fa-check-square-o fa-2" aria-hidden="true"></i> What you need

You will need a computer with internet access to complete this lesson and the
data for week 6 of the course.

[<i class="fa fa-download" aria-hidden="true"></i> Download Week 6 Data (~500 MB)](https://ndownloader.figshare.com/files/7677208){:data-proofer-ignore='' .btn }
</div>




In the previous lessons, we opened landsat and MODIS data in R. In this lesson,
we will learn how to refine our plots in R to make our report look nicer and
in turn more professional. First, let's import some data.




### Titles using plotRGB()


You can try to add a title but it won't plot
<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-rgb-1.png" title="Remove axes labels." alt="Remove axes labels." width="90%" />


If we add the `axes=T` argument to our plot, our title plots but we also get
the x and y axis in black which doesn't look nice. We don't need those. As a work
around we can set the x and y axis labels to
plot using "white" (`col.axis="white"`). Also we turn off the tick marks using `tck=0`.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-rgb2-1.png" title="Remove axes labels." alt="Remove axes labels." width="90%" />

The final step is to turn off box which leaves that annoying line on the left
hand side and bottom of the plot.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-rgb3-1.png" title="Remove axes labels." alt="Remove axes labels." width="90%" />

This looks nice, but now the plot itself is too tall. There is extra white space
above and below the plot that we don't need. This is because the `dev` space
where R studio renders plots is set to a particular size based upon how you've
adjusted it and your monitor resolution. I can account for this too - using the
code chunk arguments: `fig.width` and `fig.height`.

In my plot below, I used `fig.width=7, fig.height=6` in  my code chunk arguments.
The units are in inches.

My code chunk looks like this: `{r plot-rgb4, fig.cap="Adjust figure width and height.", fig.width=7, fig.height=6}`

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-rgb4-1.png" title="Adjust figure width and height." alt="Adjust figure width and height." width="90%" />

Notice that now my plot has less white space above and below the image. This is because
it's no longer plotting using a square aspect ratio - we've adjusted that! You can
experiment with different fig width and height values depending upon the aspect
ratio of the
plot that you are trying to print to your report.


## Reset dev

Any time you modify the parameters, you should consider resetting the `dev()` space.
Why? Because if you plot another plot in R, it will use the parameters that you
set previously! In the case above we set the axes to white. This setting will
become a global setting until you clear your plot space! To clear the plot dev
space programmatically, use `dev.off()`.



## Adjusting legends

Legends can also be trick to figure out in R. Take a look at the plot below.
It's not pretty. For one, we don't need the x and y axes on this plot.





<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-data1-1.png" title="ndvi plot - no legend" alt="ndvi plot - no legend" width="90%" />
First let's get rid of the unnecessary axes and turn off the legend.
We can remote the axes & box that surrounds our image using: `axes=F` and `box=F`.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-data2-1.png" title="ndvi plot - no legend" alt="ndvi plot - no legend" width="90%" />

Next, we turn off the legend and add our own legend. However, the legend isn't
exactly where we want it to be here. It's above the plot and we'd like it to be
to the right of the plot.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/plot-data3-1.png" title="ndvi plot - no legend" alt="ndvi plot - no legend" width="90%" />

We can force the legend to plot outside of our axes using the parameter
`xpd=T`. We can locate the legend in the upper right hand corner OUTSIDE
of our plot by specifying the max x and y values derived from the `extent` of
the spatial object that we are plotting

Here I set the x max value to me the **furthest east** hand corner of my object extent.
`x = ndvi_classified@extent@xmax`

Here I set the y max value to me the **furthest north** of my object extent.
`y = ndvi_classified@extent@ymax`


<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/fix-plot-legend-1.png" title="plot with legend in the upper right. " alt="plot with legend in the upper right. " width="90%" />

Now, another problem with our legend. *The order of our colors is all wrong:* Grey should represent "no vegetation" and green should represent health vegetation. We can use `rev()` on our list of colors to reverse the order of colors drawn on the legend.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/fix-plot-legend22-1.png" title="plot with legend in the upper right. " alt="plot with legend in the upper right. " width="90%" />

On to the pesky white space on either side of the plot. There are several
ways to handle this. One is by specifying margins for out plot.

Notice in the plot above that there is
too much white space above and below the plot. In the case of my plot, the aspect
ratio of width:height is not right. We want the height to be SMALLER to remove
some of the white space. The white space is there because R is trying to plot
our figure using a 1:1 aspect ratio. We can use the `mar` argument to adjust
the white space on the bottom, left, top, or right sides of the plot.
This makes room for our legend.

Below, we adjusted the top margin to `2` and the right margin to `5`.
This makes space for our legend but also makes a bit of space for our plot title.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/fix-plot-legend3-1.png" title="plot with legend in the upper right. " alt="plot with legend in the upper right. " width="90%" />


```
## RStudioGD 
##         2
```

I can do better than that however. That box around the legend is annoying. Let's
remove it using the legend argument: `bty = "n"`. Let's also make the legend
fonts a bit smaller using the argument `cex = .9`.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/fix-plot-legend33-1.png" title="plot with legend in the upper right. " alt="plot with legend in the upper right. " width="90%" />

```
## RStudioGD 
##         2
```

If things are still not looking right, we can adjust the size of our output
figure. Try adding the arguments fig.width and fig.height to your code chunk.
`{r fix-plot-legend3, fig.cap="plot with legend in the upper right.", fig.width=6, fig.height=4}`

I use these values because i want the HEIGHT of the figure to be smaller. I want
the WIDTH of the figure to be "landscape" or wider so i'll leave that value alone.

Below, I set `fig.width=6`, `fig.height=4` in my code chunk. setting a smaller
height pulled the title down closer to my plot. I've also decreased the font
size of my legend using the `cex` legend argument. Note that you may have to play with the
figure size a bit to get it just right.

HINT: use `dev.size()`` to figure out the size of your plot dev space in RStudio.

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/fix-plot-legend4-1.png" title="plot with legend in the upper right." alt="plot with legend in the upper right." width="90%" />

While we are at it, let's add a border using a crop extent
shapefile that was used to clip these data

<img src="{{ site.url }}/images/rfigs/courses/earth-analytics/week08/how-to/2017-03-01-howto02-refine-plots-R/import-shape-1.png" title="add crop box" alt="add crop box" width="90%" />

Again - always reset dev when you've been adjusting `par()` values for
plots!



That looks better, doesn't it? Leave comments below if you find other
tricks to make your plot look better!