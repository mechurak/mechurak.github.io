# flutter에서 theme 사용 및 TextStyle, Color 관리


`Color`, `TextStyle`을 앱 전체에서 일관되게 적용을 위해 `Theme`을 사용하자.  
<!--more-->

{{< figure src="/images/logo/flutter.png" >}}


## 요약
아래 코드 느낌으로 theme 정의해서 사용 가능하다.   
[google_fonts 패키지](https://pub.dev/packages/google_fonts) 사용하면 폰트 적용도 쉽게 해 볼 수 있다.  

```dart
import 'package:flutter/material.dart';  
import 'package:google_fonts/google_fonts.dart';

class EllasNotesThemeData {  
  static ThemeData lightThemeData = themeData();  // 실제 쓸 때는 요걸로 쓸 거임
  
  static ThemeData themeData() {  // 실제 ThemeData 만듬
    final base = ThemeData.light();  
    return base.copyWith(  
      textTheme: _buildEllasNotesTextTheme(base.textTheme),  
      // ...
      // textTheme 외에도 appBarTheme, primaryTheme, colorScheme 등 override 할 수 있는 항목 매우 많음
    );  
  }  
  
  static TextTheme _buildEllasNotesTextTheme(TextTheme base) {  // TextTheme 생성
    return base.copyWith(  
      titleLarge: GoogleFonts.robotoSlab(textStyle: base.titleLarge), // main text  
      bodyMedium: GoogleFonts.nanumGothic(textStyle: base.bodyMedium), // note  
      // ...
    );  
  }  
}

```

<br/>

앱 진입 부분에서 theme 지정
```dart
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      title: "Ella's Notes",  
      theme: EllasNotesThemeData.lightThemeData,  // 위에서 정의한 theme 적용!!
      initialRoute: 'home',  
      routes: {  
        "home": (context) => const HomePage(),  
        "game": (context) => const GamePage(),  
      },  
      builder: EasyLoading.init(),  
    );  
  }  
}

```

<br/>

---


## TextStyle, Color 모아두기
theme 으로 아래처럼 `TextStyle`와 `Color` 들을 모아두고, 사용하는 것도 괜찮아 보인다.  
(참고: [{책} 쉽고 빠른 플러터 앱 개발 / 권태형 / 2022.04.28 / 8.3. 앱 스타일 파일로 정의하여 관리하기](https://product.kyobobook.co.kr/detail/S000001842218))

```dart
import 'package:flutter/material.dart';  
  
class TextStyles {  
  static const hintTextStyle = TextStyle(fontStyle: FontStyle.italic);  
  static const noteTextStyle = TextStyle(fontSize: 12, color: Colors.black54);  
  static const memoTextStyle = TextStyle(fontSize: 12, color: Colors.black45, fontStyle: FontStyle.italic);  
  // ...
}
```


```dart
import 'package:flutter/material.dart';  
  
class ColorStyles {  
  static const Color darkGray = Color(0xff494949);  
  // ...
}
```

<br/>

---

## 추가 텍스트 이름 지정
[`TextTheme`](https://api.flutter.dev/flutter/material/TextTheme-class.html) 에서 제공하는 이름 외에 다른 이름을 만들어서 쓰고 싶으면, 아래와 같이 사용 가능하다.  
(참고: [{stackoverflow} How to define custom text theme in flutter?](https://stackoverflow.com/a/53797935/16111308))


```dart
class CustomTextStyle {  
  static TextStyle hint(BuildContext context) {  
    return GoogleFonts.nanumBrushScript(textStyle: Theme.of(context).textTheme.titleLarge);  
  }  
  
  static TextStyle memo(BuildContext context) {  
    return Theme.of(context).textTheme.bodyMedium!.copyWith(  
          color: ColorStyles.darkGray,  
        );  
  }  
}
```

<br/>

---

## 샘플 프로젝트, Gallery
[Gallery](https://flutter.github.io/samples/gallery.html) 프로젝트가 flutter 대표 샘플 프로젝트로 보이는데, 웹에서 바로 실행해 볼 수도 있다.  

{{< figure src="https://flutter.github.io/samples/images/gallery1.png" >}}

소스코드에 각 하위 샘플앱들의 theme 적용 부분도 꽤 있어서, 요 프로젝트들에서 어떻게 활용하는지 참고하면 좋을 듯 하다.  
- lib/studies 밑의 각 하위 샘플앱들 : `app.dart` 혹은 `theme.dart` 에서 theme 정의
- lib/themes 의 파일들 : 첫 화면의 theme 정의

<br/>

---

## Reference
- [{Flutter 문서} Use themes to share colors and font styles](https://docs.flutter.dev/cookbook/design/themes)
- [{책} 쉽고 빠른 플러터 앱 개발 / 권태형 / 2022.04.28 / 8.3. 앱 스타일 파일로 정의하여 관리하기](https://product.kyobobook.co.kr/detail/S000001842218)
- [{Flutter 문서} TextTheme class](https://api.flutter.dev/flutter/material/TextTheme-class.html)
- [{stackoverflow} How to define custom text theme in flutter?](https://stackoverflow.com/a/53797935/16111308)
- [{pub.dev} google_fonts 패키지](https://pub.dev/packages/google_fonts)

<br/>

---
