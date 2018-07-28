---
title: "Chapter 0"
author: "RK"
date: "7/26/2018"
output:
  html_document:
    keep_md: TRUE
---


Code for this page was tested in R version 3.5.1 with following packages:


```
##  package     * version date       source        
##  backports     1.1.2   2017-12-13 CRAN (R 3.5.0)
##  clisymbols    1.2.0   2017-05-21 CRAN (R 3.5.0)
##  digest        0.6.15  2018-01-28 CRAN (R 3.5.0)
##  evaluate      0.11    2018-07-17 CRAN (R 3.5.0)
##  htmltools     0.3.6   2017-04-28 CRAN (R 3.5.0)
##  knitr         1.20    2018-02-20 CRAN (R 3.5.0)
##  magrittr      1.5     2014-11-22 CRAN (R 3.5.0)
##  Rcpp          0.12.17 2018-05-18 CRAN (R 3.5.0)
##  rmarkdown     1.10    2018-06-11 CRAN (R 3.5.0)
##  rprojroot     1.3-2   2018-01-03 CRAN (R 3.5.0)
##  sessioninfo   1.0.0   2017-06-21 CRAN (R 3.5.0)
##  stringi       1.2.4   2018-07-20 CRAN (R 3.5.0)
##  stringr       1.3.1   2018-05-10 CRAN (R 3.5.0)
##  withr         2.1.2   2018-03-15 CRAN (R 3.5.0)
##  yaml          2.1.19  2018-05-01 CRAN (R 3.5.0)
```
### Getting Started

#### Basic Intallation and setup




```r
library(downloader) 
url <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/femaleMiceWeights.csv"
filename <- "femaleMiceWeights.csv" 
download(url, destfile=filename)
```
#### Problem 1

Read in the file femaleMiceWeights.csv and report the body weight of the mouse in the exact name of the column containing the weights.

```r
dat <- read.csv(filename)
```

#### Problem 2
The [ and ] symbols can be used to extract specific rows and specific columns of the table. What is the entry in the 12th row and second column?

```r
# What is the data type of dat
typeof(dat)
```

```
## [1] "list"
```

```r
#df <- as.data.frame(dat)
dat[12,2]
```

```
## [1] 26.25
```
#### Problem 3
You should have learned how to use the \$ character to extract a column from a table and return it as a vector. Use \$ to extract the weight column and report the weight of the mouse in the 11th row.


```r
print(names(dat))
```

```
## [1] "Diet"       "Bodyweight"
```

```r
dat$Bodyweight[11]
```

```
## [1] 26.91
```

#### Problem 4
The length function returns the number of elements in a vector. How many mice are included in our dataset?


```r
length(dat[,1])
```

```
## [1] 24
```


#### Problem 5

To create a vector with the numbers 3 to 7, we can use seq(3,7) or, because they are consecutive, 3:7. View the data and determine what rows are associated with the high fat or hf diet. Then use the mean function to compute the average weight of these mice.


```r
mean(dat[13:24,2])
```

```
## [1] 26.83417
```

#### Problem 6

One of the functions we will be using often is sample. Read the help file for sample using ?sample. Now take a random sample of size 1 from the numbers 13 to 24 and report back the weight of the mouse represented by that row. Make sure to type set.seed(1) to ensure that everybody gets the same answer.


```r
set.seed(1)
sample(x = dat[13:24,2], size=1)
```

```
## [1] 25.34
```


