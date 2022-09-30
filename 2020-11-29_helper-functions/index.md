# 'Better way 5. 복잡한 식을 쓰는 대신 도우미 함수를 작성하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다. 혹 누군가에게도 도움이 되기를 바랍니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>

**현재 위치**
{{< admonition note >}}
<1. Pythonic Thinking>  
Item 5: Write Helper Functions Instead of Complex Expressions
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

코드를 짧게 할 수 있다는 생각으로 너무 복잡하게 만들어서는 안됩니다.
항상 나중에 코드를 읽을 사람(미래의 나를 포함)을 배려해야 한다는 생각을 합시다.

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Python's syntax makes it easy to write single-line expressions that are overly complicated and difficult to read.  
**파이썬 문법을 사용하면 아주 복잡하고 어려운 한 줄짜리 식을 쉽게 작성할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Move complex expressions into helper functions, expecially if you need to use the same logic repeatedly.  
**복잡한 식을 도우미 함수로 옮겨라. 특히 같은 로직을 반복해 사용할 때는 도우미 함수를 꼭 사용하라.**
{{< /admonition >}}

{{< admonition tip >}}
An `if/else` expression provides a more readable alternative to using the Boolean operators `or` and `and` in expressions.  
**불(boolean) 연산자 `or`나 `and`를 식에 사용하는 것보다 `if/else` 식을 쓰는 편이 더 가독성이 좋다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
