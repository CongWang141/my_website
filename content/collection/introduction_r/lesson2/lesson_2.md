---
title: "Lesson 2"
author: "Cong Wang"
date: "2022-08-25"
draft: false
excerpt: 
weight: 1
---



## Introduction of plot in R
In high school, we all studied the plot of the function `\(y=x^2 - 5\)`, which is a upward fan shape. Using R we can plot this curve

```r
curve(x^2 - 5)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-1-1.png" width="672" />

We can include some options to make the plot better, also add horizontal and vertical lines to this plot 

```r
curve(x^2, xlim = c(-5,5), col = "red")
abline(h = 0)
abline(v = 0)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-2-1.png" width="672" />

## Plot a Chi-squared distribution
Except this simple function, R can plot all the statistic distributions, here shows an example of plotting a Chi-squared distribution

```r
curve(dchisq(x, df = 10), xlim = c(0, 40), ylab = "density", xlab = "Personal income") 
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-3-1.png" width="672" />

## Drawing a sample
Here I will show you how to draw a sample from a Chi-squared distribution

### example of a Chi-squared distribution
 ``set.seed(1)`` to make sure you will get the same result as I do.

```r
set.seed(1)
sample1 <- rchisq(n = 1000000, df = 12)
y1 <- sample1[1]
ybar <- mean(sample1)
```

If we repeat the process for a lot of times **y1** will have the same distribution with the population, the mean of **y1** is the mean of population (12), so **y1** is unbiased. The variance of **y1** is also the same with population, `\(2*12=24\)`. **ybar** is unbaised with mean 12. The variance of **ybar** will be 2*12/99, simply from the formula `$$S^2=\frac{\sum(x_i-\bar{x})^2}{n-1}$$`
Bacause the variance of sample mean is smaller than the population mean, which means that the sample mean is a more efficient estimate for the population mean.

### Properites of sample mean
To investigate the properties of sample mean, we can generate multiple samples from the same population

```r
pop <- rnorm(10000, 10, 1) #Population from a normal distribution, mean=10 and sd=1
```

Get a sample from the population and estimate the mean

```r
sample(x=pop, size=5) #Draw a sample from our population
```

```
## [1]  9.934007 11.116345  8.491058 10.907192  9.720582
```

```r
mean(sample(x=pop, size=5)) #Mean of the sample
```

```
## [1] 10.2328
```

Use function ``replicate``, we can replicate the process multiple times. The argument ``expr = `` specify what we need to replicate, ``n = `` specify how many times we want to repeat

```r
est1 <- replicate(expr = mean(sample(x = pop, size = 5)), n = 25000) #Get the mean 25000 times

est2 <- replicate(expr = mean(sample(x = pop, size = 25)), n = 25000) #Get the mean 25000 times, but with a larger sample

fo <- replicate(expr = sample(x = pop, size = 5)[1], n = 25000) #The first observation of the sample, y1
```

Plot the density estimate for the distribution in different cases

```r
# pot density estimate for the ditribution of y1, first element of each sample
plot(density(fo), 
     col = "green", 
     lwd = 2,
     ylim = c(0, 2),
     xlab = "estimates",
     main = "Sampling Distributions of Unbiased Estimators")
# add density estimate for the distribution of the sample mean with n=5 to the plot
lines(density(est1), 
      col = "steelblue", 
      lwd = 2, 
      bty = "l") #Lines, instead of plot, plots over the already existing graph
# add density estimate for the distribution of the sample mean with n=25 to the plot
lines(density(est2), 
      col = "red2", 
      lwd = 2)
# add a vertical line at the true parameter
abline(v = 10, lty = 2)
# add N(10,1) density to the plot
curve(dnorm(x, mean = 10), 
      lwd = 2,
      lty = 2,
      add = T)

# add a legend
legend("topleft",
       legend = c("N(10,1)",
                  expression(Y[1]),
                  expression(bar(Y) ~ n == 5),
                  expression(bar(Y) ~ n == 25)
       ), 
       lty = c(2, 1, 1, 1), 
       col = c("black","green", "steelblue", "red2"),
       lwd = 2)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-8-1.png" width="672" />

 As we can see from the plot, the sampling distribution of **y1** tracks the density of **N(10,1)**. As the number of observations increases, the sampling distribution gets closer to the true parameter. In another word, the probability of obtaining estimates that are close to the true value increases when the sample size increased. 

## The importance of random sampling
By using ``?sample`` we can see the documentation about this fucntion. You can add argument ``prob`` to specify the probability of an element being sampled

