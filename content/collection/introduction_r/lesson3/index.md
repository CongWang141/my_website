---
title: "Lesson 3"
author: "Cong Wang & Renan Serenini"
date: "2022-08-26"
output: html_document
---



### author
Cong Wang & Renan Serenini

## Solution to lesson 2
1. Draw a sample of size=100 from a Standard Normal Distribution

```r
sample1 <- rnorm(100)
```

2. Perform a t-test "by hands" (without the R function)

```r
t_by_hand <- mean(sample1)/sqrt(var(sample1)/100)
t_by_hand
```

```
## [1] -0.1899364
```

3. Perform the t-test using the function from R and compare the values

```r
t.test(sample1)
```

```
## 
## 	One Sample t-test
## 
## data:  sample1
## t = -0.18994, df = 99, p-value = 0.8497
## alternative hypothesis: true mean is not equal to 0
## 95 percent confidence interval:
##  -0.2167145  0.1788497
## sample estimates:
##   mean of x 
## -0.01893242
```

```r
t.test(sample1)$statistic==mean(sample1)/sqrt(var(sample1)/100) #Check!
```

```
##    t 
## TRUE
```

4. Interpret the result of the t-test
NUll hypothesis: true mean is equal to zero
According to the statistics, we do not reject the null hypothesis at the level of 95%

5. Repeat **step 1.** 1000 times, storing the mean on a vector and plot the histogram
For this exercise we do do it in two different ways

By using function ``replicate``
  
  ```r
  vector1 <- replicate(expr = mean(rnorm(100)), n=1000)
  hist(vector1)
  ```
  
  <img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

By using a loop
  
  ```r
  vector2 <- numeric(1000)
  for (i in 1:1000) {
  vector2[i] <- mean(rnorm(100))
  }
  hist(vector2)
  ```
  
  <img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

 We can plot those two in 1 figure
  
  ```r
  par(mfrow=c(1,2)) #Command to plot more than 1 figure at the same time
  hist(vector1)
  hist(vector2)
  ```
  
  <img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />
  
  ```r
  par(mfrow=c(1,1)) #Back to default
  ```

Simple linear regression

```r
STH <- c(12,15,10,20,8,12,16,13,5,9,12) #Hours studied
TestScore <- 0.5*STH + rnorm(11,0,1) #Exam score as a function of studied hours + random error
```

A simple linear regression is `$$y=\beta_1x+\epsilon$$`

```r
plot(STH, TestScore, xlim = c(0,20), ylim = c(0,15))
abline(a=0, b=0.5)  #a=intercept b=slope. 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

Including an intercept 

```r
TestScore <- 2+ 0.5*STH + rnorm(11,0,1) # Data Generation Process of Exam score
plot(STH, TestScore, xlim = c(0,20), ylim = c(0,15))
abline(a=2, b=0.5)  
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

## Estimate coefficient
We need some packages for this session

```r
library(AER) #Provides some datasets
library(MASS) #Collection of functions for applied statistics
```

Exploring the dataset

```r
data(CASchools) #california Schools Dataset
View(CASchools)
```


```r
#Create a new Variable, Students-teacher ratio.
#Compute STR and append it to CASchools
CASchools$STR <- CASchools$students/CASchools$teachers 

#Compute TestScore (average between Math and Read) and append it to the dataframe
CASchools$score <- (CASchools$read + CASchools$math)/2     

#Compute sample averages of STR and score
avg_STR <- mean(CASchools$STR) 
avg_score <- mean(CASchools$score)

#Compute sample standard deviations of STR and score
sd_STR <- sd(CASchools$STR) 
sd_score <- sd(CASchools$score)
```



