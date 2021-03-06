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

### Linear Models Introduction Exercises
The standard error of an estimate is the standard deviation of the sampling distribution of an estimate. In previous chapters, we saw that our estimate of the mean of a population changed depending on the sample that we took from the population. If we repeatedly sampled from the population and each time estimated the mean, the collection of mean estimates would form the sampling distribution of the estimate. When we took the standard deviation of those estimates, that was the standard error of our mean estimate.

In the case of a linear model written as:

$$ Y_i = \beta_0 + \beta_1 X_i + \varepsilon_i, i=1,\dots,n$$

$\varepsilon$ is considered random. Every time we re-run the experiment, we will see different $\varepsilon$ dichotomous. This implies that in different application $\varepsilon$ represents different things: measurement error or variability between individuals for example.

If we were to re-run the experiment many times and estimate linear model terms $\hat{\beta}$ each time, the distribution of these $\hat{\beta}$ is called the sampling distribution of the estimates. If we take the standard deviation of all of these estimates from repetitions of the experiment, this is called the standard error of the estimate. While we are not necesarily sampling individuals, you can think about the repetition of the experiment as "sampling" new errors in our observation of $Y$.

#### Problem 1
We have shown how to find the least squares estimates with matrix algebra. These estimates are random variables as they are linear combinations of the data. For these estimates to be useful, we also need to compute the standard errors. Here we review standard errors in the context of linear models. To see this, we can run a Monte Carlo simulation to imitate the collection of falling object data. Specifically, we will generate the data repeatedly and compute the estimate for the quadratic term each time.
```{}
g = 9.8 
h0 = 56.67
v0 = 0
n = 25
tt = seq(0,3.4,len=n) 
y = h0 + v0 *tt - 0.5* g*tt^2 + rnorm(n,sd=1)
```
Now we act as if we didn’t know `h0`, `v0` and `-0.5*g` and use regression to estimate these. We can rewrite the model as $y = \beta_0 + \beta_1 t + \beta_2 t^2 + \varepsilon$ and obtain the LSE we have used in this class. Note that $g = -2 \beta_2$.
To obtain the LSE in R we could write:

```{}
X = cbind(1,tt,tt^2)
A = solve(crossprod(X))%*%t(X)
```
Given how we have defined A, which of the following is the LSE of g, the acceleration due to gravity? Hint: try the code in R.

A) `9.8`
B) `A %*% y`
C) `-2 * (A %*% y) [3]`
D) `A[3,3]`

#### Problem 2
In the lines of code above, the function `rnorm` introduced randomness. This means that each time the lines of code above are repeated, the estimate of g will be different.

Use the code above in conjunction with the function `replicate` to generate 100,000 Monte Carlo simulated datasets. For each dataset, compute an estimate of g. (Remember to multiply by -2.)

What is the standard error of this estimate?

#### Problem 3
In the father and son height examples, we have randomness because we have a random sample of father and son pairs. For the sake of illustration, let’s assume that this is the entire population:

```{}
library(UsingR)
x = father.son$fheight
y = father.son$sheight
n = length(y)
```
Now let’s run a Monte Carlo simulation in which we take a sample of size 50 over and over again. Here is how we obtain one sample:
```{}
N = 50
index = sample(n,N)
sampledat = father.son[index,]
x = sampledat$fheight
y = sampledat$sheight
betahat = lm(y~x)$coef
```
Use the function replicate to take 10,000 samples.

What is the standard error of the slope estimate? That is, calculate the standard deviation of the estimate from the observed values obtained from many random samples.

#### Problem 4
Later in this chapter we will introduce a new concept: covariance. The covariance of two lists of numbers $X=x_1,...,x_n$ and $Y=y_1,...,y_n$ is:
```{}
n <- 100
Y <- rnorm(n)
X <- rnorm(n)
mean( (Y - mean(Y))*(X-mean(X) ) )
```
Which of the following is closest to the covariance between father heights and son heights?

