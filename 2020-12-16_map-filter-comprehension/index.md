# 'Better way 27. map과 filter 대신 컴프리헨션을 사용하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 27: Use Comprehensions Instead of `map` and `filter`  
**Better Way 27. map과 filter 대신 컴프리헨션을 사용하라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

기존 리스트에서 파생하는 간단한 리스트 생성은 리스트 컴프리헨션을 활용하면 편리합니다.  
([리스트 컴프리헨션](https://mechurak.github.io/ko/posts/python/2020-11-26_list-comprehension/) 참고)

<br/>

---

## 사용 예시

```python
# Example 2 (List comprehension 으로 list 생성)
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
squares = [x**2 for x in a]
print(squares)  # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# Example 4 (List comprehension 에 if 도 사용)
even_squares = [x**2 for x in a if x % 2 == 0]
print(even_squares)  # [4, 16, 36, 64, 100]

# Example 6 (dict, set 도 comprehension 으로 생성)
even_squares_dict = {x: x**2 for x in a if x % 2 == 0}
threes_cubed_set = {x**3 for x in a if x % 3 == 0}
print(even_squares_dict)  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}
print(threes_cubed_set)  # {216, 729, 27}
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
List comprehensions are clearer than the `map` and `filter` built-in functions because they don't require `lambda` expressions.  
**리스트 컴프리헨션은 lambda 식을 사용하지 않기 때문에 같은 일을 하는 map과 filter 내장 함수를 사용하는 것보다 더 명확하다.**
{{< /admonition >}}

{{< admonition tip >}}
List comprehensions allow you to easily skip items from the input `list`, a behavior that `map` doesn't support without help from `filter`.  
**리스트 컴프리헨션을 사용하면 쉽게 입력 리스트의 원소를 건너뛸 수 있다. 하지만 map을 사용하는 경우에는 filter의 도움을 받아야만 한다.**
{{< /admonition >}}

{{< admonition tip >}}
Dictionaries and sets may also be created using comprehensions.  
**딕셔너리와 집합도 컴프리헨션으로 생성할 수 있다.**
{{< /admonition >}}

<br/>

---

## map, filter를 사용한다면

List Comprehension 이 없다면 아래처럼 구현했어야 합니다.

```python
# Example 1 (Example 2와 같은 동작)
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
squares = []
for x in a:
    squares.append(x**2)
print(squares)  # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]


# Example 3 (map 사용해서 Example 2와 같은 동작)
alt = map(lambda x: x ** 2, a)  # map 객체 (이터레이터)
print(alt)  # <map object at 0x02ECC370>
print(list(alt))  # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
<br/>

filter 까지 사용한 경우

```python
# Example 5 (map, filter 사용해서 Example 4와 같은 동작)
alt = map(lambda x: x**2, filter(lambda x: x % 2 == 0, a))
print(list(alt))  # [4, 16, 36, 64, 100]

# filter 참고
temp = filter(lambda x: x % 2 == 0, a)
print(list(temp))  # [2, 4, 6, 8, 10]
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_27.py)

<br/>

---
