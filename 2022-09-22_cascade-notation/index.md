# Dart의 cascade notation (.., dot dot)


`Dart` 에는 cascade notation(`..`, `?..`) 이란게 있음. (점 두개)  
한 object의 여러 method들을 호출할 때 object 이름을 생략하려는 목적으로 보임.  
처음보는 사람은 모를 수 있으니 안쓰는게 깔끔할 것 같은데, 가끔 보이는 것 같으니 알아두도록 하자.


<br/>

---

## 1. cascade notation 사용 예시

```dart
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```

<br/>

아래 코드와 같은 의미

```dart
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;
```

---

## 99. Reference

- [Language Tour - Cascade notation | 공식 문서](https://dart.dev/guides/language/language-tour#cascade-notation)
- [flutter_easyloading | pub.dev](https://pub.dev/packages/flutter_easyloading/example)
<br/>

---
