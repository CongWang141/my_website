---
title: "GARCH Model with R"
author: "Cong Wang"
date: "2022-08-04"
output: 
  html_document:
    toc: true
    number_sections: true
    toc_float: true
tags:
- R
categories:
- Financial models
excerpt: One fundamental assumptions for classic linear regression model is **homoskedasticity**, which mean the variance of the error term is constant. But with financial data, we often observe time-varying variance namely **heteroskedasticity**. We know that a GMM model would be more suitable in such case. This might be the first time for most students countering such a concept.


---

<style type="text/css">
  h1,h2,h3{
  font-size: 16pt;
  color: steelblue
}
</style>



# Model Introduction

One fundamental assumptions for classic linear regression model is **homoskedasticity**, which mean the variance of the error term is constant. But with financial data, we often observe time-varying variance namely **heteroskedasticity**. We know that a GMM model would be more suitable in such case. This might be the first time for most students countering such a concept.

**GARCH model** (Generalized Autoregressive Conditional Heteroskedasticity model) describes the variance of the current error term follows an **ARMA** model (Autoregressive Moving Average) instead of constant. 

`$$y_t = x_t + \epsilon_t$$`
`\(y_t\)` is a stock return, `\(x_t\)` is a mean reverting process, and `\(\epsilon_t\)` is the error term. Usually `\(\epsilon_t \sim N(0, \delta_t)\)` and `\(\delta_t\)` is constant. But in GARCH model `\(\delta_t\)` is time-varying and follow an ARMA process as follow:
`$$\delta_t = \omega + \alpha_1 \epsilon^2_{t-1} + ... + \alpha_q \epsilon^2_{t-q} + \beta_1 \delta^2_{t-1} +...+ \beta_p \delta^2_{t-p}$$`

## Use rugarch Package to Fit a GARCH Model{-}
The easy way to fit a GARCH model is using **rugarch** package through those two simple steps:

  1. Setting the model specification.
  2. Fit the model and get the parameters.

# Data Exploration
## Import Data
In this example I gonna use the Google stock data. It is downloaded through the **quantmod** package.


```r
library(quantmod)
googl <- getSymbols("GOOGL", from="2010-01-01", to="2022-08-03", auto.assign = F)
head(googl[, 1:5], 5)
```

```
##            GOOGL.Open GOOGL.High GOOGL.Low GOOGL.Close GOOGL.Volume
## 2010-01-04   15.68944   15.75350  15.62162    15.68443     78169752
## 2010-01-05   15.69520   15.71171  15.55405    15.61537    120067812
## 2010-01-06   15.66216   15.66216  15.17417    15.22172    158988852
## 2010-01-07   15.25025   15.26526  14.83108    14.86737    256315428
## 2010-01-08   14.81481   15.09635  14.74249    15.06557    188783028
```

Calculate daily return.

```r
daily_ret <- (googl$GOOGL.Close-stats::lag(googl$GOOGL.Close))/stats::lag(googl$GOOGL.Close)
```

Convet the data into data frame, and change the row and column names.

```r
daily_ret <- data.frame(index(daily_ret), daily_ret)
colnames(daily_ret) <- c("date", "return")
rownames(daily_ret) <- 1:nrow(daily_ret)
```

## Plot the Return

```r
library(ggplot2)
p1 <- ggplot(daily_ret, aes(x=date, y=return))
p1 + geom_line(colour="steelblue")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

As it shows in the plot, the return of stock is not stationary. Approximately the mean is 0, but the volatility seems to be random. In the following, I will plot the histogram of the return and compare it with normal distribution. 


```r
p2 <- ggplot(daily_ret) 

p2 + geom_histogram(aes(x=return, y=..density..), binwidth = 0.005, color="steelblue", fill="grey", size=1) +
  stat_function(fun = dnorm, args = list(mean = mean(daily_ret$return, na.rm = T), sd = sd(daily_ret$return, na.rm = T)), size=1)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

**1. Skewness**

  As we can see from the plot the return is not symmetrically distributed, rather it is positive skewed (or right skewed). A positive skewness indicating on average it gives us a positive return. But the level of skewness is not very high. *p.s. If the distribution is highly right skewed we can use a log transformation to convert it into normal in avioding any misleading results.*

**2. Kurtosis**

  The plot show us positive Kurtosis comparing with normal distribution. Kurtosis can be used as a measure of risk. A large Kurtosis is associated with a high level of risk because it indicates that there are high probabilities of extremely large and small returns (**Heavy-tailed**). Meanwhile, a small kurtosis shows a moderate level of risk for the probabilities of extreme returns are relatively low. 

## Calculate the Monthly Volatility

