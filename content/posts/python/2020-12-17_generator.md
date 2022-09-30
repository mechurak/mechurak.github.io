---
title: "'Better way 30. 리스트를 반환하기보다는 제너레이터를 사용하라' 정리"
date: 2020-12-17T01:02:00+09:00
summary: "제너레이터는 이터레이터를 반환하는 함수인데, 좋습니다."
categories: [ python ]
series: [ effective python 4. 컴프리헨션과 제너레이터 ]
series_weight: 30
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
<4. Comprehensions and Generators>  
Item 30: Consider Generators Instead of Returning Lists  
**Better Way 30. 리스트를 반환하기보다는 제너레이터를 사용하라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

제너레이터는 이터레이터를 반환하는 함수인데, 이터레이터가 `next` 내장 함수를 호출할 때마다 `yield` 식을 만날 때까지 제너레이터 함수가 진행됩니다.  
입력이 아주 커도 메모리 이슈 없이 잘 동작하고, 코드도 깔끔해지니 적극 고려합시다.

<br/>

---

## 사용 예시

문장의 각 단어들의 인덱스에 대해서 뭔가를 해야 하는 상황

```python
address = 'Four score and seven years ago...'


# Example 3 (제너레이터)
def index_words_iter(text):
    if text:
        yield 0  # yield
    for index, letter in enumerate(text):
        if letter == ' ':
            yield index + 1  # yield


# 실제로는 이런식으로 사용
for i in index_words_iter(address):
    pass

# Example 4
it = index_words_iter(address)
print(next(it))  # 0
print(next(it))  # 5
print(list(it))  # [11, 15, 21, 27]. 이터레이터에 상태가 있으므로 주의! 재사용 불가.

# Example 5
result = list(index_words_iter(address))  # 리스트로 쉽게 변환 가능
print(result[:10])  # [0, 5, 11, 15, 21, 27]
```

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
> Using generators can be clearer than the alternative of having a function return a `list` of accumulated results.  
> **제너레이터를 사용하면 결과를 리스트에 합쳐서 반환하는 것보다 더 깔끔하다.**
{{< /admonition >}}

{{< admonition tip >}}
> The iterator returned by a generator produces the set of values passed to `yield` expressions within the generator function's body.  
> **제너레이터가 반환하는 이터레이터는 제너레이터 함수의 본문에서 yield가 반환하는 값들로 이뤄진 집합을 만들어낸다.**
{{< /admonition >}}

{{< admonition tip >}}
> Generators can produce a sequence of outputs for arbitrarily large inputs because their working memory doesn't include all inputs and outputs.  
> **제너레이터를 사용하면 작업 메모리에 모든 입력과 출력을 저장할 필요가 없으므로 입력이 아주 커도 출력 시퀀스를 만들 수 있다.**
{{< /admonition >}}

<br/>

---

## 리스트로 구현 한다면

```python
address = 'Four score and seven years ago...'


# Example 1
def index_words(text):
    result = []
    if text:
        result.append(0)
    for index, letter in enumerate(text):
        if letter == ' ':
            result.append(index + 1)
    return result


result = index_words(address)
print(result[:10])  # [0, 5, 11, 15, 21, 27]
```

코드에 잡음이 많고 핵심을 알아보기 어렵습니다.  
반환하기 전에 리스트에 모든 결과를 다 저장해야 합니다. 입력이 매우 큰 경우에 문제가 될 수 있습니다.


<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_30.py)

<br/>

---