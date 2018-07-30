---
title: "Chapter 0 Introduction"
author: "RK"
date: "7/26/2018"
output:
  html_document:
    keep_md: yes
    theme: united
    toc: yes
---


Code for this page was tested in R version 3.5.1. Check RMarkdown file for list of all packages.


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
dat <- read.csv("femaleMiceWeights.csv")
```

#### Problem 2
The [ and ] symbols can be used to extract specific rows and specific columns of the table. What is the entry in the 12th row and second column?

```r
# What is the data type of dat
#typeof(dat)
#class(dat)
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


### dplyr Exercises

#### Basic Intallation and setup

```r
??msleep
```


```r
library(downloader) 
url <- "https://raw.githubusercontent.com/tidyverse/ggplot2/master/data-raw/msleep.csv"
filename <- "msleep.csv"
download(url, destfile=filename)
```

#### Problem 1

Read in the msleep_ggplot2.csv file with the function read.csv and use the function class to determine what type of object is returned.

```r
dat <- read.csv("msleep.csv")
class(dat)
```

```
## [1] "data.frame"
```

```r
typeof(dat)
```

```
## [1] "list"
```

#### Problem 2
Now use the filter function to select only the primates. How many animals in the table are primates? Hint: the nrow function gives you the number of rows of a data frame or matrix.

```r
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
filter(dat, order=="Primates") %>% nrow
```

```
## [1] 12
```

#### Problem 3
What is the class of the object you obtain after subsetting the table to only include primates?

```r
class(filter(dat, order=="Primates"))
```

```
## [1] "data.frame"
```

#### Problem 4
Now use the select function to extract the sleep (total) for the primates. What class is this object? Hint: use %>% to pipe the results of the filter function to select.

```r
primates_sleep <- filter(dat, order=="Primates") %>% select(sleep_total)
head(primates_sleep)
```

```
##   sleep_total
## 1        17.0
## 2        10.0
## 3        10.9
## 4         9.8
## 5         8.0
## 6         9.5
```

```r
class(primates_sleep)
```

```
## [1] "data.frame"
```

#### Problem 5
Now we want to calculate the average amount of sleep for primates (the average of the numbers computed above). One challenge is that the mean function requires a vector so, if we simply apply it to the output above, we get an error. Look at the help file for unlist and use it to compute the desired average.

```r
mean(primates_sleep)
```

```
## Warning in mean.default(primates_sleep): argument is not numeric or
## logical: returning NA
```

```
## [1] NA
```

```r
mean(unlist(primates_sleep))
```

```
## [1] 10.5
```

#### Problem 6
For the last exercise, we could also use the dplyr summarize function. We have not introduced this function, but you can read the help file and repeat exercise 5, this time using just filter and summarize to get the answer.

```r
#?summarize
filter(dat, order=="Primates") %>% summarise(mean = mean(sleep_total))
```

```
##   mean
## 1 10.5
```