Convert back the data frame to xts, then use `rollapply` to calculate rolling volatility.  For monthly rolling volatility I choose 20 trading days for one moth.


```r
library(PerformanceAnalytics)
library(xts)
daily_ret_xts <- xts(daily_ret[,-1], order.by=daily_ret[,1])
realizedvol <- rollapply(daily_ret_xts, width = 20, FUN=sd.annualized)
```

Convert back the xts data to dataframe.

```r
vol <- data.frame(index(realizedvol), realizedvol)
colnames(vol) <- c("date", "volatility")
```

Plot the monthly volatility.

```r
p3 <- ggplot(vol, aes(x=date, y=volatility))
p3 +
  geom_line( color="steelblue")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

We can see that most of the time the volatility is around 0.25, while there are 4 times the volatility become very high. 

# Fit a GARCH Model 

## 1. Model Specification
Setting the model specification using the `ugarchspec` function.

```r
library(rugarch)
garch_spec <- ugarchspec(variance.model=list(model="sGARCH", garchOrder=c(1,1)), mean.model=list(armaOrder=c(0,0)))
```

For the standard GARCH model, I specify a constant to mean ARMA model, which means that arma0rder = c(0,0). We consider the GARCH(1,1) model and the distribution of the conditional error term is the normal distribution.

## 2. Fit the Model

```r
fit_garch <- ugarchfit(spec = garch_spec, data = vol[-c(1:19),2])
fit_garch
```

```
## 
## *---------------------------------*
## *          GARCH Model Fit        *
## *---------------------------------*
## 
## Conditional Variance Dynamics 	
## -----------------------------------
## GARCH Model	: sGARCH(1,1)
## Mean Model	: ARFIMA(0,0,0)
## Distribution	: norm 
## 
## Optimal Parameters
## ------------------------------------
##         Estimate  Std. Error   t value Pr(>|t|)
## mu      0.183618    0.001297 141.60640  0.00000
## omega   0.000276    0.000027  10.22687  0.00000
## alpha1  0.999000    0.033681  29.66070  0.00000
## beta1   0.000000    0.007195   0.00001  0.99999
## 
## Robust Standard Errors:
##         Estimate  Std. Error   t value Pr(>|t|)
## mu      0.183618    0.010063 18.247557 0.000000
## omega   0.000276    0.000121  2.285651 0.022275
## alpha1  0.999000    0.084218 11.862082 0.000000
## beta1   0.000000    0.005298  0.000014 0.999989
## 
## LogLikelihood : 4348.208 
## 
## Information Criteria
## ------------------------------------
##                     
## Akaike       -2.7600
## Bayes        -2.7523
## Shibata      -2.7600
## Hannan-Quinn -2.7572
## 
## Weighted Ljung-Box Test on Standardized Residuals
## ------------------------------------
##                         statistic p-value
## Lag[1]                       1862       0
## Lag[2*(p+q)+(p+q)-1][2]      2695       0
## Lag[4*(p+q)+(p+q)-1][5]      4905       0
## d.o.f=0
## H0 : No serial correlation
## 
## Weighted Ljung-Box Test on Standardized Squared Residuals
## ------------------------------------
##                         statistic p-value
## Lag[1]                     0.1021  0.7493
## Lag[2*(p+q)+(p+q)-1][5]    0.1275  0.9969
## Lag[4*(p+q)+(p+q)-1][9]    0.2006  0.9999
## d.o.f=2
## 
## Weighted ARCH LM Tests
## ------------------------------------
##             Statistic Shape Scale P-Value
## ARCH Lag[3]  0.004844 0.500 2.000  0.9445
## ARCH Lag[5]  0.040714 1.440 1.667  0.9963
## ARCH Lag[7]  0.062791 2.315 1.543  0.9998
## 
## Nyblom stability test
## ------------------------------------
## Joint Statistic:  1.2256
## Individual Statistics:             
## mu     0.2599
## omega  0.4897
## alpha1 0.6476
## beta1  0.4083
## 
## Asymptotic Critical Values (10% 5% 1%)
## Joint Statistic:     	 1.07 1.24 1.6
## Individual Statistic:	 0.35 0.47 0.75
## 
## Sign Bias Test
## ------------------------------------
##                    t-value   prob sig
## Sign Bias           1.2671 0.2052    
## Negative Sign Bias  0.3633 0.7164    
## Positive Sign Bias  1.0105 0.3123    
## Joint Effect        2.3665 0.4999    
## 
## 
## Adjusted Pearson Goodness-of-Fit Test:
## ------------------------------------
##   group statistic p-value(g-1)
## 1    20      7769            0
## 2    30      7843            0
## 3    40      9979            0
## 4    50      8988            0
## 
## 
## Elapsed time : 0.254529
```












