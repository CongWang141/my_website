---
title: "Lesson 1"
author: "Cong Wang"
date: "2022-08-25"
draft: false
excerpt: 
weight: 1
---



## Set language
Set system language to English. Useful to search for error solutions

```r
Sys.setenv(LANG = "en")
```

To simplify the work with R,  you can set working directory. Shortcut: Ctrl + Shift + H

```r
setwd("C:/...")
```

## Basic skills for R
### Basic operations
With R you can do some simple calculations such as:

```r
3+4
```

```
## [1] 7
```

```r
sqrt(25)
```

```
## [1] 5
```

```r
6^2
```

```
## [1] 36
```

```r
3**3
```

```
## [1] 27
```

Two instructions can be placed on the same row separeted by ";"

```r
4 * 2; 5 ^ 2
```

```
## [1] 8
```

```
## [1] 25
```

### Objects
Usual assignment operator in R. Shortcut: Alt + "-",  by doing this we simply assignment the value of **a** to **3**.

```r
a <- 3
```

### Vectors
Vector of numbers

```r
x <- c(10.4, 5.6, 3.1, 6.4, 21.7)
v1 <- c(1:10)
```

Abstract some elements from vector

```r
x[1]
```

```
## [1] 10.4
```

Operations with vectors

```r
1/x
```

```
## [1] 0.09615385 0.17857143 0.32258065 0.15625000 0.04608295
```

Create a new vector,  using an existing operations

```r
y <- c(x, 0, x)
```

Vectors arithemetic 

```r
v <- 2*x + 3*x
v
```

```
## [1]  52.0  28.0  15.5  32.0 108.5
```

Mean of vector x

```r
sum(x) / length(x)
```

```
## [1] 9.44
```

Use a function to get the mean of vector x

```r
mean(x)
```

```
## [1] 9.44
```

We can calculate the variance of the vector **x**, remind the variance formula `\(S^2=\frac{\sum{(x_i-\bar{x})}}{n-1}\)` 

```r
sum((x-mean(x))^2)/(length(x)-1)
```

```
## [1] 53.853
```

Or we can use a function

```r
var(x)
```

```
## [1] 53.853
```

We can also creat a vectorof strings.

```r
s <- c('age', 'height', 'weight')
```

Create a vector by using ``sequence``

```r
s1 <- seq(length=10, from=2,  by=1)
s2 <- seq(3, 13, 1)
```

Create a vector by using ``repeat``

```r
s3 <- rep(3, times=5)
s4 <- rep(x, 5)
```

Use ``replicate`` to create a vector

```r
x <- 3
replicate(n=10, expr = x+1)
```

```
##  [1] 4 4 4 4 4 4 4 4 4 4
```

Use ``replicate`` to creat a matrix

```r
v <- c(2, 3, 4)
replicate(n=10, expr = v)
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
## [1,]    2    2    2    2    2    2    2    2    2     2
## [2,]    3    3    3    3    3    3    3    3    3     3
## [3,]    4    4    4    4    4    4    4    4    4     4
```

### Label a vector
use function to label a vector ``names``

```r
country <- c(2, 4, 1, 5)
names(country) <- c("China", "Italy", "Germany", "France")
country
```

```
##   China   Italy Germany  France 
##       2       4       1       5
```

## Simple loop structure

