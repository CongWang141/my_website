---
title: "Lesson 4"
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

## Solution to lesson 3

1. Consider the model `\(y=\beta_0+\beta_1x+\mu\)`
2. Define a value to the parameters `\(\beta_0\)` and `\(\beta_1\)`, creating these objects

```r
b0=3
b1=4
```

3. Generate the variable x, drawing a sample (size=50) from a normal distribution

4. Generate the variable `\(\mu\)`, drawing from the standard normal distribution

5. Create the variable `\(y=\beta_0+\beta_1x+\mu\)`

```r
x=rnorm(n=50, mean = 5, sd=1)
u=rnorm(50)
Y=b0+b1*x+u
```

6. Estimate the parameters via OLS.

7. Get the coefficients. The values might be close to the parameters.

```r
model1 <- lm(Y~x)

model1$coefficients
```

```
## (Intercept)           x 
##    2.934035    3.987505
```

```r
model1$coefficients[1] #b0
```

```
## (Intercept) 
##    2.934035
```

```r
model1$coefficients[2] #b1
```

```
##        x 
## 3.987505
```

8. Repeat the previous routine (steps 3 to 7) 1000 times. Tip: use "for". For every estimation, store the coefficients `\(\beta_0\)` and `\(\beta_1\)` in empty vectors previously created.

```r
vector_b0 <- numeric(1000)
vector_b1 <- numeric(1000)
for (i in 1:1000) {
  x=rnorm(50, mean = 5, sd=1) 
  u=rnorm(50) 
  Y=b0+b1*x+u 
  model1 <- lm(Y~x)
  vector_b0[i] <- model1$coefficients[1] #get the estimate of b0 and store
  vector_b1[i] <- model1$coefficients[2] #get the estimate of b1 and store
}
```

9. Get the mean of each vector ($\beta_0$ and `\(\beta_1\)`).


```r
mean(vector_b0)
```

```
## [1] 3.021223
```

```r
mean(vector_b1)
```

```
## [1] 3.996308
```

Are they close to the true parameters? Are the OLS estimators biased?

#10-Plot the histogram of each of them

```r
par(mfrow=c(1,2))
hist(vector_b0, main = expression(beta[0]),col='steelblue')
hist(vector_b1, main = expression(beta[1]), col = 'red')
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />


11. REPEAT ALL THE SIMULATIONS, BUT WITH SAMPLE SIZE EQUAL TO 200, THEN EQUAL TO 1000. Plot the 3 histograms (sample size =50, 200 and 1000) of `\(\beta_0\)` side by side to compare.

```r
# SAMPLE SIZE=200
vector_b0_200 <- numeric(1000)
vector_b1_200 <- numeric(1000)
for (i in 1:1000) {
  x=rnorm(200, mean = 5, sd=1) 
  u=rnorm(200) 
  Y=b0+b1*x+u 
  model1 <- lm(Y~x)
  vector_b0_200[i] <- model1$coefficients[1] #get the estimate of b0 and store
  vector_b1_200[i] <- model1$coefficients[2] #get the estimate of b1 and store
}
```


```r
#SAMPLE SIZE =1000
vector_b0_1000 <- numeric(1000)
vector_b1_1000 <- numeric(1000)
for (i in 1:1000) {
  x=rnorm(1000, mean = 5, sd=1) 
  u=rnorm(1000) 
  Y=b0+b1*x+u 
  model1 <- lm(Y~x)
  vector_b0_1000[i] <- model1$coefficients[1] #get the estimate of b0 and store
  vector_b1_1000[i] <- model1$coefficients[2] #get the estimate of b1 and store
}
```


Plot the 3 histograms (sample size =50, 200 and 1000) of b0 side by side to compare.

Do the same with the 3 histograms of `\(\beta_1\)`?


Are the OLS estimators consistent?

```r
#TIP: USE THE SAME RANGE ON X-AXIS (argument xlim= ) TO A BETTER COMPARISON
par(mfrow=c(1,3))
hist(vector_b0,xlim = c(1,6), main = "Sample=50")
hist(vector_b0_200, xlim = c(1,6),main = "Sample=200")
hist(vector_b0_1000,xlim = c(1,6), main = "Sample=1000")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

```r
#b1
hist(vector_b1, xlim = c(3.4,4.6),main = "Sample=50")
hist(vector_b1_200, xlim = c(3.4,4.6),main = "Sample=200")
hist(vector_b1_1000, xlim = c(3.4,4.6),main = "Sample=1000")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-2.png" width="672" />

```r
par(mfrow=c(1,1))
```

## Hypothesis test in OLS

Test two-sided hypothesis

```r
# call packages
library(AER)
library(scales)

