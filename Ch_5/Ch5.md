---
title: "Chapter 5 Linear Models"
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

### Linear models and randomness

### Expressing Design Formula Exercises
Suppose we have an experiment with the following design: on three different days, we perform an experiment with two treated and two control units. We then measure some outcome $Y_i$, and we want to test the effect of treatment as well the effects of different days (perhaps the temperature in the lab affects the measuring device). Assume that the true condition effect is the same for each day (no interaction between condition and day). We then define factors in R for day and for condition.

condition/day	A	B	C

treatment   	2	2	2

control	      2	2	2

#### Problem 1
Given the factors we have defined above and without defining any new ones, which of the following R formula will produce a design matrix (model matrix) that lets us analyze the effect of condition, controlling for the different days?

A) `~ day + condition`
B) `~ condition ~ day`
C) `~ A + B + C + control + treated`
D) `~ B + C + treated`

Remember that using the `~` and the names for the two variables we want in the model will produce a design matrix controlling for all levels of day and all levels of condition. We do not use the levels in the design formula.

### Linear Models in Practice Exercises

The function `lm` can be used to fit a simple, two group linear model. The test statistic from a linear model is equivalent to the test statistic we get when we perform a t-test with the equal variance assumption. Though the linear model in this case is equivalent to a t-test, we will soon explore more complicated designs, where the linear model is a useful extension (confounding variables, testing contrasts of terms, testing interactions, testing many terms at once, etc.).

Here we will review the mathematics on why these produce the same test statistic and therefore p-value.

We already know that the numerator of the t-statistic in both cases is the difference between the average of the groups, so we only have to see that the denominator is the same. Of course, it makes sense that the denominator should be the same, since we are calculating the standard error of the same quantity (the difference) under the same assumptions (equal variance), but here we will show equivalence of the formula.

In the linear model, we saw how to calculate this standard error using the design matrix $X$ and the estimate of $\sigma^2$ from the residuals. The estimate of $\sigma^2$ was the sum of squared residuals divided by $N - p$, where $N$ is the total number of samples and $p$ is the number of terms (an intercept and a group indicator, so here p=2).

In the t-test, the denominator of the t-value is the standard error of the difference. The t-test formula for the standard error of the difference, if we assume equal variance in the two groups, is the square root of the variance:

$$\frac{1}{1/N_x + 1/N_y}  
\frac{  \sum_{i=1}^{N_x} (X_i - \mu_x)^2  + \sum_{i=1} (Y_i - \mu_y)^2  }{N_x + N_y - 2}$$
Here $N_x$ is the number of samples in the first group and $N_y$ is the number of samples in the second group.

If we look carefully, the second part of this equation is the sum of squared residuals, divided by $N - 2$.

All that is left to show is that the entry in the second row, second column of $(X^\top X)^{-1}$ is $(1/N_x + 1/N_y)$

#### Problem 1
You can make a design matrix $X$ for a two group comparison, either using `model.matrix` or simply with:
```{}
X <- cbind(rep(1,Nx + Ny),rep(c(0,1),c(Nx, Ny)))
```
In order to compare two groups, where the first group has `Nx=5` samples and the second group has `Ny=7` samples, what is the element in the 1st row and 1st column of $X^\top X$?

#### Problem 2
The other entries of $X^\top X$ are all the same. What is this number?

Now we just need to invert the matrix to obtain $(X^\top X)^{-1}$. The formula for matrix inversion for a 2x2 matrix is actually relatively easy to memorize:

$$
\begin{pmatrix}
a&b\\
c&d
\end{pmatrix}^{-1} = \frac{1}{ad - bc}
\begin{pmatrix}
d&-b\\
-c&a
\end{pmatrix} $$
The element of the inverse in the 2nd row and the 2nd column is the element which will be used to calculate the standard error of the second coefficient of the linear model. This is $a / (ad - bc)$. And for our two group comparison, we saw that $a = N_x + N_y$ and the $b = c = d = N_y$. So it follows that this element is:

$$\frac{N_x + N_y}{(N_x + N_y) N_y - N_y N_y}$$
which simplifies to:

$$\frac{N_x + N_y}{N_x N_y} = 1/N_y + 1/N_x$$

### Standard Errors Exercises

### Interactions and Contrasts

### Collinearity exercises

#### Problem 1:
Consider these design matrices:

$$A=\begin{pmatrix}
1&0& 0& 0\\
1& 0& 0& 0\\
1& 1& 1& 0\\
1& 1& 0& 1
\end{pmatrix}\,\,
B=\begin{pmatrix}
1& 0& 0& 1\\
1& 0& 1& 1\\
1& 1& 0& 0\\
1& 1& 1& 0
\end{pmatrix}\,\,
C=\begin{pmatrix}
1& 0& 0\\
1& 1& 2\\
1& 2& 4\\
1& 3& 6
\end{pmatrix} $$

