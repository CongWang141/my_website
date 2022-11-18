---
author: Cong Wang
date: "2022-10-1"
description: |
Datasets in real life are rarely complete, so handling missing data is a common topic in data science. 
The way we handle such data may have a substantial effect on our results, so caution when dealing with NAs is advisable.
excerpt: Datasets in real life are rarely complete, so handling missing data is a common topic in data science. 
The way we handle such data may have a substantial effect on our results, so caution when dealing with NAs is advisable.
layout: single-series
publishDate: "2022-10-01"
show_author_byline: true
show_post_date: true
show_post_thumbnail: true
subtitle: Handle missing data
title: Course 1
weight: 1

links:
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/CongWang141/data_science.git
---


### Handling Missing Data
Missing data is so often, we need to handle them carefully


```python
## Load the relevant modules
import matplotlib.pylab as plt
import pandas as pd
import numpy as np
import seaborn as sns
import sklearn
```


```python
# set random seed for reproducibility
np.random.seed(20221001)
```


```python
# Load the data and view its shape and content
data = pd.read_csv("https://github.com/barcelonagse-datascience/academic_files/raw/master/data/titanic3.csv")
```


```python
# explore the data
print(data.shape)
print(data.head())
print(data.columns)
```

    (1309, 14)
       pclass  survived                                             name     sex  \
    0       1         1                    Allen, Miss. Elisabeth Walton  female   
    1       1         1                   Allison, Master. Hudson Trevor    male   
    2       1         0                     Allison, Miss. Helen Loraine  female   
    3       1         0             Allison, Mr. Hudson Joshua Creighton    male   
    4       1         0  Allison, Mrs. Hudson J C (Bessie Waldo Daniels)  female   
    
         age  sibsp  parch  ticket      fare    cabin embarked boat   body  \
    0  29.00      0      0   24160  211.3375       B5        S    2    NaN   
    1   0.92      1      2  113781  151.5500  C22 C26        S   11    NaN   
    2   2.00      1      2  113781  151.5500  C22 C26        S  NaN    NaN   
    3  30.00      1      2  113781  151.5500  C22 C26        S  NaN  135.0   
    4  25.00      1      2  113781  151.5500  C22 C26        S  NaN    NaN   
    
                             home.dest  
    0                     St Louis, MO  
    1  Montreal, PQ / Chesterville, ON  
    2  Montreal, PQ / Chesterville, ON  
    3  Montreal, PQ / Chesterville, ON  
    4  Montreal, PQ / Chesterville, ON  
    Index(['pclass', 'survived', 'name', 'sex', 'age', 'sibsp', 'parch', 'ticket',
           'fare', 'cabin', 'embarked', 'boat', 'body', 'home.dest'],
          dtype='object')
    


```python
data = data.drop(['name', 'body', 'ticket', 'embarked', 'home.dest', 'boat'], axis=1)
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pclass</th>
      <th>survived</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>cabin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>29.00</td>
      <td>0</td>
      <td>0</td>
      <td>211.3375</td>
      <td>B5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>male</td>
      <td>0.92</td>
      <td>1</td>
      <td>2</td>
      <td>151.5500</td>
      <td>C22 C26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>0</td>
      <td>female</td>
      <td>2.00</td>
      <td>1</td>
      <td>2</td>
      <td>151.5500</td>
      <td>C22 C26</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>male</td>
      <td>30.00</td>
      <td>1</td>
      <td>2</td>
      <td>151.5500</td>
      <td>C22 C26</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
      <td>female</td>
      <td>25.00</td>
      <td>1</td>
      <td>2</td>
      <td>151.5500</td>
      <td>C22 C26</td>
    </tr>
  </tbody>
</table>
</div>




```python
# check how may missing values in each col
nulls = data.isnull().mean()*100
nulls
```




    pclass       0.000000
    survived     0.000000
    sex          0.000000
    age         20.091673
    sibsp        0.000000
    parch        0.000000
    fare         0.076394
    cabin       77.463713
    dtype: float64




```python
# plot null in each col
fig = plt.figure(figsize=(9,4))
plt.bar(nulls.index, nulls.values)
# rotate the x ticks
# plt.xticks(rotation='vertical')

