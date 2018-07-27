---
title: "Ch0"
author: "Rajan Kapoor"
date: "7/26/2018"
output:
  html_document:
    keep_md: TRUE
---



## Basic Intallations

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:




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
