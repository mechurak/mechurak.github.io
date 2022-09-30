---
title: "with 구문 지원하기 (With Statement Context Manager)"
date: 2020-11-29T01:03:00+09:00
summary: "자원 관리가 필요한 클래스라면 with를 지원해 봅시다."
categories: [ python ]
# featuredImage: /images/python.jpg
enableToc: false
tags:
- python
---

파일 읽을 때, 보통 `with` 구문을 사용합니다. 그러면 `with` 구문이 끝날 때 해당 파일의 `close()`가 자동으로 불리게 되지요.

```python
with open('output.txt', 'w') as output_file:
    output_file.write('Life is too short, You need Python')    

# 아래와 같은 의미
output_file = open('output.txt', 'w')
try:
    output_file.write('Life is too short, You need Python')
finally:
    output_file.close()
```

<br/>
<br/>

직접 만든 클래스도 `with` 구문을 지원하게 만들 수 있는데, 이는 사용 측에서 해당 인스턴스의 사용 준비(`__enter__`) 및 리소스 정리(`__exit__`) 작업을 신경 안써도 되는 효과가 있습니다.

이를 위해, `__enter__`(with 구문 시작할 때 사용 준비),`__exit__`(with 구분 끝날 때 정리 작업) 을 구현해주면 됩니다.


<br/>

---


## 사용 예시
[스택오버플로우 답변](https://stackoverflow.com/a/1984346) 내용입니다.

```python
# with 구문 지원하는 클래스
class DatabaseConnection(object):

    def __enter__(self):
        # make a database connection and return it
        ...
        return self.dbconn # as 로 받아서 쓸 인스턴스. 보통은 self 를 리턴할 듯

    def __exit__(self, exc_type, exc_val, exc_tb):
        # make sure the dbconnection gets closed
        self.dbconn.close()
        ...
```

<br/>
<br/>

해당 클래스의 사용측은 아래처럼 with 구문으로 사용하면 됩니다.

```python
# 해당 클래스 사용처
with DatabaseConnection() as mydbconn:
    # do stuff
```


<br/>
<br/>

`__enter__` 에서 보통은 `self`를 리턴할 것 같은데, 아닌 케이스도 가끔 있나 봅니다.  
요건 케이스가 나오면 나중에 다시 고민해보도록 하겠습니다.


<br/>

---

## Reference

- [The with statement (파이썬 공식 문서)](https://docs.python.org/3/reference/compound_stmts.html#with)
- [With Statement Context Managers (파이썬 공식 문서)](https://docs.python.org/3/reference/datamodel.html#with-statement-context-managers)
- [Context Managers (Python Tips)](https://book.pythontips.com/en/latest/context_managers.html)
- [Explaining Python's `__enter__` and `__exit__` (StackOverflow 답변)](https://stackoverflow.com/a/1984346)