# add y axis label
plt.ylabel('Missing data persentage')
print('Our data has %s rows and %s colums.' % data.shape)

```

    Our data has 1309 rows and 8 colums.
    


    
![png](output_7_1.png)
    


we can see that _age_, _fare_, and _carbn_ have missing values, for _age_ and _fare_ the number of missing value is large.

### Methods of handling missing values

1. Remove empty colums/rows


```python
# remove col 'survived'
X = data.drop('survived', axis=1)
print(data.shape)
print(X.shape)
```

    (1309, 8)
    (1309, 7)
    


```python
# remove col with any missing value
missing_data_col = data.columns[nulls > 0]
print(missing_data_col)
data_nona = data.drop(missing_data_col, axis=1)
data_nona.shape
```

    Index(['age', 'fare', 'cabin'], dtype='object')
    




    (1309, 5)




```python
data_nona.isna().sum()
```




    pclass      0
    survived    0
    sex         0
    sibsp       0
    parch       0
    dtype: int64



We have dealt with the NAS by removing the columns that had any NAs. But this may cost us a lot. In particular, we can no longer use the information in _age_ and _fare_ to predict _survived_ . 


```python
# check the corr between fare and survived
data.fare.corr(data.survived)
```




    0.24426546891481227




```python
# or 
print(data.groupby('survived').fare.mean())
data.groupby('survived').fare.median()
```

    survived
    0    23.353831
    1    49.361184
    Name: fare, dtype: float64
    




    survived
    0    10.5
    1    26.0
    Name: fare, dtype: float64




```python
# also 
data['children'] = (data.age <= 18)
data.groupby('children').survived.mean()
```




    children
    False    0.362903
    True     0.492228
    Name: survived, dtype: float64



This may not be a great idea.

The correlation between _fare_ and _survived_ is not negligible, so we would like to keep this variable. In addition to that, children also were more likely to survive than adults, so we also would like to keep _age_ as a predictor

An alternative to completely removing columns with some missing data is to remove rows with missing data.

Clearly, this may cost us observations, so a better strategy is to *mix* both column and row deletion:

1. Remove columns (features) with high percentage of missing data;
2. Then, for the remaining dataset, remove rows with missing data

We can use the *dropna* function to delete nas on each axis (0=rows,1=columns)
``` python
    data.dropna(axis=1, thresh=round(my_percentage_valid*len(data.index)))
```

### Propagate previous or next value

This method basically consists of filling a gap with previous or posterior observations of the same feature.

This approach would typically make sense when there is a natural ordering in data (e.g. time series), **but we have to be careful!** if the goal is prediction, it may not be a great idea to use future values to replace past values.

For an example of when this could be sensible, if we have a sequence of daily stock prices where one day is a holiday, we may just want to fill this using the last available price.

One can easily code it using $fillna$ method for dataframes:

``` python
    # backward
    my_data.fillna(method='bfill',inplace=True)

    # forward
    my_data.fillna(method='ffill',inplace=True)

```

If there are consecutive observations with missing data for the same feature, the method is recursively applied to make sure that all gaps are filled.


```python
# An example of forward filling works
test_data = pd.DataFrame({'x':[1,2,3,4,5,6,7,8,9,10],'y':[1,None,3,4,5,None,None,None,9,10]})
test_data.fillna(method='ffill', inplace=True)
test_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>10.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# and backward filling
test_data = pd.DataFrame({'x':[1,2,3,4,5,6,7,8,9,10],'y':[1,None,3,4,5,None,None,None,9,10]})
test_data.fillna(method='bfill', inplace=True)
test_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>10.0</td>
    </tr>
  </tbody>
