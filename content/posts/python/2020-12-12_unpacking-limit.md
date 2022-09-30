---
title: "'Better way 19. 함수가 여러 값을 반환하는 경우 절대로 네 값 이상을 언패킹하지 말라' 정리"
date: 2020-12-12T01:00:00+09:00
summary: "언패킹은 세 개까지만 합시다. 너무 많아지면 헷갈리니까요."
categories: [ python ]
series: [ effective python 3. 함수 ]
series_weight: 19
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
Item 19: Never Unpack More Than Three Variables When Functions Return Multiple Values  
**Better Way 19. 함수가 여러 값을 반환하는 경우 절대로 네 값 이상을 언패킹하지 말라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

언패킹은 세 개까지만 하는게 좋다고 합니다. 너무 많아지면 헷갈리기 쉬우니까요.

<br/>

---

## 사용 예시

```python
def get_stats(numbers):
    minimum = min(numbers)
    maximum = max(numbers)
    return minimum, maximum


lengths = [63, 73, 72, 60, 67, 66, 71, 61, 72, 70]

minimum, maximum = get_stats(lengths)  # 반환 값이 두 개

print(f'최소: {minimum}, 최대: {maximum}')
# 최소: 60, 최대: 73
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
You can have functions return multiple values by putting them in a `tuple` and having the caller take advantage of Python's unpacking syntax.  
**함수가 여러 값을 반환하기 위해 값들을 튜플에 넣어서 반환하고, 호출하는 쪽에서는 파이썬 언패킹 구문을 쓸 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Multiple return values from a function can also be unpacked by catch-all starred expressions.  
**함수가 반환한 여러 값을, 모든 값을 처리하는 별표 식을 사용해 언패킹할 수도 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Unpacking into four or more variables is error prone and should be avoided; instead, return a small class or `namedtuple` instance.  
**언패킹 구문에 변수가 네 개 이상 나오면 실수하기 쉬우므로 변수를 네 개 이상 사용하면 안 된다. 대신 작은 클래스를 반환하거나 namedtuple 인스턴스를 반환하라.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter3/Better%20way19.py)

<br/>

---