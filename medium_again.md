---
title: "test_medium_markdown"
author: "Haris Zafeiropoulos"
date: '2020-03-16'
output:
  html_document: 
    keep_md: yes
editor_options:
  chunk_output_type: console
---

# Import the e.coli dataset from bigg and create a matrix in R

### Here are the libraries needed




```r
library(jsonlite)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
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

## First, we try the .json format

### We first get the data


```r
data_url = "http://bigg.ucsd.edu/static/models/e_coli_core.json"
destination_file = "/home/haris/Desktop/gsoc2020/gsoc2020/e_coli_core.json"
download.file(data_url, destination_file)
```

### Then we parse them
We need to parse the .json file we got, to get a matrix with the reactions and the metabolites that participate in them.


```r
data = jsonlite::fromJSON( destination_file )

reactions = data$reactions

new_data = data["reactions"]


final_reactions = new_data$reactions$metabolites
reactions_names = reactions$id
table = cbind(reactions_names,final_reactions)

table_as_matrix = as.matrix(table)

head(table_as_matrix, 4)
```

```
##   reactions_names adp_c   atp_c    f6p_c     fdp_c h_c     accoa_c   coa_c    
## 1 "PFK"           " 1.00" " -1.00" "-1.0000" " 1"  " 1.00" NA        NA       
## 2 "PFL"           NA      NA       NA        NA    NA      " 1.0000" "-1.0000"
## 3 "PGI"           NA      NA       " 1.0000" NA    NA      NA        NA       
## 4 "PGK"           " 1.00" " -1.00" NA        NA    NA      NA        NA       
##   for_c pyr_c     g6p_c    13dpg_c 3pg_c    6pgc_c 6pgl_c h2o_c acald_c nad_c
## 1 NA    NA        NA       NA      NA       NA     NA     NA    NA      NA   
## 2 " 1"  "-1.0000" NA       NA      NA       NA     NA     NA    NA      NA   
## 3 NA    NA        "-1.000" NA      NA       NA     NA     NA    NA      NA   
## 4 NA    NA        NA       " 1"    "-1.000" NA     NA     NA    NA      NA   
##   nadh_c akg_c akg_e h_e 2pg_c pi_c pi_e etoh_c acald_e ac_c actp_c co2_c oaa_c
## 1 NA     NA    NA    NA  NA    NA   NA   NA     NA      NA   NA     NA    NA   
## 2 NA     NA    NA    NA  NA    NA   NA   NA     NA      NA   NA     NA    NA   
## 3 NA     NA    NA    NA  NA    NA   NA   NA     NA      NA   NA     NA    NA   
## 4 NA     NA    NA    NA  NA    NA   NA   NA     NA      NA   NA     NA    NA   
##   pep_c acon_C_c cit_c icit_c ac_e amp_c succoa_c e4p_c g3p_c gln__L_c glu__L_c
## 1 NA    NA       NA    NA     NA   NA    NA       NA    NA    NA       NA      
## 2 NA    NA       NA    NA     NA   NA    NA       NA    NA    NA       NA      
## 3 NA    NA       NA    NA     NA   NA    NA       NA    NA    NA       NA      
## 4 NA    NA       NA    NA     NA   NA    NA       NA    NA    NA       NA      
##   nadp_c nadph_c r5p_c pyr_e co2_e ru5p__D_c xu5p__D_c succ_c succ_e o2_c q8_c
## 1 NA     NA      NA    NA    NA    NA        NA        NA     NA     NA   NA  
## 2 NA     NA      NA    NA    NA    NA        NA        NA     NA     NA   NA  
## 3 NA     NA      NA    NA    NA    NA        NA        NA     NA     NA   NA  
## 4 NA     NA      NA    NA    NA    NA        NA        NA     NA     NA   NA  
##   q8h2_c lac__D_c lac__D_e etoh_e fum_c s7p_c dhap_c for_e fru_e fum_e glc__D_e
## 1 NA     NA       NA       NA     NA    NA    NA     NA    NA    NA    NA      
## 2 NA     NA       NA       NA     NA    NA    NA     NA    NA    NA    NA      
## 3 NA     NA       NA       NA     NA    NA    NA     NA    NA    NA    NA      
## 4 NA     NA       NA       NA     NA    NA    NA     NA    NA    NA    NA      
##   gln__L_e glu__L_e h2o_e mal__L_e nh4_e o2_e mal__L_c nh4_c glx_c
## 1 NA       NA       NA    NA       NA    NA   NA       NA    NA   
## 2 NA       NA       NA    NA       NA    NA   NA       NA    NA   
## 3 NA       NA       NA    NA       NA    NA   NA       NA    NA   
## 4 NA       NA       NA    NA       NA    NA   NA       NA    NA
```

### Result 
Here you can see finally how a single reaction from the network we had, can be seen in the first line of our matrix. One molecule of f6p_c and one atp_c are cosnumed to produce one of adp_c, one of h_c and one of fdp_c.

<div class="figure">
<img src="https://raw.githubusercontent.com/hariszaf/gsoc2020/master/test_medium_files/figure-html/gsoc2020_reactions.png" alt="One of the reactions of the metabolic network. You can see the metabolites that are consumed as well as those that are produced; the network (left) confirms what the matrix shows (right)." width="100%" />
<p class="caption">One of the reactions of the metabolic network. You can see the metabolites that are consumed as well as those that are produced; the network (left) confirms what the matrix shows (right).</p>
</div>


## Second, the .mat format
### Get the data.. again

```r
data_url = "http://bigg.ucsd.edu/static/models/e_coli_core.mat"
destination_file = "/home/haris/Desktop/gsoc2020/gsoc2020/e_coli_core.mat"
download.file(data_url, destination_file)
```

### Read the data and parse them properly

```r
data_from_mat = readMat(destination_file)

s_matrix = data_from_mat[1]
s_matrix = s_matrix[[1]]
s_matrix = matrix(s_matrix,ncol = ncol(s_matrix), nrow = nrow(s_matrix))

head(s_matrix)
```

```
##      [,1]      
## [1,] List,72   
## [2,] List,72   
## [3,] List,72   
## [4,] Numeric,72
## [5,] List,137  
## [6,] ?
```


### and finally, keep some more info from the data as variables


```r
reactions_from_mat = s_matrix[8]
metabolites_from_mat = s_matrix[1]
genes_from_mat = s_matrix[5]
```

### and what we have is this:

An enzyme from those that catalyze the reactions:

```r
reactions_from_mat[[1]][[1]]
```

```
## [[1]]
##      [,1] 
## [1,] "PFK"
```
The metabolites that take part: 

```r
metabolites_from_mat[[1]][[1]]
```

```
## [[1]]
##      [,1]      
## [1,] "glc__D_e"
```
And the genes from which these molecules come from:

```r
genes_from_mat[[1]][[1]]
```

```
## [[1]]
##      [,1]   
## [1,] "b1241"
```


## Discussion
It seems that the .mat format is easier to handle. However, as this project will be implemented to a great extent in Python, we need to handle the .json format as well. That is because .json format is commonly used when working with Python.

