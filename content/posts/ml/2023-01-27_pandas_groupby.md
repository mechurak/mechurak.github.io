---
title: "Pandas DataFrame 그룹 연산 - groupby()"
date: 2023-01-27T23:30:00+09:00
categories: [ Machine Learning ]
tags:
- python
- pandas
---

DataFrame을 특정 열의 값들 기준으로 그룹화 해서 살펴보는 경우가 많다. `groupby()`를 알아보자.
<!--more-->

{{< figure src="/images/logo/ml.jpg" >}}

## 1. 기본 사용법
1개의 열을 기준으로 그룹화
```python
df.groupby(['class'])[['age', 'fare']].mean().reset_index()
```
- `class` 열 기준으로 그룹화
- `age`, `fare` 열이 관심 있음
- 각 열에 대해서 `mean()` 집계
- `reset_index()` 하면 결과 DataFrame을 또 조작하기 용이함

<br/>

2개의 열을 기준으로 그룹화
```python
df.groupby(['class', 'sex']).mean().reset_index()
```

---

## 2. 세부 동작 확인
`groupby()` 만 하면 그룹별로 DataFrame을 갖고 있는 상태이고,  
거기에 mean() 같은 집계 합수를 적용한 뒤,  
다시 합쳐서 테이블로 보여주게 된다.

<br/>

아래 노트북을 살펴보자

<br/>

{{< pybook pandas_practice_group1 1900 >}}

---

## 3. 여러 열 기준으로 그룹화
아래 노트북은 여러 열 기준으로 그룹화 하는 예제이다.

<br/>

{{< pybook pandas_practice_group2 1750 >}}

---

## 4. 사용자 집계 함수 사용
집계함수로는 `mean()`, `max()`, `min()`, `sum()`, `count()`, `size()`, `var()`, `std()`, `describe()`, `info()`, `first()`, `last()` 등이 지원된다.  
`agg()`를 사용하여, 여러 집계 함수를 동시에 적용하거나, 사용자 함수를 집계 함수로 사용할 수 있다.  
열별로 다른 함수를 적용 하는 것도 가능하다.

<br/>

{{< pybook pandas_practice_group3 1200 >}}

---

## Reference
- [{책} 파이썬 머신러닝 판다스 데이터 분석 / 오승환 - 6.5 그룹 연산](https://product.kyobobook.co.kr/detail/S000000833232)
- [{Pandas User Guide} 10minutes to pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html#grouping)

<br/>

---