---
title: "Chapter 1 Inference"
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
# This will give an approximation limited by number of samples
#result_norm <- rnorm(N, mean = 23.9, sd = 0.43)
#sum(23<=result_norm & result_norm<=25)/N*100
pnorm(25, mean = 23.9, sd = 0.43)-pnorm(23, mean = 23.9, sd = 0.43)
```

```
## [1] 0.9765648
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
x <- filter(dat, Diet=="chow" & Sex=="M") %>% select(Bodyweight) %>% unlist
pop_mean_control <- mean(x)
pop_mean_control
```

```
## [1] 30.96381
```

#### Problem 2
Now use the rafalib package and use the popsd function to compute the population standard deviation.

```r
library(rafalib)
popsd(x)
```

```
## [1] 4.420501
```


#### Problem 3
Set the seed at 1. Take a random sample X of size 25 from x. What is the sample average?

```r
set.seed(1)
sample_mean_control <- mean(sample(x,size=25))
sample_mean_control
```

```
## [1] 32.0956
```

#### Problem 4
Use dplyr to create a vector y with the body weight of all males on the high fat (hf) diet. What is this population’s average?

```r
y <- filter(dat, Sex=="M" & Diet=="hf")  %>% select(Bodyweight) %>% unlist
pop_mean_hf <- mean(y)
pop_mean_hf
```

```
## [1] 34.84793
```

#### Problem 5
Now use the rafalib package and use the popsd function to compute the population standard deviation.

```r
popsd(y)
```

```
## [1] 5.574609
```


#### Problem 6
Set the seed at 1. Take a random sample Y of size 25 from y. What is the sample average?

```r
set.seed(1)
sample_mean_hf <- mean(sample(y,size=25))
sample_mean_hf
```

```
## [1] 34.768
```


#### Problem 7
What is the difference in absolute value between $\bar{y}-\bar{x}$ and $\bar{X}-\bar{Y}$?

```r
# Difference in control and hf population means $\bar{y}-\bar{x}$
pop_diff <- pop_mean_hf - pop_mean_control
sample_diff <- sample_mean_control - sample_mean_hf
abs(pop_diff - sample_diff)
```

```
## [1] 6.556516
```


#### Problem 8
Repeat the above for females. Make sure to set the seed to 1 before each sample call. What is the difference in absolute value between $\bar{y}-\bar{x}$ and $\bar{X}-\bar{Y}$?

```r
# Filter Bodyweight for control females
x <- filter(dat, Diet=="chow" & Sex=="F") %>% select(Bodyweight) %>% unlist
pop_mean_control <- mean(x)
popsd(x)
```

```
## [1] 3.416438
```

```r
set.seed(1)
sample_mean_control <- mean(sample(x,size=25))

# Filter Bodyweight for hf females
y <- filter(dat, Sex=="F" & Diet=="hf")  %>% select(Bodyweight) %>% unlist
pop_mean_hf <- mean(y)
popsd(y)
```

```
## [1] 5.06987
```

```r
set.seed(1)
sample_mean_hf <- mean(sample(y,size=25))

