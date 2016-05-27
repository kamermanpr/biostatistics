Lecture 5
=========
css: ../custom.css
transition: none
width: 960
height: 720
autosize: false



## Commonly used statistical tests

- Base R graphics
- The grammer of graphics and ggplot2

<div style="position:fixed;bottom:10%">
    <h3 style="margin:0;">
        Introduction to Biostatistics
    </h3>
    <p style="font-style:italic;font-size:80%;margin-top:1%;margin-bottom:1%;">
        By: Peter Kamerman &nbsp&nbsp (view at
        <a href="//painblogr.org/biostatistics/" target="_blank">painblogR</a>)
    </p>
    <img src="./resources/cc-by.png" width="128" style="margin:0;"/>
</div>

Data visualization
==================

<div class="center", style="width:80%;">
    <p style="font-size:150%;font-style:italic;text-align:center;margin-top:60px;">
    "The greatest value of a picture is when it forces us to notice what we never expected to see."
    </p>
    <p style="text-align:center">
     John Tukey<br>
        <span style="font-style:italic;font-size:80%;">(Amongst many other things, developer of the boxplot)</span>
    </p>
</div>

Types of graphical analysis
===========================
type: twocol

**For exploratory analysis**
* Getting to know your data
* Rough and ready
<img src="./resources/rough-1.pdf" title="plot of chunk rough" alt="plot of chunk rough" style="display: block; margin: auto;" />

****

**For reporting**
* Presenting your data to others
* Tells a standalone story
* Polished
<img src="./resources/polished-1.pdf" title="plot of chunk polished" alt="plot of chunk polished" style="display: block; margin: auto;" />

Base graphics
=============
title: hide
type: aside

Base _R_ graphics

Base graphics
==============
class: vcenter

The _graphics_ and _grDevices_ packages are included in the base installation of _R_.

These packages add quite powerful graphing functions to base _R_.

**Graphing process in base R**
- Initalize a new plot
- Add to the new plot

Initializing a plot
===================
class: centre

Common plotting function calls

```r
# Scatter plots
plot(x, y, ...)

# Boxplots
boxplot(x, ...)

# Histograms
hist(x, ...)

# Bar charts
barplot(height, ...)

# Mosaic plots
mosaicplot(x, ...)
```

Examples
========
type: hcenter

**Scatter plots**

```r
plot(x = airquality$Ozone,
     y = airquality$Temp)
```

<img src="./resources/unnamed-chunk-2-1.pdf" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />

Examples
========
**Boxplots**

```r
boxplot(airquality$Ozone~airquality$Month)
```

<img src="./resources/unnamed-chunk-3-1.pdf" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

Examples
========
**Histograms**

```r
par(cex.label = 1.3, cex = 2, mar = c(4,4,1,1))
```

<img src="./resources/unnamed-chunk-4-1.pdf" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

Examples
========
**Bar charts**

```r
foo <- table(mtcars$cyl)
barplot(foo)
```

<img src="./resources/unnamed-chunk-5-1.pdf" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

Examples
========
**Mosaic plots**

```r
bar <- xtabs(~mtcars$cyl + mtcars$am)
mosaicplot(bar)
```

<img src="./resources/unnamed-chunk-6-1.pdf" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

Improving your plot
===================
**Key plot parameters to tweak the main features of the plot**<br>
_(find more parameters with `?par`)_

- **pch:** plotting symbol (default is open circle);
- **lty:** line type (default is solid line);
- **lwd:** line width;
- **col:** plotting color;
- **main:** overall title for the plot;
- **xlab:** title for the x-axis label;
- **ylab:** title for the y-axis label;
- **xlim, ylim:** set the axis limits.

Examples
========

```r
plot(x = airquality$Ozone,
     y = airquality$Temp,
     main = 'Look, blue traingles',
     xlab = 'Ozone', ylab = 'Temperature',
     pch = 24, col = 'blue',
     xlim = c(0, 200), ylim = c(50, 100))
```

<img src="./resources/unnamed-chunk-7-1.pdf" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />


Adding to your plot
===================
Once you have the plot, you can add to it.

**Add an abline**
<img src="./resources/unnamed-chunk-8-1.pdf" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

Adding to your plot
===================
type: twocol

**For example, add:**
- `abline()`
- `lines()`
- `points()`
- `text()`

****

![plot of chunk fancify](./resources/fancify-1.pdf)

Saving your plot
================
You can save your plot to file by specifying the graphics device before you start plotting the figure [e.g., `png()`, `pdf()`]. I like _pdf_ because the image is saved in vector format.

Remember to give a file name [e.g., `pdf('foo.pdf')`].

For _pdf_ outputs, you can also change the page orientation to landscape [e.g., `pdf('foo.pdf, paper = 'a4r')`].

If you open a device, remember to close it at the end [`dev.off()`], otherwise the file won't save. By default, the file will be saved to your working directory.

Example
=======
class: vcenter


```r
# Open the graphics device
pdf('foo.pdf', paper = 'a4r')

# Plot the figure
plot(x = airquality$Ozone, y = airquality$Temp,
     main = 'Look, blue traingles',
     xlab = 'Ozone', ylab = 'Temperature',
     pch = 24, col = 'blue',
     xlim = c(0, 200), ylim = c(50, 100))

# Close the graphics device
dev.off()
```

ggplot2
=======
title: hide
type: aside

