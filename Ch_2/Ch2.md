---
title: "Chapter 2 Exploratory Data Analysis"
author: "RK"
date: "8/1/2018"
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

#### Problem 1
<img src="Ch2_files/exercise1.png" style="display: block; margin: auto;" />

Given the above histogram, how many people are between the ages of 35 and 45?

```
## [1] "6."
```
#### Problem 2
The InsectSprays data set is included in R. The dataset reports the counts of insects in agricultural experimental units treated with different insecticides. Make a boxplot and determine which insecticide appears to be most effective.

```r
groups <- split(unlist(InsectSprays),InsectSprays[,2])
boxplot(groups,ylab="counts of insects after treatment",xlab="insecticides")
```

![](Ch2_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```
## [1] "C appears to be most effective."
```
#### Problem 3
Download and load this dataset into R. Use exploratory data analysis tools to determine which two columns are different from the rest. Which is the column that is positively skewed?

```r
par(mfrow = c(2,3))

ids <- c('A','B','C','D','E','F')
par(mfrow = c(2,3))
for(id in ids){
   hist(unlist(groups[id]), main = id, xlab = "insects after treatment")
}
```

![](Ch2_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
par(mfrow = c(1,1))
```

```
## [1] "A, B, C, D, F: right (positively) skewed."
```

#### Problem 4
Which is the column that is negatively skewed?

```
## [1] "E: left (negatively) skewed."
```


#### Problem 5
Let’s consider a random sample of finishers from the New York City Marathon in 2002. This dataset can be found in the UsingR package. Load the library and then load the nym.2002 dataset.

```r
#install.packages("UsingR")
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
data(nym.2002, package="UsingR")
```

Use boxplots and histograms to compare the finishing times of males and females. Which of the following best describes the difference?

A) Males and females have the same distribution.
B) Most males are faster than most women.
C) Male and females have similar right skewed distributions with the former, 20 minutes shifted to the left.
D) Both distribution are normally distributed with a difference in mean of about 30 minutes.


```r
group <- split(unlist(nym.2002['time']),nym.2002['gender']) 
par(mfrow = c(1,3))
boxplot(group)
timeM <- unlist(group['Male'])
hist(timeM, main = "Males", xlab = "time")
timeF <- unlist(group['Female'])
hist(timeF, main = "Females", xlab = "time")
```

![](Ch2_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
par(mfrow = c(1,1))

median(timeM)-median(timeF)
```

```
## [1] -21.70833
```

#### Problem 6
Use `dplyr` to create two new data frames: males and females, with the data for each gender. For males, what is the Pearson correlation between age and time to finish?

```r
males <- filter(nym.2002, gender=='Male') %>% select(c(age,time))
females <- filter(nym.2002, gender=='Female') %>% select(c(age,time))
cor(males['age'],males['time'])
```

```
##          time
## age 0.2432273
```

#### Problem 7
For females, what is the Pearson correlation between age and time to finish?

```r
cor(females['age'],females['time'])
```

```
##          time
## age 0.2443156
```

#### Problem 8
If we interpret these correlations without visualizing the data, we would conclude that the older we get, the slower we run marathons, regardless of gender. Look at scatterplots and boxplots of times stratified by age groups (20-25, 25-30, etc..). After examining the data, what is a more reasonable conclusion?

A) Finish times are constant up until about our 40s, then we get slower.
B) On average, finish times go up by about 7 minutes every five years.
C) The optimal age to run a marathon is 20-25.
D) Coding errors never happen: a five year old boy completed the 2012 NY city marathon.

```r
groups <- split(unlist(nym.2002['time']),round(unlist(nym.2002['age'])/5)*5) 
boxplot(groups, xlab  = "age", ylab = "time to finish")
```

![](Ch2_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

```
## [1] "C. The optimal age to run a marathon is 20-25."
```

#### Problem 9
When is it appropriate to use pie charts or donut charts?

A) When you are hungry.
B) To compare percentages.
C) To compare values that add up to 100%.
D) Never.


```
## [1] "D. Never."
```

#### Problem 10
The use of pseudo-3D plots in the literature mostly adds:

A) Pizzazz.
B) The ability to see three dimensional data.
C) Ability to discover.
D) Confusion.


```
## [1] "D. Confusion."
```