</table>
</div>



### Replace by statistics (mean/ median/ mode)

One strategy that is typically used is to replace missing data by unconditional statistics obtained over the whole sample:
- *Mean* for numerical features where ouliers are not significant;
- *Median* for numerical features where outliers clearly affect the mean;
- *Mode* for categorical features;

E.g.:
``` python
 my_data.my_column.fillna(my_data.my_column.mean(),inplace=True)
```
This imputation strategy is interesting as it preserves the unconditional mean of the series.
This simple imputation shrinks overall values towards the unconditional mean or median, reducing variability. 

We can think of different ways to avoid this, such as replacing by *conditional* statistics, i.e using particular subgroups of data and categorical variables.

``` python
    my_data[my_col].fillna(my_data.groupby(my_category)[my_col].transform("mean"), inplace=True)
```


```python
X_complete = X.copy()
X_complete['age'].fillna(X_complete.groupby('sex')['age'].transform('mean'), inplace=True)
print(X_complete.isnull().sum())
X_complete.tail(10)
```

    pclass       0
    sex          0
    age          0
    sibsp        0
    parch        0
    fare         1
    cabin     1014
    dtype: int64
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>cabin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1299</th>
      <td>3</td>
      <td>male</td>
      <td>27.000000</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1300</th>
      <td>3</td>
      <td>female</td>
      <td>15.000000</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1301</th>
      <td>3</td>
      <td>male</td>
      <td>45.500000</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1302</th>
      <td>3</td>
      <td>male</td>
      <td>30.585228</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1303</th>
      <td>3</td>
      <td>male</td>
      <td>30.585228</td>
      <td>0</td>
      <td>0</td>
      <td>14.4583</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1304</th>
      <td>3</td>
      <td>female</td>
      <td>14.500000</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1305</th>
      <td>3</td>
      <td>female</td>
      <td>28.687088</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1306</th>
      <td>3</td>
      <td>male</td>
      <td>26.500000</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1307</th>
      <td>3</td>
      <td>male</td>
      <td>27.000000</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1308</th>
      <td>3</td>
      <td>male</td>
      <td>29.000000</td>
      <td>0</td>
      <td>0</td>
      <td>7.8750</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
X.tail(10)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>cabin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1299</th>
      <td>3</td>
      <td>male</td>
      <td>27.0</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1300</th>
      <td>3</td>
      <td>female</td>
      <td>15.0</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1301</th>
      <td>3</td>
      <td>male</td>
      <td>45.5</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1302</th>
      <td>3</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1303</th>
      <td>3</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>14.4583</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1304</th>
      <td>3</td>
      <td>female</td>
      <td>14.5</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1305</th>
      <td>3</td>
      <td>female</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>14.4542</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1306</th>
      <td>3</td>
      <td>male</td>
      <td>26.5</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1307</th>
      <td>3</td>
      <td>male</td>
      <td>27.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.2250</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1308</th>
      <td>3</td>
      <td>male</td>
      <td>29.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.8750</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



