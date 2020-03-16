---
title: "test_easy_markdown"
author: "Haris Zafeiropoulos"
date: '2020-03-16'
output:
  html_document: 
    keep_md: yes
editor_options:
  chunk_output_type: console
---



## Compile and run VolEsti. Use the R extension to visualize sampling in a polytope.

Here are the libraries needed


```r
options(tinytex.verbose = TRUE)
library(ggplot2)
library(volesti)
```

```
## Loading required package: Rcpp
```




### keep a polytope as a variable after reading a .ine file 


```r
options(tinytex.verbose = TRUE)
polytope = file_to_polytope("/home/haris/volume_approximation/R-proj/inst/extdata/cross_10.ine")
```



### random sampling with different algorithms 


```r
options(tinytex.verbose = TRUE)
points_cdhr =  sample_points(P = polytope, n = 5000, distribution = list("density" = "uniform"),  random_walk = list("walk"="CDHR"))
points_rdhr =  sample_points(P = polytope, n = 5000, distribution = list("density" = "uniform"),  random_walk = list("walk"="RDHR"))
points_biw =  sample_points(P = polytope, n = 5000, distribution = list("density" = "uniform"),  random_walk = list("walk"="BiW"))
points_baw =  sample_points(P = polytope, n = 5000, distribution = list("density" = "uniform"),  random_walk = list("walk"="BaW"))
```


### keep sample points 


```r
options(tinytex.verbose = TRUE)
BaW = data.frame(x=points_baw[1,], y=points_baw[2,])
BiW = data.frame(x=points_biw[1,], y=points_biw[2,])
CDHR = data.frame(x=points_cdhr[1,], y=points_cdhr[2,])
RDHR = data.frame(x=points_rdhr[1,], y=points_rdhr[2,])
```


### bind all the points in a dataframe

```r
options(tinytex.verbose = TRUE)
walks = rbind(BaW,BiW,CDHR,RDHR)
```


### add a column in the dataframe to tag the points of each algorithm

```r
walks$random_walk = c(rep("BaW",5000),rep("BiW",5000),rep("CDHR",5000),rep("RDHR",5000)) 
```



### plot it 

```r
ggplot(walks, aes(x=x, y=y, group=random_walk,col=random_walk)) + 
  geom_point() + geom_smooth() + labs(title = "uniform sampling on cross_10.ine file / H-polytope")
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](test_easy_files/figure-html/plot-1.png)<!-- -->




### more sampling, more plots


```r
P = gen_cube(2, 'H')
p=sample_points(P, n=2000, random_walk = list("walk" = "BiW", "walk_length" = 1))
r = t(p)
q = as.data.frame(r)
```



```r
names(q)[1]='x'
names(q)[2]='y'
ggplot(q, aes(x=x,y=y)) +
       geom_point() + geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](test_easy_files/figure-html/more_plots_1-1.png)<!-- -->


### Get sample points from the inside of polytope.

```r
P = gen_rand_hpoly(2,17, seed=1917)
p=sample_points(P, n=2000, random_walk = list("walk" = "RDHR", "walk_length" = 1))
r = t(p)
q1 = as.data.frame(r)
```



```r
names(q1)[1]='x'
names(q1)[2]='y'
ggplot(q1, aes(x=x,y=y)) +
       geom_point() + geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](test_easy_files/figure-html/more_plots_2-1.png)<!-- -->



### Get boundary points from the same polytope.

```r
p=sample_points(P, n=2000, random_walk = list("walk" = "BRDHR", "walk_length" = 1))
r = t(p)
q2 = as.data.frame(r)
```



```r
names(q2)[1]='x'
names(q2)[2]='y'
ggplot(q2, aes(x=x,y=y)) +
       geom_point() + geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](test_easy_files/figure-html/more_plots_3-1.png)<!-- -->


### and together..

```r
final_points=rbind(q1,q2)
final_points$random_walk = c(rep("RDHR",2000), rep("BRDHR",2000))

ggplot(final_points, aes(x=x, y=y, group=random_walk,col=random_walk)) + 
  geom_point() + geom_smooth() + labs(title = "in and out random walks")
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](test_easy_files/figure-html/final_plot-1.png)<!-- -->
