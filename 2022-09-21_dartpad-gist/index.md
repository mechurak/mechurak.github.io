# DartPad와 Gist를 활용한 소스 공유


[DartPad](https://dartpad.dev/)에서 dart 코드(flutter 포함)를 테스트 해 볼 수 있다.  
[Gist](https://gist.github.com/) 에 `.dart` 소스를 저장하고, 해당 id를 DartPad의 url에 추가하면 해당 소스가 보인다.



<br/>

---

## 1. DartPad

[DartPad](https://dartpad.dev/)에서 간단한 dart 소스 테스트 가능하다.  
flutter 소스도 아래처럼 실행해 볼 수 있다.  
'sample' 버튼을 눌러 보면 dart 및 flutter 샘플이 몇 개 있다.

![dartpad1.jpg](/images/flutter/dartpad1.jpg)

단, 저장 기능은 없다. 대신에 Gist 를 활용해서 저장 효과를 볼 수 있다.

<br/>


## 2. Gist 활용한 공유
먼저 [DartPad](https://dartpad.dev/)에서 실행이 잘 되는 것을 확인한 코드를 [Gist](https://gist.github.com/)에 public gist로 생성한다.

![dartpad1.jpg](/images/flutter/dartpad_gist.jpg)

위 스크린샷은 https://gist.github.com/mechurak/a3109d6d8c9c4c67191cd36152560667 에 gist가 생성되었다.  
여기서 빨간색으로 표시한 id 부분 (여기서는 `a3109d6d8c9c4c67191cd36152560667`)을  
`https://dartpad.dev/` 뒤에 붙여서 주소창에 넣으면 (https://dartpad.dev/a3109d6d8c9c4c67191cd36152560667),  
해당 Gist 소스를 보여주는 [DartPad](https://dartpad.dev/a3109d6d8c9c4c67191cd36152560667)가 바로 열린다.

결과물을 공유할때 유용하게 사용할 수 있을 것으로 보인다.

아래처럼 embedding 도 가능하다.

```html
<!--일반 dart : embed-inline.html 사용. theme=dark -->
<iframe style="width:100%;height:400px;" src="https://dartpad.dev/embed-inline.html?id=5d70bc1889d055c7a18d35d77874af88&split=80&theme=dark"></iframe>

<!--flutter : embed-flutter.html 사용 -->
<iframe style="width:100%;height:400px;" src="https://dartpad.dev/embed-flutter.html?id=a3109d6d8c9c4c67191cd36152560667"></iframe>
```

<br/>
<br/>

일반 dart 예제 embedding (dark 테마)
<iframe style="width:100%;height:400px;" src="https://dartpad.dev/embed-inline.html?id=5d70bc1889d055c7a18d35d77874af88&split=80&theme=dark"></iframe>

<br/>
<br/>

flutter 예제 embedding
<iframe style="width:100%;height:400px;" src="https://dartpad.dev/embed-flutter.html?id=a3109d6d8c9c4c67191cd36152560667"></iframe>

---

## 99. Reference

- [DartPad](https://dartpad.dev/)
- [Saring Guide | 공식 가이드](https://github.com/dart-lang/dart-pad/wiki/Sharing-Guide)
- [Embedding Guide | 공식 가이드](https://github.com/dart-lang/dart-pad/wiki/Embedding-Guide)

<br/>

---