# load the "CASchools" data
data(CASchools)
```


```r
# add student-teacher ratio
CASchools$STR <- CASchools$students/CASchools$teachers

# add average test-score
CASchools$score <- (CASchools$read + CASchools$math)/2
```

The formula for t-test of estimates is 
`$$t=\frac{(estimated \: value - hypothesized \: value)}{standard \: error \: of \: the \: estimator}$$`

```r
# estimate the model and get coefficients
linear_model <- lm(score ~ STR, data = CASchools) 
linear_model$coefficients
```

```
## (Intercept)         STR 
##  698.932949   -2.279808
```

```r
B1=linear_model$coefficients[2]
```

standard error of `\(\hat{beta_1}\)`

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
ls(summary(linear_model)) #ls gives us the possible outcomes
```

```
##  [1] "adj.r.squared" "aliased"       "call"          "coefficients" 
##  [5] "cov.unscaled"  "df"            "fstatistic"    "r.squared"    
##  [9] "residuals"     "sigma"         "terms"
```

```r
summary(linear_model)$coefficients  
```

```
##               Estimate Std. Error   t value      Pr(>|t|)
## (Intercept) 698.932949  9.4674911 73.824516 6.569846e-242
## STR          -2.279808  0.4798255 -4.751327  2.783308e-06
```

```r
summary(linear_model)$coef #just "coef" can be used as well
```

```
##               Estimate Std. Error   t value      Pr(>|t|)
## (Intercept) 698.932949  9.4674911 73.824516 6.569846e-242
## STR          -2.279808  0.4798255 -4.751327  2.783308e-06
```

```r
linear_model$coefficients
```

```
## (Intercept)         STR 
##  698.932949   -2.279808
```

```r
coef1=summary(linear_model)$coefficients
```


```r
coef1[2] #B1
```

```
## [1] -2.279808
```

```r
coef1[,2] #Access second column, standard errors for intercept and B1
```

```
## (Intercept)         STR 
##   9.4674911   0.4798255
```

```r
SEB1= coef1[,2][2] #Second element of second column
#or
SEB11=coef1[4] #4th element in general
SEB1==SEB11 # they are exactly the same
```

```
##  STR 
## TRUE
```

t-statistics by hands

```r
(t_by_hand=(B1-0)/SEB1) # t-statistics by hands
```

```
##       STR 
## -4.751327
```

```r
coef1[,3][2] # access from regression results
```

```
##       STR 
## -4.751327
```

```r
(t_by_hand=(B1-0)/SEB1)==coef1[,3][2] # check if they are the same value
```

```
##  STR 
## TRUE
```

p-value by hands, there are 420 obeservations in our dataset, we have 1 parameter in our model, so the Degrees of freedom in our model is 

```r
#Degrees of freedom = n-k-1
df=420-1-1
```

Pt function provides the student distribution

```r
p_value=2*pt(t_by_hand, df=df)
p_value
```

```
##          STR 
## 2.783308e-06
```

NOTE: As we are dealing with a large sample, the normal density could be used as well:

```r
2*pnorm(t_by_hand)
```

```
##          STR 
## 2.020859e-06
```

We reject the null and conclude that the coefficient is significantly different from zero.

Null: the class size has no influence on the students test scores at 5% level.

## Plotting
Plot the standard normal on the support [-6,6]

```r
t <- seq(-6, 6, 0.01)

plot(x = t, 
     y = dnorm(t, 0, 1), 
     type = "l", 
     col = "steelblue", 
     lwd = 2, 
     yaxs = "i", 
     axes = F, 
     ylab = "", 
     main = expression("Calculating the p-value of a Two-sided Test when" ~ t^act ~ "=-4.75"), 
     cex.lab = 0.7,
     cex.main = 1)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="672" />

Add x axis

```r
plot(x = t, 
     y = dnorm(t, 0, 1), 
     type = "l", 
     col = "steelblue", 
     lwd = 2, 
     yaxs = "i", 
     axes = F, 
     ylab = "", 
     main = expression("Calculating the p-value of a Two-sided Test when" ~ t^act ~ "=-4.75"), 
     cex.lab = 0.7,
     cex.main = 1)

tact <- -4.75
axis(1, at = c(0, -1.96, 1.96, -tact, tact), cex.axis = 0.7)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />

Shade the critical regions using ``polygon()``

