---
title: "Fama-French Five Factor Model"
author: "Cong Wang"
date: "2022-08-02"
output:
  html_document:
    toc: true
    number_sections: true
    toc_float: true
excerpt: Fama-French Five Factor Model is an extension of the classic CAPM to include SMB (Small Minus Big), 	HML (High Minus Low), RMW (Robust Minus Weak), and CMA (Conservative Minus Aggressive).

tags: 
- R
categories:
- Financial models

draft: false

---
<style type="text/css">
  h1,h2,h3{
  font-size: 16pt;
  color: steelblue
}
</style>




# Model Introduction

Fama-French Five Factor Model is an extension of the classic CAPM to include SMB (Small Minus Big), 	HML (High Minus Low), RMW (Robust Minus Weak), and CMA (Conservative Minus Aggressive).

The Fama/French 5 factors (2x3) are constructed using the 6 value-weight portfolios formed on size and book-to-market, the 6 value-weight portfolios formed on size and operating profitability, and the 6 value-weight portfolios formed on size and investment. 
	
1. SMB (Small Minus Big) is the average return on the nine small stock portfolios minus the average return on the nine big stock portfolios.
2. HML (High Minus Low) is the average return on the two value portfolios minus the average return on the two growth portfolios.
3. RMW (Robust Minus Weak) is the average return on the two robust operating profitability portfolios minus the average return on the two weak operating profitability portfolios. 
4. CMA (Conservative Minus Aggressive) is the average return on the two conservative investment portfolios minus the average return on the two aggressive investment portfolios.
5. R~m~-R~f~, the excess return on the market, value-weight return of all CRSP firms incorporated in the US and listed on the NYSE, AMEX, or NASDAQ.

# Data Import

## The Five Factors Daily Data
The five factors daily data is from the link here: http://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html, you can also download it directly through the [link](http://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/F-F_Research_Data_5_Factors_2x3_daily_CSV.zip).

Here I import the data from the unfolded zip file,

```r
library(readr)
ffd <- read_csv("F-F_Research_Data_5_Factors_2x3_daily.CSV", skip = 3)
head(ffd, 5)
```

```
## # A tibble: 5 × 7
##       ...1 `Mkt-RF`   SMB   HML   RMW   CMA    RF
##      <dbl>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
## 1 19630701    -0.67  0.01 -0.35  0.03  0.11 0.012
## 2 19630702     0.79 -0.31  0.24 -0.08 -0.25 0.012
## 3 19630703     0.63 -0.16 -0.09  0.13 -0.24 0.012
## 4 19630705     0.4   0.09 -0.26  0.07 -0.28 0.012
## 5 19630708    -0.63  0.07 -0.19 -0.27  0.06 0.012
```

Change the first column's name.

```r
colnames(ffd)[1] <- "Date"
head(ffd, 5)
```

```
## # A tibble: 5 × 7
##       Date `Mkt-RF`   SMB   HML   RMW   CMA    RF
##      <dbl>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
## 1 19630701    -0.67  0.01 -0.35  0.03  0.11 0.012
## 2 19630702     0.79 -0.31  0.24 -0.08 -0.25 0.012
## 3 19630703     0.63 -0.16 -0.09  0.13 -0.24 0.012
## 4 19630705     0.4   0.09 -0.26  0.07 -0.28 0.012
## 5 19630708    -0.63  0.07 -0.19 -0.27  0.06 0.012
```

## Amazon Stock Data
Here I use the Amazon stock price data as an example. It is downloaded through the **quantmod** package.

```r
library(quantmod)
amazon <- getSymbols("AMZN", from="2010-01-01", to="2022-08-01", auto.assign = F)
```

Calculate the daily return.

```r
amazon$return1 <- dailyReturn(amazon$AMZN.Close, type = "log")
```

Or we can use this to get the exactly the same result.

```r
amazon$return2 <- (amazon$AMZN.Close-stats::lag(amazon$AMZN.Close))/stats::lag(amazon$AMZN.Close)
```


```r
head(amazon[, 7:8], 5)
```

