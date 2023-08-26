---
title: "Pandas 그룹내에서 순서대로 인덱스 부여 - cumcount()"
date: 2023-08-26T10:30:00+09:00
categories: [ pandas ]
tags:
- pandas
---

`cumcount()`를 이용해서 그룹별로 각각 번호를 매길 수 있다.
<!--more-->

{{< image src="/images/logo/pandas.svg" caption="pandas 로고" width=50% linked=false >}}


## 1. 핵심 요약
- groupby() 한 거에 `cumcount()`해서 그룹내에서 각각 순서대로 번호를 매길 수 있음
- 원래 DataFrame 모양 및 index 유지됨 (transformation)

```python
df['per_group_index'] = df.groupby('class').cumcount() + 1
```
<br/>

---

## 2. 사용 예시

```python
import pandas as pd

df = pd.DataFrame({
    'class': ['a', 'a', 'a', 'b', 'b', 'a'],
    # 'class_temp': list('aaabba'),  # 이렇게도 되네
})
df
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
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>a</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
    </tr>
    <tr>
      <th>5</th>
      <td>a</td>
    </tr>
  </tbody>
</table>
</div>


위와 같은 데이터가 있다고 하자.

<br/>

```python
# cumcount() 사용. 원래 index(0~5) 유지됨
df.groupby(['class']).cumcount()
```

    0    0
    1    1
    2    2
    3    0
    4    1
    5    3
    dtype: int64


groupby 한 거에다 `cumcount()`한 결과를 보면 Series가 나왔다. 원래 DataFrame의 index도 유지되어 있다.

<br/>

```python
# 1부터 번호 매기고 싶음
df.groupby(['class']).cumcount() + 1
```

    0    1
    1    2
    2    3
    3    1
    4    2
    5    4
    dtype: int64

1부터 번호를 매기고 싶으니, 1을 더해주자.

<br/>

```python
# 실제로 df에 column 추가
df['per_group_index'] = df.groupby(['class']).cumcount() + 1
df
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
      <th>class</th>
      <th>per_group_index</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>a</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>a</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>

확인해 본 코드로 실제 DataFrame에 `per_group_index` column을 추가했다.

<br/>

```python
# 보기 좋게 소팅
df = df.sort_values(['class', 'per_group_index'])
df
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
      <th>class</th>
      <th>per_group_index</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>a</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>a</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>

필요에 따라 소팅도 해주면 끝.

<br/>

---

## 3. 기타
[{Pandas 문서} DataFrameGroupBy.cumcount](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.core.groupby.DataFrameGroupBy.cumcount.html#pandas.core.groupby.DataFrameGroupBy.cumcount) 따르면, 아래 코드와 비슷한 느낌 이라고 한다.



```python
import numpy as np

df.groupby('class').apply(lambda x: pd.Series(np.arange(len(x)) + 1, index=x.index))
```

    class   
    a      0    1
           1    2
           2    3
           5    4
    b      3    1
           4    2
    dtype: int32


결과 Series가 멀티인덱스라 인덱스가 달라져서 바로 대입은 안되는데, `cumcount()` 쓰면 되니까 그런가 부다 하자.





<br/>

---

## Reference
- [{Stackoverflow} Add a sequential counter column on groups to a pandas dataframe](https://stackoverflow.com/a/23435320)
- [{Pasdas 문서} Group by > Enumerate group items](https://pandas.pydata.org/pandas-docs/stable/user_guide/groupby.html#enumerate-group-items)
- [{Pandas 문서} DataFrameGroupBy.cumcount](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.core.groupby.DataFrameGroupBy.cumcount.html#pandas.core.groupby.DataFrameGroupBy.cumcount)

<br/>

---
