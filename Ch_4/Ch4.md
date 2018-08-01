---
title: "Chapter 4 Matrix Algebra"
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

### Introduction to using regression exercises

#### Basic setup and installation

Here we include some refresher questions. If you haven’t done so already, install the library UsingR. Once you load it, you have access to Galton’s father and son heights:

```r
# install.packages("UsingR")
data("father.son",package="UsingR")
```

#### Problem 1
What is the average height of the sons (don’t round off)?

#### Problem 2
One of the defining features of regression is that we stratify one variable based on others. In Statistics, we use the verb “condition”. For example, the linear model for son and father heights answers the question: how tall do I expect a son to be if I condition on his father being x inches? The regression line answers this question for any x.

Using the father.son dataset described above, we want to know the expected height of sons, if we condition on the father being 71 inches. Create a list of son heights for sons that have fathers with heights of 71 inches, rounding to the nearest inch.

What is the mean of the son heights for fathers that have a height of 71 inches (don’t round off your answer)? Hint: use the function round on the fathers’ heights.

#### Problem 3
We say a statistical model is a linear model when we can write it as a linear combination of parameters and known covariates, plus random error terms. In the choices below, Y represents our observations, time t is our only covariate, unknown parameters are represented with letters a,b,c,d and measurement error is represented by ε. If t is known, then any transformation of t is also known. So, for example, both $Y=a+bt+ε$ and $Y=a+bf(t)+ε$ are linear models. Which of the following cannot be written as a linear model?

A) $Y=a+bt+ε$
B) $Y=a+bcos(t)+ε$
C) $Y=a+bt+ε$
D) $Y=a+bt+ct2+dt3+ε$

#### Problem 4
Suppose you model the relationship between weight and height across individuals with a linear model. You assume that the height of individuals for a fixed weight x follows a linear model $Y=a+bx+ε$. Which of the following do you feel best describes what e represents?

A) Measurement error: scales are not perfect.
B) Within individual random fluctuations: you don’t weigh the same in the morning as in the afternoon.
C) Round off error introduced by the computer we use to analyze the data.
D) Between individual variability: people of the same height vary in their weight.



### Matrix notation exercises

#### Problem 1
In R we have vectors and matrices. You can create your own vectors with the function c.

```r
 c(1,5,3,4)
```

```
## [1] 1 5 3 4
```
They are also the output of many functions such as:

```r
rnorm(10)
```

```
##  [1]  1.0750610 -0.9333051 -0.8994620  1.7992412  1.1340068  0.5025653
##  [7] -0.6708234  2.3959160 -1.1096775  0.5045432
```
You can turn vectors into matrices using functions such as rbind, cbind or matrix.

Create the matrix from the vector 1:1000 like this:

```r
X = matrix(1:1000,100,10)
```
What is the entry in row 25, column 3?
#### Problem 2
Using the function cbind, create a 10 x 5 matrix with first column `x=1:10`. Then add `2*x`, `3*x`, `4*x` and `5*x` to columns 2 through 5. What is the sum of the elements of the 7th row?

#### Problem 3
Which of the following creates a matrix with multiples of 3 in the third column?

A) `matrix(1:60,20,3)`
B) `matrix(1:60,20,3,byrow=TRUE)`
C) `x=11:20; rbind(x,2*x,3*x)`
D) `x=1:40; matrix(3*x,20,2)`



### Matrix operations exercises

#### Problem 1
Suppose `X` is a matrix in R. Which of the following is not equivalent to `X`?
A) `t( t(X) )`
B) `X %*% matrix(1,ncol(X) )`
C) `X*1`
D) `X%*%diag(ncol(X))`

#### Problem 2
Solve the following system of equations using R:
$$3a+4b−5c+d=10$$
$$2a+2b+2c−d=5$$
$$a−b+5c−5d=7$$
$$5a+d=4$$
What is the solution for $c$?

#### Problem 3
Load the following two matrices into R:

```r
 a <- matrix(1:12, nrow=4)
 b <- matrix(1:15, nrow=3)
```

Note the dimension of `a` and the dimension of `b`.

In the question below, we will use the matrix multiplication operator in R, `%*%`, to multiply these two matrices.

What is the value in the 3rd row and the 2nd column of the matrix product of `a` and `b`?

#### Problem 4
Multiply the 3rd row of `a` with the 2nd column of `b`, using the element-wise vector multiplication with `*`.

What is the sum of the elements in the resulting vector?



### Matrix algebra examples exercises

Suppose we are analyzing a set of 4 samples. The first two samples are from a treatment group A and the second two samples are from a treatment group B. This design can be represented with a model matrix like so:

```r
X <- matrix(c(1,1,1,1,0,0,1,1),nrow=4)
rownames(X) <- c("a","a","b","b")
X
```

```
##   [,1] [,2]
## a    1    0
## a    1    0
## b    1    1
## b    1    1
```
Suppose that the fitted parameters for a linear model give us:

```r
beta <- c(5, 2)
```

Use the matrix multiplication operator, `%*%`, in R to answer the following questions:

What is the fitted value for the A samples? (The fitted Y values.)

#### Problem 2
What is the fitted value for the B samples? (The fitted Y values.)

#### Problem 3
Suppose now we are comparing two treatments B and C to a control group A, each with two samples. This design can be represented with a model matrix like so:

```r
X <- matrix(c(1,1,1,1,1,1,0,0,1,1,0,0,0,0,0,0,1,1),nrow=6)
rownames(X) <- c("a","a","b","b","c","c")
X
```

```
##   [,1] [,2] [,3]
## a    1    0    0
## a    1    0    0
## b    1    1    0
## b    1    1    0
## c    1    0    1
## c    1    0    1
```
Suppose that the fitted values for the linear model are given by:

```r
beta <- c(10,3,-3)
```

What is the fitted value for the B samples?

#### Problem 4
What is the fitted value for the C samples?


