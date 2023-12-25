---
title: "Pandas - DataFrame의 각 columns 분석을 위한 유틸리티 함수"
date: 2023-12-25T17:00:00+09:00
categories: [ pandas ]
tags:
- pandas
---

DataFrame의 각 열이 어떤 데이터들을 가지고 있는지 간편하게 살펴보자.
<!--more-->

{{< image src="/images/logo/pandas.svg" caption="pandas 로고" width=50% linked=false >}}


## 1. 핵심 요약
- 아래의 `inspect_columns()` 함수로 각 열의 데이터를 확인할 수 있음

```python
def inspect_columns(df):
    df_describe = df.describe()
    df_result = pd.DataFrame(
        data={
            'dtype': df.dtypes,
            'unique': (df.nunique() == len(df)) + 0,  # 0: False, 1: True
            'nunique': df.nunique().map('{:,d}'.format),
            'null_count': df.isna().sum().map('{:,d}'.format),
            'null_pct': ((df.isnull().sum() / len(df)) * 100).map('{:,.2f}'.format),
            '|': '▮',
            'min': df_describe.loc['min'].map('{:,.2f}'.format),
            'mean': df_describe.loc['mean'].map('{:,.2f}'.format),
            'median': df_describe.loc['50%'].map('{:,.2f}'.format),
            'max': df_describe.loc['max'].map('{:,.2f}'.format),
            'std': df_describe.loc['std'].map('{:,.2f}'.format),
            '||': '▮',
            '1st_row': df.iloc[0],
            'random_row': df.iloc[np.random.randint(low=0, high=len(df))],
            'last_row': df.iloc[-1],
        },
        index=df.columns,
    )

    return df_result
```


<br/>

---

## 2. 사용 예시

```python
import numpy as np
import pandas as pd
import seaborn as sns
```

<br/>

### 2.1. 유틸리티 함수

```python
def inspect_columns(df):
    df_describe = df.describe()
    df_result = pd.DataFrame(
        data={
            'dtype': df.dtypes,
            'unique': (df.nunique() == len(df)) + 0,  # 0: False, 1: True
            'nunique': df.nunique().map('{:,d}'.format),
            'null_count': df.isna().sum().map('{:,d}'.format),
            'null_pct': ((df.isnull().sum() / len(df)) * 100).map('{:,.2f}'.format),
            '|': '▮',
            'min': df_describe.loc['min'].map('{:,.2f}'.format),
            'mean': df_describe.loc['mean'].map('{:,.2f}'.format),
            'median': df_describe.loc['50%'].map('{:,.2f}'.format),
            'max': df_describe.loc['max'].map('{:,.2f}'.format),
            'std': df_describe.loc['std'].map('{:,.2f}'.format),
            '||': '▮',
            '1st_row': df.iloc[0],
            'random_row': df.iloc[np.random.randint(low=0, high=len(df))],
            'last_row': df.iloc[-1],
        },
        index=df.columns,
    )

    return df_result
```


<br/>

### 2.2. titanic


```python
df_titanic = sns.load_dataset('titanic')
print(df_titanic.shape)
df_titanic.head()
```

    (891, 15)
    


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
      <th>survived</th>
      <th>pclass</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>fare</th>
      <th>embarked</th>
      <th>class</th>
      <th>who</th>
      <th>adult_male</th>
      <th>deck</th>
      <th>embark_town</th>
      <th>alive</th>
      <th>alone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Cherbourg</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
      <td>Third</td>
      <td>woman</td>
      <td>False</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>53.1000</td>
      <td>S</td>
      <td>First</td>
      <td>woman</td>
      <td>False</td>
      <td>C</td>
      <td>Southampton</td>
      <td>yes</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
      <td>Third</td>
      <td>man</td>
      <td>True</td>
      <td>NaN</td>
      <td>Southampton</td>
      <td>no</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>


<br/>
<br/>