```python
# do it manully 

X_complete = X.copy()
na_idx = X_complete['age'].isna() # find the indices of the missing age value

sex_na = X_complete[na_idx]['sex'] # find the gender of those observations which missing age value

my_mean = X.groupby('sex').age.mean() # get the mean age for each gender
replace_age = my_mean[sex_na].reset_index()# merge my_mean with sex_na & add index to replace values 
replace_age.set_index(sex_na.index, inplace=True) # set replacement index as those that need to be replaced

X_complete.loc[sex_na.index, 'age'] = replace_age['age']
X_complete[na_idx][['age', 'sex']]


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>37</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>40</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>46</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>59</th>
      <td>28.687088</td>
      <td>female</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1293</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>1297</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>1302</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>1303</th>
      <td>30.585228</td>
      <td>male</td>
    </tr>
    <tr>
      <th>1305</th>
      <td>28.687088</td>
      <td>female</td>
    </tr>
  </tbody>
</table>
<p>263 rows × 2 columns</p>
</div>



### Using models to impute data

We can also use formal statistical models to impute data. 

In particular, we can just treat the missing data as a prediction problem, and predict the missing value using any of the models we will see in the upcoming lectures. There are also other more complex ways of dealing with this, but we will not go in more detail. See [here](https://scikit-learn.org/stable/modules/generated/sklearn.impute.IterativeImputer.html#sklearn.impute.IterativeImputer) for more details.


### Feature transformation

### Binning

This is process of aggregating a continuous variable into bins. For example, from *grades* to *grade_class* (A, B, C).

Check $sklearn$ function [`KBinsDiscretizer()`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.KBinsDiscretizer.html#sklearn.preprocessing.KBinsDiscretizer).

### Log transform
You may want to consider log transformations of your variables (this is very standard in economics and finance). Takings logs may decrease the effect of outliers on our models and provide a natural interpretation of coefficients in percentage terms.

Note that the log should be used with caution for negative or zero valued random variables (one may use log (abs(x+1)) plus a dummy for negatives)

### Example
Toy example with titanic dataset.



```python
# load it again to use features we had previously deleted
data = pd.read_csv("https://github.com/barcelonagse-datascience/academic_files/raw/master/data/titanic3.csv")

data.columns
```




    Index(['pclass', 'survived', 'name', 'sex', 'age', 'sibsp', 'parch', 'ticket',
           'fare', 'cabin', 'embarked', 'boat', 'body', 'home.dest'],
          dtype='object')




```python
data.fare.describe()
```




    count    1308.000000
    mean       33.295479
    std        51.758668
    min         0.000000
    25%         7.895800
    50%        14.454200
    75%        31.275000
    max       512.329200
    Name: fare, dtype: float64




```python
# plot the distribution of fare with histogram, density plot
fig = plt.figure(figsize=(9, 6))
plt.hist(data.fare, bins=20)

# by using sns
sns.displot(data.fare, kde=True, bins=20)
```




    <seaborn.axisgrid.FacetGrid at 0x167a84ba1d0>




    
![png](output_32_1.png)
    



    
![png](output_32_2.png)
    



```python
# construct the log of the fare
data['log_fare'] = np.log(data.fare + 1) # remember there is 0 in the value

sns.displot(data.log_fare, kde=True, bins=20)
```




    <seaborn.axisgrid.FacetGrid at 0x1679d4c6620>




    
![png](output_33_1.png)
    


### Scaling (Normalization/Standardization)

We typically want variables to have, loosely speaking, a sensible scale.
A few usual options are ( see also [$sklearn.preprocessing$](https://scikit-learn.org/stable/modules/preprocessing.html) ):

+ [`StandardScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html): Removes the mean and scales to unit variance. This is the standard definition of scaling. After the transformation, all variables have the same mean (0) and variance (1).

+ [`RobustScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html): Similar to the StandardScaler but more robust to outliers. Removes the median and scales the data according to the quantile range (defaults to IQR: Interquartile Range). Note that now output won't have a mean of 0 and a standard deviation of 1
+ [`MinMaxScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html): Linear scaling to an output range, just considering the minimum and maximum. Just note that default range is (0,1). The output is not necessarily _well behaved_  and this is very sensible to outliers.



