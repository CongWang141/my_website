---
title: "Lesson 5"
author: "Cong Wang & Renan Serenini"
date: "2022-08-25"
draft: false
excerpt: 
weight: 1
links:
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/CongWang141/statistics-with-r.git
---



### author
Cong Wang & Renan Serenini

## Solution to lesson 4

Setting a confidence level of 95%, in about 5% of cases we will wrongly reject the null. However, the fraction of rejections will be greater if we do not use robust standard errors when errors are heteroskedastic!

Test this, via Monte Carlo Simulations!

Loop Routine:

1. Consider the simple linear regression model `\(y=\beta_0+\beta_1x+\mu\)`

2. Generate `\(x\)`, `\(\mu\)` and `\(y\)` and violating the homoskedasticity assumption. Sample size=1000

3. Estimate the model

4. Perform t-test for B1 using non-robust and robust standard errors

checking to false rejection. 

Tip1:Use the function linearHypothesis()

Tip2: store the results in logic vectors

Compare the fraction of false rejections in each case. What are the implications of not using robust standard errors in this case?


```r
library(car)
# initialize vectors t and t.rob
t <- c()
t.rob <- c()

# loop sampling and estimation
for (i in 1:1000) {
  # sample data
  X <- 1:1000
  u <- rnorm(1000, 0, 0.5*X)
  Y <- 5-3*X+u
  # estimate regression model
  reg <- lm(Y ~ X) 
  # homoskedasdicity-only significance test
  t[i] <- linearHypothesis(reg, "X = -3")$'Pr(>F)'[2] < 0.05 # WRONG REJECTION
  # robust significance test
  t.rob[i] <- linearHypothesis(reg, "X = -3", white.adjust = "hc1")$'Pr(>F)'[2] < 0.05
}

# compute the fraction of false rejections
cbind(t = mean(t), t.rob = mean(t.rob))
```

```
##          t t.rob
## [1,] 0.073 0.053
```

After using robust standard errors the probability of false rejection has declined. From 56% to 36%, not a big increase but better than nothing.

## Multiple regressors Linear model


```r
# call packages
library(AER)
library(MASS)
library(mvtnorm)
```

We continue working on the dataset and the model we studied before. Let's consider our model with another variable, percenteage of English learners


```r
data(CASchools)

CASchools$STR <- CASchools$students/CASchools$teachers #create a new variable "student to teach ratio"       
CASchools$score <- (CASchools$read + CASchools$math)/2 #Score

mod <- lm(score ~ STR, data = CASchools) 
summary(mod)
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

Measure the correlations between chosen variables

```r
cor(CASchools$STR, CASchools$score) 
```

```
## [1] -0.2263627
```

```r
cor(CASchools$STR, CASchools$english) #Possible omitted variable bias. What if we estimate the model with both variables?
```

```
## [1] 0.1876424
```

Estimate multiple regression model

```r
mult.mod <- lm(score ~ STR + english, data = CASchools)
summary(mult.mod)
```

```
## 
## Call:
## lm(formula = score ~ STR + english, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -48.845 -10.240  -0.308   9.815  43.461 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 686.03224    7.41131  92.566  < 2e-16 ***
## STR          -1.10130    0.38028  -2.896  0.00398 ** 
## english      -0.64978    0.03934 -16.516  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14.46 on 417 degrees of freedom
## Multiple R-squared:  0.4264,	Adjusted R-squared:  0.4237 
## F-statistic:   155 on 2 and 417 DF,  p-value: < 2.2e-16
```

## Measure of fit in multiple regression


```r
# define the components
n <- nrow(CASchools)                            # number of observations (rows)
k <- 2                                          # number of regressors

y_mean <- mean(CASchools$score)                 # mean of avg. test-scores