A) 0
B) -4
C) 4
D) 0.5

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
In the previous assessment, we used a Monte Carlo technique to see that the linear model coefficients are random variables when the data is a random sample. Now we will use the previously seen matrix algebra to try to estimate the standard error of the linear model coefficients. Again, take a random sample of the `father.son` heights data:
```{}
library(UsingR)
N <- 50
set.seed(1)
index <- sample(n,N)
sampledat <- father.son[index,]
x <- sampledat$fheight
y <- sampledat$sheight
betahat <- lm(y~x)$coef
```
The formula for the standard error is:

$$\mbox{SE}(\hat{\beta}) = \sqrt{\mbox{var}(\hat{\beta})}$$
with:

$$\mbox{var}(\hat{\beta}) = \sigma^2 (X^\top X)^{-1}$$
We will estimate or calculate each part of this equation and then combine them.

First, we want to estimate $\sigma^2$, the variance of $Y$. As we have seen in the previous unit, the random part of Y is only coming from $\varepsilon$, because we assume $X\beta$ is fixed. So we can try to estimate the variance of the $\varepsilon$ s from the residuals, the $Y_i$ minus the fitted values from the linear model.

#### Problem 1
The fitted values $\hat{Y}$ from a linear model can be obtained with:
```{}
fit <- lm(y ~ x)
fit$fitted.values
```
What is the sum of the squared residuals, where residuals are given by $r_i = Y_i - \hat{Y}_i$ ?

#### Problem 2
Our estimate of $\sigma^2$ will be the sum of squared residuals divided by $N - p$, the sample size minus the number of terms in the model. Since we have a sample of 50 and 2 terms in the model (an intercept and a slope), our estimate of $\sigma^2$ will be the sum of squared residuals divided by 48. Use the answer from exercise 1 to provide an estimate of $\sigma^2$.

#### Problem 3
Form the design matrix $X$ (Note: use a capital `X`). This can be done by combining a column of 1’s with a column containg ‘`x`’ , the fathers’ heights.
```{}
N <- 50
X <- cbind(rep(1,N), x)
```
Now calculate $(X^\top X)^{-1}$. Use the solve function for the inverse and t for the transpose. What is the element in the first row, first column?

#### Problem 4
Now we are one step away from the standard error of $\hat{\beta}$. Take the diagonals from the $(X^\top X)^{-1}$ matrix above, using the diag function. Multiply our estimate of $\sigma^2$ and the diagonals of this matrix. This is the estimated variance of $\hat{\beta}$, so take the square root of this. You should end up with two numbers: the standard error for the intercept and the standard error for the slope.

What is the standard error for the slope?

Compare your answer to this last question, to the value you estimated using Monte Carlo in the previous set of exercises. It will not be the same because we are only estimating the standard error given a particular sample of 50 (which we obtained with `set.seed(1)`).

Notice that the standard error estimate is also printed in the second column of:

```{}
summary(fit)
```

### Interactions and Contrasts Exercises

#### Basic installation and setup
Suppose we have an experiment with two species A and B, and two conditions, control and treated.

```r
species <- factor(c("A","A","B","B"))
condition <- factor(c("control","treated","control","treated"))
```
We will use the formula of `~ species + condition` to create the model matrix:

```r
model.matrix(~ species + condition)
```

```
##   (Intercept) speciesB conditiontreated
## 1           1        0                0
## 2           1        0                1
## 3           1        1                0
## 4           1        1                1
## attr(,"assign")
## [1] 0 1 2
## attr(,"contrasts")
## attr(,"contrasts")$species
## [1] "contr.treatment"
## 
## attr(,"contrasts")$condition
## [1] "contr.treatment"
```

#### Problem 1
Suppose we want to build a contrast of coefficients for the above experimental design.

You can either figure this question out by looking at the design matrix, or by using the `contrast` function from the contrast library with random numbers for `y`. The contrast vector will be returned as `contrast(...)$X`.

What should the contrast vector be, to obtain the difference between the species B control group and the species A treatment group (species B control - species A treatment)? Assume that the coefficients (columns of design matrix) are: Intercept, speciesB, conditiontreated.