```r
#Creating a summary dataframe
# set up a vector of percentiles and compute the quantiles 
quantiles <- c(0.10, 0.25, 0.4, 0.5, 0.6, 0.75, 0.9)
quant_STR <- quantile(CASchools$STR, quantiles) #Arguments: series and vector of percentiles

quant_score <- quantile(CASchools$score, quantiles)

# gather everything in a data.frame 
DistributionSummary <- data.frame(Average = c(avg_STR, avg_score), 
                                  StandardDeviation = c(sd_STR, sd_score), 
                                  quantile = rbind(quant_STR, quant_score))
#rbind combines two or more vectors
DistributionSummary
```

```
##               Average StandardDeviation quantile.10. quantile.25. quantile.40.
## quant_STR    19.64043          1.891812      17.3486     18.58236     19.26618
## quant_score 654.15655         19.053347     630.3950    640.05000    649.06999
##             quantile.50. quantile.60. quantile.75. quantile.90.
## quant_STR       19.72321      20.0783     20.87181     21.86741
## quant_score    654.45000     659.4000    666.66249    678.85999
```


```r
#Plot
plot(score ~ STR, 
     data = CASchools,
     main = "Scatterplot of TestScore and STR", 
     xlab = "STR (X)",
     ylab = "Test Score (Y)")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-14-1.png" width="672" />

```r
cor(CASchools$STR, CASchools$score) #Correlation. Negative, but weak
```

```
## [1] -0.2263627
```

## OLS estimation
First we try to get the estimation by hands

Function ``attach`` allows to use the variables contained in CASchools directly. **BE CAREFUL! REMEMBER TO DETACH**

```r
attach(CASchools)
```

To get the estimate of `\(\hat{\beta_1}\)` and `\(\hat{\beta_0}\)`

```r
beta_1 <- sum((STR - mean(STR)) * (score - mean(score))) / sum((STR - mean(STR))^2)
beta_0 <- mean(score) - beta_1 * mean(STR)
beta_1
```

```
## [1] -2.279808
```

```r
beta_0
```

```
## [1] 698.9329
```

R also provides packages to get the estimates, it will get the exactly same value as we did by hands

```r
linear_model <- lm(score ~ STR, data = CASchools) 
summary(linear_model)
```

```
## 
## Call:
## lm(formula = score ~ STR, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -47.727 -14.251   0.483  12.822  48.540 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 698.9329     9.4675  73.825  < 2e-16 ***
## STR          -2.2798     0.4798  -4.751 2.78e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 18.58 on 418 degrees of freedom
## Multiple R-squared:  0.05124,	Adjusted R-squared:  0.04897 
## F-statistic: 22.58 on 1 and 418 DF,  p-value: 2.783e-06
```

Then we can plot the data, and add the regression line

```r
# plot the data
plot(score ~ STR, 
     data = CASchools,
     main = "Scatterplot of TestScore and STR", 
     xlab = "STR (X)",
     ylab = "Test Score (Y)",
     xlim = c(10, 30),
     ylim = c(600, 720))

# add the regression line
abline(linear_model) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />

## Measure of fit
R provides a bunch of summary statistics

```r
summary(linear_model)
```

```
## 
## Call:
## lm(formula = score ~ STR, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -47.727 -14.251   0.483  12.822  48.540 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 698.9329     9.4675  73.825  < 2e-16 ***
## STR          -2.2798     0.4798  -4.751 2.78e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 18.58 on 418 degrees of freedom
## Multiple R-squared:  0.05124,	Adjusted R-squared:  0.04897 
## F-statistic: 22.58 on 1 and 418 DF,  p-value: 2.783e-06
```

```r
ls(summary(linear_model)) #Check available outputs
```

```
##  [1] "adj.r.squared" "aliased"       "call"          "coefficients" 
##  [5] "cov.unscaled"  "df"            "fstatistic"    "r.squared"    
##  [9] "residuals"     "sigma"         "terms"
```

```r
r_squared <- summary(linear_model)$"r.squared" #Accessing a single output
mod_summary <- summary(linear_model) #Put the outputs in an object
```

we can also get the `\(R^2\)` by hands (without all those packages)

```r
SSR <- sum(mod_summary$residuals^2)
TSS <- sum((score - mean(score))^2)
R2 <- 1 - SSR/TSS; R2 
```