SSR <- sum(residuals(mult.mod)^2)               # sum of squared residuals
TSS <- sum((CASchools$score - y_mean )^2)       # total sum of squares
ESS <- sum((fitted(mult.mod) - y_mean)^2)       # explained sum of squares
```

## Compute the measures

```r
SER <- sqrt(1/(n-k-1) * SSR)                    # standard error of the regression
Rsq <- 1 - (SSR / TSS)                          # R^2
adj_Rsq <- 1 - (n-1)/(n-k-1) * SSR/TSS          # adj. R^2

# print the measures to the console
c("SER" = SER, "R2" = Rsq, "Adj.R2" = adj_Rsq)
```

```
##        SER         R2     Adj.R2 
## 14.4644831  0.4264315  0.4236805
```

## Ols assumption

Multicolinearity (2 or more regressors are strongly correlated). Perfect multicolinearity makes it impossible to estimate the model, for example:


```r
# define the fraction of English learners        
CASchools$FracEL <- CASchools$english / 100 #FracEL is a linear combination of 'english'

# estimate the model
mult.mod <- lm(score ~ STR + english + FracEL, data = CASchools) 

# obtain a summary of the model
summary(mult.mod) 
```

```
## 
## Call:
## lm(formula = score ~ STR + english + FracEL, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -48.845 -10.240  -0.308   9.815  43.461 
## 
## Coefficients: (1 not defined because of singularities)
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 686.03224    7.41131  92.566  < 2e-16 ***
## STR          -1.10130    0.38028  -2.896  0.00398 ** 
## english      -0.64978    0.03934 -16.516  < 2e-16 ***
## FracEL             NA         NA      NA       NA    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14.46 on 417 degrees of freedom
## Multiple R-squared:  0.4264,	Adjusted R-squared:  0.4237 
## F-statistic:   155 on 2 and 417 DF,  p-value: < 2.2e-16
```

Another example:


```r
# if STR smaller 12, NS = 0, else NS = 1
CASchools$NS <- ifelse(CASchools$STR < 12, 0, 1) #IFELSE

# estimate the model
mult.mod <- lm(score ~ computer + english + NS, data = CASchools)

# obtain a model summary
summary(mult.mod) # see what will happen!
```

```
## 
## Call:
## lm(formula = score ~ computer + english + NS, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -49.492  -9.976  -0.778   8.761  43.798 
## 
## Coefficients: (1 not defined because of singularities)
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 663.704837   0.984259 674.319  < 2e-16 ***
## computer      0.005374   0.001670   3.218  0.00139 ** 
## english      -0.708947   0.040303 -17.591  < 2e-16 ***
## NS                  NA         NA      NA       NA    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14.43 on 417 degrees of freedom
## Multiple R-squared:  0.4291,	Adjusted R-squared:  0.4263 
## F-statistic: 156.7 on 2 and 417 DF,  p-value: < 2.2e-16
```

```r
table(CASchools$NS)
```

```
## 
##   1 
## 420
```

## Bias-variance tradeoff 

New function: ```rmvnorm``` - Draw from multivariate normal density

```r
?rmvnorm
```

```
## starting httpd help server ... done
```

```r
rmvnorm(n=10, mean = c(1,3), sigma = cbind(c(1,5),c(5,2)))
```

```
## Warning in rmvnorm(n = 10, mean = c(1, 3), sigma = cbind(c(1, 5), c(5, 2))):
## sigma is numerically not positive semidefinite
```

```r
#first column(X1)= N(1,1)
#second column(X2)= N(3,2)
#Cov(x1,x2)=5

var_cov=cbind(c(1,5),c(5,2));var_cov
```

Consider the model: `\(y=5+2.5x_1+sx_2+\mu\)`. Let's estimate it changing the value of Cov(X1,X2)


```r
# set number of observations
n <- 50

# initialize vectors of coefficients
coefs1 <- cbind("hat_beta_1" = numeric(10000), "hat_beta_2" = numeric(10000))
coefs2 <- coefs1

# set seed
set.seed(1)

