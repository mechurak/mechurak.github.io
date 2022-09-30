# 'Better way 28. 컴프리헨션 내부에 제어 하위 식을 세 개 이상 사용하지 말라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 28: Avoid More Than Two Control Subexpressions in Comprehensions  
**Better way 28. 컴프리헨션 내부에 제어 하위 식을 세 개 이상 사용하지 말라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

리스트 컴프리헨션은 간단하게만 씁시다.
([리스트 컴프리헨션](https://mechurak.github.io/ko/posts/python/2020-11-26_list-comprehension/) 참고)

<br/>

---

## 사용 예시

```python
# Example 1 (요정도는 괜찮지만)
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [x for row in matrix for x in row]  # for 루프 중첩 가능 (matrix 의 row, 고 row 안의 x 이용)
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

my_lists = [
    [[1, 2, 3], [4, 5, 6]],
    [[7, 8, 9], [10, 11, 12]],
]

# Example 4 (복잡한건 그냥 이렇게 풀어 쓰자)
flat = []
for sublist1 in my_lists:
    for sublist2 in sublist1:
        flat.extend(sublist2)
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]

# Example 3 (너무 이해하기 어려움!)
flat = [x for sublist1 in my_lists
        for sublist2 in sublist1
        for x in sublist2]
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Comprehensions support multiple levels of loops and multiple conditions per loop level.  
**컴프리헨션은 여러 수준의 루프를 지원하며 각 수준마다 여러 조건을 지원한다.**
{{< /admonition >}}

{{< admonition tip >}}
Comprehensions with more than two control subexpressions are very difficult to read and should be avoided.  
**제어 하위 식이 세 개 이상인 컴프리헨션은 이해하기 매우 어려우므로 가능하면 피해야 한다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_28.py)

<br/>

---
