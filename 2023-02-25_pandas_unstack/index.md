# Pandas Dataframe 구조 변경 - unstack()


`unstack()` 으로 DataFrame을 조작하기 편한 모양으로 바꿀 수 있다.
<!--more-->

{{< figure src="/images/logo/ml.jpg" >}}

## 1. 핵심 요약
- `unstack()` 을 이용해서 DataFrame의 모양을 다루기 쉽게 바꿀 수 있다.
- groupby 한 결과를 group 별로 더 조작해야 될 때 유용하다.
- 아래 그림을 참고하자.

{{< image src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*DYDOif_qBEgtWfFKUDSf0Q.png" caption="이미지 출처: [{Medium Blog} Reshaping a DataFrame with Pandas stack() and unstack()](https://towardsdatascience.com/reshaping-a-dataframe-with-pandas-stack-and-unstack-925dc9ce1289)" width=100% >}}

<br/>

---

## 2. 사용 예시
요런 느낌으로 `unstack()` 을 사용할 수 있다.

```python
df.groupby(['class', 'survived']).size().unstack().reset_index()
```

<br/>

---

## 3. 세부 동작 확인
타이타닉 데이터 셋에서 class별 생존 비율이 필요한 상황이라고 가정 해보자.

<br/>

{{< pybook pandas_unstack 1900 >}}

<br/>

---

## Reference
- [{Pandas User Guide} Reshaping by stacking and unstacking](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-by-stacking-and-unstacking)
- [{Medium Blog} Reshaping a DataFrame with Pandas stack() and unstack()](https://towardsdatascience.com/reshaping-a-dataframe-with-pandas-stack-and-unstack-925dc9ce1289)

<br/>

---

