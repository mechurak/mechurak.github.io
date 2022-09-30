# 'Better way 10. 대입식을 사용해 반복을 피하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다. 혹 누군가에게도 도움이 되기를 바랍니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<1. Pythonic Thinking>  
Item 10: Prevent Repetition with Assignment Expressions  
**Better way 10. 대입식을 사용해 반복을 피하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

if문이나 while문 등의 조건식에 왈러스 연산자`:=`를 활용하면 코드 중복을 없앨 수 있습니다.  
표현식 안에 쓰여서 대입 후 실제 평가가 이루어 집니다.  

'왈러스'라는 이름은 연산자가 바다코끼리(walrus)의 눈과 어금니처럼 생겨서 붙여졌다고 합니다.
![walrus](https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Walrus2.jpg/320px-Walrus2.jpg)

<br/>

---

## 사용 예시

```python
a = [10, 2 , 34, 4, 5, 1]

if n := len(a):  # 변수 'n' 정의 및 대입. 이 후 if 조건 평가
    print(f'List has {n} elements.')  # 블록 안에서도 계산된 'n' 사용

print(f'outside. n: {n}')  # `n` 은 여기서도 유효함. (if 바로 앞 줄에서 n 을 대입한 것과 같은 결과)

>>>
List has 6 elements.
outside. n: 6
```

if 조건식 안에서 대입식 사용

<br/>

```python
if (n := len(a)) > 10:  # 변수 'n' 정의 및 대입. 이 후 if 조건 평가
    print(f"List is too long ({n} elements, expected <= 10)")
```

더 큰 표현식의 일부라면 괄호로 감싸야 함. (`:=`의 연산자 우선순위가 부등호 등보다 낮음)


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Assignment expressions use the walrus operator (`:=`) to both assign and evaluate variable names in a single expression, thus reducing repetition.  
**대입식에서는 왈러스 연산자(:=)를 사용해 하나의 식 안에서 변수 이름에 값을 대입하면서 이 값을 평가할 수 있고, 중복을 줄일 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
When an assignment expression is a subexpression of a larger expression, it must be surrounded with parentheses.  
**대입식이 더 큰 식의 일부분으로 쓰일 때는 괄로로 둘러싸야 한다.**
{{< /admonition >}}

{{< admonition tip >}}
Although switch/case statements and do/while loops are not available in Python, their functionality can be emulated much more clearly by using assignment expressions.  
**파이썬에서는 switch/case 문이나 do/while 루프를 쓸 수 없지만, 대입식을 사용하면 이런 기능을 더 깔끔하게 흉내 낼 수 있다.**
{{< /admonition >}}

<br/>

---

## 추가 사용 예시

```python
# Loop over fixed length blocks
while (block := f.read(256)) != '':
    process(block)
```

while 문의 조건식 안에서 대입식 사용

<br/>

```python
fresh_fruit = {
    '사과': 10,
    '바나나': 8,
    '레몬': 5,
}

if (count := fresh_fruit.get('바나나', 0)) >= 2:
    pieces = slice_bananas(count)
    to_enjoy = make_smoothies(pieces)
elif (count := fresh_fruit.get('사과', 0)) >= 4:
    to_enjoy = make_cider(count)
elif count := fresh_fruit.get('레몬', 0):
    to_enjoy = make_lemonade(count)
else:
    to_enjoy = '아무것도 없음'
```

파이썬에는 없는 switch/case 느낌으로 사용


<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter1/Better%20way10.py)
- [What’s New In Python 3.8 (파이썬 공식 문서)](https://docs.python.org/3/whatsnew/3.8.html)
