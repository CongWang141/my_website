---
title: "Lesson 6"
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

## Solutions to lesson 5

1. Load the dataset 'mtcars' (check the help for information about it)

2. Check the descriptive statistics of the variables and the correlation


```r
data(mtcars)
?mtcars
```

3. Estimate a multiple linear model to identify the variables that impact the fuel consumption of a car


```r
attach(mtcars)
model1 <- lm(mpg~cyl+wt)
summary(model1)
```

```
## 
## Call:
## lm(formula = mpg ~ cyl + wt)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.2893 -1.5512 -0.4684  1.5743  6.1004 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  39.6863     1.7150  23.141  < 2e-16 ***
## cyl          -1.5078     0.4147  -3.636 0.001064 ** 
## wt           -3.1910     0.7569  -4.216 0.000222 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.568 on 29 degrees of freedom
## Multiple R-squared:  0.8302,	Adjusted R-squared:  0.8185 
## F-statistic: 70.91 on 2 and 29 DF,  p-value: 6.809e-12
```

```r
plot(model1$residuals)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />

4. Test the assumption of heteroskedasticity


```r
library(lmtest)
bptest(model1) #NUll: The error variances are all equal
```

```
## 
## 	studentized Breusch-Pagan test
## 
## data:  model1
## BP = 3.2548, df = 2, p-value = 0.1964
```

5. Interpret the results of the model

6. From the same model, include another variable and repeat the tests and interpretations


```r
#New variable
model2 <- lm(mpg~cyl+wt+gear)
summary(model2)
```

```
## 
## Call:
## lm(formula = mpg ~ cyl + wt + gear)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.8443 -1.5455 -0.3932  1.4220  5.9416 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  42.3864     4.3790   9.679 1.97e-10 ***
## cyl          -1.5280     0.4198  -3.640 0.001093 ** 
## wt           -3.3921     0.8208  -4.133 0.000294 ***
## gear         -0.5229     0.7789  -0.671 0.507524    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.592 on 28 degrees of freedom
## Multiple R-squared:  0.8329,	Adjusted R-squared:  0.815 
## F-statistic: 46.53 on 3 and 28 DF,  p-value: 5.262e-11
```

7. What was the effect of including this new variable? Would you keep it in the model?

8. Create a dummy variable and include it in your model, commenting the result.


```r
#Create a dummy variable
mtcars$carburetors <- ifelse(carb>2,1,0)
attach(mtcars)
model3 <- lm(mpg~cyl+wt+carburetors)
summary(model3)
```

```
## 
## Call:
## lm(formula = mpg ~ cyl + wt + carburetors)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.4331 -1.3739 -0.3079  1.2162  6.0481 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  38.8034     1.8749  20.697  < 2e-16 ***
## cyl          -1.3407     0.4380  -3.061 0.004831 ** 
## wt           -3.0457     0.7639  -3.987 0.000435 ***
## carburetors  -1.3197     1.1610  -1.137 0.265323    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.555 on 28 degrees of freedom
## Multiple R-squared:  0.8377,	Adjusted R-squared:  0.8203 
## F-statistic: 48.18 on 3 and 28 DF,  p-value: 3.508e-11
```

9. Generate random data for a new car and predict the values of it's fuel consumption based on your model


```r
#Predict a random car
Panda <- cbind('cyl'=numeric(1), 'wt'=numeric(1))
Panda[1] <-   sample(mtcars$cyl, size=1)+rnorm(1) #Sample + random error term
Panda[2] <-   sample(mtcars$wt, size=1)+rnorm(1)  
Panda <- data.frame(Panda) #Transform into data frame to use function predict.lm
predict.lm(model1, Panda)
```

```
##        1 
## 15.78318
```

check the function ``predict`` and ``predict.lm`` if you are not familiar with them

```r
?predict
?predict.lm
```

## Hypothesis Tests in Multiple Regression 


```r
library(AER)
library(stargazer)
data(CASchools)
```


```r
#Our new variables
CASchools$STR <- CASchools$students/CASchools$teachers #STR of classes       
CASchools$score <- (CASchools$read + CASchools$math)/2 #Score

model <- lm(score ~ STR + english, data = CASchools)
coeftest(model, vcov. = vcovHC, type = "HC1")
```

```
## 
## t test of coefficients:
## 
##               Estimate Std. Error  t value Pr(>|t|)    
## (Intercept) 686.032245   8.728225  78.5993  < 2e-16 ***
## STR          -1.101296   0.432847  -2.5443  0.01131 *  
## english      -0.649777   0.031032 -20.9391  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


```r
# compute two-sided p-value MANUALLY
t_b1=coeftest(model, vcov. = vcovHC, type = "HC1")[2, 3];t_b1 #t_statistic
```

```
## [1] -2.544306
```

```r
p_b1=pt(abs(t_b1),df = model$df.residual); p_b1 #Distribution functions
```

```
## [1] 0.9943454
```

```r
p_value_b1=2 * (1 - p_b1); p_value_b1
```

```
## [1] 0.01130921
```

## Confidence Intervals in Multiple Regression 

warning: confint does not use robust standard errors


```r
model <- lm(score ~ STR + english, data = CASchools)
confint(model, level = 0.9)
```

```
##                     5 %        95 %
## (Intercept) 673.8145793 698.2499098
## STR          -1.7281904  -0.4744009
## english      -0.7146336  -0.5849200
```


```r
# compute robust standard errors
rob_se <- diag(vcovHC(model, type = "HC1"))^0.5
rob_se
```

```
## (Intercept)         STR     english 
##  8.72822452  0.43284720  0.03103176
```


```r
# compute robust 95% confidence intervals
rbind("lower" = coef(model) - qnorm(0.975) * rob_se,
      "upper" = coef(model) + qnorm(0.975) * rob_se)
```