```python
inspect_columns(df_titanic)
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
      <th>dtype</th>
      <th>unique</th>
      <th>nunique</th>
      <th>null_count</th>
      <th>null_pct</th>
      <th>|</th>
      <th>min</th>
      <th>mean</th>
      <th>median</th>
      <th>max</th>
      <th>std</th>
      <th>||</th>
      <th>1st_row</th>
      <th>random_row</th>
      <th>last_row</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>survived</th>
      <td>int64</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>0.38</td>
      <td>0.00</td>
      <td>1.00</td>
      <td>0.49</td>
      <td>▮</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pclass</th>
      <td>int64</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>1.00</td>
      <td>2.31</td>
      <td>3.00</td>
      <td>3.00</td>
      <td>0.84</td>
      <td>▮</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>sex</th>
      <td>object</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>male</td>
      <td>male</td>
      <td>male</td>
    </tr>
    <tr>
      <th>age</th>
      <td>float64</td>
      <td>0</td>
      <td>88</td>
      <td>177</td>
      <td>19.87</td>
      <td>▮</td>
      <td>0.42</td>
      <td>29.70</td>
      <td>28.00</td>
      <td>80.00</td>
      <td>14.53</td>
      <td>▮</td>
      <td>22.0</td>
      <td>27.0</td>
      <td>32.0</td>
    </tr>
    <tr>
      <th>sibsp</th>
      <td>int64</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>0.52</td>
      <td>0.00</td>
      <td>8.00</td>
      <td>1.10</td>
      <td>▮</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>parch</th>
      <td>int64</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>0.38</td>
      <td>0.00</td>
      <td>6.00</td>
      <td>0.81</td>
      <td>▮</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>fare</th>
      <td>float64</td>
      <td>0</td>
      <td>248</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>32.20</td>
      <td>14.45</td>
      <td>512.33</td>
      <td>49.69</td>
      <td>▮</td>
      <td>7.25</td>
      <td>7.8958</td>
      <td>7.75</td>
    </tr>
    <tr>
      <th>embarked</th>
      <td>object</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>0.22</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>S</td>
      <td>S</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>class</th>
      <td>category</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>Third</td>
      <td>Third</td>
      <td>Third</td>
    </tr>
    <tr>
      <th>who</th>
      <td>object</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>man</td>
      <td>man</td>
      <td>man</td>
    </tr>
    <tr>
      <th>adult_male</th>
      <td>bool</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>True</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>deck</th>
      <td>category</td>
      <td>0</td>
      <td>7</td>
      <td>688</td>
      <td>77.22</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>embark_town</th>
      <td>object</td>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>0.22</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>Southampton</td>
      <td>Southampton</td>
      <td>Queenstown</td>
    </tr>
    <tr>
      <th>alive</th>
      <td>object</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
    </tr>
    <tr>
      <th>alone</th>
      <td>bool</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>


<br/>


###  2.3. iris


```python
df_iris = sns.load_dataset('iris')
print(df_iris.shape)
df_iris.head()
```

    (150, 5)
    

<br/>


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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>


<br/>
<br/>


```python
inspect_columns(df_iris)
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
      <th>dtype</th>
      <th>unique</th>
      <th>nunique</th>
      <th>null_count</th>
      <th>null_pct</th>
      <th>|</th>
      <th>min</th>
      <th>mean</th>
      <th>median</th>
      <th>max</th>
      <th>std</th>
      <th>||</th>
      <th>1st_row</th>
      <th>random_row</th>
      <th>last_row</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>sepal_length</th>
      <td>float64</td>
      <td>0</td>
      <td>35</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>4.30</td>
      <td>5.84</td>
      <td>5.80</td>
      <td>7.90</td>
      <td>0.83</td>
      <td>▮</td>
      <td>5.1</td>
      <td>6.8</td>
      <td>5.9</td>
    </tr>
    <tr>
      <th>sepal_width</th>
      <td>float64</td>
      <td>0</td>
      <td>23</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>2.00</td>
      <td>3.06</td>
      <td>3.00</td>
      <td>4.40</td>
      <td>0.44</td>
      <td>▮</td>
      <td>3.5</td>
      <td>3.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>petal_length</th>
      <td>float64</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>1.00</td>
      <td>3.76</td>
      <td>4.35</td>
      <td>6.90</td>
      <td>1.77</td>
      <td>▮</td>
      <td>1.4</td>
      <td>5.5</td>
      <td>5.1</td>
    </tr>
    <tr>
      <th>petal_width</th>
      <td>float64</td>
      <td>0</td>
      <td>22</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.10</td>
      <td>1.20</td>
      <td>1.30</td>
      <td>2.50</td>
      <td>0.76</td>
      <td>▮</td>
      <td>0.2</td>
      <td>2.1</td>
      <td>1.8</td>
    </tr>
    <tr>
      <th>species</th>
      <td>object</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>setosa</td>
      <td>virginica</td>
      <td>virginica</td>
    </tr>
  </tbody>
</table>
</div>


<br/>

###  2.4. diamonds


```python
df_diamonds = sns.load_dataset('diamonds')
print(df_diamonds.shape)
df_diamonds.head()
```

    (53940, 10)
    

