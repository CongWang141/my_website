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
##    3.861201    3.900657
```

```r
model1$coefficients[1] #b0
```

```
## (Intercept) 
##    3.861201
```

```r
model1$coefficients[2] #b1
```

```
##        x 
## 3.900657
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
## [1] 3.060234
```

```r
mean(vector_b1)
```

```
## [1] 3.987656
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


#Are the OLS estimators consistent?

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






