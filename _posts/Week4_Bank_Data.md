
# Week 4 Bank Data 
Chris Richards, Regis University  
Data Collection and Preparation, Spring 2019  
  
This project takes the Bank Marketing data set from the UCI ML Archive and conducts some clean up of the data and column headers.  
Several headers contain underscores and periods which will be removed, as well as series values with similar issues.  In many columns missing data is labeled as "unknown".  In these cases, the series data types are "category" and "unknown" as a value is appropriate.  These values will remain as is.  

One column, pdays, records number of days and is an integer data type.  The original researchers coded missing data as 999.  This is problematic for performing calculations and will be replaced with NaN.


```python
import pandas as pd, numpy as np
```


```python
#Can't seem to access a datafile in the same directory as the notebook.
# Using a magic command to change the working directory to the current dir
%cd "C:\Users\cr117\Documents\Regis\Data_Collection\Week4\"
```

    C:\Users\cr117\Documents\Regis\Data_Collection\Week4
    


```python
df = pd.read_csv("bank-additional.csv", delimiter=';')
```


```python
df.head()
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
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>30</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>may</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.8</td>
      <td>92.893</td>
      <td>-46.2</td>
      <td>1.313</td>
      <td>5099.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>39</td>
      <td>services</td>
      <td>single</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>fri</td>
      <td>...</td>
      <td>4</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.855</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>jun</td>
      <td>wed</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>94.465</td>
      <td>-41.8</td>
      <td>4.962</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38</td>
      <td>services</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>no</td>
      <td>unknown</td>
      <td>unknown</td>
      <td>telephone</td>
      <td>jun</td>
      <td>fri</td>
      <td>...</td>
      <td>3</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>94.465</td>
      <td>-41.8</td>
      <td>4.959</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>47</td>
      <td>admin.</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.191</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
df.shape
```




    (4119, 21)




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4119 entries, 0 to 4118
    Data columns (total 21 columns):
    age               4119 non-null int64
    job               4119 non-null object
    marital           4119 non-null object
    education         4119 non-null object
    default           4119 non-null object
    housing           4119 non-null object
    loan              4119 non-null object
    contact           4119 non-null object
    month             4119 non-null object
    day_of_week       4119 non-null object
    duration          4119 non-null int64
    campaign          4119 non-null int64
    pdays             4119 non-null int64
    previous          4119 non-null int64
    poutcome          4119 non-null object
    emp.var.rate      4119 non-null float64
    cons.price.idx    4119 non-null float64
    cons.conf.idx     4119 non-null float64
    euribor3m         4119 non-null float64
    nr.employed       4119 non-null float64
    y                 4119 non-null object
    dtypes: float64(5), int64(5), object(11)
    memory usage: 675.9+ KB
    


```python
# make copy
dfc = df.copy()
```


```python
dfc.head()
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
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>30</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>may</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.8</td>
      <td>92.893</td>
      <td>-46.2</td>
      <td>1.313</td>
      <td>5099.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>39</td>
      <td>services</td>
      <td>single</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>fri</td>
      <td>...</td>
      <td>4</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.855</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>jun</td>
      <td>wed</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>94.465</td>
      <td>-41.8</td>
      <td>4.962</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38</td>
      <td>services</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>no</td>
      <td>unknown</td>
      <td>unknown</td>
      <td>telephone</td>
      <td>jun</td>
      <td>fri</td>
      <td>...</td>
      <td>3</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>94.465</td>
      <td>-41.8</td>
      <td>4.959</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>47</td>
      <td>admin.</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.191</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
dfc.tail()
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
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4114</th>
      <td>30</td>
      <td>admin.</td>
      <td>married</td>
      <td>basic.6y</td>
      <td>no</td>
      <td>yes</td>
      <td>yes</td>
      <td>cellular</td>
      <td>jul</td>
      <td>thu</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>93.918</td>
      <td>-42.7</td>
      <td>4.958</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4115</th>
      <td>39</td>
      <td>admin.</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>jul</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>93.918</td>
      <td>-42.7</td>
      <td>4.959</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4116</th>
      <td>27</td>
      <td>student</td>
      <td>single</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>2</td>
      <td>999</td>
      <td>1</td>
      <td>failure</td>
      <td>-1.8</td>
      <td>92.893</td>
      <td>-46.2</td>
      <td>1.354</td>
      <td>5099.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4117</th>
      <td>58</td>
      <td>admin.</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>aug</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>93.444</td>
      <td>-36.1</td>
      <td>4.966</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4118</th>
      <td>34</td>
      <td>management</td>
      <td>single</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>wed</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.120</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
dfc.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4119 entries, 0 to 4118
    Data columns (total 21 columns):
    age               4119 non-null int64
    job               4119 non-null object
    marital           4119 non-null object
    education         4119 non-null object
    default           4119 non-null object
    housing           4119 non-null object
    loan              4119 non-null object
    contact           4119 non-null object
    month             4119 non-null object
    day_of_week       4119 non-null object
    duration          4119 non-null int64
    campaign          4119 non-null int64
    pdays             4119 non-null int64
    previous          4119 non-null int64
    poutcome          4119 non-null object
    emp.var.rate      4119 non-null float64
    cons.price.idx    4119 non-null float64
    cons.conf.idx     4119 non-null float64
    euribor3m         4119 non-null float64
    nr.employed       4119 non-null float64
    y                 4119 non-null object
    dtypes: float64(5), int64(5), object(11)
    memory usage: 675.9+ KB
    

##### Cleaning up punctuation in column headers.  
Some of the column names have underscores or periods.  I'll replace those with a space.


```python
# remove periods from series 'job' and 'education'
dfc[['job', 'education']] = dfc[['job', 'education']].replace("\.", " ", regex=True)