# Difference in control and hf population means $\bar{y}-\bar{x}$
pop_diff <- pop_mean_hf - pop_mean_control
sample_diff <- sample_mean_control - sample_mean_hf
abs(pop_diff - sample_diff)
```

```
## [1] 5.487517
```

#### Problem 9
For the females, our sample estimates were closer to the population difference than with males. What is a possible explanation for this?

A) The population variance of the females is smaller than that of the males; thus, the sample variable has less variability.
B) Statistical estimates are more precise for females.
C) The sample size was larger for females.
D) The sample size was smaller for females.

```
## [1] "The population variance for females is smaller both for control group (3.42 vs 4.42) and hf group (5.07 vs 5.57). So the samples have less variablility. A."
```


### CLT and t-distribution exercises

#### Basic Intallation and setup

```r
library(downloader) 
url <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/mice_pheno.csv"
filename <- basename(url)
download(url, destfile=filename)
dat <- na.omit( read.csv(filename) )
```

#### Problem 1
If a list of numbers has a distribution that is well approximated by the normal distribution, what proportion of these numbers are within one standard deviation away from the list’s average?

```r
pnorm(1,0,1)-pnorm(-1,0,1)
```

```
## [1] 0.6826895
```

#### Problem 2
What proportion of these numbers are within two standard deviations away from the list’s average?

```r
pnorm(2,0,1)-pnorm(-2,0,1)
```

```
## [1] 0.9544997
```

#### Problem 3
What proportion of these numbers are within three standard deviations away from the list’s average?

```r
pnorm(3,0,1)-pnorm(-3,0,1)
```

```
## [1] 0.9973002
```
#### Problem 4
Define y to be the weights of males on the control diet. What proportion of the mice are within one standard deviation away from the average weight (remember to use popsd for the population sd)?

```r
y <- filter(dat, Diet=="chow" & Sex=="M") %>% select(Bodyweight) %>% unlist
pop_mean_control <- mean(y)
pop_sd_control <- rafalib::popsd(y)
mean(pop_mean_control-pop_sd_control<=y & y<=pop_mean_control+pop_sd_control)*100
```

```
## [1] 69.50673
```

#### Problem 5
What proportion of these numbers are within two standard deviations away from the list’s average?

```r
mean(pop_mean_control-2*pop_sd_control<=y & y<=pop_mean_control+2*pop_sd_control)*100
```

```
## [1] 94.61883
```

#### Problem 6
What proportion of these numbers are within three standard deviations away from the list’s average?

```r
mean(pop_mean_control-3*pop_sd_control<=y & y<=pop_mean_control+3*pop_sd_control)*100
```

```
## [1] 99.10314
```

#### Problem 7
Note that the numbers for the normal distribution and our weights are relatively close. Also, notice that we are indirectly comparing quantiles of the normal distribution to quantiles of the mouse weight distribution. We can actually compare all quantiles using a qqplot. Which of the following best describes the qq-plot comparing mouse weights to the normal distribution?

A) The points on the qq-plot fall exactly on the identity line.
B) The average of the mouse weights is not 0 and thus it can’t follow a normal distribution.
C) The mouse weights are well approximated by the normal distribution, although the larger values (right tail) are larger than predicted by the normal. This is consistent with the differences seen between question 3 and 6.
D) These are not random variables and thus they can’t follow a normal distribution.


```
## [1] "C. The mouse weights are well approximated by the normal distribution, although the larger values (right tail) are larger than predicted by the normal. This is consistent with the differences seen between question 3 and 6."
```

#### Problem 8
Create the above qq-plot for the four populations: male/females on each of the two diets. What is the most likely explanation for the mouse weights being well approximated? What is the best explanation for all these being well approximated by the normal distribution?

A) The CLT tells us that sample averages are approximately normal.
B) This just happens to be how nature behaves. Perhaps the result of many biological factors averaging out.
C) Everything measured in nature follows a normal distribution.
D) Measurement error is normally distributed.

```r
y_chowM <- filter(dat, Diet=="chow" & Sex=="M") %>% select(Bodyweight) %>% unlist
y_chowF <- filter(dat, Diet=="chow" & Sex=="F") %>% select(Bodyweight) %>% unlist
y_hfM <- filter(dat, Diet=="hf" & Sex=="M") %>% select(Bodyweight) %>% unlist
y_hfF <- filter(dat, Diet=="hf" & Sex=="F") %>% select(Bodyweight) %>% unlist
par(mfrow=c(1,2),oma=c(0,0,2,0))
qqnorm(y_chowM, main="Males")
qqline(y_chowM)
qqnorm(y_chowF, main="Females")
qqline(y_chowF)
title("Normal Q-Q Plots for Diet:Chow",outer=TRUE)
```

![](Ch1_files/figure-html/unnamed-chunk-35-1.png)<!-- -->

```r
qqnorm(y_hfM, main="Males")
qqline(y_hfM)
qqnorm(y_hfF, main="Females")
qqline(y_hfF)
title("Normal Q-Q Plots for Diet:hf",outer=TRUE)
```

![](Ch1_files/figure-html/unnamed-chunk-35-2.png)<!-- -->

```r
# reset graph settings to normal
par(mfrow=c(1,1))
```


```
## [1] "B. This just happens to be how nature behaves. Perhaps the result of many biological factors averaging out."
```
#### Problem 9
Here we are going to use the function replicate to learn about the distribution of random variables. All the above exercises relate to the normal distribution as an approximation of the distribution of a fixed list of numbers or a population. We have not yet discussed probability in these exercises. If the distribution of a list of numbers is approximately normal, then if we pick a number at random from this distribution, it will follow a normal distribution. However, it is important to remember that stating that some quantity has a distribution does not necessarily imply this quantity is random. Also, keep in mind that this is not related to the central limit theorem. The central limit applies to averages of random variables. Let’s explore this concept.

We will now take a sample of size 25 from the population of males on the chow diet. The average of this sample is our random variable. We will use the replicate to observe 10,000 realizations of this random variable. Set the seed at 1, generate these 10,000 averages. Make a histogram and qq-plot of these 10,000 numbers against the normal distribution.

We can see that, as predicted by the CLT, the distribution of the random variable is very well approximated by the normal distribution.

```r
#y <- filter(dat, Sex=="M" & Diet=="chow") %>% select(Bodyweight) %>% unlist
avgs <- replicate( 10000, mean(sample(y, 25)) )
mypar(1,2)
hist(avgs)
qqnorm(avgs)
qqline(avgs)
```

![](Ch1_files/figure-html/unnamed-chunk-37-1.png)<!-- -->

What is the average of the distribution of the sample average?

```r
mean(avgs)
```

```
## [1] 30.9556
```

#### Problem 10
What is the standard deviation of the distribution of sample averages?

```r
sd(avgs)
```

```
## [1] 0.8368371
```


#### Problem 11
According to the CLT, the answer to exercise 9 should be the same as mean(y). You should be able to confirm that these two numbers are very close. Which of the following does the CLT tell us should be close to your answer to exercise 10?

A) popsd(y)
B) popsd(avgs)/sqrt(25)
C) sqrt(25) / popsd(y)
D) popsd(y)/sqrt(25)

```r
mean(y)
```

```
## [1] 30.96381
```

```r
library(rafalib)
popsd(y)
```

```
## [1] 4.420501
```

```r
popsd(avgs)/sqrt(25)
```

```
## [1] 0.167359
```

```r
sqrt(25) / popsd(y)
```

```
## [1] 1.131094
```

```r
popsd(y)/sqrt(25)
```

```
## [1] 0.8841001
```


CLT: if we take many samples of size N, then the quantity: $\dfrac{\bar{Y}−\mu}{\sigma_{Y}/N}$ is approximated with a normal distribution centered at 0 and with standard deviation 1. OR $\bar{Y}$ is normally distributed with mean $\mu$ and sd ${\sigma_{Y}/N}$ where $\sigma_{Y}$ is population sd. So, `D. popsd(y)/sqrt(25)`


#### Problem 12
In practice we do not know σ (popsd(y)) which is why we can’t use the CLT directly. This is because we see a sample and not the entire distribution. We also can’t use popsd(avgs) because to construct averages, we have to take 10,000 samples and this is never practical. We usually just get one sample. Instead we have to estimate popsd(y). As described, what we use is the sample standard deviation. Set the seed at 1, using the replicate function, create 10,000 samples of 25 and now, instead of the sample average, keep the standard deviation. Look at the distribution of the sample standard deviations. It is a random variable. The real population SD is about 4.5. What proportion of the sample SDs are below 3.5?

```r
set.seed(1)
sds <- replicate( 10000, sd(sample(y, 25)) )
mypar(1,2)
hist(sds)
qqnorm(sds)
qqline(sds)
```

![](Ch1_files/figure-html/unnamed-chunk-42-1.png)<!-- -->

```r
mean(sds<3.5)*100
```

```
## [1] 9.64
```

```r
mypar(1,1)
```


#### Problem 13
What the answer to question 12 reveals is that the denominator of the t-test is a random variable. By decreasing the sample size, you can see how this variability can increase. It therefore adds variability. The smaller the sample size, the more variability is added. The normal distribution stops providing a useful approximation. When the distribution of the population values is approximately normal, as it is for the weights, the t-distribution provides a better approximation. We will see this later on. Here we will look at the difference between the t-distribution and normal. Use the function qt and qnorm to get the quantiles of x=seq(0.0001,0.9999,len=300). Do this for degrees of freedom 3, 10, 30, and 100. Which of the following is true?

A) The t-distribution and normal distribution are always the same.
B) The t-distribution has a higher average than the normal distribution.
C) The t-distribution has larger tails up until 30 degrees of freedom, at which point it is practically the same as the normal distribution.
D) The variance of the t-distribution grows as the degrees of freedom grow.

```r
x <- seq(0.0001,0.9999,len=300)
t3 <- qt(x, df = 3) 
t10 <- qt(x, df = 10) 
t30 <- qt(x, df = 30) 
t100 <- qt(x, df = 100)
norm_dis <- qnorm(x)
print("t-distribution is normal with fatter tails so means should remain same.")
```

```
## [1] "t-distribution is normal with fatter tails so means should remain same."
```

```r
round(mean(t3),4)
```

```
## [1] 0
```

```r
round(mean(t10),4)
```

```
## [1] 0
```

```r
round(mean(t30),4)
```

```
## [1] 0
```

```r
round(mean(t100),4)
```

```
## [1] 0
```

```r
round(mean(norm_dis),4)
```

```
## [1] 0
```

```r
par(mfrow=c(2,2),oma=c(0,0,2,0))
plot(norm_dis,t3, main="df=3",xlab="Normal quantiles",ylab="t-dis quantiles")
qqline(t3)
plot(norm_dis,t10, main="df=10",xlab="Normal quantiles",ylab="t-dis quantiles")
qqline(t10)
plot(norm_dis,t30, main="df=30",xlab="Normal quantiles",ylab="t-dis quantiles")
qqline(t30)
plot(norm_dis,t100, main="df=100",xlab="Normal quantiles",ylab="t-dis quantiles")
qqline(t100)
title("QQ plots: Normal vs t distribution",outer=TRUE)
```

![](Ch1_files/figure-html/unnamed-chunk-43-1.png)<!-- -->

```r
# reset graph settings to normal
par(mfrow=c(1,1))
```


```
## [1] "C. The t-distribution has larger tails up until 30 degrees of freedom, at which point it is practically the same as the normal distribution."
```


### CLT in practice exercises

### Power calculations exercises

### Monte carlo exercises

### Permutation tests exercises

#### Basic Intallation and setup

```r
url <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/babies.txt"
filename <- basename(url)
download(url, destfile=filename)
babies <- read.table("babies.txt", header=TRUE)
library(dplyr)
bwt.nonsmoke <- filter(babies, smoke==0) %>% select(bwt) %>% unlist 
bwt.smoke <- filter(babies, smoke==1) %>% select(bwt) %>% unlist
```

#### Problem 1
We will generate the following random variable based on a sample size of 10 and observe the following difference:


```r
N=10
set.seed(1)
nonsmokers <- sample(bwt.nonsmoke , N)
smokers <- sample(bwt.smoke , N)
obs <- mean(smokers) - mean(nonsmokers)
```
The question is whether this observed difference is statistically significant. We do not want to rely on the assumptions needed for the normal or t-distribution approximations to hold, so instead we will use permutations. We will reshuffle the data and recompute the mean. We can create one permuted sample with the following code:

```r
dat <- c(smokers,nonsmokers)
shuffle <- sample( dat )
smokersstar <- shuffle[1:N]
nonsmokersstar <- shuffle[(N+1):(2*N)]
mean(smokersstar)-mean(nonsmokersstar)
```

```
## [1] -8.5
```

The last value is one observation from the null distribution we will construct. Set the seed at 1, and then repeat the permutation 1,000 times to create a null distribution. What is the permutation derived p-value for our observation?

#### Problem 2
Repeat the above exercise, but instead of the differences in mean, consider the differences in `median obs <- median(smokers) - median(nonsmokers)`. What is the permutation based p-value?

### Association tests exercises

#### Basic Intallation and setup

```r
library(downloader) 
url <- "https://studio.edx.org/c4x/HarvardX/PH525.1x/asset/assoctest.csv"
filename <- basename(url)
download(url, destfile=filename)
dat <- read.csv(filename)
```
#### Problem 1
This dataframe reflects the allele status (either AA/Aa or aa) and the case/control status for 72 individuals. Compute the Chi-square test for the association of genotype with case/control status (using the table function and the chisq.test function). Examine the table to see if there appears to be an association. What is the X-squared statistic?


#### Problem 2
Compute Fisher’s exact test fisher.test for the same table. What is the p-value?


