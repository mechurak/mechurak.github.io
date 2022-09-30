---
title: "'Better way 16. in을 사용하고 딕셔너리 키가 없을 때 KeyError를 처리하기보다는 get을 사용하라' 정리"
date: 2020-12-10T01:00:00+09:00
summary: "딕셔너리 키가 없을 때 처리는 get을 사용하는 게 좋습니다."
categories: [ python ]
series: [ effective python 2. 리스트와 딕셔너리 ]
series_weight: 16
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
<2. Lists and Dictionaries>
Item 16: Prefer get Over in and KeyError to Handle Missing Dictionary Keys
**Better Way 16. in을 사용하고 딕셔너리 키가 없을 때 KeyError를 처리하기보다는 get을 사용하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

딕셔너리 키가 없을 때 처리는 `get`을 사용하는 게 좋습니다.

<br/>

---

## 사용 예시

```python
counters = {
    '품퍼니켈': 2,
    '사워도우': 1,
}

key = '밀'
count = counters.get(key, 0)  # key 가 딕셔너리에 없으면 0(디폴트 값) 리턴. 두 번째 파라미터 없으면 None 리턴
counters[key] = count + 1

print(counters)
# {'품퍼니켈': 2, '사워도우': 1, '밀': 1}
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
There are four common ways to detect and handle missing keys in dictionaries: using `in` expressions, `KeyError` exceptions, the `get` method, and the `setdefault` method.  
**딕셔너리 키가 없는 경우를 처리하는 방법으로는 in 식을 이용하는 방법, KeyError 예외를 사용하는 방법, get 메서드를 사용하는 방법, setdefault 메서드를 사용하는 방법이 있다.**
{{< /admonition >}}

{{< admonition tip >}}
The `get` method is best for dictionaries that contain basic types like counters, and it is preferable along with assignment expressions when creating dictionary values has a high cost or may raise exceptions.  
**카운터와 같이 기본적인 타입의 값이 들어가는 딕셔너리를 다룰 때는 get 메서드가 가정 좋고, 딕셔너리에 넣을 값을 만드는 비용이 비싸거나 만드는 과정에 예외가 발생할 수 있는 경우에도 get 메서드를 사용하는 편이 낫다.**
{{< /admonition >}}

{{< admonition tip >}}
When the `setdefault` method of `dict` seems like the best fit for your problem, you should consider using `defaultdict` instead.  
**해결하려는 문제에 dict의 setdefault 메서드를 사용하는 방법이 가장 적합해 보인다면 setdefault 대신 defaultdict를 사용할지 고려해 보라.**
{{< /admonition >}}

<br/>

---

## 상세 설명

### in, KeyError 로 확인 (안티패턴)

아래 처럼 하지 말고, `get`을 추천합니다.

```python
counters = {
    '품퍼니켈': 2,
    '사워도우': 1,
}
key = '밀'

# 해당 키가 존재하는 지 'in' 으로 확인 (요것보다 get이 좋음)
if key in counters:
    count = counters[key]
else:
    count = 0
counters[key] = count + 1

# KeyError 예외 처리 (요것보다 get이 좋음) 
try:
    count = counters[key]
except KeyError:
    count = 0
counters[key] = count + 1
```

### setdefault

요거는 키가 없는 경우에, 넘겨준 디폴트 값을 바로 딕셔너리에 추가를 해줍니다.  
하지만, 네이밍과 동작이 직관적으지 않을 수 있으니 사용을 지향해야 한다고 합니다. (`defaultdict` 고려)

```python
votes = {
    '바게트': ['철수', '순이'],
    '치아바타': ['하니', '유리'],
}

# get() 으로 key 존재 여부 확인
key = '브리오슈'
who = '단이'
names = votes.get(key)
if names is None:
    votes[key] = names = []
names.append(who)
print(votes)
# {'바게트': ['철수', '순이'], '치아바타': ['하니', '유리'], '브리오슈': ['단이']}


# setdefault() 로 key 없으면 바로 딕셔너리에 추가
key = '베이글'
who = '현아'
names = votes.setdefault(key, [])
names.append(who)
print(votes)
# {'바게트': ['철수', '순이'], '치아바타': ['하니', '유리'], '브리오슈': ['단이'], '베이글': ['현아']}
```

<br/>

---


## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way16.py)

<br/>

---