```r
plot(x = t, 
     y = dnorm(t, 0, 1), 
     type = "l", 
     col = "steelblue", 
     lwd = 2, 
     yaxs = "i", 
     axes = F, 
     ylab = "", 
     main = expression("Calculating the p-value of a Two-sided Test when" ~ t^act ~ "=-4.75"), 
     cex.lab = 0.7,
     cex.main = 1)

tact <- -4.75
axis(1, at = c(0, -1.96, 1.96, -tact, tact), cex.axis = 0.7)

# critical region in left tail
polygon(x = c(-6, seq(-6, -1.96, 0.01), -1.96),
        y = c(0, dnorm(seq(-6, -1.96, 0.01)), 0), 
        col = 'orange')

# critical region in right tail

polygon(x = c(1.96, seq(1.96, 6, 0.01), 6),
        y = c(0, dnorm(seq(1.96, 6, 0.01)), 0), 
        col = 'orange')
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-21-1.png" width="672" />

Add arrows and texts indicating critical regions and the p-value

```r
plot(x = t, 
     y = dnorm(t, 0, 1), 
     type = "l", 
     col = "steelblue", 
     lwd = 2, 
     yaxs = "i", 
     axes = F, 
     ylab = "", 
     main = expression("Calculating the p-value of a Two-sided Test when" ~ t^act ~ "=-4.75"), 
     cex.lab = 0.7,
     cex.main = 1)

tact <- -4.75
axis(1, at = c(0, -1.96, 1.96, -tact, tact), cex.axis = 0.7)

# critical region in left tail
polygon(x = c(-6, seq(-6, -1.96, 0.01), -1.96),
        y = c(0, dnorm(seq(-6, -1.96, 0.01)), 0), 
        col = 'orange')

# critical region in right tail

polygon(x = c(1.96, seq(1.96, 6, 0.01), 6),
        y = c(0, dnorm(seq(1.96, 6, 0.01)), 0), 
        col = 'orange')

arrows(-3.5, 0.2, -2.5, 0.02, length = 0.1)
arrows(3.5, 0.2, 2.5, 0.02, length = 0.1)

arrows(-5, 0.16, -4.75, 0, length = 0.1)
arrows(5, 0.16, 4.75, 0, length = 0.1)

text(-3.5, 0.22, 
     labels = expression("0.025"~"="~over(alpha, 2)),
     cex = 0.7)
text(3.5, 0.22, 
     labels = expression("0.025"~"="~over(alpha, 2)),
     cex = 0.7)

text(-5, 0.18, 
     labels = expression(paste("-|",t[act],"|")), 
     cex = 0.7)
text(5, 0.18, 
     labels = expression(paste("|",t[act],"|")), 
     cex = 0.7)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />

Add ticks indicating critical values at the 0.05-level, t^act and -t^act 

```r
plot(x = t, 
     y = dnorm(t, 0, 1), 
     type = "l", 
     col = "steelblue", 
     lwd = 2, 
     yaxs = "i", 
     axes = F, 
     ylab = "", 
     main = expression("Calculating the p-value of a Two-sided Test when" ~ t^act ~ "=-4.75"), 
     cex.lab = 0.7,
     cex.main = 1)

tact <- -4.75
axis(1, at = c(0, -1.96, 1.96, -tact, tact), cex.axis = 0.7)

# critical region in left tail
polygon(x = c(-6, seq(-6, -1.96, 0.01), -1.96),
        y = c(0, dnorm(seq(-6, -1.96, 0.01)), 0), 
        col = 'orange')

# critical region in right tail

polygon(x = c(1.96, seq(1.96, 6, 0.01), 6),
        y = c(0, dnorm(seq(1.96, 6, 0.01)), 0), 
        col = 'orange')

arrows(-3.5, 0.2, -2.5, 0.02, length = 0.1)
arrows(3.5, 0.2, 2.5, 0.02, length = 0.1)

arrows(-5, 0.16, -4.75, 0, length = 0.1)
arrows(5, 0.16, 4.75, 0, length = 0.1)

text(-3.5, 0.22, 
     labels = expression("0.025"~"="~over(alpha, 2)),
     cex = 0.7)
text(3.5, 0.22, 
     labels = expression("0.025"~"="~over(alpha, 2)),
     cex = 0.7)

text(-5, 0.18, 
     labels = expression(paste("-|",t[act],"|")), 
     cex = 0.7)
text(5, 0.18, 
     labels = expression(paste("|",t[act],"|")), 
     cex = 0.7)

rug(c(-1.96, 1.96), ticksize  = 0.145, lwd = 2, col = "darkred")
rug(c(-tact, tact), ticksize  = -0.0451, lwd = 2, col = "darkgreen")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-23-1.png" width="672" />

## Confidence interval
Generate a sample with 100 obeservations from a normal distribution with mean=5 and standard deviation=5, and plot

```r
set.seed(1)

