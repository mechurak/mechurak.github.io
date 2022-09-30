# 'Better way 13. 슬라이싱보다는 나머지를 모두 잡아내는 언패킹을 사용하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<2. Lists and Dictionaries>  
Item 13: Prefer Catch-All Unpacking Over Slicing  
**Better Way 13. 슬라이싱보다는 나머지를 모두 잡아내는 언패킹을 사용하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

리스트를 언패킹할때, 나머지 부분은 별표식(starred expression) 으로 잡아낼 수 있습니다.

<br/>

---

## 사용 예시

```python
car_ages = [0, 9, 4, 8, 7, 20, 19, 1, 6, 15]
car_ages_descending = sorted(car_ages, reverse=True)
print(car_ages_descending)  # [20, 19, 15, 9, 8, 7, 6, 4, 1, 0]

# 요런 식으로 리스트의 나머지 부분 언패킹
oldest, second_oldest, *others = car_ages_descending
print(oldest, second_oldest, others)  # 20 19 [15, 9, 8, 7, 6, 4, 1, 0]

# 요렇게 쓰지 말자
oldest = car_ages_descending[0]
second_oldest = car_ages_descending[1]
others = car_ages_descending[2:]
print(oldest, second_oldest, others)  # 20 19 [15, 9, 8, 7, 6, 4, 1, 0]
```

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Unpacking assignments may use a starred expression to catch all values that weren't assigned to the other parts of the unpacking pattern into a list.  
**언패킹 대입에 별표 식을 사용하면 언패킹 패턴에서 대입되지 않는 모든 부분을 리스트에 잡아낼 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Starred expressions may appear in any position, and they will always become a list containing the zero or more values they receive.  
**별표 식은 언패킹 패턴의 어떤 위치에든 놓을 수 있다. 별표 식에 대입된 결과는 항상 리스트가 되며, 이 리스트에는 별표 식이 받은 값이 0개 또는 그 이상 들어간다.**
{{< /admonition >}}

{{< admonition tip >}}
When dividing a list into non-overlapping pieces, catch-all unpacking is much less error prone than slicing and indexing.  
**리스트를 서로 겹치지 않게 여러 조각으로 나눌 경우, 슬라이싱과 인덱싱을 사용하기보다는 나머지를 모두 잡아내는 언패킹을 사용해야 실수할 여지가 훨씬 줄어든다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way13.py)

<br/>

---
