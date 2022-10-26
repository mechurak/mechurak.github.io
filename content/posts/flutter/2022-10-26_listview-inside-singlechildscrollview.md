---
title: "SingleChildScrollView 안에 ListView 배치"
date: 2022-10-26T23:00:00+09:00
categories: [ flutter ]
tags:
- flutter
- layout
---

`ListView` 위에 아래에 위젯을 따로 배치하고 싶으면 `Column` 을 감싸고 있는 `SingleChildScrollView`를 이용하자.  
<!--more-->

{{< figure src="/images/logo/flutter.png" >}}

<br/>
<br/>

## 배경 및 코드
아래 그림과 같이 ListView 아래에 다른 위젯(적당한 여벽 후 버튼 모음)을 배치하고 싶었다.

{{< figure src="/images/flutter/listview_inside_singlechildscrollview.jpg" >}}

<br/>
<br/>


검색하던 중 [TOP 12 ListView Widgets - 5. SingleChildScrollView](https://www.youtube.com/watch?v=L3NJkkOC4Ko&t=431s) 영상를 발견했다.  

<br/>

결론은 아래 코드 느낌으로 `SingleChildScrollView` 를 사용하면 된다.  
내부의 `Listview` 에 `shrinkWrap: true` (리스트뷰 크기 고정), `primary: false` (리스트뷰 내부는 스크롤 없음) 를 지정하는 것이 포인트!!  


```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('ListView inside SingleChildScrollView'),
        ),
        body: SingleChildScrollView(  // <==== 주인공. Column 하나를 child로 가짐
          child: Column(  // 물론 Row도 가능
            children: [
              Container(
                height: 150,
                color: Colors.red,
                child: const Center(
                  child: Text("Some Widgets"),
                ),
              ),
              ListView.builder(  // <=== 성능을 생각한다면 ListView.builder 로 사용
                shrinkWrap: true, // <==== limit height. 리스트뷰 크기 고정
                primary: false, // <====  disable scrolling. 리스트뷰 내부는 스크롤 안할거임
                itemCount: 10,
                itemBuilder: (context, index) => ListTile(
                  title: Text("Item ${index + 1}"),
                ),
              ),
              Container(
                height: 150,
                color: Colors.green,
                child: const Center(
                  child: Text("Some Widgets"),
                ),
              ),
              Container(
                height: 150,
                color: Colors.blue,
                child: const Center(
                  child: Text("Some Widgets"),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

<br/>

이렇게 하면 `ListView` 도 크기가 정해진 하나의 위젯으로 보고 `Column` 안에 적절히 배치할 수 있다.  
(물론, `Column` 대신에 `Row`도 가능하다.)  
전체 스크롤은 `SingleChildScrollView` 책임지고 있다.

<br/>

---

## Dartpad
적당히 바꿔서 테스트도 해보자.

<iframe style="width:100%;height:700px;" src="https://dartpad.dev/embed-flutter.html?id=46f62f6f46e44b041a59e936bfe5eb4c&split=60&theme=dark&run=true"></iframe>


<br/>

---

## Reference
- [{유튜브} TOP 12 ListView Widgets - 5. SingleChildScrollView](https://www.youtube.com/watch?v=L3NJkkOC4Ko&t=431s) 
- [{API문서} SingleChildScrollView class](https://api.flutter.dev/flutter/widgets/SingleChildScrollView-class.html)

<br/>

---