Y <- rnorm(n = 100, 
           mean = 5, 
           sd = 5)

plot(Y, 
     pch = 19, 
     col = "steelblue")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.png" width="672" />

Assuming `\(Y_i= \mu + \epsilon_i\)`, the confidence interval is

```r
cbind(CIlower = mean(Y) - 1.96 * 5 / 10, CIupper = mean(Y) + 1.96 * 5 / 10) #cbind combine objects
```

```
##       CIlower  CIupper
## [1,] 4.564437 6.524437
```

From the same intuition, we can have confidence interval in our previous linear model


```r
linear_model$coefficients
```

```
## (Intercept)         STR 
##  698.932949   -2.279808
```

```r
confint(linear_model)
```

```
##                 2.5 %     97.5 %
## (Intercept) 680.32312 717.542775
## STR          -3.22298  -1.336636
```

Let's do it "by hand", ``qt`` gives the quantile function for the t distribution

```r
lm_summ <- summary(linear_model)
c("lower" = lm_summ$coef[2,1] - qt(0.975, df = lm_summ$df[2]) * lm_summ$coef[2, 2],
  "upper" = lm_summ$coef[2,1] + qt(0.975, df = lm_summ$df[2]) * lm_summ$coef[2, 2])
```

```
##     lower     upper 
## -3.222980 -1.336636
```

## Dummy regression

Create a dummy variable and plot it with score

```r
# Create the dummy variable 
CASchools$D <- CASchools$STR < 20
# Plot the data
plot(CASchools$D, CASchools$score,            
     pch = 20,                                
     cex = 0.6,   #set size of plot symbols to 0.6
     col = "Steelblue",                       
     xlab = expression(D[i]),                 
     ylab = "Test Score",
     main = "Dummy Regression")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-28-1.png" width="672" />

Fit a model and summary

```r
#Model
dummy_model <- lm(score ~ D, data = CASchools)
summary(dummy_model)
```

```
## 
## Call:
## lm(formula = score ~ D, data = CASchools)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -50.496 -14.029  -0.346  12.884  49.504 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  650.077      1.393 466.666  < 2e-16 ***
## DTRUE          7.169      1.847   3.882  0.00012 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 18.74 on 418 degrees of freedom
## Multiple R-squared:  0.0348,	Adjusted R-squared:  0.0325 
## F-statistic: 15.07 on 1 and 418 DF,  p-value: 0.0001202
```


```r
plot(CASchools$D, CASchools$score,            
     pch = 20,                                
     cex = 0.6,   #set size of plot symbols to 0.6
     col = "Steelblue",                       
     xlab = expression(D[i]),                 
     ylab = "Test Score",
     main = "Dummy Regression")

