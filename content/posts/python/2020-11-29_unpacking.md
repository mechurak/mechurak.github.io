---
title: "'Better way 6. 인덱스를 사용하는 대신 대입을 사용해 데이터를 언패킹하라' 정리"
date: 2020-11-29T01:02:00+09:00
summary: "언패킹 좋습니다. 두 번 쓰세요."
categories: [ python ]
series: [ effective python 1. 파이썬답게 생각하기 ]
series_weight: 6
# featuredImage: http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg
tags:
- python
---

## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다. 혹 누군가에게도 도움이 되기를 바랍니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<1. Pythonic Thinking>
Item 6: Prefer Multiple Assignment Unpacking Over Indexing
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

코드를 이해하기 쉽도록, 알아 볼 수 있는 이름으로 언패킹해야 합니다. 인덱스로 접근은 지양합시다.

<br/>

---

## 사용 예시

```python
>>> item = ('호박엿', '식혜')
>>>
>>> # 인덱스를 사용해 접근 (이해하기 어려움)
>>> print(item[0], '&', item[1])
호박엿 & 식혜

>>> # 언패킹 사용 (요렇게 쓰자)
>>> first, second = item # 언패킹
>>> print(first, '&', second)
호박엿 & 식혜
```

<br/>
<br/>

튜플도 원소 개 수 알고 있다면, 인덱스로 접근하지 말고 언패킹으로.

```python
snacks = [('베이컨', 350), ('도넛', 240), ('머핀', 190)]

# 요렇게 쓰자
for i, (name, calories) in enumerate(snacks, 1):  # enumerate() 의 1은 i 시작을 1부터 하라는 의미
    print(f'#{i}: {name} 은 {calories} 칼로리입니다.')

# 요렇게 쓰지 말자
for i in range(len(snacks)):
    item = snacks[i]
    name = item[0]
    calories = item[1]
    print(f'#{i+1}: {name} 은 {calories} 칼로리입니다.')

>>>

#1: 베이컨 은 350 칼로리입니다.
#2: 도넛 은 240 칼로리입니다.
#3: 머핀 은 190 칼로리입니다.
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Python has special syntax called unpacking for assigning multiple values in a single statement.  
**파이썬은 한 문장 안에서 여러 값을 대입할 수 있는 언패킹이라는 특별한 문법을 제공한다.**
{{< /admonition >}}

{{< admonition tip >}}
Unpacking is generalized in Python and can be applied to any iterable, including many levels of iterables within iterables.  
**파이썬 언패킹은 일반화돼 있으므로 모든 이터러블에 적용할 수 있다. 그리고 이터러블이 여러 계층으로 내포된 경우에도 언패킹을 적용할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Reduce visual noise and increase code clarity by using unpacking to avoid explicitly indexing into sequences.  
**인덱스를 사용해 시퀀스 내부에 접근하는 대신 언패킹을 사용해 시각적인 잡음을 줄이고 코드를 더 명확하게 만들라.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter1/Better%20way6.py)