```


```python
dfc['education'].head(10)
```




    0               basic 9y
    1            high school
    2            high school
    3               basic 9y
    4      university degree
    5      university degree
    6      university degree
    7      university degree
    8    professional course
    9               basic 9y
    Name: education, dtype: object



##### Change selected series to "category" type


```python
cols = ['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact', 'month', 'day_of_week', 'poutcome', 'y']
for col in cols:  
    dfc[col] = dfc[col].astype('category')
```


```python
# check the conversion
dfc.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4119 entries, 0 to 4118
    Data columns (total 21 columns):
    age               4119 non-null int64
    job               4119 non-null category
    marital           4119 non-null category
    education         4119 non-null category
    default           4119 non-null category
    housing           4119 non-null category
    loan              4119 non-null category
    contact           4119 non-null category
    month             4119 non-null category
    day_of_week       4119 non-null category
    duration          4119 non-null int64
    campaign          4119 non-null int64
    pdays             4119 non-null int64
    previous          4119 non-null int64
    poutcome          4119 non-null category
    emp.var.rate      4119 non-null float64
    cons.price.idx    4119 non-null float64
    cons.conf.idx     4119 non-null float64
    euribor3m         4119 non-null float64
    nr.employed       4119 non-null float64
    y                 4119 non-null category
    dtypes: category(11), float64(5), int64(5)
    memory usage: 368.3 KB
    

##### Replace "999" pdays series
According to the data definition document, "999" is used in the "pdays" series to indicate that a client was not previously contacted.  The "pdays" series records the number of days since the client's last contact.  Using a numeric value of 999 is misleading.  This will cause issues with the accuracy of any numeric calculations performed on this series.  I'll convert the "999"s to NaN to prevent them from interfering.


```python
# Check number of 999s in pdays
# Looking at the data file it seems there's a large number of 999 values.
nines = pd.value_counts(dfc['pdays'])
nines
```




    999    3959
    3        52
    6        42
    4        14
    7        10
    10        8
    12        5
    5         4
    2         4
    9         3
    1         3
    13        2
    18        2
    16        2
    15        2
    0         2
    14        1
    19        1
    21        1
    17        1
    11        1
    Name: pdays, dtype: int64




```python
dfc['pdays'].replace(999, np.NaN, inplace=True)
dfc["pdays"].tail(5)
```




    4114   NaN
    4115   NaN
    4116   NaN
    4117   NaN
    4118   NaN
    Name: pdays, dtype: float64



Checking the conversion it appears that the values have been converted to floats by design  
Next, I'll check the count of values in the series.


```python
nines = pd.value_counts(dfc['pdays'])
nines
```




    3.0     52
    6.0     42
    4.0     14
    7.0     10
    10.0     8
    12.0     5
    5.0      4
    2.0      4
    1.0      3
    9.0      3
    18.0     2
    15.0     2
    0.0      2
    16.0     2
    13.0     2
    17.0     1
    11.0     1
    14.0     1
    19.0     1
    21.0     1
    Name: pdays, dtype: int64



No 999s are found but as an extra check, I'll count the NaNs.  

This should return 3959 if correct.  That value was shown as the result of counting the number of 999s in the pdays series. 


```python
dfc['pdays'].isnull().sum()
```




    3959



##### Convert month series 
In the month series, month names and abbreviations are used.  I'll convert them to the month's corresponding numbers.