<br/>


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
      <th>carat</th>
      <th>cut</th>
      <th>color</th>
      <th>clarity</th>
      <th>depth</th>
      <th>table</th>
      <th>price</th>
      <th>x</th>
      <th>y</th>
      <th>z</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.23</td>
      <td>Ideal</td>
      <td>E</td>
      <td>SI2</td>
      <td>61.5</td>
      <td>55.0</td>
      <td>326</td>
      <td>3.95</td>
      <td>3.98</td>
      <td>2.43</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.21</td>
      <td>Premium</td>
      <td>E</td>
      <td>SI1</td>
      <td>59.8</td>
      <td>61.0</td>
      <td>326</td>
      <td>3.89</td>
      <td>3.84</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.23</td>
      <td>Good</td>
      <td>E</td>
      <td>VS1</td>
      <td>56.9</td>
      <td>65.0</td>
      <td>327</td>
      <td>4.05</td>
      <td>4.07</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.29</td>
      <td>Premium</td>
      <td>I</td>
      <td>VS2</td>
      <td>62.4</td>
      <td>58.0</td>
      <td>334</td>
      <td>4.20</td>
      <td>4.23</td>
      <td>2.63</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.31</td>
      <td>Good</td>
      <td>J</td>
      <td>SI2</td>
      <td>63.3</td>
      <td>58.0</td>
      <td>335</td>
      <td>4.34</td>
      <td>4.35</td>
      <td>2.75</td>
    </tr>
  </tbody>
</table>
</div>


<br/>
<br/>


```python
inspect_columns(df_diamonds)
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
      <th>dtype</th>
      <th>unique</th>
      <th>nunique</th>
      <th>null_count</th>
      <th>null_pct</th>
      <th>|</th>
      <th>min</th>
      <th>mean</th>
      <th>median</th>
      <th>max</th>
      <th>std</th>
      <th>||</th>
      <th>1st_row</th>
      <th>random_row</th>
      <th>last_row</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>carat</th>
      <td>float64</td>
      <td>0</td>
      <td>273</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.20</td>
      <td>0.80</td>
      <td>0.70</td>
      <td>5.01</td>
      <td>0.47</td>
      <td>▮</td>
      <td>0.23</td>
      <td>0.71</td>
      <td>0.75</td>
    </tr>
    <tr>
      <th>cut</th>
      <td>category</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>Ideal</td>
      <td>Premium</td>
      <td>Ideal</td>
    </tr>
    <tr>
      <th>color</th>
      <td>category</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>E</td>
      <td>I</td>
      <td>D</td>
    </tr>
    <tr>
      <th>clarity</th>
      <td>category</td>
      <td>0</td>
      <td>8</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>▮</td>
      <td>SI2</td>
      <td>VS2</td>
      <td>SI2</td>
    </tr>
    <tr>
      <th>depth</th>
      <td>float64</td>
      <td>0</td>
      <td>184</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>43.00</td>
      <td>61.75</td>
      <td>61.80</td>
      <td>79.00</td>
      <td>1.43</td>
      <td>▮</td>
      <td>61.5</td>
      <td>59.3</td>
      <td>62.2</td>
    </tr>
    <tr>
      <th>table</th>
      <td>float64</td>
      <td>0</td>
      <td>127</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>43.00</td>
      <td>57.46</td>
      <td>57.00</td>
      <td>95.00</td>
      <td>2.23</td>
      <td>▮</td>
      <td>55.0</td>
      <td>59.0</td>
      <td>55.0</td>
    </tr>
    <tr>
      <th>price</th>
      <td>int64</td>
      <td>0</td>
      <td>11,602</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>326.00</td>
      <td>3,932.80</td>
      <td>2,401.00</td>
      <td>18,823.00</td>
      <td>3,989.44</td>
      <td>▮</td>
      <td>326</td>
      <td>2300</td>
      <td>2757</td>
    </tr>
    <tr>
      <th>x</th>
      <td>float64</td>
      <td>0</td>
      <td>554</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>5.73</td>
      <td>5.70</td>
      <td>10.74</td>
      <td>1.12</td>
      <td>▮</td>
      <td>3.95</td>
      <td>5.89</td>
      <td>5.83</td>
    </tr>
    <tr>
      <th>y</th>
      <td>float64</td>
      <td>0</td>
      <td>552</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>5.73</td>
      <td>5.71</td>
      <td>58.90</td>
      <td>1.14</td>
      <td>▮</td>
      <td>3.98</td>
      <td>5.81</td>
      <td>5.87</td>
    </tr>
    <tr>
      <th>z</th>
      <td>float64</td>
      <td>0</td>
      <td>375</td>
      <td>0</td>
      <td>0.00</td>
      <td>▮</td>
      <td>0.00</td>
      <td>3.54</td>
      <td>3.53</td>
      <td>31.80</td>
      <td>0.71</td>
      <td>▮</td>
      <td>2.43</td>
      <td>3.47</td>
      <td>3.64</td>
    </tr>
  </tbody>
</table>
</div>

<br/>

---

## 99. Reference
- [{Kaggle Notebook} Inspect a DataFrame](https://www.kaggle.com/sunghoshim/inspect-a-dataframe)
- [{Kaggle Notebook} Explain the Data📝 | LightGBM Baseline🚀](https://www.kaggle.com/code/a27182818/explain-the-data-lightgbm-baseline#1.-Explore-&-Explain-the-Data)


<br/>

---
