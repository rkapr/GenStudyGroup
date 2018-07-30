---
title: "Chapter 1"
author: "RK"
date: "7/29/2018"
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

### Random Variables Exercises

#### Basic Intallation and setup


```r
library(downloader) 
url <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/femaleControlsPopulation.csv"
filename <- basename(url)
download(url, destfile=filename)
x <- unlist( read.csv(filename) )
```
Here x represents the weights for the entire population.

#### Problem 1
What is the average of these weights?

```r
mean(x)
```

```
## [1] 23.89338
```

#### Problem 2
After setting the seed at 1, set.seed(1) take a random sample of size 5. What is the absolute value (use abs) of the difference between the average of the sample and the average of all the values?

```r
set.seed(1)
x_sample <- sample(x, size=5)
abs(mean(x_sample) - mean(x))
```

```
## [1] 0.2706222
```

#### Problem 3
After setting the seed at 5, set.seed(5) take a random sample of size 5. What is the absolute value of the difference between the average of the sample and the average of all the values?

```r
set.seed(5)
x_sample <- sample(x, size=5)
abs(mean(x_sample) - mean(x))
```

```
## [1] 1.433378
```

#### Problem 4
Why are the answers from 2 and 3 different?

A) Because we made a coding mistake.
B) Because the average of the x is random.
C) Because the average of the samples is a random variable.
D) All of the above.


```
## [1] "C. Because the average of the samples is a random variable."
```

#### Problem 5
Set the seed at 1, then using a for-loop take a random sample of 5 mice 1,000 times. Save these averages. What percent of these 1,000 averages are more than 1 ounce away from the average of x ?


```r
set.seed(1)
N <- 1000
result5 <- vector("numeric", N) # prepare a container
for (i in 1:N){
    result5[i] <- mean(sample(x, size=5))
}

sum(abs(result5-mean(x))>1.0)/N*100
```

```
## [1] 49.8
```


#### Problem 6
We are now going to increase the number of times we redo the sample from 1,000 to 10,000. Set the seed at 1, then using a for-loop take a random sample of 5 mice 10,000 times. Save these averages. What percent of these 10,000 averages are more than 1 ounce away from the average of x ?

```r
set.seed(1)
N <- 10000
result <- vector("numeric", N) # prepare a container
for (i in 1:N){
    result[i] <- mean(sample(x, size=5))
}

sum(abs(result-mean(x))>1.0)/N*100
```

```
## [1] 49.76
```

#### Problem 7
Note that the answers to 4 and 5 barely changed. This is expected. The way we think about the random value distributions is as the distribution of the list of values obtained if we repeated the experiment an infinite number of times. On a computer, we can’t perform an infinite number of iterations so instead, for our examples, we consider 1,000 to be large enough, thus 10,000 is as well. Now if instead we change the sample size, then we change the random variable and thus its distribution.

Set the seed at 1, then using a for-loop take a random sample of 50 mice 1,000 times. Save these averages. What percent of these 1,000 averages are more than 1 ounce away from the average of x ?

```r
set.seed(1)
N <- 1000
result50 <- vector("numeric", N) # prepare a container
for (i in 1:N){
    result50[i] <- mean(sample(x, size=50))
}

sum(abs(result50-mean(x))>1.0)/N*100
```

```
## [1] 1.9
```

#### Problem 8
Use a histogram to “look” at the distribution of averages we get with a sample size of 5 and a sample size of 50. How would you say they differ?

A) They are actually the same.
B) They both look roughly normal, but with a sample size of 50 the spread is smaller.
C) They both look roughly normal, but with a sample size of 50 the spread is larger.
D) The second distribution does not look normal at all.


```r
library(ggplot2)
p1 <- qplot(result5, geom="histogram") 
p2 <- qplot(result50, geom="histogram") 
gridExtra::grid.arrange(p1, p2, ncol = 2)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Ch1_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```
## [1] "B. They both look roughly normal, but with a sample size of 50 the spread is smaller."
```

#### Problem 9
For the last set of averages, the ones obtained from a sample size of 50, what percent are between 23 and 25?

```r
sum(23<=result50 & result50<=25)/N*100
```

```
## [1] 97.6
```

#### Problem 10
Now ask the same question of a normal distribution with average 23.9 and standard deviation 0.43.

```r
result_norm <-rnorm(N, mean = 23.9, sd = 0.43)
sum(23<=result_norm & result_norm<=25)/N*100
```

```
## [1] 97.2
```

The answer to 9 and 10 were very similar. This is because we can approximate the distribution of the sample average with a normal distribution. We will learn more about the reason for this next.



### Population, Samples, and Estimates Exercises

#### Basic Intallation and setup

```r
library(downloader) 
url <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/mice_pheno.csv"
filename <- basename(url)
download(url, destfile=filename)
dat <- read.csv(filename)
```
We will remove the lines that contain missing values:

```r
dat <- na.omit( dat )
```

#### Problem 1
Use dplyr to create a vector x with the body weight of all males on the control (chow) diet. What is this population’s average?

#### Problem 2
Now use the rafalib package and use the popsd function to compute the population standard deviation.

#### Problem 3
Set the seed at 1. Take a random sample X of size 25 from x. What is the sample average?

#### Problem 4
Use dplyr to create a vector y with the body weight of all males on the high fat (hf) diet. What is this population’s average?

#### Problem 5
Now use the rafalib package and use the popsd function to compute the population standard deviation.

#### Problem 6
Set the seed at 1. Take a random sample Y of size 25 from y. What is the sample average?

#### Problem 7
What is the difference in absolute value between $\bar{Y}-\bar{X}$ and $\bar{X}-\bar{Y}$?

#### Problem 8
Repeat the above for females. Make sure to set the seed to 1 before each sample call. What is the difference in absolute value between $\bar{Y}-\bar{X}$ and $\bar{X}-\bar{Y}$?

#### Problem 9
For the females, our sample estimates were closer to the population difference than with males. What is a possible explanation for this?

A) The population variance of the females is smaller than that of the males; thus, the sample variable has less variability.
B) Statistical estimates are more precise for females.
C) The sample size was larger for females.
D) The sample size was smaller for females.