##### Count the month values used


```python
dfc['month'].unique()
```




    [may, jun, nov, sep, jul, aug, mar, oct, apr, dec]
    Categories (10, object): [may, jun, nov, sep, ..., mar, oct, apr, dec]



##### Create dictionary for month number


```python
m = {
    "jan": '1', 
    "feb": '2', 
    "mar": '3',
    "apr": '4', 
    "may": '5', 
    "jun": '6', 
    "jul": '7', 
    "aug": '8', 
    "sep": '9', 
    "oct": '10', 
    "nov": '11', 
    "dec": '12', 
}
```


```python
dfc['month'].replace(m, inplace=True)
dfc['month'].head(10)
```




    0     5
    1     5
    2     6
    3     6
    4    11
    5     9
    6     9
    7    11
    8    11
    9     5
    Name: month, dtype: object



##### Count the unique values in the month series.


```python
dfc['month'].unique()
```




    array(['5', '6', '11', '9', '7', '8', '3', '10', '4', '12'], dtype=object)



#### Rename columns
Some of the column headers in this data set are unclear.  I'll rename them to something more descriptive.  


```python
dfc.rename(columns={'job': 'job type', 'marital': 'marital status', 
                    'housing': 'housing loan', 'loan': 'personl loan', 
                    'contact': 'contact type', 'month': 'month of last contact',                      
                    'day_of_week': 'day of last contact', 'duration': 'duration of last contact',
                    'campaign': 'number of contacts', 'pdays': 'number of days from last campaign', 
                    'previous': 'number of previous contacts', 'poutcome': 'previous outcome',  
                    'emp.var.rate': 'employment variation rate', 
                    'cons.price.idx':'consumer price index', 'cons.conf.idx':'consumer confidence index', 
                    'euribor3m': 'euribor 3 month rate', 'nr.employed':"number of employees",
                    "y":"subscribed"}, inplace=True) 
```


```python
dfc.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4119 entries, 0 to 4118
    Data columns (total 21 columns):
    age                                  4119 non-null int64
    job type                             4119 non-null category
    marital status                       4119 non-null category
    education                            4119 non-null category
    default                              4119 non-null category
    housing loan                         4119 non-null category
    personl loan                         4119 non-null category
    contact type                         4119 non-null category
    month of last contact                4119 non-null object
    day of last contact                  4119 non-null category
    duration of last contact             4119 non-null int64
    number of contacts                   4119 non-null int64
    number of days from last campaign    160 non-null float64
    number of previous contacts          4119 non-null int64
    previous outcome                     4119 non-null category
    employment variation rate            4119 non-null float64
    consumer price index                 4119 non-null float64
    consumer confidence index            4119 non-null float64
    euribor 3 month rate                 4119 non-null float64
    number of employees                  4119 non-null float64
    subscribed                           4119 non-null category
    dtypes: category(10), float64(6), int64(4), object(1)
    memory usage: 396.0+ KB
    


```python
dfc.head(10)
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
      <th>job type</th>
      <th>marital status</th>
      <th>education</th>
      <th>default</th>
      <th>housing loan</th>
      <th>personl loan</th>
      <th>contact type</th>
      <th>month of last contact</th>
      <th>day of last contact</th>
      <th>...</th>
      <th>number of contacts</th>
      <th>number of days from last campaign</th>
      <th>number of previous contacts</th>
      <th>previous outcome</th>
      <th>employment variation rate</th>
      <th>consumer price index</th>
      <th>consumer confidence index</th>
      <th>euribor 3 month rate</th>
      <th>number of employees</th>
      <th>subscribed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>30</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>basic 9y</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>5</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.8</td>
      <td>92.893</td>
      <td>-46.2</td>
      <td>1.313</td>
      <td>5099.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>39</td>
      <td>services</td>
      <td>single</td>
      <td>high school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>5</td>
      <td>fri</td>
      <td>...</td>
      <td>4</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.855</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25</td>
      <td>services</td>
      <td>married</td>
      <td>high school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>6</td>
      <td>wed</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>94.465</td>
      <td>-41.8</td>
      <td>4.962</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>38</td>
      <td>services</td>
      <td>married</td>
      <td>basic 9y</td>
      <td>no</td>
      <td>unknown</td>
      <td>unknown</td>
      <td>telephone</td>
      <td>6</td>
      <td>fri</td>
      <td>...</td>
      <td>3</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>94.465</td>
      <td>-41.8</td>
      <td>4.959</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>47</td>
      <td>admin</td>
      <td>married</td>
      <td>university degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>11</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.191</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
    <tr>
      <th>5</th>
      <td>32</td>
      <td>services</td>
      <td>single</td>
      <td>university degree</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>9</td>
      <td>thu</td>
      <td>...</td>
      <td>3</td>
      <td>NaN</td>
      <td>2</td>
      <td>failure</td>
      <td>-1.1</td>
      <td>94.199</td>
      <td>-37.5</td>
      <td>0.884</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>6</th>
      <td>32</td>
      <td>admin</td>
      <td>single</td>
      <td>university degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>9</td>
      <td>mon</td>
      <td>...</td>
      <td>4</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.199</td>
      <td>-37.5</td>
      <td>0.879</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>7</th>
      <td>41</td>
      <td>entrepreneur</td>
      <td>married</td>
      <td>university degree</td>
      <td>unknown</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>11</td>
      <td>mon</td>
      <td>...</td>
      <td>2</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.191</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
    <tr>
      <th>8</th>
      <td>31</td>
      <td>services</td>
      <td>divorced</td>
      <td>professional course</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>11</td>
      <td>tue</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
      <td>failure</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.153</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
    <tr>
      <th>9</th>
      <td>35</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>basic 9y</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>5</td>
      <td>thu</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.855</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 21 columns</p>
