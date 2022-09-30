---
title: "'Better way 14. 복잡한 기준을 사용해 정렬할 때는 key 파라미터를 사용하라' 정리"
date: 2020-12-08T01:00:00+09:00
summary: "객체 리스트를 커스텀하게 정렬해야 한다면, 정렬 기준이 될 값을 리턴하는 함수를 key 파라미터로 넘기면 됩니다."
categories: [ python ]
series: [ effective python 2. 리스트와 딕셔너리 ]
series_weight: 14
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
<2. Lists and Dictionaries>  
Item 14: Sort by Complex Criteria Using the key Parameter  
**14. 복잡한 기준을 사용해 정렬할 때는 key 파라미터를 사용하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

객체 리스트를 커스텀하게 정렬해야 한다면, 정렬 기준이 될 값을 리턴하는 함수를 key 파라미터로 넘기면 됩니다.

혹 애트리뷰트가 많고 정렬 기준이 복잡하다면, 행렬 데이터 처리에 많이 쓰이는 pandas 모듈을 사용하는 것이 좋은 선택일 수 있습니다.

<br/>

---

## 사용 예시

```python
class Tool:
    def __init__(self, name, weight):
        self.name = name
        self.weight = weight

    def __repr__(self):
        return f'Tool({self.name!r}, {self.weight})'


tools = [
    Tool('수준계', 3.5),
    Tool('해머', 1.25),
    Tool('스크류드라이버', 0.5),
    Tool('끌', 0.25),
]

print('미정렬:', repr(tools))
# 미정렬: [Tool('수준계', 3.5), Tool('해머', 1.25), Tool('스크류드라이버', 0.5), Tool('끌', 0.25)]

tools.sort(key=lambda x: x.name)
print('정렬: ', tools)
# 정렬:  [Tool('끌', 0.25), Tool('수준계', 3.5), Tool('스크류드라이버', 0.5), Tool('해머', 1.25)]

tools.sort(key=lambda x: x.weight)
print('무게순 정렬:', tools)
# 무게순 정렬: [Tool('끌', 0.25), Tool('스크류드라이버', 0.5), Tool('해머', 1.25), Tool('수준계', 3.5)]
```

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `sort` method of the `list` type can be used to rearrange a list's content by the natural ordering of built-in types like strings, integers, tuples, and so on.  
**리스트 타입에 들어 있는 sort 메서드를 사용하면 원소 타입이 문자열, 정수, 튜플 등과 같은 내장 타입인 경우 자연스러운 순서로 리스트의 원소를 정렬할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
The `sort` method doesn't work for objects unless they define a natural ordering using special methods, which is uncommon.  
**원소 타입에 특별 메서드를 통해 자연스러운 순서가 정의돼 있지 않으면 sort 메서드를 쓸 수 없다. 하지만 원소 타입에 순서 특별 메서드를 정의하는 경우는 드물다.**
{{< /admonition >}}

{{< admonition tip >}}
The `key` parameter of the `sort` method can be used to supply a helper function that returns the value to use for sorting in place of each item for the `list`.  
**sort 메서드의 key 파라미터를 사용하면 리스트의 각 원소 대신 비교에 사용할 객체를 반환하는 도우미 함수를 제공할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Returning a `tuple` from the `key` function allows you to combine multiple sorting criteria together. The unary minus operator can be used to reverse individual sort orders for types that allow it.  
**key 함수에서 튜플을 반환하면 여러 정렬 기준을 하나로 엮을 수 있다. 단항 부호 반전 연산자를 사용하면 부호를 바꿀 수 있는 타입이 정렬 기준인 경우 정렬 순서를 반대로 바꿀 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
For types that can't be negated, you can combine many sorting criteria together by calling the `sort` method multiple times using different `key` functions and `reverse` values, in the order of lowest rank `sort` call to highest rank `sort` call.  
**부호를 바꿀 수 없는 타입의 경우 여러 정렬 기준을 조합하려면 각 정렬 기준마다 reverse 값으로 정렬 순서를 지정하면서 sort 메서드를 여러 번 사용해야 한다. 이때 정렬 기준의 우선순위가 점점 높아지는 순서로 sort를 호출해야 한다.**
{{< /admonition >}}

<br/>

---

## 추가 사용 예시

```python
class Tool:
    def __init__(self, name, weight):
        self.name = name
        self.weight = weight

    def __repr__(self):
        return f'Tool({self.name!r}, {self.weight})'


power_tools = [
    Tool('연마기', 4),
    Tool('드릴', 4),
    Tool('원형 톱', 5),
    Tool('착암기', 40),
]

# 튜플을 리턴하는 함수를 넘기면, 여러 기준으로 정렬 가능
power_tools.sort(key=lambda x: (x.weight, x.name))  # weight 로 먼저 정렬한 후, 같은 weight 안에서는 name 으로 정렬
print(power_tools)
# [Tool('드릴', 4), Tool('연마기', 4), Tool('원형 톱', 5), Tool('착암기', 40)]

power_tools.sort(key=lambda x: (x.weight, x.name), reverse=True)  # 모든 비교 기준을 내림차순으로 만든다
print(power_tools)
# [Tool('착암기', 40), Tool('원형 톱', 5), Tool('연마기', 4), Tool('드릴', 4)]

power_tools.sort(key=lambda x: (-x.weight, x.name))  # 숫자 값의 경우 정렬 방향 반대로 할 수 있음 (문자열은 안됨)
print(power_tools)
# [Tool('착암기', 40), Tool('원형 톱', 5), Tool('드릴', 4), Tool('연마기', 4)]


# sort 를 두 번 해서 원하는 기준들로 정렬하기 (weight 로 먼저 정렬한 후, 같은 weight 안에서는 name 으로 정렬)
power_tools.sort(key=lambda x: x.name)   # name 기준 오름차순 (요걸 먼저 불러야 함)
power_tools.sort(key=lambda x: x.weight, reverse=True)  # weight 기준 내림차순
print(power_tools)
# [Tool('착암기', 40), Tool('원형 톱', 5), Tool('드릴', 4), Tool('연마기', 4)]

```



<br/>

---


## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way14.py)

<br/>

---