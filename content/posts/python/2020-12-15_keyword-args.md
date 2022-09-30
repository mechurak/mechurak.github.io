---
title: "'Better way 23. 키워드 인자로 선택적인 기능을 제공하라' 정리"
date: 2020-12-15T01:02:00+09:00
summary: "`**kwargs`를 파라미터로 아무 키워드나 받는 함수를 만들 수 있습니다. (확장성이 좋아집니다.)"
categories: [ python ]
series: [ effective python 3. 함수 ]
series_weight: 23
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
<3. Functions>  
Item 23: Provide Optional Behavior with Keyword Arguments  
**Better Way 23. 키워드 인자로 선택적인 기능을 제공하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

`**kwargs`를 파라미터로 아무 키워드나 받는 함수를 만들 수 있습니다. (확장성이 좋아집니다.)

<br/>

---

## 사용 예시

```python
# Example 8 (아무 키워드나 받을 수 있는 함수)
def print_parameters(**kwargs):
    for key, value in kwargs.items():
        print(f'{key} = {value}')

print_parameters(alpha=1.5, beta=9, gamma=4)
# alpha = 1.5
# beta = 9
# gamma = 4
```

<br/>
<br/>

```python
def remainder(number, divisor):
    return number % divisor

# Example 5
my_kwargs = {
    'number': 20,
    'divisor': 7,
}

# '**' 연산자를 통해 딕셔너리에 있는 값들을 함수에 전달, 여러 딕셔너리 전달도 가능
assert remainder(**my_kwargs) == 6
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Functions arguments can be specified by position or by keyword.  
**함수 인자를 위치에 따라 지정할 수도 있고, 키워드를 사용해 지정할 수도 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Keywords make it clear what the purpose of each argument is when it would be confusing with only positional arguments.  
**키워드를 사용하면 위치 인자만 사용할 때는 혼동할 수 있는 여러 인자의 목적을 명확히 할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Keyword arguments with default values make it easy to add new behaviors to a function without needing to migrate all existing callers.  
**키워드 인자와 디폴트 인자를 함께 사용하면 기본 호출 코드를 마이그레이션하지 않고도 함수에 새로운 기능을 쉽게 추가할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Optional keyword arguments should always be passed by keyword instead of by position.  
**선택적 키워드 인자는 항상 위치가 아니라 키워드를 사용해 전달되어야 한다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter3/Better%20way23.py)
- [원작자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_23.py)

<br/>

---