</div>




```python
dfc.tail(10)
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
      <th>job type</th>
      <th>marital status</th>
      <th>education</th>
      <th>default</th>
      <th>housing loan</th>
      <th>personl loan</th>
      <th>contact type</th>
      <th>month of last contact</th>
      <th>day of last contact</th>
      <th>...</th>
      <th>number of contacts</th>
      <th>number of days from last campaign</th>
      <th>number of previous contacts</th>
      <th>previous outcome</th>
      <th>employment variation rate</th>
      <th>consumer price index</th>
      <th>consumer confidence index</th>
      <th>euribor 3 month rate</th>
      <th>number of employees</th>
      <th>subscribed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4109</th>
      <td>63</td>
      <td>retired</td>
      <td>married</td>
      <td>high school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>10</td>
      <td>wed</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-3.4</td>
      <td>92.431</td>
      <td>-26.9</td>
      <td>0.740</td>
      <td>5017.5</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4110</th>
      <td>53</td>
      <td>housemaid</td>
      <td>divorced</td>
      <td>basic 6y</td>
      <td>unknown</td>
      <td>unknown</td>
      <td>unknown</td>
      <td>telephone</td>
      <td>5</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.855</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4111</th>
      <td>30</td>
      <td>technician</td>
      <td>married</td>
      <td>university degree</td>
      <td>no</td>
      <td>no</td>
      <td>yes</td>
      <td>cellular</td>
      <td>6</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
      <td>failure</td>
      <td>-1.7</td>
      <td>94.055</td>
      <td>-39.8</td>
      <td>0.748</td>
      <td>4991.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4112</th>
      <td>31</td>
      <td>technician</td>
      <td>single</td>
      <td>professional course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>11</td>
      <td>thu</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.076</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4113</th>
      <td>31</td>
      <td>admin</td>
      <td>single</td>
      <td>university degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>11</td>
      <td>thu</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.076</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4114</th>
      <td>30</td>
      <td>admin</td>
      <td>married</td>
      <td>basic 6y</td>
      <td>no</td>
      <td>yes</td>
      <td>yes</td>
      <td>cellular</td>
      <td>7</td>
      <td>thu</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>93.918</td>
      <td>-42.7</td>
      <td>4.958</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4115</th>
      <td>39</td>
      <td>admin</td>
      <td>married</td>
      <td>high school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>7</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>93.918</td>
      <td>-42.7</td>
      <td>4.959</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4116</th>
      <td>27</td>
      <td>student</td>
      <td>single</td>
      <td>high school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>5</td>
      <td>mon</td>
      <td>...</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
      <td>failure</td>
      <td>-1.8</td>
      <td>92.893</td>
      <td>-46.2</td>
      <td>1.354</td>
      <td>5099.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4117</th>
      <td>58</td>
      <td>admin</td>
      <td>married</td>
      <td>high school</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>8</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.4</td>
      <td>93.444</td>
      <td>-36.1</td>
      <td>4.966</td>
      <td>5228.1</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4118</th>
      <td>34</td>
      <td>management</td>
      <td>single</td>
      <td>high school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>11</td>
      <td>wed</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-0.1</td>
      <td>93.200</td>
      <td>-42.0</td>
      <td>4.120</td>
      <td>5195.8</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 21 columns</p>
</div>


