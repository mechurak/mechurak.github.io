# 'Better way 7. range 보다는 enumerate를 사용하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다. 혹 누군가에게도 도움이 되기를 바랍니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<1. Pythonic Thinking>  
Item 7: Prefer `enumerate` Over `range`
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

`list` 에 `for` 루프 돌 일 있으면, `enumerate`!!

<br/>

---

## 사용 예시

두 리스트에 대해서 같은 인덱스를 봐야하는 상황에서 사용할 수 있습니다.

```python
flavor_list = ['바닐라', '초콜릿', '피칸', '딸기']

for i, flavor in enumerate(flavor_list, 1):  # enumerate() 두번째 인자로 i의 시작 번호 지정
    print(f'{i}: {flavor}')

>>>

1: 바닐라
2: 초콜릿
3: 피칸
4: 딸기
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
`enumerate` provides concise syntax for looping over an iterator and getting the index of each item from the iterator as you go.  
**`enumerate`를 사용하면 이터레이터에 대해 루프를 돌면서 이터레이터에서 가져오는 원소의 인덱스까지 얻는 코드를 간결하게 작성할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Prefer `enumerate` instead of looping over a `range` and indexing into a sequence.  
**`range`에 대해 루프를 돌면서 시퀀스의 원소를 인덱스로 가져오기보다는 `enumerate`를 사용하라.**
{{< /admonition >}}

{{< admonition tip >}}
You can supply a second parameter to `enumerate` to specify the number from which to begin counting (zero is the default).  
**`enumerate`의 두 번째 파라미터로 몇 부터 번호를 부여할지 지정할 수 있다(디폴트 값은 0이다).**
{{< /admonition >}}

<br/>

---


## 상세 설명

내부적으로 제너레이터로 동작합니다.

```python
flavor_list = ['바닐라', '초콜릿', '피칸', '딸기']
it = enumerate(flavor_list, 1)
print(next(it))
print(next(it))

>>>

(1, '바닐라')
(2, '초콜릿')
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter1/Better%20way7.py)
