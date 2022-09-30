---
title: "'Better way 15. 딕셔너리 삽입 순서에 의존할 때는 조심하라' 정리"
date: 2020-12-09T01:00:00+09:00
summary: "파이썬 3.7부터는 딕셔너리에 추가한 순서대로 순회가 됩니다. 이전 버전의 파이썬도 고려해야 한다면 collections.OrderedDict을 사용해야 합니다."
categories: [ python ]
series: [ effective python 2. 리스트와 딕셔너리 ]
series_weight: 15
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
Item 15: Be Cautious When Relying on `dict` Insertion Ordering  
**Better way 15. 딕셔너리 삽입 순서에 의존할 때는 조심하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

파이썬 3.7부터는 딕셔너리에 추가한 순서대로 순회가 됩니다.  
이전 버전의 파이썬도 고려해야 한다면 `collections.OrderedDict`을 사용해야 합니다.

혹 `dict`라는 게 확실하지 않다면, 추가 순서에 의존하는 로직을 짜는 건 위험합니다.

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Since Python 3.7, you can rely on the fact that iterating a `dict` instance's contents will occur in the same order in which the keys were initially added.  
**파이썬 3.7부터는 dict 인스턴스에 들어 있는 내용을 이터레이션할 때 키를 삽입한 순서대로 돌려받는다는 사실에 의존할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Python makes it easy to define objects that act like dictionaries but that aren't `dict` instances. For these types, you can't assume that insertion ordering will be preserved.  
**파이썬은 dict는 아니지만 딕셔너리와 비슷한 객체를 쉽게 만들 수 있게 해준다. 이런 타입의 경우 키 삽입 순서가 그대로 보존된다고 가정할 수 없다.**
{{< /admonition >}}

{{< admonition tip >}}
There are three ways to be careful about dictionary-like classes: Write code that doesn't rely on insertion ordering, explicitly check for the `dict` type at runtime, or require `dict` values using type annotations and static analysis.  
**딕셔너리와 비슷한 클래스를 조심스럽게 다루는 방법으로는 dict 인스턴스의 삽입 순서 보존에 의존하지 않고 코드를 작성하는 방법, 실행 시점에 명시적으로 dict 타입을 검사하는 방법, 타입 애너테이션과 정적 분석(static analysis)을 사용해 dict 값을 요구하는 방법이 있다.**
{{< /admonition >}}


<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way15.py)

<br/>

---