A) 0 0 1
B) 0 -1 0
C) 0 1 1
D) 0 1 -1
E) 0 -1 1
F) 1 0 1

#### Problem 2
Use the Rmd script to load the spider dataset. Suppose we build a model using two variables: `~ type + leg`.

What is the t-statistic for the contrast of leg pair L4 vs. leg pair L2?

#### Problem 3
The t-statistic for the contrast of leg pair L4 vs. leg pair L2 is constructed by taking the difference of the estimated coefficients legL4 and legL2, and then dividing by the standard error of the difference.

For a contrast vector $\mathbf{C}$, the standard error of the contrast $\mathbf{C}\hat{\boldsymbol{\beta}}$ is:

$$\sqrt{ \mathbf{C} \hat{\boldsymbol{\Sigma}} \mathbf{C}^\top }$$
with $\hat{\boldsymbol{\Sigma}}$ the estimated covariance matrix of the coefficient estimates $\hat{\boldsymbol{\beta}}$. The covariance matrix contains elements which give the variance or covariance of elements in $\hat{\beta}$. The elements on the diagonal of the $\boldsymbol{\hat{\Sigma}}$ matrix give the estimated variance of each element in $\hat{\boldsymbol{\beta}}$. The square root of these is the standard error of the elements in $\hat{\boldsymbol{\beta}}$. The off-diagonal elements of $\hat{\Sigma}$ give the estimated covariance of two different elements of $\hat{\boldsymbol{\beta}}$. So $\hat{\Sigma}_{1,2}$ gives the covariance of the first and second element of $\hat{\boldsymbol{\beta}}$. The $\hat{\boldsymbol{\beta}}$ matrix is symmetric, which means $\hat{\Sigma}_{i,j}=\hat{\Sigma}_{j,i}$.

For the difference, we have that:

$$\mbox{var}(\hat{\beta }_{L4} - \hat{\beta }_{L2}) = \mbox{var}(\hat{\beta }_{L4}) + \mbox{var}(\hat{\beta }_{L2}) - 2 \mbox{cov}(\hat{\beta }_{L4}, \hat{\beta }_{L2})$$
We showed how to estimate \hat{\Sigma} using:
```{}
X <- model.matrix(~ type + leg, data=spider)
Sigma.hat <- sum(fitTL$residuals^2)/(nrow(X) - ncol(X)) * solve(t(X) %*% X)
```
Using the estimate of $\Sigma$, what is your estimate of $\mbox{cov}(\hat{\beta}_{L4}, \hat{\beta}_{L2})$ ?
Our contrast matrix for the desired comparison is:

```r
 C <- matrix(c(0,0,-1,0,1),1,5)
```

#### Problem 4
Suppose that we notice that the within-group variances for the groups with smaller frictional coefficients are generally smaller, and so we try to apply a transformation to the frictional coefficients to make the within-group variances more constant.

Add a new variable `log2friction` to the spider dataframe:
```{}
spider$log2friction <- log2(spider$friction)
```
The Y values now look like:
```{}
boxplot(log2friction ~ type*leg, data=spider)
```
Run a linear model of `log2friction` with type, leg and interactions between `type` and `leg`.

What is the t-statistic for the interaction of type push and leg L4? If this t-statistic is sufficiently large, we would reject the null hypothesis that the push vs. pull effect on `log2(friction)` is the same in L4 as in L1.

#### Problem 5
Using the same analysis of log2 transformed data, What is the F-value for all of the `type:leg` interaction terms in an ANOVA? If this value is sufficiently large, we would reject the null hypothesis that the push vs. pull effect on `log2(friction)` is the same for all leg pairs.

#### Problem 6
What is the L2 vs. L1 estimate in `log2(friction)` for the pull samples?

#### Problem 7
What is the L2 vs. L1 estimate in `log2(friction)` for the push samples? Remember, because of the interaction terms, this is not the same as the L2 vs L1 difference for the pull samples. If you’re not sure use the `contrast` function. Another hint: consider the arrows plot for the model with interactions.

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