```python
from sklearn.preprocessing import RobustScaler, StandardScaler, MinMaxScaler

SS = StandardScaler()
RS = RobustScaler()
MMS = MinMaxScaler()

data['fare_SS'] = SS.fit_transform(data[['fare']])
data['fare_RS'] = RS.fit_transform(data[['fare']])
data['fare_MMS'] = MMS.fit_transform(data[['fare']])

data[['fare_SS', 'fare_RS', 'fare_MMS']].describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fare_SS</th>
      <th>fare_RS</th>
      <th>fare_MMS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.308000e+03</td>
      <td>1308.000000</td>
      <td>1308.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>-8.691654e-17</td>
      <td>0.805899</td>
      <td>0.064988</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.000382e+00</td>
      <td>2.213877</td>
      <td>0.101026</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-6.435292e-01</td>
      <td>-0.618250</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-4.909206e-01</td>
      <td>-0.280523</td>
      <td>0.015412</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-3.641609e-01</td>
      <td>0.000000</td>
      <td>0.028213</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>-3.905147e-02</td>
      <td>0.719477</td>
      <td>0.061045</td>
    </tr>
    <tr>
      <th>max</th>
      <td>9.258680e+00</td>
      <td>21.295639</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plot the standardized data

fig, axes = plt.subplots(nrows=1, ncols=3, 
                         figsize=(16, 9),
                         sharey=True)

sns.histplot(data.fare_SS, kde=True, bins=20, ax=axes[0])
sns.histplot(data.fare_RS, kde=True, bins=20, ax=axes[1])
sns.histplot(data.fare_MMS, kde=True, bins=20, ax=axes[2])
```




    <AxesSubplot: xlabel='fare_MMS', ylabel='Count'>




    
![png](output_36_1.png)
    


### Feature Split

Especially valid for categorical data. After data exploration, you may observe that there is some structure in your data that you may want to exploit. One way to do so would be to split the data using some heuristics.

For example, for the titanic dataset, the *cabin* column has values such as B22, C12, A4, which indicates that it's probably worth to split into two (e.g. B+22, C+12, A+4) for a better representation of a spatial distribution of cabins. With hotel rooms, it may be similar (e.g. room 112 into 1(floor)+12(room number)).


```python
data.cabin.head()
```




    0         B5
    1    C22 C26
    2    C22 C26
    3    C22 C26
    4    C22 C26
    Name: cabin, dtype: object




