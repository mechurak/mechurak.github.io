---
title: "'Better way 36. 이터레이터나 제너레이터를 다룰 때는 itertools를 사용하라' 정리"
date: 2020-12-21T01:02:00+09:00
summary: "작성하고 있는 이터레이션 코드가 뭔가 복잡하다면 itertools 에 솔루션이 있을지 모릅니다."
categories: [ python ]
series: [ effective python 4. 컴프리헨션과 제너레이터 ]
series_weight: 36
# featuredImage: http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg
tags:
- python
---

## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 36: Consider `itertools` for Working with Iterators and Generators  
**Better way 36. 이터레이터나 제너레이터를 다룰 때는 itertools를 사용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

작성하고 있는 이터레이션 코드가 뭔가 복잡하다면 itertools 에 솔루션이 있을지 모릅니다.

<br/>

---

## 2. 사용 예시

N/A

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `itertools` functions fall into three main categories for working with iterators and generators: linking iterators together, filtering items they output, and producing combinations of items.  
**이터레이터나 제너레이터를 다루는 itertools 함수는 세 가지 범주로 나눌 수 있다. 여러 이터레이터를 연결함. 이터레이터의 원소를 걸러냄. 원소의 조합을 만들어냄.**
{{< /admonition >}}

{{< admonition tip >}}
There are more advanced functions, additional parameters, and useful recipes available in the documentation at `help(itertools)`.  
**파이썬 인터프리터에서 `help(itertools)`를 입력한 수 표시되는 문서를 살펴보면 더 많은 고급 함수와 추가 파라미터를 알 수 있으며, 이를 사용하는 유용한 방법도 확인할 수 있다.**
{{< /admonition >}}

<br/>

---


## 4. 더 알아보기

### 4.1 이터레이터 연결

**순서대로 연결 (chain)**

```python
# Example 2
it = itertools.chain([1, 2, 3], [4, 5, 6])
print(list(it))  # [1, 2, 3, 4, 5, 6]
```

<br/>

**한 값을 계속 반복 (repeat)**

```python
# Example 3
it = itertools.repeat('hello', 3)
print(list(it))  # ['hello', 'hello', 'hello']
```

<br/>

**계속 반복 (cycle)**

```python
# Example 4
it = itertools.cycle([1, 2])
result = [next(it) for _ in range(10)]
print(result)  # [1, 2, 1, 2, 1, 2, 1, 2, 1, 2]

it = itertools.cycle([1, 2])
print(next(it))  # 1
print(next(it))  # 2
print(next(it))  # 1
print(next(it))  # 2
```

<br/>

**동일한 여러 이터레이터 생성(tee)**  
생성한 이터레이터들은 같은 속도로 쓰는게 좋음

```python
# Example 5
it1, it2, it3 = itertools.tee(['first', 'second'], 3)
print(list(it1))  # ['first', 'second']
print(list(it2))  # ['first', 'second']
print(list(it3))  # ['first', 'second']
```

<br/>

**여러 이터레이터 중 짧은 쪽 다 쓰면 지정한 값 채워줌 (zip\_longest)**  
zip의 변종. zip 은 한 쪽 다 쓰면 끝.

```python
# Example 6
keys = ['one', 'two', 'three']  # 3개
values = [1, 2]  # 2개

normal = list(zip(keys, values))
print('zip:        ', normal)  # zip:         [('one', 1), ('two', 2)]

it = itertools.zip_longest(keys, values, fillvalue='nope')
longest = list(it)
print('zip_longest:', longest)  # zip_longest: [('one', 1), ('two', 2), ('three', 'nope')]
```

<br/>

### 4.2 이터레이터에서 원소 거르기

**인덱스로 슬라이싱(islice)**

```python
import itertools

# Example 7
values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

first_five = itertools.islice(values, 5)  # 끝 지정 (인자 하나만 넘기면 끝)
print('First five: ', list(first_five))  # First five:  [1, 2, 3, 4, 5]

middle_odds = itertools.islice(values, 2, 8, 2)  # 시작, 끝, 증가값 지정
print('Middle odds:', list(middle_odds))  # Middle odds: [3, 5, 7]
```

<br/>

**조건이 True 인 동안 동작(takewhile)**  
False 인 녀석 만나면 끝.

```python
# Example 8
values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 2]
less_than_seven = lambda x: x < 7
it = itertools.takewhile(less_than_seven, values)
print(list(it))  # [1, 2, 3, 4, 5, 6]
```

<br/>

**조건이 True 인 동안 건너 뜀(dropwhile)**  
맨 처음 False 나오면 시작.

```python
# Example 9
values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 2]
less_than_seven = lambda x: x < 7
it = itertools.dropwhile(less_than_seven, values)
print(list(it))  # [7, 8, 9, 10, 2]
```

<br/>

**조건이 False 인 녀석들만(filterfalse)**  
filter 내장 함수의 반대

```python
# Example 10
values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = lambda x: x % 2 == 0

filter_result = filter(evens, values)
print('Filter:      ', list(filter_result))  # [2, 4, 6, 8, 10]

filter_false_result = itertools.filterfalse(evens, values)
print('Filter false:', list(filter_false_result))  # [1, 3, 5, 7, 9]
```

<br/>

### 4.3 이터레이터에서 원소 조합 만들기

**누적 합 혹은 계산(accumulate)**  
이전 결과와 현재 원소로 결과 도출

```python
# Example 11
values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 함수를 지정하지 않으면 두 인자의 합
sum_reduce = itertools.accumulate(values)
print('Sum:   ', list(sum_reduce))  # Sum:    [1, 3, 6, 10, 15, 21, 28, 36, 45, 55]


# 두 개의 인자를 받는 함수
def sum_modulo_20(first, second):
    output = first + second
    return output % 20


modulo_reduce = itertools.accumulate(values, sum_modulo_20)
print('Modulo:', list(modulo_reduce))  # Modulo: [1, 3, 6, 10, 15, 1, 8, 16, 5, 15]
```

<br/>

**데카르트 곱(product)**

```python
# Example 12
single = itertools.product([1, 2], repeat=2)
print('Single:  ', list(single))  # Single:   [(1, 1), (1, 2), (2, 1), (2, 2)]

multiple = itertools.product([1, 2], ['a', 'b'])
print('Multiple:', list(multiple))  # Multiple: [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]
```

<br/>

**순열(permutations)과 조합(combinations, combinations\_with\_replacement)**

```python
# Example 13
it = itertools.permutations([1, 2, 3], 2)
print(list(it))  # [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]


# Example 14
it = itertools.combinations([1, 2, 3], 2)
print(list(it))  # [(1, 2), (1, 3), (2, 3)]

# Example 15 (원소의 반복 허용)
it = itertools.combinations_with_replacement([1, 2, 3], 2)
print(list(it))  # [(1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)]
```

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_36.py)
- [itertools — Functions creating iterators for efficient looping / 파이썬 공식 문서](https://docs.python.org/3/library/itertools.html?highlight=itertools)

<br/>

---