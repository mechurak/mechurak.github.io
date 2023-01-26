---
title: "Pandas DataFrame 필터링 - boolean indexing 과 isin() 메소드"
date: 2023-01-25T23:00:00+09:00
categories: [ Machine Learning ]
tags:
- python
- pandas
---

특정 조건에 해당하는 행들만 필터링 하는 것을 `boolean indexing` 이라고 한다. `isin()` 메소드도 유용하다.
<!--more-->

{{< figure src="/images/logo/ml.jpg" >}}

## 1. boolean indexing
아래 노트북처럼 각 행을 `True`, `False` 로 판단하는 `mask`를 준비하고, `df.loc()` 로 해당 행들만 필터링한다.

<br/>

{{< pybook pandas_filter_boolean_indexing 760 >}}


`Out [2]`에서 `True`로 마킹된 행이 102 행이고, `Out [3]`에서 보면 102행이 출력되었다.

<br/>

---

## 2. isin() 메소드 사용
[`Series.isin()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.isin.html#pandas.Series.isin)을 이용해서 조건을 명시할 수도 있다.

<br/>

{{< pybook pandas_filter_isin 1280 >}}

and(`&`)를 이용해 mask 를 준비한 경우와 `isin()`을 사용하는 경우의 결과가 동일하다.  
경우에 따라 `isin()`을 쓰는게 조건을 더 알기 쉽게 표현할 수 있을 것으로 보인다.

<br/>

## Reference
- [{책} 파이썬 머신러닝 판다스 데이터 분석 / 오승환 - 6.3 필터링](https://product.kyobobook.co.kr/detail/S000000833232)
- [{pandas User Guide} 10minutes to pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html#boolean-indexing)

<br/>

---