points(x = CASchools$D,  #Plot the two predicted points
       y = predict(dummy_model), 
       col = "red", 
       pch = 20,
       cex=1.2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-30-1.png" width="672" />

Use a function in R to get the confidence interval

```r
confint(dummy_model) #Confidence intervals
```

```
##                  2.5 %    97.5 %
## (Intercept) 647.338594 652.81500
## DTRUE         3.539562  10.79931
```

## Heteroskadasticity
All inference made so far relies on the assumption that error variance does not vary as regressors values changes, which is called homoskadasticity


```r
# load scales package for adjusting color opacities
library(scales)
```

Generate some Heteroskadasticity data

```r
# set seed for reproducibility
set.seed(1)

# set up vector of x coordinates
x <- rep(c(10, 15, 20, 25), each = 25)

#Vector of errors
e <- c() # Empty Vector. We could also use numeric(100), as before

# sample 100 errors such that the variance increases with x
e[1:25] <- rnorm(25, sd = 10) # by default mean=0
e[26:50] <- rnorm(25, sd = 20)
e[51:75] <- rnorm(25, sd = 30)
e[76:100] <- rnorm(25, sd = 40)

# set up y
y <- 720 - 3.3 * x + e

# Estimate the model 
mod <- lm(y ~ x)

# Plot the data
plot(x = x, 
     y = y, 
     xlab = "Student-Teacher Ratio",
     ylab = "Test Score",
     cex = 0.5, 
     pch = 19, 
     xlim = c(8, 27), 
     ylim = c(600, 710))

# Add the regression line to the plot
abline(mod, col = "darkred")

# Add boxplots to the plot
boxplot(formula = y ~ x, 
        add = TRUE, 
        at = c(10, 15, 20, 25), 
        col = alpha("gray", 0.4), 
        border = "black")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-33-1.png" width="672" />

As you can see from the plot, the scores become more and more disperse as the student to teacher ratio goes up, the variance becomes bigger. The data is heteroskadastic.

Real life example for comparison

```r
# load package and attach data
library(AER)
data("CPSSWEducation")
attach(CPSSWEducation)

#Overview
summary(CPSSWEducation)
```

```
##       age          gender        earnings        education    
##  Min.   :29.0   female:1202   Min.   : 2.137   Min.   : 6.00  
##  1st Qu.:29.0   male  :1748   1st Qu.:10.577   1st Qu.:12.00  
##  Median :29.0                 Median :14.615   Median :13.00  
##  Mean   :29.5                 Mean   :16.743   Mean   :13.55  
##  3rd Qu.:30.0                 3rd Qu.:20.192   3rd Qu.:16.00  
##  Max.   :30.0                 Max.   :97.500   Max.   :18.00
```

Estimate a simple regression model, plot observations and add the regression line


```r
labor_model <- lm(earnings ~ education)
plot(education, 
     earnings, 
     ylim = c(0, 150))

abline(labor_model, 
       col = "steelblue", 
       lwd = 2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-35-1.png" width="672" />

```r
detach(CPSSWEducation)
```

We can plot the residuls to see if there is a Heteroskadasticity. Comparing those two plots, we can see the first one is clearly has Heteroskadasticity problem, the plotting residuals is fan shaped. But the second one is not very conclusive.

```r
plot(mod$residuals) #Evident by the picture
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-36-1.png" width="672" />

```r
plot(labor_model$residuals) #Inconclusive by the picture
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-36-2.png" width="672" />

## Breausch-Pagan test
For this test the NUll Hypothesis is: **"The error variances are all equal"**


```r
library(lmtest)
bptest(labor_model) #NUll: The error variances are all equal
```

```
## 
## 	studentized Breusch-Pagan test
## 
## data:  labor_model
## BP = 33.075, df = 1, p-value = 8.867e-09
```

```r
bptest(mod)
```

```
## 
## 	studentized Breusch-Pagan test
## 
## data:  mod
## BP = 17.48, df = 1, p-value = 2.904e-05
```

Both p-values are less than 0.05, we can consider it is statistically significant, the null hypothesis should be rejected. Hence, there is heteroskadasticity in both case.

## Deal with heteroskadasticity

Compute heteroskedasticity-robust standard errors. By using ``vcovHC`` function we can get the variance covariance matrix, then get the equare root of the variance covariance diagonal elements

```r
vcov <- vcovHC(linear_model, type = "HC1") #Var-cov matrix with robust s.e.
robust_se <- sqrt(diag(vcov));robust_se #Robust s.e.
```

```
## (Intercept)         STR 
##  10.3643617   0.5194893
```

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

Carry out t-test between standard error and robust standard error. The Null hypothesis is: standard error and robust standard error are equal.

```r
coeftest(linear_model, vcov. = vcov) #t-test for coefficients using robust s.e.
```

```
## 
## t test of coefficients:
## 
##              Estimate Std. Error t value  Pr(>|t|)    
## (Intercept) 698.93295   10.36436 67.4362 < 2.2e-16 ***
## STR          -2.27981    0.51949 -4.3886 1.447e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Based on the test, we reject the Null hypothesis.

## Another example
For the model `\(y_i=x_i+\mu_i\)`, `\(\mu_i ~ N(0, 0.6x_i)\)`


```r
set.seed(905)

# generate heteroskedastic data 
X <- 1:500
Y <- X+ rnorm(500,0,0.6*X) #Standard deviation depends on X
```


```r
# estimate a simple regression model
reg <- lm(Y ~ X)

# plot the data
plot(x = X, y = Y, 
     pch = 19, 
     col = "steelblue", 
     cex = 0.8)

# add the regression line to the plot
abline(reg, 
       col = "red", 
       lwd = 2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-41-1.png" width="672" />

Test if `\(\beta_1=1\)`

```r
linearHypothesis(reg, hypothesis.matrix = "X = 1") #generic function to test a linear hypothesis
```

```
## Linear hypothesis test
## 
## Hypothesis:
## X = 1
## 
## Model 1: restricted model
## Model 2: Y ~ X
## 
##   Res.Df      RSS Df Sum of Sq      F  Pr(>F)  
## 1    499 16386000                              
## 2    498 16257553  1    128447 3.9346 0.04785 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


```r
ls(linearHypothesis(reg, hypothesis.matrix = "X = 1")) #possible outputs
```

```
## [1] "Df"        "F"         "Pr(>F)"    "Res.Df"    "RSS"       "Sum of Sq"
```

We reject the null at 5% level Based on the p-value. This example tells us, because of the heteroskadasticity problem we might get the wrong conclusion.

## Exercise
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














