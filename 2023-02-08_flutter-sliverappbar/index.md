# Flutter, SliverAppBar로 AppBar도 같이 스크롤


AppBar 를 화면 위에 고정시켜두면 뭔가 화면을 비효율적으로 쓰는 느낌이다. 이미 많은 앱들이 AppBar 도 같이 스크롤 되게 동작하고 있었다. Flutter에는 이러한 동작을 위해 `CustomScrollView`, `SliverAppBar` 가 준비되어 있다.
<!--more-->

{{< image src="/images/logo/flutter.png" caption="Flutter">}}

<br/>
<br/>

## 핵심 요약
- `CustomScrollView` 의 slivers 안에 `SliverAppBar` 배치
- 그 다음으로 주로 `SliverList`로 list형 자료를 표현
- 개별 항목이 필요하면 `SliverToBoxAdapter`

```dart
CustomScrollView(  
  slivers: [  
    SliverAppBar(  // 오늘의 주인공!!
      expandedHeight: 100.0,  // 전체 보였을때 크기
      flexibleSpace: FlexibleSpaceBar(  // 확장되는 영역
        title: Text('SilverAppBar Example'),  
        background: FlutterLogo(),  
      ),  
    ),  
    SliverList(  // 리스트형 자료 표현
      delegate: SliverChildBuilderDelegate(  // 빌더 사용해서 효율적으로
        (BuildContext context, int index) {  
          return Placeholder();  // index와 리스트 이용해서 해당 아이템 표현
        },  
        childCount: 10,  // 리스트의 아이템 개수
      ),  
    ),  
    SliverToBoxAdapter(  // 단일 위젯은 요걸로
      child: Placeholder(),  // 개별 위젯
    ),  
  ],  
),
```

<br/>

{{< image src="/images/flutter/SilverAppBar.gif" caption="결과 예시">}}

맨 위의 AppBar도 같이 스크롤 된다.

<br/>

---

## SliverAppBar의 세부 옵션

파라미터가 많다. 그 중에서도 `floating`, `pinned`, `snap`이 동작에 큰 영향을 주는 듯 하다.
- `floating`: 위로 스크롤 했을때 SliverAppBar가 나오는지 여부. (default: `false`)
- `pinned`: 아래로 스크롤 했을때 title 이 남아 있는지 여부 (default: `false`)
- `snap`: 살짝만 스크롤해도 SliverAppBar를 전체로 확장/축소하는지 여부. `floating`가 `true` 일때만 의미 있음 (default: `false`)

<br/>

설명만으로는 살짝 부족한데, [SliverAppBar API 문서의 예제](https://api.flutter.dev/flutter/material/SliverAppBar-class.html#material.SliverAppBar.2) 에서 옵션에 따른 차이를 바로 확인해 볼 수 있다. 필요하다고 느껴지면 살펴보도록 하자.

<br/>

---

## Dartpad에서 확인
적당히 바꿔서 테스트도 해보자.

<iframe style="width:100%;height:800px;" src="https://dartpad.dev/embed-flutter.html?id=ea41f9e273c88e3dfb23f2e7dac5f2a9&split=60&theme=dark&run=true"></iframe>

아래쪽에 배너광고를 넣을까 싶어서 `Column`으로 감싸 주었고, 아래쪽의 Fixed Widgets 의 사이즈를 정하고, 나머지 부분을 CustomScrollView가 모두 차지하도록 `Expanded`로 감쌌다.

<br/>

---

## Etc
sliver의 단어 뜻

{{< admonition type=tip title="sliver" >}}
a very small, thin piece of something, usually broken off something larger
- [Cambridge Dictionary](https://dictionary.cambridge.org/us/dictionary/english/sliver)
{{< /admonition >}}

{{< admonition type=tip title="sliver" open=true >}}
가늘고 긴 조각. 쪼개진 조각
- [다음 사전](https://dic.daum.net/search.do?q=sliver)
{{< /admonition >}}

<br/>

---

## Reference
- [{API 문서} SilverAppBar class](https://api.flutter.dev/flutter/material/SliverAppBar-class.html)
- [{API 문서} CustomScrollView class](https://api.flutter.dev/flutter/widgets/CustomScrollView-class.html)
- [{티스토리 블로그} CustomScrollView / 코딩전사의 기록](https://dalgoodori.tistory.com/62)
- [{외국 블로그} How to add SliverAppBar to your Flutter app](https://blog.logrocket.com/how-to-add-sliverappbar-to-your-flutter-app/)
- [{Flutter Cookbook} Place a floating app bar above a list](https://docs.flutter.dev/cookbook/lists/floating-app-bar)

<br/>

---
