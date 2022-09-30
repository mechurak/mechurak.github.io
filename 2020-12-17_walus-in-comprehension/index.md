# 'Better way 29. 대입식을 사용해 컴프리헨션 안에서 반복 작업을 피하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 29: Avoid Repeated Work in Comprehensions by Using Assignment Expressions  
**Better way 29. 대입식을 사용해 컴프리헨션 안에서 반복 작업을 피하라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

Comprehension 의 조건 부분에서 대입식(왈러스 연산자)를 사용하면 코드가 간결해질 수 있습니다.

<br/>

---

## 사용 예시

```python
stock = {
    'nails': 125,
    'screws': 35,
    'wingnuts': 8,
    'washers': 24,
}

BATCH_SIZE = 8  # 부품을 8개씩 써야 함

# 각 부품의 몇 세트 분량의 재고가 있는지 알고 싶음.
order = ['screws', 'wingnuts', 'clips']


def get_batches(count, size):
    return count // size  # screws 의 경우 35개 있으니 4세트


# Example 4 (요렇게, Comprehension 의 조건 부분에 대입식 사용) 
found = {name: batches for name in order
         if (batches := get_batches(stock.get(name, 0), BATCH_SIZE))}  # 조건 부분에 대입식 사용
print(type(found))  # <class 'dict'>
print(found)  # {'screws': 4, 'wingnuts': 1}

# Example 10 (제너레이터 생성에도 사용 가능)
# Comprehension 을 () 로 묶으면 제너레이터가 되는구나! 
found = ((name, batches) for name in order
         if (batches := get_batches(stock.get(name, 0), BATCH_SIZE)))  # 조건 부분에 대입식 사용
print(type(found))  # <class 'generator'>
print(next(found))  # ('screws', 4)
print(next(found))  # ('wingnuts', 1)
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Assignment expressions make it possible for comprehensions and generator expressions to reuse the value from one condition elsewhere in the same comprehension, which can improve readability and performance.  
**대입식을 통해 컴프리헨션이나 제너리에터 식의 조건 부분에서 사용한 값을 같은 컴프리헨션이나 제너레이터의 다른 위치에서 재사용할 수 있다. 이를 통해 가독성과 성능을 향상시킬 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Although it's possible to use an assignment expression outside of a comprehension or generator expression's condition, you should avoid doing so.  
**조건이 아닌 부분에도 대입식을 사용할 수 있지만, 그런 형태의 사용은 피해야 한다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_29.py)
- [Better way 10. 대입식을 사용해 반복을 피하라](https://mechurak.github.io/ko/posts/python/2020-12-04_walrus-operator/)

<br/>

---