```
## [1] 0.05124009
```

```r
summary(linear_model)$"r.squared" 
```

```
## [1] 0.05124009
```

```r
#Same value!
detach(CASchools)
```

## OLS assumptions
Assumption 1 There is no endogeneity problem `\(E(\epsilon_i|x_i)=0\)`. The conditional mean should be zero.


```r
# set a seed to make the results reproducible
set.seed(321)

#Generate the data 
X <- runif(50, min = -5, max = 5)
u <- rnorm(50, 0,1)  

# Data generating process (The true relationship)
Y <- X^2 + 2 * X + u                

# estimate a simple regression model 
mod_simple <- lm(Y ~ X)

# predict using a quadratic model 
prediction <- predict(lm(Y ~ X + I(X^2)), data.frame(X = sort(X)))
```


```r
# plot the results
plot(Y ~ X)
abline(mod_simple, col = "red")
lines(sort(X), prediction)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />

```r
#E(Ei|Xi) varies with the Xi.
```

**endogeneity assumption violated**

Assumption 2 The obeservations are i.i.d. There is a random sampling of observations.

```r
# set seed
set.seed(123)

# generate a date vector
Date <- seq(as.Date("1951/1/1"), as.Date("2000/1/1"), "years")

# initialize the employment vector
X <- c(5000, rep(NA, length(Date)-1))

# generate time series observations with random influences
for (i in 2:length(Date)) {
  X[i] <- -50 + 0.98 * X[i-1] + rnorm(n = 1, sd = 200)
}
```


```r
#plot the results
plot(x = Date, 
     y = X, 
     type = "l", 
     col = "steelblue", 
     ylab = "Workers", 
     xlab = "Time")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.png" width="672" />

The  level of today's employment is correlated with tomorrows employment level. Thus, the i.i.d. assumption is violated.

Assumption 3 

```r
# set seed
set.seed(123)

# generate the data
X <- sort(runif(10, min = 30, max = 70))
Y <- rnorm(10 , mean = 200, sd = 50)
Y[9] <- 2000  #Create an outlier


# fit model with outlier
fit <- lm(Y ~ X)

# fit model without outlier
fitWithoutOutlier <- lm(Y[-9] ~ X[-9])
```


```r
# plot the results

plot(Y ~ X,pch=16)
abline(fit)
abline(fitWithoutOutlier, col = "red")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-26-1.png" width="672" />

## Exercise 
We can check the unbiasedness and consistency of OLS estimators via MONTE CARLO simulations

Routine:

1. Consider the model `\(y=\beta_0+\beta_1x+\mu\)`

2. Define a value to the parameters `\(\beta_0\)` and `\(\beta_1\)`, creating these objects

3. Generate the variable x, drawing a sample (size=50) from a normal distribution

4. Generate the variable `\(\mu\)`, drawing from the standard normal distribution

5. Create the variable `\(y=\beta_0+\beta_1x+\mu\)`

6. Estimate the parameters via OLS.

7. Get the coefficients. The values might be close to the parameters.

8. Repeat the previous routine (steps 3 to 7) 1000 times. Tip: use "for". For every estimation, store the coefficients `\(\beta_0\)` and `\(\beta_1\)` in empty vectors previously created.

9. Get the mean of each vector ($\beta_0$ and `\(\beta_1\)`).

Are they close to the true parameters? Are the OLS estimators biased?

10. Plot the histogram of each of them

11. REPEAT ALL THE SIMULATIONS, BUT WITH SAMPLE SIZE EQUAL TO 200, THEN EQUAL TO 1000. Plot the 3 histograms (sample size =50, 200 and 1000) of `\(\beta_0\)` side by side to compare.

Do the same with the 3 histograms of `\(\beta_1\)`?

TIP: USE THE SAME RANGE ON X-AXIS (argument xlim= ) TO A BETTER COMPARISON

Are the OLS estimators consistent?