# loop sampling and estimation
for (i in 1:10000) {
  
  # for cov(X_1,X_2) = 0.25
  X <- rmvnorm(n, c(50, 100), sigma = cbind(c(10, 2.5), c(2.5, 10)))
  u <- rnorm(n, sd = 5)
  Y <- 5 + 2.5 * X[, 1] + 3 * X[, 2] + u
  coefs1[i, ] <- lm(Y ~ X[, 1] + X[, 2])$coefficients[-1]
  
  # for cov(X_1,X_2) = 0.85
  X <- rmvnorm(n, c(50, 100), sigma = cbind(c(10, 8.5), c(8.5, 10)))
  Y <- 5 + 2.5 * X[, 1] + 3 * X[, 2] + u
  coefs2[i, ] <- lm(Y ~ X[, 1] + X[, 2])$coefficients[-1]
}
```


```r
# obtain variance estimates
diag(var(coefs1)) #In this case, 
```

```
## hat_beta_1 hat_beta_2 
## 0.05674375 0.05712459
```

```r
diag(var(coefs2))
```

```
## hat_beta_1 hat_beta_2 
##  0.1904949  0.1909056
```

```r
cbind(c(10, 8.5), c(8.5, 10))
```

```
##      [,1] [,2]
## [1,] 10.0  8.5
## [2,]  8.5 10.0
```

```r
par(mfrow=c(1,2))
hist(coefs1[,1], xlim = c(1,4), ylim = c(0,2000))
hist(coefs2[,1], xlim = c(1,4), ylim = c(0,2000))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

The plot of distribution of OLS estimators in multiple regression can tell us the probem brought by multicolinearity. When the correlation between `\(x_1\)` and `\(x_2\)` is small 0.25 on the left, the OLS estimator for `\(\beta_1\)` is more arcuate. When the correlation is high 0.85 on the right, the OLS estimator is not biased but not efficient. The conclusion we can make from this example is that in a multiple regression if two or more independent variables are highly correlated the estimate for the coefficients are not arcuate. 


```r
# set sample size
n <- 50

# initialize vector of coefficients
coefs <- cbind("hat_beta_1" = numeric(10000), "hat_beta_2" = numeric(10000))

# set seed for reproducibility
set.seed(1)
```


```r
# loop sampling and estimation
for (i in 1:10000) {
  
  X <- rmvnorm(n, c(50, 100), sigma = cbind(c(10, 2.5), c(2.5, 10)))
  u <- rnorm(n, sd = 5)
  Y <- 5 + 2.5 * X[, 1] + 3 * X[, 2] + u
  coefs[i,] <- lm(Y ~ X[, 1] + X[, 2])$coefficients[-1]
  
}

# compute density estimate
kde <- kde2d(coefs[, 1], coefs[, 2])
```


```r
# plot the density estimate
persp(kde, 
      theta = 310, 
      phi = 30, 
      xlab = "beta_1", 
      ylab = "beta_2", 
      zlab = "Est. Density"
      ,ticktype="detailed")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />


```r
#Bivariate normal density 
library(mnormt)
x     <- seq(-5, 5, 0.25) 
y     <- seq(-5, 5, 0.25)
mu    <- c(0, 0)
sigma <- matrix(c(2, -1, -1, 2), nrow = 2)
f     <- function(x, y) dmnorm(cbind(x, y), mu, sigma)
z     <- outer(x, y, f) #Outer product. Arguments: X,Y, FUNCTION
persp(x, y, z, theta = -30, phi = 25, 
      shade = 0.75, col = "gold", expand = 0.5, r = 2, 
      ltheta = 25, ticktype = "detailed")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="672" />

## Exercise

1. Load the dataset 'mtcars' (check the help for information about it)

2. Check the descriptive statistics of the variables and the correlation
3. Estimate a multiple linear model to identify the variables that impact the fuel consumption of a car
4. Test the assumption of heteroskedasticity

5. Interpret the results of the model

6. From the same model, include another variable and repeat the tests and interpretations

7. What was the effect of including this new variable? Would you keep it in the model?

8. Create a dummy variable and include it in your model, commenting the result.

9. Generate random data for a new car and predict the values of it's fuel consumption based on your model
