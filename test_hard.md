---
title: "test_hard_markdown"
author: "Haris Zafeiropoulos"
date: '2020-03-17'
output:
  html_document: 
    keep_md: yes
editor_options:
  chunk_output_type: console
---



## Support lower dimensional polytopes in volesti and use existing methods to sample from them


Here are the libraries needed


```r
library(ggplot2)
library(R.matlab)
```

```
## R.matlab v3.6.2 (2018-09-26) successfully loaded. See ?R.matlab for help.
```

```
## 
## Attaching package: 'R.matlab'
```

```
## The following objects are masked from 'package:base':
## 
##     getOption, isOpen
```

```r
library(volesti)
```

```
## Loading required package: Rcpp
```

### Get the rounded polytope file (.mat) and read it 

```r
rounded_polytope_file = "/home/haris/Desktop/gsoc2020/gsoc2020/roundedPolytope.mat"
rounded_polytope = readMat(rounded_polytope_file)
rounded_polytope = rounded_polytope$poly
```

### We keen all the variables needed in order to move from the rounded polytope back to the initial as well as from the low back to full dimension.

```r
A_init = rounded_polytope[1]
A_init = A_init[[1]]
A = A_init[[1]]

b_init = rounded_polytope[2]
b_init = b_init[[1]]
b = b_init[[1]]

G_init = rounded_polytope[5]
G_init = G_init[[1]]
G = G_init[[1]]

shift_init = rounded_polytope[6]
shift_init = shift_init[[1]]
shift = shift_init[[1]]
```

And now we have A and b defining our initial polytope (before the transformations occured in the Matlab step), N for getting fron n-m dimensions back to n and shift a variable that allows us to move our polytope. 
### Now we need to sample from our rounded, low dimension polytope

```r
N = 2000
full_dim_polytope = Hpolytope$new(A=A, b=b)
points = sample_points(full_dim_polytope, n=N, random_walk = list("walk" = "BiW", "walk_length" = 1))
dim(points)
```

```
## [1]   24 2000
```


### Now we need to map our sampling points, back to the full dimension polytope

```r
maped_points = G%*%points + matrix(rep(shift,N), ncol = N)
dim(maped_points)
```

```
## [1]   95 2000
```








