---
title: "test_medium_markdown"
author: "Haris Zafeiropoulos"
date: '2020-03-15'
output:
  html_document: 
    keep_md: yes
editor_options:
  chunk_output_type: console
---

## Import the e.coli data from bigg and create a matrix in R

Here are the libraries needed




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


## We first get the data.


```r
data_url = "http://bigg.ucsd.edu/static/models/e_coli_core.json"
destination_file = "/home/haris/Desktop/gsoc2020/gsoc2020/e_coli_core.json"
download.file(data_url, destination_file)
```

## Then we parse them.
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