```
##                 return1      return2
## 2010-01-04  0.000000000           NA
## 2010-01-05  0.005882589  0.005899925
## 2010-01-06 -0.018281771 -0.018115673
## 2010-01-07 -0.017159620 -0.017013233
## 2010-01-08  0.026716829  0.027076923
```

## Meger the stock return data with the five factors data

```r
amazon_ret <- amazon$return1
amazon_ret2 <- fortify.zoo(amazon_ret)
colnames(amazon_ret2)[1] <- "Date"
library(lubridate)
ffd$Date <- ymd(ffd$Date)
library(tidyverse)
data_merged <- left_join(amazon_ret2, ffd, by="Date")
```


```r
head(data_merged, 5)
```

```
##         Date      return1 Mkt-RF   SMB  HML   RMW   CMA RF
## 1 2010-01-04  0.000000000   1.69  0.79 1.13 -0.18  0.21  0
## 2 2010-01-05  0.005882589   0.31 -0.41 1.24 -0.20  0.19  0
## 3 2010-01-06 -0.018281771   0.13 -0.13 0.57 -0.05  0.20  0
## 4 2010-01-07 -0.017159620   0.40  0.25 0.98 -0.69  0.22  0
## 5 2010-01-08  0.026716829   0.33  0.32 0.01  0.22 -0.37  0
```
Plot the return 

```r
library(ggplot2)
p1 <- ggplot(amazon_ret2[-1, ], aes(x=Date, y=return1))
p1 + geom_line(color="steelblue")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

Density plot with normal density curve

```r
p2 <- ggplot(amazon_ret2)
p2 + geom_histogram(mapping = aes(x=return1, y=..density..), binwidth=0.005, color="steelblue", fill="grey", size=1) +
  stat_function(fun = dnorm, args = list(mean = mean(amazon_ret2$return1, na.rm = T), sd = sd(amazon_ret2$return1, na.rm = T)), size=1)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />

# Regress Amazon's stock return on the five factors

```r
names(data_merged)[3] <- "Mkt.RF"
fit <- lm(return1 ~  SMB + HML  + CMA + Mkt.RF + RMW, data = data_merged)
fit2 <- lm(return1 ~  SMB + HML + CMA + Mkt.RF, data = data_merged)
```


```r
library(stargazer)
stargazer(fit, fit2, title = "Fama-French Five Factor Regression OLS", type="text")
```

```
## 
## Fama-French Five Factor Regression OLS
## =======================================================================
##                                     Dependent variable:                
##                     ---------------------------------------------------
##                                           return1                      
##                                (1)                       (2)           
## -----------------------------------------------------------------------
## SMB                         -0.002***                 -0.002***        
##                             (0.0005)                  (0.0005)         
##                                                                        
## HML                         -0.004***                 -0.004***        
##                             (0.0005)                  (0.0005)         
##                                                                        
## CMA                         -0.009***                 -0.009***        
##                              (0.001)                   (0.001)         
##                                                                        
## Mkt.RF                      0.011***                  0.011***         
##                             (0.0003)                  (0.0003)         
##                                                                        
## RMW                          0.00000                                   
##                              (0.001)                                   
##                                                                        
## Constant                     0.0004                    0.0004          
##                             (0.0003)                  (0.0003)         
##                                                                        
## -----------------------------------------------------------------------
## Observations                  3,145                     3,145          
## R2                            0.453                     0.453          
## Adjusted R2                   0.452                     0.452          
## Residual Std. Error     0.015 (df = 3139)         0.015 (df = 3140)    
## F Statistic         519.464*** (df = 5; 3139) 649.537*** (df = 4; 3140)
## =======================================================================
## Note:                                       *p<0.1; **p<0.05; ***p<0.01
```

Plot the regression 

```r
par(mar = c(2, 2, 2, 2))
par(mfrow = c(2, 2))
plot(fit)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-1.png" width="672" />


```r
par(mar = c(2, 2, 2, 2))
par(mfrow = c(2, 2))
plot(fit2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-14-1.png" width="672" />













