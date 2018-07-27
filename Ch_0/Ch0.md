---
title: "Ch0"
author: "Rajan Kapoor"
date: "7/26/2018"
output:
  html_document:
    keep_md: TRUE
---



## Basic Intallation and setup




```r
library(downloader) ##use install.packages to install
dir <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/"
filename <- "femaleMiceWeights.csv" 
url <- paste0(dir, filename)
if (!file.exists(filename)) download(url, destfile=filename)
```

## Including Plots

You can also embed plots, for example:

![](Ch0_files/figure-html/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
