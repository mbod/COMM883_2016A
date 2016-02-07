# Intro to ggplot2
Matt O'Donnell  
February 5, 2016  

## ggplot2 - The Grammar of Graphics


```r
library(ggplot2)
```

__ggplot2__ is an implementation of the Grammar of Graphics theory and method of data visualization that builds up plots through layers and components. Data is mapped to particular aesthetics, i.e. which variable in your data set should be mpapped to which element of a plot (e.g. the X and Y axis or a grouping element, such as different lines, colored bars or panels).

Having your data in a data frame is the most natural and straight forward representation for using ggplot2. 

#### This is a __wide__ data frame


```r
df_wide <- data.frame(
  id = paste('p', 1:5, sep=''),
  var1 = sample(1:100, 5),
  var2 = sample(500:1000, 5),
  var3 = sample(1000:10000,5),
  var4 = c('A','A','B','C','C')
)

df_wide
```

```
##   id var1 var2 var3 var4
## 1 p1   29  752 9063    A
## 2 p2   78  555 2374    A
## 3 p3   16  670 3030    B
## 4 p4   83  950 6775    C
## 5 p5   45  839 5385    C
```

## Simple Example: Scatter plot


```r
ggplot(df_wide, aes(x=var1, y=var2)) + geom_point()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-3-1.png)


## The components of a ggplot

### ``ggplot()`` function

* ``ggplot()`` is the main function required in every plot and is usually where you specify the data frame you are plotting and specify the mapping from data to plot element using the ``aes`` (aesthetics) function. 

You can build up the layers of a plot by assigning the result of the ``ggplot`` function to a variable and then adding layers. The following sets up the plot by specifying the data frame and the aesthetics, i.e. which variable should go to which axis in the plot. But it doesn't plot anything.


```r
p <- ggplot(df_wide, aes(x=var1, y=var2))
```

### A __geom__ function

Plots require a __geom__ function to display the data. Examples include:

* ``geom_point()`` - a scatter plot
* ``geom_histogram()`` - a histogram
* ``geom_density()`` - a density plot
* ``geom_bar()`` - a bar chart
* ``geom_boxplot()`` - a box plot
* ``geom_line()`` - a line plot
* ``geom_path()`` - a point to point path line plot (useful for geolocation data etc)
* ``geom_text()`` - a scatter plot with labels instead of points

#### A scatter plot of var1 (x) against var2 (y)


```r
p + geom_point()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-5-1.png)


#### A line plot connecting x,y points


```r
p + geom_line()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-6-1.png)


#### A path line plot connecting x,y points in order




```r
df_wide
```

```
##   id var1 var2 var3 var4
## 1 p1   29  752 9063    A
## 2 p2   78  555 2374    A
## 3 p3   16  670 3030    B
## 4 p4   83  950 6775    C
## 5 p5   45  839 5385    C
```

```r
p + geom_path()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-7-1.png)

If we reorder the data frame the plot will look different.


```r
df2_wide2 <- df_wide[order(df_wide$var1,df_wide$var2),]
p <- ggplot(df2_wide2, aes(x=var1, y=var2))
p + geom_path()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-8-1.png)


#### A bar graph counting the number of items in each group (var4)


```r
p <- ggplot(df_wide, aes(var4)) 
p + geom_bar()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-9-1.png)


#### A boxplot 


```r
p <- ggplot(df_wide, aes(x=var4, y=var1)) 
p + geom_boxplot()
```

![](ggplot_intro_files/figure-html/unnamed-chunk-10-1.png)

You can add aesthetic mappings to the __geom__ function as well to control specific features of the the plot, e.g. the fill color of each boxplot or bar


```r
p + geom_boxplot(aes(fill=var4))
```

![](ggplot_intro_files/figure-html/unnamed-chunk-11-1.png)


```r
p <- ggplot(df_wide, aes(var4))
p + geom_bar(aes(fill=var4))
```

![](ggplot_intro_files/figure-html/unnamed-chunk-12-1.png)


### Graph labels

* ``xlab()`` - specify label for x axis
* ``ylab()`` - specify label for y axis
* ``ggtitle()`` - specify title for graph and legends
* ``labs()`` - set all of the above with parameters in one function


```r
p <- ggplot(mtcars, aes(x=wt, y=mpg))
p + geom_point() +
  xlab('Miles per gallon') +
  ylab('Weight') +
  ggtitle('Car weight to fuel efficiency')
```

![](ggplot_intro_files/figure-html/unnamed-chunk-13-1.png)

is equivalent to


```r
p + geom_point() + 
  labs(x='Miles per gallon', 
       y='Weight',
       title='Car weight to fuel efficiency')
```

![](ggplot_intro_files/figure-html/unnamed-chunk-14-1.png)

### Adding additional aesthetics 

If you have more than two variables you want to represent you can make use of other graphic features such as:

* shape of points
* color of points
* size of points

These can be added as aesthetics to the specific __geom__ function, e.g. to ``geom_point`` for a scatter plot, by including a ``aes`` function


```r
# 1. set up the main mapping from data to x and y axes
p <- ggplot(mtcars, aes(x=mpg, y=wt))

# 2. set up a mapping with the color of points and the number of cylinders (needs to be treated as a factor not an numeric using factor())
p + geom_point(aes(colour=factor(cyl)))
```

![](ggplot_intro_files/figure-html/unnamed-chunk-15-1.png)


```r
# 3. add a fourth dimension for the displacement variable using point size
p + geom_point(aes(colour=factor(cyl), size=disp))
```

![](ggplot_intro_files/figure-html/unnamed-chunk-16-1.png)



```r
# 4. add in nicer labels for title, axes and legends

p + geom_point(aes(colour=factor(cyl), size=disp)) +
  ggtitle('Car relationship between weight and fuel efficency') +
  xlab('Car weight') +
  ylab('Miles per gallon') +
  ggtitle(aes(colour='Cylinders',
              size='Displacement'))
```

![](ggplot_intro_files/figure-html/unnamed-chunk-17-1.png)


### Adding lines to a plot

* ``geom_hline``
* ``geom_vline``
* ``geom_abline``






```r
ggplot(mtcars, aes(cyl,wt)) + stat_summary(fun.y=mean, geom='bar', aes(fill=factor(cyl))) + stat_summary(fun.data=mean_cl_boot, geom='errorbar', width=0.2)
```

```
## Warning: replacing previous import by 'ggplot2::unit' when loading 'Hmisc'
```

```
## Warning: replacing previous import by 'ggplot2::arrow' when loading 'Hmisc'
```

```
## Warning: replacing previous import by 'scales::alpha' when loading 'Hmisc'
```

![](ggplot_intro_files/figure-html/unnamed-chunk-18-1.png)