```r
for (i in 1:5){
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

## Check missing value

```r
z <- c(1:4, NA)
```

**z** is a vector with last element as NA, we can check it out

```r
ind <- is.na(z)
ind
```

```
## [1] FALSE FALSE FALSE FALSE  TRUE
```

## Matrix and arrays
A matrix is a two-dimensional (m x n) object, use function ``matrix`` to create a matrix

```r
m1 <- matrix(c(1, 2, 3, 4), nrow = 2, ncol = 2)
m1
```

```
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
```

An array is a three-dimensional object. All elements must be of the same type, use function ``array``, 

```r
a1 <- array(2, dim = c(1, 2, 3))
a1
```

```
## , , 1
## 
##      [,1] [,2]
## [1,]    2    2
## 
## , , 2
## 
##      [,1] [,2]
## [1,]    2    2
## 
## , , 3
## 
##      [,1] [,2]
## [1,]    2    2
```


```r
vector <- c(1:12)
multi_array <- array(vector, dim = c(2, 3, 2))
multi_array
```

```
## , , 1
## 
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
## 
## , , 2
## 
##      [,1] [,2] [,3]
## [1,]    7    9   11
## [2,]    8   10   12
```

Matrix operations, element by element product

```r
m2 <- matrix(c(2, 3, 4, 5), 2, 2)
m1*m2
```

```
##      [,1] [,2]
## [1,]    2   12
## [2,]    6   20
```

Matrix multiplication

```r
m1%*%m2
```

```
##      [,1] [,2]
## [1,]   11   19
## [2,]   16   28
```

Transpose

```r
t(m1)
```

```
##      [,1] [,2]
## [1,]    1    2
## [2,]    3    4
```

Get the inverce of matrix

```r
solve(m1)
```

```
##      [,1] [,2]
## [1,]   -2  1.5
## [2,]    1 -0.5
```

Solve equation system
`\begin{equation}
`\begin{cases}
5x + 6y = 12\\
2x + 8y = 10
\end{cases}`
\end{equation}`

```r
a <- matrix(c(5, 6, 2, 8), 2, 2); a
```

```
##      [,1] [,2]
## [1,]    5    2
## [2,]    6    8
```

```r
y <- c(12, 10); y
```

```
## [1] 12 10
```

```r
solve(a, y)
```

```
## [1]  2.7142857 -0.7857143
```

## Dataframe
Dataframe is similar to a matrix, but each column is independent. Elements must be of the same type just in each column.

```r
students <-   data.frame (index = 1:10,
              height=c(1.2, 1.3, 1.1, 1.35, 1.21,1.32, 1.33, 1.22, 1.40, 1.32),
              gender=c("male", "female ", "female", "male", "female","female", "male", "female","male","male"),
              bus_transport =c(TRUE, TRUE, FALSE, FALSE, FALSE, TRUE, TRUE,FALSE, TRUE, TRUE))
students
```

```
##    index height  gender bus_transport
## 1      1   1.20    male          TRUE
## 2      2   1.30 female           TRUE
## 3      3   1.10  female         FALSE
## 4      4   1.35    male         FALSE
## 5      5   1.21  female         FALSE
## 6      6   1.32  female          TRUE
## 7      7   1.33    male          TRUE
## 8      8   1.22  female         FALSE
## 9      9   1.40    male          TRUE
## 10    10   1.32    male          TRUE
```

check the names of each column

```r
names(students)
```

```
## [1] "index"         "height"        "gender"        "bus_transport"
```

Create another variable ``weight`` to the Dataframe

```r
students$weight <- c(40,38,28,50,34, 43, 38, 33 ,45, 37)
```

plot students' height and weight

```r
plot(students$height, students$weight)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-35-1.png" width="672" />

Several arguments can be passed to improve the plot

```r
plot(students$height, students$weight, pch=15, col='blue')
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-36-1.png" width="672" />

## Load and save data
install a package we need to use, by using ``install.packages("openxlsx")``
loading the package

```r
library(openxlsx)
```

Save the dataframe we used before ``students`` in xlsx format

```r
write.xlsx(students, file = "students.xlsx")
```

Save the dataframe we used before ``students`` in csv format

```r
write.csv(students, file = "students.csv")
```

Clear the workspace by using the code below. Shortcut: Ctrl + L

```r
rm(list = ls())
```

Load data from an online source

```r
accidents=read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/airline-safety/airline-safety.csv")
```

load from the device, "Import Dataset" button is highly recommended!
students <- read.xlsx("students.xlsx")

## Data manipulation
filter the data, create a sub Dataset with ``fatal_accidents_85_99==0`` name it as ``no_accident``,  create another sub Dataset with ``fatalities_00_14>100`` and name it as ``fatalities``, you can check those new datasets by yourself

```r
no_accident <- subset(accidents, accidents$fatal_accidents_85_99==0)
fatalities <- subset(accidents, fatalities_00_14>100)
```

## Exercise 1
The first exercise we use daily air quality measurements in NY, May to September 1973

```r
data("airquality")
```

1. you are required to drop observations with one or more missing values and create a new dataset 
2. plot a line graph with temperature over time

## Exercise 2
Here is a random sample of male and female named **x1**, and a random sample from normal distribution with mean 1.7 and standared deviation 0.1 named **x2**, **x3** is depends on **x2** plus a random error with mean=1, sd=5

```r
x1 <- sample(c('male', 'female'),1000, replace = TRUE)
x2 <- rnorm(1000, 1.7, 0.1)
x3 <- x2*45 + rnorm(1000, 1, 5)
```
1. you are required to create a dataset based on those vectors above
2. Name columns as x1=Gender, x2=Height, x3=Weight
3. Create  a new column BMI which denotes Body mass Index with the following formula: bmi= weight/height^2
4. Suppose obese condition applies to male if BMI>28 or female IF BMI>30, filter observations with obese condition and make a scatterplot (Weight ~ Height)
