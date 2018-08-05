---
title: "Chapter 3 Robust Summaries and Rank Tests"
author: "RK"
date: "8/4/2018"
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

#### Basic installation and setup
We are going to explore the properties of robust statistics. We will use one of the datasets included in R, which contains weight of chicks in grams as they grow from day 0 to day 21. This dataset also splits up the chicks by different protein diets, which are coded from 1 to 4. We use this dataset to also show an important operation in R (not related to robust summaries): reshape.

This dataset is built into R and can be loaded with:

```r
data(ChickWeight)
```
To begin, take a look at the weights of all observations over time and color the points to represent the Diet:

```r
head(ChickWeight)
```

```
##   weight Time Chick Diet
## 1     42    0     1    1
## 2     51    2     1    1
## 3     59    4     1    1
## 4     64    6     1    1
## 5     76    8     1    1
## 6     93   10     1    1
```

```r
plot( ChickWeight$Time, ChickWeight$weight, col=ChickWeight$Diet)
```

![](Ch_3_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
First, notice that the rows here represent time points rather than individuals. To facilitate the comparison of weights at different time points and across the different chicks, we will reshape the data so that each row is a chick. In R we can do this with the reshape function:

```r
chick = reshape(ChickWeight, idvar=c("Chick","Diet"), timevar="Time",
                direction="wide")
```
The meaning of this line is: reshape the data from long to wide, where the columns Chick and Diet are the ID’s and the column Time indicates different observations for each ID. Now examine the head of this dataset:

```r
head(chick)
```

```
##    Chick Diet weight.0 weight.2 weight.4 weight.6 weight.8 weight.10
## 1      1    1       42       51       59       64       76        93
## 13     2    1       40       49       58       72       84       103
## 25     3    1       43       39       55       67       84        99
## 37     4    1       42       49       56       67       74        87
## 49     5    1       41       42       48       60       79       106
## 61     6    1       41       49       59       74       97       124
##    weight.12 weight.14 weight.16 weight.18 weight.20 weight.21
## 1        106       125       149       171       199       205
## 13       122       138       162       187       209       215
## 25       115       138       163       187       198       202
## 37       102       108       136       154       160       157
## 49       141       164       197       199       220       223
## 61       141       148       155       160       160       157
```
We also want to remove any chicks that have missing observations at any time points (NA for “not available”). The following line of code identifies these rows and then removes them:

```r
chick = na.omit(chick)
```


#### Problem 1

Focus on the chick weights on day 4 (check the column names of ‘chick’ and note the numbers). How much does the average of chick weights at day 4 increase if we add an outlier measurement of 3000 grams? Specifically, what is the average weight of the day 4 chicks, including the outlier chick, divided by the average of the weight of the day 4 chicks without the outlier. Hint: use cto add a number to a vector.

#### Problem 2

In exercise 1, we saw how sensitive the mean is to outliers. Now let’s see what happens when we use the median instead of the mean. Compute the same ratio, but now using median instead of mean. Specifically, what is the median weight of the day 4 chicks, including the outlier chick, divided by the median of the weight of the day 4 chicks without the outlier.

#### Problem 3

Now try the same thing with the sample standard deviation (the sd function in R). Add a chick with weight 3000 grams to the chick weights from day 4. How much does the standard deviation change? What’s the standard deviation with the outlier chick divided by the standard deviation without the outlier chick?

#### Problem 4

Compare the result above to the median absolute deviation in R, which is calculated with the mad function. Note that the mad is unaffected by the addition of a single outlier. The mad function in R includes the scaling factor 1.4826, such that mad and sd are very similar for a sample from a normal distribution. What’s the MAD with the outlier chick divided by the MAD without the outlier chick?

#### Problem 5

Our last question relates to how the Pearson correlation is affected by an outlier as compared to the Spearman correlation. The Pearson correlation between x and y is given in R by cor(x,y). The Spearman correlation is given by cor(x,y,method="spearman").

Plot the weights of chicks from day 4 and day 21. We can see that there is some general trend, with the lower weight chicks on day 4 having low weight again on day 21, and likewise for the high weight chicks.

Calculate the Pearson correlation of the weights of chicks from day 4 and day 21. Now calculate how much the Pearson correlation changes if we add a chick that weighs 3000 on day4 and 3000 on day 21. Again, divide the Pearson correlation with the outlier chick over the Pearson correlation computed without the outliers.

#### Problem 6

Save the weights of the chicks on day 4 from diet 1 as a vector x. Save the weights of the chicks on day 4 from diet 4 as a vector y. Perform a t-test comparing x and y (in R the function t.test(x,y) will perform the test). Then perform a Wilcoxon test of x and y (in R the function wilcox.test(x,y) will perform the test). A warning will appear that an exact p-value cannot be calculated with ties, so an approximation is used, which is fine for our purposes.

Perform a t-test of x and y, after adding a single chick of weight 200 grams to x (the diet 1 chicks). What is the p-value from this test? The p-value of a test is available with the following code: t.test(x,y)$p.value

#### Problem 7

Do the same for the Wilcoxon test. The Wilcoxon test is robust to the outlier. In addition, it has less assumptions that the t-test on the distribution of the underlying data.

#### Problem 8

We will now investigate a possible downside to the Wilcoxon-Mann-Whitney test statistic. Using the following code to make three boxplots, showing the true Diet 1 vs 4 weights, and then two altered versions: one with an additional difference of 10 grams and one with an additional difference of 100 grams. Use the x and y as defined above, NOT the ones with the added outlier.
```{}
library(rafalib)
mypar(1,3)
boxplot(x,y)
boxplot(x,y+10)
boxplot(x,y+100)
```

What is the difference in t-test statistic (obtained by t.test(x,y)$statistic) between adding 10 and adding 100 to all the values in the group ‘y’? Take the the t-test statistic with x and y+10 and subtract the t-test statistic with x and y+100. The value should be positive.

#### Problem 9

Examine the Wilcoxon test statistic for x and y+10 and for x and y+100. Because the Wilcoxon works on ranks, once the two groups show complete separation, that is all points from group ‘y’ are above all points from group ‘x’, the statistic will not change, regardless of how large the difference grows. Likewise, the p-value has a minimum value, regardless of how far apart the groups are. This means that the Wilcoxon test can be considered less powerful than the t-test in certain contexts. In fact, for small sample sizes, the p-value can’t be very small, even when the difference is very large. What is the p-value if we compare c(1,2,3) to c(4,5,6) using a Wilcoxon test?

#### Problem 10

What is the p-value if we compare c(1,2,3) to c(400,500,600) using a Wilcoxon test?