In this section, I will show the outcomes for simulating sample mean when the i.i.d. assumption fails. First we need to sort the population, and then replicate the sampling process without i.i.d. assumption

```r
sort(pop) #Population sorted
```

```r
est3 <-  replicate(n = 25000, 
                   expr = mean(sample(x = sort(pop), 
                                      size = 25, 
                                      prob = c(rep(4, 2500), rep(1, 7500)))))
mean(est3)
```

```
## [1] 9.447732
```

Comparing the sampling distribution of sample mean when i.i.d. holds and fails 

```r
plot(density(est2), 
     col = "steelblue",
     lwd = 2,
     xlim = c(8, 11),
     xlab = "Estimates",
     main = "When the i.i.d. Assumption Fails")

# sampling distribution of sample mean, i.i.d. fails, n=25
lines(density(est3),
      col = "red2",
      lwd = 2)
 
# add a legend
legend("topleft",
       legend = c(expression(bar(Y)[n == 25]~", i.i.d. fails"),
                  expression(bar(Y)[n == 25]~", i.i.d. holds")
       ), 
       lty = c(1, 1), 
       col = c("red2", "steelblue"),
       lwd = 2)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-11-1.png" width="672" />

As we can see from the plot **ybar** is biased estimator if i.i.d. doesn't not hold

## Hypothesis testing

Here we will see how to visualize p-value which is a very important statistic term

```r
# plot the standard normal density on the interval [-4,4]
curve(dnorm(x),
      xlim = c(-4, 4),
      main = "Calculating a p-Value",
      yaxs = "i",
      xlab = "z",
      ylab = "",
      lwd = 2,
      axes = "F")

# add x-axis
axis(1, 
     at = c(-1.5, 0, 1.5), 
     padj = 0.75,
     labels = c(expression(-frac(bar(Y)^"act"~-~bar(mu)[Y,0], sigma[bar(Y)])),
                0,
                expression(frac(bar(Y)^"act"~-~bar(mu)[Y,0], sigma[bar(Y)]))))

# shade p-value/2 region in left tail
polygon(x = c(-6, seq(-6, -1.5, 0.01), -1.5),
        y = c(0, dnorm(seq(-6, -1.5, 0.01)),0), 
        col = "steelblue")

# shade p-value/2 region in right tail
polygon(x = c(1.5, seq(1.5, 6, 0.01), 6),
        y = c(0, dnorm(seq(1.5, 6, 0.01)), 0), 
        col = "steelblue")
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-12-1.png" width="672" />

## Sample variance, standard deviation(sd) and standard error(se)
Usually the variance of population in unknown, so we need to estimate it, and also the s.d.


```r
# vector of sample sizes
n <- c(10000, 5000, 2000, 1000, 500)
# sample observations, estimate using 'sd()' and plot the estimated distributions
sd(n)
```

```
## [1] 3930.649
```

In this case, we draw a sample of 10000 obeservations from a normal distribution with mean equals to 10, standard deviation equals to 3, then we calulate the standard deviation of the sample, we repeat this process for **n = 10000** times, finally we plot the density of the sequence of the standard deviation we got

```r
sq_y <- replicate(n = 10000, expr = sd(rnorm(n[1], mean = 10, sd = 3)))
plot(density(sq_y),
     main = expression("Sampling Distributions of" ~ s[Y]),
     xlab = expression(s[y]),
     lwd = 2)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-14-1.png" width="672" />

Next, we create a loop to draw samples of 5000, 2000, 1000, 500 obeservations from a normal distribution with mean equals to 10, standard deviation equals to 3, just like what we have done before. For each sample we calulate the standard deviations, and we repeat each process for  **n = 10000** times. Finally, we plot the density of each sequence of the standard deviation we got for comparison.

```r
plot(density(sq_y),
     main = expression("Sampling Distributions of" ~ s[Y]),
     xlab = expression(s[y]),
     lwd = 2)

for (i in 2:5) {
  sq_y <- replicate(n = 10000, expr = sd(rnorm(n[i], 10, 3)))
  lines(density(sq_y), 
        col = i, 
        lwd = 2)
  
  }
# add a legend
legend("topleft",
       legend = c(expression(n == 10000),
                  expression(n == 5000),
                  expression(n == 2000),
                  expression(n == 1000),
                  expression(n == 500)), 
       col = 1:5,
       lwd = 2)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-15-1.png" width="672" />

From the plot, we can see that when the sample size is bigger, the estimate is closer to the true value.

## P-value when standard deviation is unknown
First we draw a sample with replacement from a bernoulli distribution, with 90% probability of 0 and 10% probability of 1, sample size = 100. Then get the actual sample mean.

