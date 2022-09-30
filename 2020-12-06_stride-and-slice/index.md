# 'Better way 12. 스타라이드와 슬라이스를 한 식에 함께 사용하지 말라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<2. List and Dictionaries>  
Item 12: Avoid Striding and Slicing in a Single Expression
**Better way 12. 스타라이드와 슬라이스를 한 식에 함께 사용하지 말라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

슬라이싱과 인덱싱을 같이 쓰면 너무 복잡해져서 이해하기 어렵습니다. 나눠서 씁시다.  
증가값이 음수인 경우도 복잡해지니 웬만하면 쓰지 맙시다.

<br/>

---

## 사용 예시

```python
colors = ['빨강', '주황', '노랑', '녹색', '파랑', '자주']
odds = colors[::2]  # ['빨강', '노랑', '파랑']. 처음(0) 에서 시작. 두 칸씩
evens = colors[1::2]  # ['주황', '녹색', '자주']
temp = colors[::-2]  # ['자주', '녹색', '주황']. stride 가 음수면 뒤에서 시작.
print(odds)
print(evens)
print(temp)

x = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
y = x[::2]  # ['a', 'c', 'e', 'g']
z = y[1:-1]  # ['c', 'e']
print(y)
print(z)
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Specifying start, end, and stride in a slice can be extremely confusing.  
**슬라이스에 시작, 끝, 증가값을 함께 지정하면 코드의 의미를 혼동하기 쉽다.**
{{< /admonition >}}

{{< admonition tip >}}
Prefer using positive stride values in slices without start or end indexes. Avoid negative stride values if possible.  
**시작이나 끝 인덱스가 없는 슬라이스를 만들 때는 양수 증가값을 사용하라. 가급적 음수 증가값은 피하라.**
{{< /admonition >}}

{{< admonition tip >}}
Avoid using start, end, and stride together in a single slice. If you need all three parameters, consider doing two assignments (one to stride and another to slice) or using `islice` from the `itertools` built-in module.  
**한 슬라이스 안에서 시작, 끝, 증가값을 함께 사용하지 말라. 세 파라미터를 모두 써야 하는 경우, 두 번 대입을 사용(한 번은 스트라이딩, 한 번은 슬라이싱)하거나 itertools 내장 모듈의 islice를 사용하라.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way12.py)
