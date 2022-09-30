---
title: "리스트 컴프리헨션 (List Comprehension)"
date: 2020-11-26T01:00:00+09:00
summary: "리스트 컴프리헨션을 알아봅시다."
categories: [ python ]
# featuredImage: /images/python.jpg
tags:
- python
---

기존 리스트에서 약간의 변형이 필요하거나 필터링이 필요할 때, 리스트 컴프리헨션을 이용해 짧은 코드로 원하는 리스트를 손쉽게 만들 수 있습니다.

<br/>

아래는 각 과일에 "new_"를 붙인 새로운 리스트를 만들었습니다.

```python
>>> fruits = ['apple', 'banana', 'melon']
>>> ['new_' + fruit for fruit in fruits]
['new_apple', 'new_banana', 'new_melon']
```

<br/>


필터를 적용해서, 조건에 맞는 원소로만 리스트를 만들 수도 있습니다.

```python
>>> nums = [-1, -2, 5, 8, -5, 20]
>>> new_nums = [num * 2 for num in nums if num > 0]
>>> new_nums
[10, 16, 40]
```

<br/>

기본 문법은 아래와 같습니다.

```python
[<expression> for <variable_name> in <sequence> if <condition>]
```

<br/>


위 문법을 풀어쓰면 아래와 같은 의미가 됩니다.

```python
result = []
for variable_name in sequence:
    if condition:
        result.append(expression)
```

<br/>

***

## Reference

- [Practical Python Programming](https://dabeaz-course.github.io/practical-python/Notes/02_Working_with_data/06_List_comprehension.html)  
- [The Python Tutorial](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)