```r
random_sample <- sample(0:1, 
         prob = c(0.9, 0.1), 
         replace = T, 
         size = 100) #sampling from a bernoulli distribution
samplemean_act <- mean(random_sample) #Mean of the sample
```

We know for a bernoulli distribution, the variance of the sample mean is 
`$$var=pq$$`
So we can get the standard error of the sample mean from a bernoulli distribution `$$se=\frac{sd}{\sqrt[2]{n}}=\sqrt{\frac{pq}{n}}$$`

```r
SE_samplemean <- sqrt(samplemean_act * (1 - samplemean_act) / 100) #Standard error
```


## Null hypothesis testing

Compute t-statistic

```r
tstatistic <- (samplemean_act - 0) / SE_samplemean
tstatistic
```

```
## [1] 3.865557
```

Compute the p-value

```r
pvalue <- 2 * pnorm(- abs(tstatistic)) #pnorm gives the distribution function of Normal
pvalue
```

```
## [1] 0.0001108361
```

we can see, graphically, that the t-statistic is approximately N(0,1) if n is large.

```r
# prepare empty vector for t-statistics
tstatistics <- numeric(10000)
# set sample size
n <- 300
# simulate 10000 t-statistics
for (i in 1:10000) {
  
  s <- sample(0:1, 
              size = n,  
              prob = c(0.9, 0.1),
              replace = T)
  
  tstatistics[i] <- (mean(s)-0.1)/sqrt(var(s)/n)
  
}
# plot density and compare to N(0,1) density
plot(density(tstatistics),
     xlab = "t-statistic",
     main = "Estimated Distribution of the t-statistic when n=300",
     lwd = 2,
     xlim = c(-4, 4),
     col = "steelblue")

# N(0,1) density (dashed)
curve(dnorm(x), 
      add = T, 
      lty = 2, 
      lwd = 2)
```

<img src="/collection/introduction_r/lesson2/lesson_2_files/figure-html/unnamed-chunk-20-1.png" width="672" />

## Confidence interval

```r
# set seed
set.seed(1)

# generate some sample data
sampledata <- rnorm(100, 10, 10) #Sample from a N(10,10)

mean(sampledata)
```

```
## [1] 11.08887
```

```r
t.test(sampledata)
```

```
## 
## 	One Sample t-test
## 
## data:  sampledata
## t = 12.346, df = 99, p-value < 2.2e-16
## alternative hypothesis: true mean is not equal to 0
## 95 percent confidence interval:
##   9.306651 12.871096
## sample estimates:
## mean of x 
##  11.08887
```

```r
ls(t.test(sampledata)) #Outcomes of t.test
```

```
##  [1] "alternative" "conf.int"    "data.name"   "estimate"    "method"     
##  [6] "null.value"  "p.value"     "parameter"   "statistic"   "stderr"
```

```r
t.test(sampledata)$"conf.int" #Just the desired result. 95% confidence interval
```

```
## [1]  9.306651 12.871096
## attr(,"conf.level")
## [1] 0.95
```

```r
t.test(sampledata, conf.level = 0.99) #Covers the true value with a probability of 99%
```

```
## 
## 	One Sample t-test
## 
## data:  sampledata
## t = 12.346, df = 99, p-value < 2.2e-16
## alternative hypothesis: true mean is not equal to 0
## 99 percent confidence interval:
##   8.729838 13.447909
## sample estimates:
## mean of x 
##  11.08887
```

## Means comparison
To compare means from two different populations, we can also perform a t-test

```r
# draw data from two different populations with equal mean
sample_pop1 <- rnorm(100, 10, 10) #N(10,10)
sample_pop2 <- rnorm(100, 10, 20) #N(10,20)

mean(sample_pop1)
```

```
## [1] 9.621919
```

```r
mean(sample_pop2)
```

```
## [1] 10.59347
```

```r
# perform a two sample t-test
t.test(sample_pop1, sample_pop2) #Null hypothesis: the difference is zero
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  sample_pop1 and sample_pop2
## t = -0.4262, df = 139.59, p-value = 0.6706
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -5.478461  3.535358
## sample estimates:
## mean of x mean of y 
##  9.621919 10.593471
```

## Exercise 
1. Draw a sample of size=100 from a Standard Normal Distribution
2. Perform a t-test "by hands" (without the R function)
3. Perform the t-test using the function from R and compare the values
4. Interpret the result of the t-test
5. Repeat step **1.** 1000 times, storing the mean on a vector and plot the histogram