```python
# Cabin has a few NAS, for simplicity of exposition, lets replace them with a clear NA string

data.cabin.fillna('0000000', inplace=True)

# Split cabin in two
data['cabin1'] = [x.split()[0][0] for x in data.cabin]
data['cabin2'] = [x.split()[0][1:] for x in data.cabin]
data[['cabin','cabin1','cabin2']].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cabin</th>
      <th>cabin1</th>
      <th>cabin2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>B5</td>
      <td>B</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>C22 C26</td>
      <td>C</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C22 C26</td>
      <td>C</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C22 C26</td>
      <td>C</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>C22 C26</td>
      <td>C</td>
      <td>22</td>
    </tr>
    <tr>
      <th>5</th>
      <td>E12</td>
      <td>E</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>D7</td>
      <td>D</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A36</td>
      <td>A</td>
      <td>36</td>
    </tr>
    <tr>
      <th>8</th>
      <td>C101</td>
      <td>C</td>
      <td>101</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0000000</td>
      <td>0</td>
      <td>000000</td>
    </tr>
  </tbody>
</table>
</div>



### Date-time category extraction

From a date input, it might be useful to extract features like *day of week*, *month*, *year*, etc.. These could be important if there is some seasonality related to those units of measure.  For example, dummies like *Holiday_period* or *Working_day* might be important.

### Combine Sparse Classes
When dealing with categories, you might test the frequency of certain classes, and group the less frequent classes, but making sure that their behavior is similar with respect to the target to predict. 


### Outlier detection

Training our model on data that has *strange* observations may be misleading.

That's why it might be important to remove or treat those observations from our training dataset. Also identifying those in the test set for prediction is important, because one should expect bad performance of our model on those.

Outliers in the target output are also interesting to isolate in a regression problem.

Here we point out some methods of outlier detection:
- One dimension:
    - **Standard Deviation method**: Values beyond $|\bar{x}+k\sigma|$, for some $k>0$ (typically, $k=3$ could be a choice). 
    - **Interquartile Range method**: Values below $Q_1-1.5IQR$ or above $Q_3+1.5IQR$, which is the criteria used for boxplots. $Q_1$ and $Q_3$ are quartiles, $IQR$ the interquartile range.
- Multiple dimensions:
    - [**Mahalanobis distance** with Minimum Covariance Determinant estimator (MCD)](https://scikit-learn.org/stable/auto_examples/covariance/plot_mahalanobis_distances.html). Just for numerical features, and better if they have been normalized.
    - [**Isolation forests**](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html), a tree-based anomaly detection algorithm. Main idea: do random cuts in the dataset until we isolate observations. When a forest of random trees collectively produce shorter path lengths for particular samples, they are highly likely to be anomalies.
    - [**Local Outlier Factor**](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html). Main idea is to locate those examples that are far from the other examples in the feature space, using certain measure of distance, following nearest neighbor general idea. 
    - [**One-Class SVM**](https://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html). It turns out that we can frame outlier detection as a classification problem with only one class using SVM. Depending on the kernel, we find a non-linear boundary for outliers. Find an [example here](https://scikit-learn.org/stable/auto_examples/applications/plot_outlier_detection_wine.html#sphx-glr-auto-examples-applications-plot-outlier-detection-wine-py).
    
Check this [complete example](https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_anomaly_comparison.html#sphx-glr-auto-examples-miscellaneous-plot-anomaly-comparison-py) for comparing methods for a particular use case.

As an alternative to $sklearn$, you can check the Python Outlier Detection package [$PyOD$](https://github.com/yzhao062/pyod), that includes many more methods including neural networks and autoencoders. 

### Caution: 
Outliers should not *always* be thrown out. For instance, for economic time series, should we "trim" out the covid-19 crisis episode?

## Feature generation

### Create interaction features
Linearity is a convenient modeling assumption, but it may be restrictive. It then becomes useful to consider interaction between features (or feature transformations) to capture relevant nonlinearities. We will see more about this in a bit.

### Numerical features

We may, for instance, consider polynomials of some inputs by raising them to a certain power. We may also consider cross-products of inputs (e.g. convert units sold to revenue by multiplying by prices), sums (e.g. if they are two subcategories), differences (e.g. difference in date) or ratios (e.g. students per professor) between two input features. 

If you don't scale your data, you can also create *ratios* or *differences* of features versus an average value.

In terms of out-of-the box functions, to generate polynomial features you can use [`PolynomialFeatures`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PolynomialFeatures.html), find an [example here](https://machinelearningmastery.com/polynomial-features-transforms-for-machine-learning/).
Here you have another [example](https://towardsdatascience.com/feature-engineering-combination-polynomial-features-3caa4c77a755) that explores just for a linear model combinations of features using `itertools.combinations` to generate them. 

Alternatively, we may prefer methods that explicitly incorporates nonlinearties, such as *decision trees* or *neural networks*. 

### Adding domain knowledge
This simple step basically consists of adding as features any relevant information that you may have about your data. Generally speaking, one can easily include some information as a dummy or category.

Easiest example is adding 'events' to a time series, such as sovereign debt crisis, housing debt crisis etc. This helps the model to treat a subset of data in a different way. 

### Adding External data
Not having a good understanding of the context of your prediction problem may lead you to miss some relevant factors that affect your outcome. This is a hint for you to complement your input data with external information that might be relevant.

You may probably scrape data on internet to find this external data.

Here some examples:
+ Context Economic indicators (GDP, average salary, etc.), political indicators, social indicators, etc.
+ Time series evolution/pattern of similar entities.
+ Geographical info: From city, region info, obtain average position (UTM coordinates, latitude, longitude), so that you can identify neighbor observations (even add average features of close observations). The inverse also holds: from latitude/longitude or coordinates, compute not only features of neighbor observations, but also categories such as country, city, region, etc.

## Error Analysis
After one modeling round, this consists of analyzing your errors and trying to find patterns related to your inputs. For instance, you may infer that your performance is poor for a particular city, or social class, or a combination of those. This would give you some hints to improve your feature engineering. You might do some clustering on you *'bad prediction'* dataset, to help you detect patterns.

You can also check observations with huge errors (especially for regression problems), which may lead you to new data preprocessing (e.g. outlier management), or to a new feature engineering step.

## Hints for practitioners

### Quick imputation

For a quick fix, one can use $sklearn.impute.SimpleImputer$ function, that has following parameters:

+ *missing_values* : Which value has to be considered as a missing value.
+ *strategy* : The imputation strategy ('mean', 'median', 'most_frequent', 'constant'), by column of missing data
+ *fill_value* : Assign string or numerical value, optional (default=None)
+ *add_indicator* : boolean. Creates a boolean feature (column) per column with missing data 

### Missing indicator: Don't lose information

Add boolean columns to keep track of data that was originally missing.

``` python
dataframe['is_null_Column_Name'] = dataframe.Column_Name.isnull().astype(int)
```
As an alternative, use $sklearn.impute.MissingIndicator$.


### Time series imputation
Filling gaps in time series is a particular case of handling missing data. 
Time series vary by nature, but there are a few common patterns.

Often, there are missing values in the _beginning_ of the dataframe. A general solution here is to start analysis at a later date, when this is possible.

Alternatively, there may be missing values in the _end_ of the dataframe due to some series being updated faster than others. A general solution here is to cut-off the last, say, couple of observations.

More problematic cases happen when we have missing data in the _middle_ of the dataframe. If the missing data is randomly missing, a suggestion is to simply use the mean of the column. **However**, if the missing data is due to a particular event (say a stock market's circuit breakers), then the mean may be an overly optimistic value for the missing value, and care must be taken.

In addition, interpolation may be used for time series as well.

## Making pipelines
$Sklearn$ library *pipeline* allows sequencing your tasks and quickly build a pipeline to transform you data and apply any model.

```python
from sklearn.pipeline import make_pipeline