```
##       (Intercept)        STR    english
## lower    668.9252 -1.9496606 -0.7105980
## upper    703.1393 -0.2529307 -0.5889557
```


```r
# compute robust 90% confidence intervals
rbind("lower" = coef(model) - qnorm(0.95) * rob_se,
      "upper" = coef(model) + qnorm(0.95) * rob_se)
```

```
##       (Intercept)        STR    english
## lower    671.6756 -1.8132659 -0.7008195
## upper    700.3889 -0.3893254 -0.5987341
```

Let's include another variable

```r
# scale expenditure to thousands of dollars
CASchools$expenditure <- CASchools$expenditure/1000
```


```r
# estimate the model
model <- lm(score ~ STR + english + expenditure, data = CASchools)
summary(model)
```

```
## 
## Call:
## lm(formula = score ~ STR + english + expenditure, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -51.340 -10.111   0.293  10.318  43.181 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 649.57795   15.20572  42.719  < 2e-16 ***
## STR          -0.28640    0.48052  -0.596  0.55149    
## english      -0.65602    0.03911 -16.776  < 2e-16 ***
## expenditure   3.86790    1.41212   2.739  0.00643 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14.35 on 416 degrees of freedom
## Multiple R-squared:  0.4366,	Adjusted R-squared:  0.4325 
## F-statistic: 107.5 on 3 and 416 DF,  p-value: < 2.2e-16
```

```r
coeftest(model, vcov. = vcovHC, type = "HC1")
```

```
## 
## t test of coefficients:
## 
##               Estimate Std. Error  t value Pr(>|t|)    
## (Intercept) 649.577947  15.458344  42.0212  < 2e-16 ***
## STR          -0.286399   0.482073  -0.5941  0.55277    
## english      -0.656023   0.031784 -20.6398  < 2e-16 ***
## expenditure   3.867901   1.580722   2.4469  0.01482 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


```r
# Multicolinearity issue
cor(CASchools$STR, CASchools$expenditure)
```

```
## [1] -0.6199822
```

## F-Statistic 


```r
# estimate the multiple regression model
model <- lm(score ~ STR + english + expenditure, data = CASchools)
```

Execute the function on the model object and provide both linear restrictions 


```r
linearHypothesis(model, c("STR=0", "expenditure=0"))
```

```
## Linear hypothesis test
## 
## Hypothesis:
## STR = 0
## expenditure = 0
## 
## Model 1: restricted model
## Model 2: score ~ STR + english + expenditure
## 
##   Res.Df   RSS Df Sum of Sq      F   Pr(>F)    
## 1    418 89000                                 
## 2    416 85700  2    3300.3 8.0101 0.000386 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
#NULL: B1=B2=0

# heteroskedasticity-robust F-test
linearHypothesis(model, c("STR=0", "expenditure=0"), white.adjust = "hc1")
```

```
## Linear hypothesis test
## 
## Hypothesis:
## STR = 0
## expenditure = 0
## 
## Model 1: restricted model
## Model 2: score ~ STR + english + expenditure
## 
## Note: Coefficient covariance matrix supplied.
## 
##   Res.Df Df      F   Pr(>F)   
## 1    418                      
## 2    416  2 5.4337 0.004682 **
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


```r
#Overall F-test
linearHypothesis(model, c("STR=0", "english=0", "expenditure=0"), white.adjust = "hc1")
```

```
## Linear hypothesis test
## 
## Hypothesis:
## STR = 0
## english = 0
## expenditure = 0
## 
## Model 1: restricted model
## Model 2: score ~ STR + english + expenditure
## 
## Note: Coefficient covariance matrix supplied.
## 
##   Res.Df Df     F    Pr(>F)    
## 1    419                       
## 2    416  3 147.2 < 2.2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
summary(model)
```

```
## 
## Call:
## lm(formula = score ~ STR + english + expenditure, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -51.340 -10.111   0.293  10.318  43.181 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 649.57795   15.20572  42.719  < 2e-16 ***
## STR          -0.28640    0.48052  -0.596  0.55149    
## english      -0.65602    0.03911 -16.776  < 2e-16 ***
## expenditure   3.86790    1.41212   2.739  0.00643 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14.35 on 416 degrees of freedom
## Multiple R-squared:  0.4366,	Adjusted R-squared:  0.4325 
## F-statistic: 107.5 on 3 and 416 DF,  p-value: < 2.2e-16
```

```r
summary(model)$fstatistic
```

```
##    value    numdf    dendf 
## 107.4547   3.0000 416.0000
```

```r
ls(summary(model))
```

```
##  [1] "adj.r.squared" "aliased"       "call"          "coefficients" 
##  [5] "cov.unscaled"  "df"            "fstatistic"    "r.squared"    
##  [9] "residuals"     "sigma"         "terms"
```

## Confidence Sets for Multiple Coefficients 

draw the 95% confidence set for coefficients on STR and expenditure

```r
confidenceEllipse(model, 
                  fill = T,
                  lwd = 0,
                  which.coef = c("STR", "expenditure"),
                  main = "95% Confidence Set")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-21-1.png" width="672" />

draw the **robust** 95% confidence set for coefficients on STR and expenditure, and the 95% confidence set for coefficients on STR and expenditure

```r
confidenceEllipse(model, 
                  fill = T,
                  lwd = 0,
                  which.coef = c("STR", "expenditure"),
                  main = "95% Confidence Sets",
                  vcov. = vcovHC(model, type = "HC1"),
                  col = "red")

confidenceEllipse(model, 
                  fill = T,
                  lwd = 0,
                  which.coef = c("STR", "expenditure"),
                  add = T) #Argument add=T (TRUE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />

