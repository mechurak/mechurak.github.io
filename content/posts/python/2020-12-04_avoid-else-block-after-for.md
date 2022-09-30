---
title: "'Better way 9. for나 while 루프 뒤에 else 블록을 사용하지 말라' 정리"
date: 2020-12-04T01:00:00+09:00
summary: "루프 뒤에 else를 붙이는 문법도 있었네요. 계속 몰라도 될 것 같습니다."
categories: [ python ]
series: [ effective python 1. 파이썬답게 생각하기 ]
series_weight: 9
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
Item 9: Avoid else Blocks After for and while Loops  
**Better way 9. for나 while 루프 뒤에 else 블록을 사용하지 말라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

`for`나 `while` 루프 뒤에 `else` 를 붙이는 문법도 있었네요. 계속 몰라도 될 것 같습니다.

<br/>

---


## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Python has special syntax that allows `else` blocks to immediately follow `for` and `while` loop interior blocks.  
**파이썬은 for나 while 루프에 속한 블록 바로 뒤에 else 블록을 허용하는 특별한 문법을 제공한다.**
{{< /admonition >}}

{{< admonition tip >}}
The `else` block after a loop runs only if the loop body did not encounter a `break` statement.  
**루프 뒤에 오는 else 블록은 루프가 반복되는 도중에 break를 만나지 않은 경우에만 실행된다.**
{{< /admonition >}}

{{< admonition tip >}}
Avoid using `else` blocks after loops because their behavior isn't intuitive and can be confusing.  
**동작이 직관적이지 않고 혼동을 야기할 수 있으므로 루프 뒤에 else 블록을 사용하지 말라.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)