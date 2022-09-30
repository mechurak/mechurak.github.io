# 'Better way 11. 시퀀스를 슬라이싱하는 방법을 익혀라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<2. Lists and Dictionaries>
Item 11: Know How to Slice Sequences
**Better way 11. 시퀀스를 슬라이싱하는 방법을 익혀라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

슬라이싱은 정말 많이 쓰이고 유용한 기능입니다. list, tuple, str에 사용 가능합니다.

<br/>

---

## 사용 예시

```python
a = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
print(a[:])      # ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']  리스트를 복사하는 데 사용
print(a[:5])     # ['a', 'b', 'c', 'd', 'e']
print(a[:-1])    # ['a', 'b', 'c', 'd', 'e', 'f', 'g']
print(a[4:])     # ['e', 'f', 'g', 'h']
print(a[-3:])    # ['f', 'g', 'h']
print(a[2:5])    # ['c', 'd', 'e']
print(a[2:-1])   # ['c', 'd', 'e', 'f', 'g']
print(a[-3:-1])  # ['f', 'g']

# 길이 제한 (슬라이싱은 인덱스 범위를 넘어가도 괜찮음)
first_twenty_items = a[:20]
last_twenty_items = a[-20:]

# 슬라이스에 대입 (쓸 일이 있을까?)
print('이전:', a)  # ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
a[2:7] = [99, 22, 14]
print('이후:', a)  # ['a', 'b', 99, 22, 14, 'h']
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Avoid being verbose when slicing: Don't supply `0` for the start index or the length of the sequence for the end index.  
**슬라이싱할 때는 간결하게 하라. 시작 인덱스에 0을 넣거나, 끝 인덱스에 시퀀스의 길이를 넣지 말라.**
{{< /admonition >}}

{{< admonition tip >}}
Slicing is forgiving of start or end indexed that are our of bounds, which means it's easy to express slices on the front or back boundaries of a sequence (like `a[:20]` or `a[-20:]`).  
**슬라이싱은 범위를 넘어가는 시작 인덱스나 끝 인덱스도 허용한다. 따라서 시퀀스의 시작이나 끝에서 길이를 제한하는 슬라이스(`a[:20]`이나 `a[-20:]`)을 쉽게 표현할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Assigning to a `list` slice replaces that range in the original sequence with what's referenced even if the lengths are different.  
**리스트 슬라이스에 대입하면 원래 시퀀스에서 슬라이스가 가리키는 부분을 대입 연산자 오른쪽에 있는 시퀀스로 대치한다. 이 때 슬라이스와 대치되는 시퀀스가 달라도 된다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way11.py)