Basic concepts:<br>
The grammer of graphics and _ggplot2_

The grammer of graphics
=======================

- Grammer gives language rules.

- Language consists of _words_, and _grammer_ provides the framework under which words can be combined and expressed (_statements_), thus setting the scope of the language.

- Leland Wilkinson published the _'Grammer of Graphics'_ in 1999.

- He conceptualized a grammer for graphics, that would move graphics passed mere chart types _(words)_ like pie-chart, bar-chart, etc.

- By providing a grammer for graphics, he hoped to provide the framework to build an unlimited array of graphic forms _(statements)_.

ggplot2: the grammer of graphics in R
====================================

<div class="center", style="width:90%;">
    <p style="font-size:130%;font-style:italic;text-align:center;margin-top:60px;">
    "In brief, the grammar tells us that a statistical graphic is a mapping from data to <span style="font-weight:bold">aesthetic attributes</span> (colour, shape, size) of <span style="font-weight:bold">geometric objects</span> (points, lines, bars). The plot may also contain statistical transformations of the data and is drawn on a specific <span style="font-weight:bold">coordinate system</span>."
    </p>
    <p style="text-align:center">
     Hadley Wickham<br>
        <span style="font-style:italic;font-size:80%;">(ggplot2: Elegant Graphics for Data Analysis (Use R!))</span>
    </p>
</div>

ggplot2
=======
class: vcenter

**Basic Components of a _ggplot2_ plot:**
- Data _(in a dataframe)_
- Aesthetics _(how data are mapped to color, size, etc)_
- Geoms _(geometric objects like points, lines, shapes)_
- Coordinate system

ggplot2
=======
Data and co-ordinatre system
<img class="center" style="z-index:-1" src="./resources/ggplot-0.pdf">

<div class="footer" style="font-size:80%">
     Garret Grolemund.<a href="//cdn.oreillystatic.com/en/assets/1/event/120/ggvis_%20Interactive,%20intuitive%20graphics%20in%20R%20Presentation.pdf">
   ggvis: Interactive, intuitive graphics in R</a>, 2014
</div>

ggplot2
=======
Mapping data to a geom
<img class="center" style="z-index:-1" src="./resources/ggplot-1.pdf">

<div class="footer" style="font-size:80%">
     Garret Grolemund.<a href="//cdn.oreillystatic.com/en/assets/1/event/120/ggvis_%20Interactive,%20intuitive%20graphics%20in%20R%20Presentation.pdf">
   ggvis: Interactive, intuitive graphics in R</a>, 2014
</div>

ggplot2
=======
Mapping data to an aesthetic (colour)
<img class="center" style="z-index:-1" src="./resources/ggplot-2.pdf">

<div class="footer" style="font-size:80%">
     Garret Grolemund.<a href="//cdn.oreillystatic.com/en/assets/1/event/120/ggvis_%20Interactive,%20intuitive%20graphics%20in%20R%20Presentation.pdf">
   ggvis: Interactive, intuitive graphics in R</a>, 2014
</div>

ggplot2
=======
Mapping data to an aesthetic (shape)
<img class="center" style="z-index:-1" src="./resources/ggplot-3.pdf">

<div class="footer" style="font-size:80%">
     Garret Grolemund.<a href="//cdn.oreillystatic.com/en/assets/1/event/120/ggvis_%20Interactive,%20intuitive%20graphics%20in%20R%20Presentation.pdf">
   ggvis: Interactive, intuitive graphics in R</a>, 2014
</div>

ggplot2
=======
Mapping data onto the co-ordinate system
<img class="center" style="z-index:-1" src="./resources/ggplot-4.pdf">

<div class="footer" style="font-size:80%">
     Garret Grolemund.<a href="//cdn.oreillystatic.com/en/assets/1/event/120/ggvis_%20Interactive,%20intuitive%20graphics%20in%20R%20Presentation.pdf">
   ggvis: Interactive, intuitive graphics in R</a>, 2014
</div>

Assignment
==========
type: tutorial
class: vcenter

Get hands-on practice with _base R graphics_ and _ggplot2_ by completing (at least) sections 5, 7, 8, 9, and 10 of the _swirl_ package course: _'Exploratory Data Analysis'_.


```r
# If you need to download the course
library(swirl)
install_from_swirl('Exploratory Data Analysis')
swirl()

# Otherwise
library(swirl)
swirl()
```

Web resources
=============
class: vcenter

_**Base graphics**_
- Quick-R: [basic graphics](http://www.statmethods.net/graphs/index.html), by Robert Kabacoff.
- Quick-R: [advanced graphics](http://www.statmethods.net/graphs/index.html), by Robert Kabacoff.

_**ggplot2**_
- [Cookbook for R - Graphs](http://www.cookbook-r.com/Graphs/), by Winston Chang.

- [Documentation](http://docs.ggplot2.org/current/index.html) documentation, by Hadley Wickham and Winston Chang.

- [Cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf), by RStudio.


tl;dr
===================================

<div class="center", style="width:80%;">
    <p style="font-size:150%;font-style:italic;text-align:center;margin-top:60px;">
    "Above all else show the data."
    </p>
    <p style="text-align:center">
     Edward Tufte<br>
        <span style="font-style:italic;font-size:80%;">(Author of:'The Visual Display of Quantitative Information'<br>and 'Beautiful Evidence')</span>
    </p>
</div>
