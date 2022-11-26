# Flutter의 PopupMenuButton 위젯


컨텍스트 메뉴를 추가로 보여주기 위한 `PopupMenuButton` 사용법을 알아보자.
<!--more-->

{{< figure src="/images/logo/flutter.png" >}}

<br/>
<br/>

## 배경 및 코드
리스트로 여러 아이템을 보여줄 때, 각 아이템들에 대해 삭제 등의 기능을 추가하고 싶었다. (more 느낌의 `⫶` 버튼)  
Flutter 에는 `PopupMenuButton`이 해당 역할을 한다.  

{{< figure src="/images/flutter/popupmenubutton_demo_1.png" >}}
{{< figure src="/images/flutter/popupmenubutton_demo_2.png" >}}

<br/>
<br/>

위 스크린 샷은 아래 코드의 결과이다.  

```dart
import 'package:flutter/material.dart';

void main() => runApp(const PopupMenuButtonDemo());

class PopupMenuButtonDemo extends StatelessWidget {
  const PopupMenuButtonDemo({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'PopupMenuButton Demo',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('PopupMenuButton Demo'),
        ),
        body: const PopupMenuButtonPage(),
      ),
    );
  }
}

// 추가 기능 여러개 일때, enum 으로 관리하는 게 좋아 보임
enum MenuType {
  first,
  second,
  third,
}

class PopupMenuButtonPage extends StatelessWidget {
  const PopupMenuButtonPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ListTile(
        title: const Text("Test Item"),
        trailing: PopupMenuButton<MenuType>(  // 오늘의 주인공, PopupMenuButton. 뒤에 <>안에 type 적어주는 게 깔끔해 보임
          // 선택된 버튼에 따라 원하는 로직 수행. (여기서는 SnackBar 표시)
          onSelected: (MenuType result) {             
            final snackBar = SnackBar(
              content: Text("$result is selected."),
            );
            ScaffoldMessenger.of(context).showSnackBar(snackBar);
          },
          // itemBuilder 에서 PopMenuItem 리스트 리턴해줘야 함.
          itemBuilder: (BuildContext buildContext) {
            return [              
              for (final value in MenuType.values)
                PopupMenuItem(
                  value: value,
                  child: Text(value.toString()),
                )
            ];
          },
        ),
      ),
    );
  }
}
```


<br/>

---

## Flutter Gallery 의 예제

Flutter Gallery 의 아래 항목에서 예제를 확인해 볼 수 있다.  
- Material > 메뉴 > 컨텍스트 메뉴 (https://gallery.flutter.dev/#/demo/menu)
- [menu_demo.dart (Github 소스 확인)](https://github.com/flutter/gallery/blob/main/lib/demos/material/menu_demo.dart)
{{< figure src="/images/flutter/flutter_gallery_context_menu.png" >}}

<br/>

App bar 에서도 사용된다.
- Material > App bar (https://gallery.flutter.dev/#/demo/app-bar)
- [app_bar_demo.dart (Github 소스 확인)](https://github.com/flutter/gallery/blob/main/lib/demos/material/app_bar_demo.dart)
{{< figure src="/images/flutter/flutter_gallery_context_appbar.png" >}}

<br/>

---

## Dartpad
적당히 바꿔서 테스트도 해보자.

<iframe style="width:100%;height:700px;" src="https://dartpad.dev/embed-flutter.html?id=7ee8c885b2bf641a9908a5df4f3e87b0&split=60&theme=dark&run=true"></iframe>


<br/>

---

## Reference
- [{tistory 블로그} [플러터 2.0] 버튼 - (2) (ToggleButtons, DropdownButton, PopupMenuButton) / 2021-04-18 / mike123789-dev](https://mike123789-dev.tistory.com/entry/%ED%94%8C%EB%9F%AC%ED%84%B0-20-%EB%B2%84%ED%8A%BC-2-ToggleButtons-DropdownButton-PopupMenuButton)
- [{medium 블로그} PopupMenuButton in Flutter / 2021-01-08 / Apoorv Wadhwa](https://medium.flutterdevs.com/popupmenubutton-in-flutter-bde21c708018)
- [{App Making Academy} Flutter Popup Menu Button Example](https://appmaking.com/flutter-popup-menu-button-example/)
- [{Flutter Open} Widgets 14 | PopupMenuButton](https://flutteropen.gitbook.io/flutter-widgets/flutter-widgets-14-flutter-popup-menu-button)
- [{Flutter Gallery} 컨텍스트 메뉴](https://gallery.flutter.dev/#/demo/menu)
- [{github} Flutter Gallery](https://github.com/flutter/gallery/blob/main/lib/demos/material/menu_demo.dart)

<br/>

---
