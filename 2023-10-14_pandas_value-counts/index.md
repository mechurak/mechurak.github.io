# Pandas - value_counts(), 고유값의 빈도와 비율을 같이 표시하기


`value_counts()` 두 개를 엮어서 빈도와 비율을 같이 표시해보자.
<!--more-->

{{< image src="/images/logo/pandas.svg" caption="pandas 로고" width=50% linked=false >}}


## 1. 핵심 요약
- 아래와 같이 `value_counts()` 결과 두 개를 `join()`해서 예쁘게 표현할 수 있음


```python
# 개수랑 비율 같이. column명도 바꿔서 표시
df['embark_town'].value_counts(dropna=False).to_frame('count').join(
    df['embark_town'].value_counts(dropna=False, normalize=True).to_frame('normalize')
).round(4)
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
      <th>count</th>
      <th>normalize</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Southampton</th>
      <td>644</td>
      <td>0.7228</td>
    </tr>
    <tr>
      <th>Cherbourg</th>
      <td>168</td>
      <td>0.1886</td>
    </tr>
    <tr>
      <th>Queenstown</th>
      <td>77</td>
      <td>0.0864</td>
    </tr>
    <tr>
      <th>NaN</th>
      <td>2</td>
      <td>0.0022</td>
    </tr>
  </tbody>
</table>
</div>


<br/>

---

## 2. 사용 예시

먼저 titanic 데이터셋을 불러오자. (seaborn 패키지 이용)

```python
import pandas as pd
import seaborn as sns

# seaborn 패키지에서 titanic 데이터셋 불러와 사용
df = sns.load_dataset('titanic')
print(df.shape)
df.head()
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

**🔷 고유값들의 빈수 확인**

`Series`(여기서는 `df['embark_town']`)의 `value_count()`를 호출하면 고유값들의 빈도수를 표시해준다.  
단, 여기서 `NaN`은 따로 세지 않는다.



```python
# 고유값들의 개수 확인 (NaN은 세지 않음)
df['embark_town'].value_counts()
```




    Southampton    644
    Cherbourg      168
    Queenstown      77
    Name: embark_town, dtype: int64


<br/>
<br/>

**🔷 `NaN`의 개수도 같이 확인**

`NaN`의 개수도 같이 확인하려면 `dropna=False` 를 명시적으로 적어주어야 한다.

```python
# NaN의 개수도 같이 확인
df['embark_town'].value_counts(dropna=False)
```




    Southampton    644
    Cherbourg      168
    Queenstown      77
    NaN              2
    Name: embark_town, dtype: int64


<br/>
<br/>

**🔷 비율로 확인**

비율도 같이 보면 더 와 닿을 것 같다.  
`normalize=True` 를 주자.

```python
# 비율로 확인
df['embark_town'].value_counts(dropna=False, normalize=True)
```




    Southampton    0.722783
    Cherbourg      0.188552
    Queenstown     0.086420
    NaN            0.002245
    Name: embark_town, dtype: float64


<br/>
<br/>

**🔷 빈도와 비율 같이 표시**

빈도랑 비율 둘 다 한꺼번에 표시하고 싶다.  
아래와 같이 `agg()` 를 활용할 수 있다.

```python
# 개수랑 비율 같이 표시
df['embark_town'].agg([pd.Series.value_counts, lambda x: x.value_counts(normalize=True)])
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
      <th>value_counts</th>
      <th>&lt;lambda&gt;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Southampton</th>
      <td>644</td>
      <td>0.724409</td>
    </tr>
    <tr>
      <th>Cherbourg</th>
      <td>168</td>
      <td>0.188976</td>
    </tr>
    <tr>
      <th>Queenstown</th>
      <td>77</td>
      <td>0.086614</td>
    </tr>
  </tbody>
</table>
</div>

<br/>
<br/>

**🔷 빈도와 비율 같이 표시. column명도 변경**

그런데 `NaN`이 빠져있고, column명도 마음에 안든다.  
아래처럼 하면, column 이름까지 바꿔서 예쁘게 표시할 수 있다.


```python
# 개수랑 비율 같이 예쁘게 표시
df['embark_town'].value_counts(dropna=False).to_frame('count').join(
    df['embark_town'].value_counts(dropna=False, normalize=True).to_frame('normalize')
).round(4)
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
      <th>count</th>
      <th>normalize</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Southampton</th>
      <td>644</td>
      <td>0.7228</td>
    </tr>
    <tr>
      <th>Cherbourg</th>
      <td>168</td>
      <td>0.1886</td>
    </tr>
    <tr>
      <th>Queenstown</th>
      <td>77</td>
      <td>0.0864</td>
    </tr>
    <tr>
      <th>NaN</th>
      <td>2</td>
      <td>0.0022</td>
    </tr>
  </tbody>
</table>
</div>



<br/>

---

## Reference
- [{stackoverflow} pandas value_counts (show values and ratio)](https://stackoverflow.com/a/64326934)
- [{blog} Displaying both the values and ratios using pandas value_counts()](https://copyprogramming.com/howto/pandas-value-counts-show-values-and-ratio)
- [{Medium} 9 Pandas value_counts() tricks to improve your data analysis](https://towardsdatascience.com/9-pandas-value-counts-tricks-to-improve-your-data-analysis-7980a2b46536)
- [{pandas API} pandas.Series.value_counts](https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html)

<br/>

---

