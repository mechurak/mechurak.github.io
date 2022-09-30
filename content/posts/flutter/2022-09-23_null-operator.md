---
title: "Dart에서 ?? 연산자와 ??= 연산자"
date: 2022-09-23T23:00:00+09:00
summary: "null 일 때 대체 값을 위한 ??, null이 아닐 때만 assigin 하는 ?=="
categories: [ flutter ]
# featuredImage: /images/logo/dart.svg
tags:
- dart
---

- `??` 연산자('null operator' 라고 부르나 봄)는 왼쪽 표현식이 null이 아니면 그걸 취하고, null 이면 오른쪽 표현식을 취함.  
- `??=` 연산자('null-aware operator' 라고 부르나 봄)는 현재 변수의 값이 null 일때만 오른 쪽 값 assign.  

처음보는 사람은 모를 수 있으니 안쓰는게 깔끔할 것 같은데, 가끔 보이는 것 같으니 알아두도록 하자.

<br/>

---

## 1. ?? 연산자

🔶 `??` 연산자 사용해서 함수 구현
```dart
String playerName(String? name) => name ?? 'Guest';
```
<br/>

🔶 삼항 연산자 `(condition) ? (expr1) : (expr2)` 로 같은 함수 구현
```dart
// Slightly longer version uses ?: operator.
String playerName(String? name) => name != null ? name : 'Guest';
```
<br/>

🔶 if-else 로 같은 함수 구현
```dart
// Very long version uses if-else statement.
String playerName(String? name) {
  if (name != null) {
    return name;
  } else {
    return 'Guest';
  }
}
```
<br/>


## 2. ??= 연산자

```dart
// Assign value to a
a = value;
// Assign value to b if b is null; otherwise, b stays the same
b ??= value;
```
<br/>

다른 예제
```dart
void main() {
  int value;
  print(value); // <- null
  value ??= 5;
  print(value); // <- 5, changed from null
  value ??= 6;
  print(value); // <- 5, no change
}
```

<br/>

---

## 99. Reference

- [Language Tour - Conditional expressions | 공식 문서](https://dart.dev/guides/language/language-tour#conditional-expressions)
- [Language Tour - Assignment operators | 공식 문서](https://dart.dev/guides/language/language-tour#assignment-operators)
- [What are ??, ??=, ?., …? in Dart? | medium 블로그](https://jelenaaa.medium.com/what-are-in-dart-df1f11706dd6)

<br/>

---