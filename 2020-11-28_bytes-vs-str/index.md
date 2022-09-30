# 'Better way 3. bytes와 str의 차이를 알아두라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다. 혹 누군가에게도 도움이 되기를 바랍니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>

**현재 위치**
{{< admonition note >}}
<1. Pythonic Thinking>  
Item 3: Know the Differences Between `bytes` and `str`
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

개인적으로 아직 `bytes`를 쓸 일이 거의 없어서, 딱히 와닿지는 않습니다.
일단은 `str` 과 `bytes`는 다른 타입이고 섞어쓸 수 없다는 것을 기억해 둡시다.

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
`bytes` contains sequences of 8-bits values, and `str` contains sequence of Unicode code points.  
**`bytes` 에는 8비트 값의 시퀀스가 들어 있고, `str`에는 유니코드 코드 포인트의 시퀀스가 들어 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Use helper functions to ensure that the inputs you operate on are the type of character sequence that you expect (8-bit values, UTF-8-encoded strings, Unicode code points, etc).  
**처리할 입력이 원하는 문자 시퀀스(8비트 값, UTF-8로 인코딩된 문자열, 유니코드 코드 포인트들)인지 확실히 하려면 도우미 함수를 사용하라.**
{{< /admonition >}}

{{< admonition tip >}}
`bytes` and `str` instances can't be used together with operators (like `>`, `==`, `+`, and `%`).  
**`bytes`와 `str` 인스턴스를 (`>`, `==`, `+`, `%`와 같은) 연산자에 섞어서 사용할 수 없다.**
{{< /admonition >}}

{{< admonition tip >}}
If you want to read or write binary data to/from a file, always open the file using a binary mode (like `'rb'` or `'wb'`).  
**이진 데이터를 파일에서 읽거나 파일에 쓰고 싶으면 항상 이진 모드(`'rb'`나 `'wb'`) 로 파일을 열어라.**
{{< /admonition >}}

{{< admonition tip >}}
If you want to read or write Unicode data to/from a file, be careful about your system's default text encoding. Explicitly pass the `encoding` parameter to `open` if you want to avoid surprises.  
**유니코드 데이터를 파일에서 읽거나 파일에 쓰고 싶을 때는 시스템 디폴트 인코딩에 주의하라. 인코딩 차이로 놀라고 싶지 않으면 `open`에 `encoding` 파라미터를 명시적으로 전달하라.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
