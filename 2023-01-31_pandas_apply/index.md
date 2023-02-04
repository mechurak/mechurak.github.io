# Pandas DataFrame 함수 매핑 - apply()


DataFrame의 열 혹은 행에 특정 함수를 적용하고 싶을때는 `apply()`를 사용한다.
<!--more-->

{{< figure src="/images/logo/ml.jpg" >}}


## 1. 핵심 요약
`DataFrame.apply()` 를 이용해, 열별 혹은 행별로 함수를 적용할 수 있다. 
 

<br/>

- 열별로 함수 적용 - [4. DataFrame의 각 열에 함수 매핑](#4-dataframe의-각-열에-함수-매핑---apply)
```python
def min_max(x: pd.Series) -> float:  
    return x.max() - x.min()

result = df.apply(min_max)  # axis=0 은 기본 값이므로 생략 가능
```

<br/>

- 행별로 함수 적용 (axis=1) - [5. DataFrame의 각 행에 함수 매핑 - apply()](#5-dataframe의-각-행에-함수-매핑---apply)
```python
df['add'] = df.apply(lambda x : x['age'] + x['fare'], axis=1)  # lambda 활용
```

<br/>

---

## 2. Series의 개별 원소에 함수 매핑 - apply()
Series 객체의 개별 원소에 함수를 적용하는 경우이다.  
각 원소에 함수를 적용해서 만들어진 Series를 반환해준다.

{{< pybook pandas_practice_apply2 1250 >}}

<br/>

---

## 3. DataFrame의 모든 개별 원소에 함수 매핑 - applymap()
DataFrame의 개별 원소에 특정 함수를 매핑하려면, `applymap()` 메소드를 활용한다.

{{< pybook pandas_practice_apply3 800 >}}

<br/>

---

## 4. DataFrame의 각 열에 함수 매핑 - apply()
DataFrame의 `apply(매핑함수, axis=0)` 메소드를 호출하면, DataFrame의 각 열을 매핑 함수의 인자로 전달한다.  
매핑 함수의 return type에 따라 결과 type이 달라진다.
- Series를 리턴하는 함수 매핑 -> DataFrame 반환
- 값을 리턴하는 함수 매핑 -> Series 반환

{{< pybook pandas_practice_apply4 1050 >}}

<br/>

---

## 5. DataFrame의 각 행에 함수 매핑 - apply()
`axis=1`을 적용하면, DataFrame의 각 행을 매핑 함수의 인자로 전달한다.  
인자로 넘기는 Series의 index가 원래 DataFrame의 colums 라서 `axis=1` 이다.  

{{< pybook pandas_practice_apply5 1000 >}}

<br/>

---

## 6. DataFrame 객체 자체에 함수 매핑 - pipe()
DataFrame 객체에 함수를 매핑하려면 `pipe()` 메소드를 활용한다.  
매핑 함수의 return type에 따라 결과 type 이 달라진다.
- DataFrame을 리턴하는 함수 매핑 -> DataFrame 반환
- Series를 리턴하는 함수 매핑 -> Series 반환
- 값을 리턴하는 함수 매핑 -> 값 반환

{{< pybook pandas_practice_apply6 1300 >}}

<br/>

---

## Reference
- [{책} 파이썬 머신러닝 판다스 데이터 분석 / 오승환 - 6.1 함수 매핑](https://product.kyobobook.co.kr/detail/S000000833232)
- [{Pandas User Guide} Essential basic functionality - Function application](https://pandas.pydata.org/docs/user_guide/basics.html#function-application)


<br/>

---

