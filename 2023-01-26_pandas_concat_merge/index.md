# Pandas DataFrame 합치기 - concat()과 merge()


두 DataFrame을 위아래로 합칠때는 `pd.concat()`, 특정 열을 기준으로 join 할 때는 `pd.merge()`를 사용하자.
<!--more-->

{{< image src="/images/logo/pandas.svg" caption="pandas 로고" width=50% linked=false >}}

## pd.concat()
여러 소스를 통해 얻은(ex. 다른 csv 파일) 데이터 프레임들을 위아래로 합치고 싶은 상황이다. 아래 코드로 합칠 수 있다.

<br/>

```python
pd.concat([df1, df2], ignore_index=True)
```

<br/>

아래 노트북은 `pd.concat()` 참고 예시

<br/>

{{< pybook pandas_practice_concat 1400 >}}

보통은 인덱스를 새로 매겨야 하는 경우가 많아서, `ignore_index=True` 를 많이 사용할 듯 하다.

<br/>

---

## pd.merge()

```python
pd.merge(df_left, df_right, how='inner', left_on='name', right_on='member')
```

SQL에서 테이블 조인하는 느낌이다.  
`pd.join()`이 또 있기는 한데, `pd.merge()`가 주로 쓰인다고 한다.

<br/>

아래 노트북은 `pd.merge()` 참고 예시

<br/>

{{< pybook pandas_practice_merge 1000 >}}

<br/>

## Reference
- [{책} 파이썬 머신러닝 판다스 데이터 분석 / 오승환 - 6.4 데이터프레임 합치기](https://product.kyobobook.co.kr/detail/S000000833232)
- [{Pandas User Guide} 10minutes to pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html#merge)

<br/>

---