$$D=\begin{pmatrix}
1& 0& 0& 0& 0\\
1& 0& 0& 0& 1\\
1& 1& 0& 1& 0\\
1& 1& 0& 1& 1\\
1& 0& 1& 1& 0\\
1& 0& 1& 1& 1
\end{pmatrix}\,\,
E=\begin{pmatrix}
1& 0& 0& 0\\
1& 0& 1& 0\\
1& 1& 0& 0\\
1& 1& 1& 1
\end{pmatrix} \,\,
F=\begin{pmatrix}
1& 0& 0& 1\\
1& 0& 0& 1\\
1& 0& 1& 1\\
1& 1& 0& 0\\
1& 1& 0& 0\\
1& 1& 1& 0
\end{pmatrix}$$

Which of the above design matrices does NOT have the problem of collinearity?

#### Problem 2:
The following exercises are advanced. Let’s use the example from the lecture to visualize how there is not a single best $\hat{\beta}$, when the design matrix has collinearity of columns. An example can be made with:

```r
sex <- factor(rep(c("female","male"),each=4))
trt <- factor(c("A","A","B","B","C","C","D","D"))
```
The model matrix can then be formed with:

```r
X <- model.matrix( ~ sex + trt)
```
And we can see that the number of independent columns is less than the number of columns of X:

```r
qr(X)$rank
```

```
## [1] 4
```
Suppose we observe some outcome Y. For simplicity, we will use synthetic data:

```r
Y <- 1:8
```
Now we will fix the value for two coefficients and optimize the remaining ones. We will fix $\beta_{male}$ and $\beta_D$. Then we will find the optimal value for the remaining betas, in terms of minimizing the residual sum of squares. We find the value that minimize:

$\sum_{i=1}^8  \{ (Y_i - X_{i,male} \beta_{male} - X_{i,D} \beta_{i,D}) - \mathbf{Z}_i \boldsymbol{\gamma} )^2 \}$
where $X_{male}$ is the male column of the design matrix, $X_D$ is the $D$ column, $\mathbf{Z}_i$ is a 1 by 3 matrix with the remaining column entries for unit $i$, and $\boldsymbol{\gamma}$ is a 3 x 1 matrix with the remaining parameters.

So all we need to do is redefine $Y$ as $Y^* = Y - X_{male} \beta_{male} - X_{D} \beta_D$ and fit a linear model. The following line of code creates this variable $Y^*$, after fixing $\beta_{male}$ to a value `a`, and $\beta_D$ to a value, `b`:

```r
makeYstar <- function(a,b) Y - X[,2] * a - X[,5] * b
```

Now we’ll construct a function which, for a given value a and b, gives us back the sum of squared residuals after fitting the other terms.

```r
 fitTheRest <- function(a,b) {
   Ystar <- makeYstar(a,b)
   Xrest <- X[,-c(2,5)]
   betarest <- solve(t(Xrest) %*% Xrest) %*% t(Xrest) %*% Ystar
   residuals <- Ystar - Xrest %*% betarest
   sum(residuals^2)
 }
```

What is the sum of squared residuals when the male coefficient is 1 and the D coefficient is 2, and the other coefficients are fit using the linear model solution?

#### Problem 3:
We can apply our function fitTheRest to a grid of values for $\beta_{male}$ and $\beta_D$, using the outer function in R. outer takes three arguments: a grid of values for the first argument, a grid of values for the second argument, and finally a function which takes two arguments.

Try it out:

```r
outer(1:3,1:3,`*`)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    2    4    6
## [3,]    3    6    9
```

We can run `fitTheRest` on a grid of values, using the following code (the `Vectorize` is necessary as `outer` requires only vectorized functions):

```r
outer(-2:8,-2:8,Vectorize(fitTheRest))
```

```
##       [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10] [,11]
##  [1,]  102   83   66   51   38   27   18   11    6     3     2
##  [2,]   83   66   51   38   27   18   11    6    3     2     3
##  [3,]   66   51   38   27   18   11    6    3    2     3     6
##  [4,]   51   38   27   18   11    6    3    2    3     6    11
##  [5,]   38   27   18   11    6    3    2    3    6    11    18
##  [6,]   27   18   11    6    3    2    3    6   11    18    27
##  [7,]   18   11    6    3    2    3    6   11   18    27    38
##  [8,]   11    6    3    2    3    6   11   18   27    38    51
##  [9,]    6    3    2    3    6   11   18   27   38    51    66
## [10,]    3    2    3    6   11   18   27   38   51    66    83
## [11,]    2    3    6   11   18   27   38   51   66    83   102
```

In the grid of values, what is the smallest sum of squared residuals?
