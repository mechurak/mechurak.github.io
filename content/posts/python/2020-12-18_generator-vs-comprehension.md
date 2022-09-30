---
title: "'Better way 32. 긴 리스트 컴프리헨션보다는 제너레이터 식을 사용하라' 정리"
date: 2020-12-18T01:01:00+09:00
summary: "()사이에 리스트 컴프리헨션과 비슷한 구문을 넣어 제너레이터 식(generator expression)을 만들 수 있습니다. 입력의 길이가 긴 경우에 적극 고려합시다."
categories: [ python ]
series: [ effective python 4. 컴프리헨션과 제너레이터 ]
series_weight: 32
# featuredImage: http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg
tags:
- python
---

## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 32: Consider Generator Expressions for Large List Comprehensions  
**Better way 32. 긴 리스트 컴프리헨션보다는 제너레이터 식을 사용하라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

()사이에 리스트 컴프리헨션과 비슷한 구문을 넣어 제너레이터 식(generator expression)을 만들 수 있습니다.  
입력의 길이가 긴 경우에 적극 고려합시다.

<br/>

---

## 사용 예시

두 리스트에 대해서 같은 인덱스를 봐야하는 상황에서 사용할 수 있습니다.

```python
path = 'my_file.txt'
# aaaaaa
# aaaa
# aaa
#
# aaaa
#

# 리스트 컴프리헨션
value = [len(x) for x in open('my_file.txt')]
print(value)  # [7, 5, 4, 1, 5] (뉴라인 문자가 있어 눈에 보이는 것 보다 1 길다)


# Example 2 (제너레이터 식)
it = (len(x) for x in open('my_file.txt'))
print(it)  # <generator object <genexpr> at 0x000002F4E761DAC0>
print(next(it))  # 7
print(next(it))  # 5
```

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
List comprehensions can cause problems for large inputs by using too much memory.  
**입력이 크면 메모리를 너무 많이 사용하기 때문에 리스트 컴프리헨션은 문제를 일으킬 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Generator expressions avoid memory issues by producing outputs one at a time as iterators.  
**제너레이터 식은 이터레이터처럼 한 번에 원소를 하나씩 출력하기 때문에 메모리 문제를 피할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Generator expressions can be composed by passing the iterator from one generator expression into the `for` subexpression of another.  
**제너레이터 식이 반환한 이터레이터를 다른 제너레이터 식의 하위 식으로 사용함으로써 제너레이터 식을 서로 합성할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Generator expressions execute very quickly when chained together and are memory efficient.  
**서로 연결된 제너레이터 식은 매우 빠르게 실행되며 메모리도 효율적으로 사용한다.**
{{< /admonition >}}

<br/>

---


## 제너레이터 식 합성

제너레이터 식이 반환한 이터레이터를 다른 제너레이터 식의 입력으로 사용.

```python
# Example 2 (제너레이터 식)
it = (len(x) for x in open('my_file.txt'))
print(it)  # <generator object <genexpr> at 0x000002F4E761DAC0>

# 제너레이터 식 합성 (roots 에 next가 불리면 it 먼저 한 칸 진행함)
roots = ((x, x**0.5) for x in it)
print(next(roots))  # (7, 2.6457513110645907)
print(next(roots))  # (5, 2.23606797749979)
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter4/Better%20way32.py)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_32.py)

<br/>

---