model = make_pipeline(Imputer(strategy='mean'),
                      PolynomialFeatures(degree=2),
                      LinearRegression())
model.fit(X, y)
```

## *ColumnTransformer*

sklearn $ColumnTransformer$ allows to generate different pipelines for different columns. Very useful when splitting the preprocessing for categorical and numerical columns, for instance.

Check [this example](https://scikit-learn.org/stable/auto_examples/compose/plot_column_transformer_mixed_types.html#sphx-glr-auto-examples-compose-plot-column-transformer-mixed-types-py) from sklearn documention for a joint use of ColumnTransformer and Pipeline.



```python
# Example of missing indicator generation

from sklearn.impute import MissingIndicator
indicator = MissingIndicator(features='missing-only')

indicator.fit(X)
print(X.isnull().sum())

X2 = pd.DataFrame(indicator.transform(X))
nulls = X.isnull().sum()
X2.columns = X.columns[nulls > 0]
X2.sum()


```

    pclass       0
    sex          0
    age        263
    sibsp        0
    parch        0
    fare         1
    cabin     1014
    dtype: int64
    




    age       263
    fare        1
    cabin    1014
    dtype: int64



### References

Casari, A., Zheng, A., 2018. *Feature Engineering for Machine Learning*. O'Reilly Media.

Kuhn, M., Johnson, K. . 2019. [*Feature Engineering and Selection: A Practical Approach for Predictive Models*](http://www.feat.engineering/)

Skit-Learn documentation. [*Feature extraction*](https://scikit-learn.org/stable/modules/feature_extraction.html)

Scikit-learn documentation. *Imputing missing values with variants of IterativeImputer*.  https://scikit-learn.org/stable/auto_examples/impute/plot_iterative_imputer_variants_comparison.html


