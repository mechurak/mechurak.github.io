# 'Better way 8. 여러 이터레이터에 나란히 루프를 수행하려면 zip을 사용하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다. 혹 누군가에게도 도움이 되기를 바랍니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<1. Pythonic Thinking>  
Item 8: Use `zip` to Process Iterators in Parallel
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

리스트 여러개에 대해서 같은 인덱스를 봐야 한다면, `zip`!!  
리스트 길이가 다를 때는 쓰지 않는 게 좋을 듯 합니다.

<br/>

---

## 사용 예시

두 리스트에 대해서 같은 인덱스를 봐야하는 상황에서 사용할 수 있습니다.

```python
names = ['Cecilia', '남궁민수', '毛泽东']
counts = [len(n) for n in names]  # [7, 4, 3]
print(counts)

longest_name = None
max_count = 0

for name, count in zip(names, counts):
    if count > max_count:
        longest_name = name
        max_count = count

print(longest_name, max_count)

>>>

[7, 4, 3]
Cecilia 7
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `zip` built-in function can be used to iterate over multiple iterators in parallel.  
**zip 내장 함수를 사용해 여러 이터레이터를 나란히 이터레이션할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
`zip` creates a lazy generator that produces tuples, so it can be used on infinitely long inputs.  
**zip은 튜플을 지연 계산하는 제너레이터를 만든다. 따라서 무한히 긴 입력에도 zip을 쓸 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
`zip` truncates its output silently to the shortest iterator if you supply it with iterators of different lengths.  
**입력 이터레이터의 길이가 서로 다르면 zip은 아무런 경고도 없이 가장 짧은 이터레이터 길이까지만 튜플을 내놓고 더 긴 이터레이터의 나머지 원소는 무시한다.**
{{< /admonition >}}

{{< admonition tip >}}
Use the `zip_longest` function from the `itertools` built-in module if you want to use `zip` on iterators of unequal lengths without truncation.  
**가장 짧은 이터레이터에 맞춰 길이를 제한하지 않고 길이가 서로 다른 이터레이터에 대해 루프를 수행하려면 itertools 내장 모듈의 zip_longest 함수를 사용하라.**
{{< /admonition >}}

<br/>

---


## 상세 설명

내부적으로 제너레이터(next 불릴때마다 하나씩 뽑아줌)로 동작한다고 합니다.

```python
>>> names = ['Cecilia', '남궁민수', '毛泽东']
>>> counts = [len(n) for n in names]  # [7, 4, 3]
>>> temp_generator = zip(names, counts)
>>> next(temp_generator)
('Cecilia', 7)
>>> next(temp_generator)
('남궁민수', 4)
>>> next(temp_generator)
('毛泽东', 3)
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter1/Better%20way8.py)
