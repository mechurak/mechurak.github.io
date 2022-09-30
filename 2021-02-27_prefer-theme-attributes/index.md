# 'Android Styling: prefer theme attributes' 번역


스터디 하면서 샘플 프로젝트 따라 해보다가, 조금 된 예제를 따라하다보니 뭔가 theme 관련해서 마음대로 안되는 상황이 왔습니다.

찾다보니 요 시리즈를 알아두면 되겠다 싶네요.

아래 포스트를 번역한 내용입니다.(안드로이드 공식 블로그인듯 합니다.)

[Android Stylling: prefer theme attributes](https://medium.com/androiddevelopers/android-styling-prefer-theme-attributes-412caa748774)

![android.png](/images/logo/android.png)


<br/>

---

## Android Styling: prefer theme attributes

Android styling에 관한 이 시리즈의 지난 포스트들에서,  
theme과 style의 차이점을 알아봤고,  
theme과 theme attribute들을 통해 변화를 줘야할 부분들을 별도로 분리하는 게 왜 좋은지 알아 봤습니다.

[Android Styling: Themes vs Styles](https://medium.com/androiddevelopers/android-styling-themes-vs-styles-ebe05f917578)
[Android Styling: Common Theme Attributes](https://medium.com/androiddevelopers/android-styling-common-theme-attributes-8f7c50c9eaba)

이것은 변경을 theme으로 한정하고, 더 적은 layout과 style만 유지해도 되게 합니다.  
실전에서, theme별로 색을 다르게 해야 하는 경우가 대부분이고, 이 때 **항상\*** theme attribute를 참조해야 합니다.

> Always\* refer to colors via theme attributes

즉, 아래 코드는 문제가 될 수 있습니다.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<View …
  android:background="@color/white"/>
```

<br/>

대신에 theme attribute를 참조해야 합니다. 그래야 theme별로 색상에 변화를 줄 수 있습니다. 예를 들면 [dark theme](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme)에서 다른 값을 제공할 수 있죠.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<View …
  android:background="?attr/colorSurface"/>
```

만드시는 앱이 아직 다른 theme을 지원하지 않더라도(아직 dark theme이 없어요?), 테마 작업을 훨씬 쉽게 해줄 이 접근법을 추천 합니다.

<br/>

---

## Qualified Colors?

다른 설정에 다른 값을 제공하는 것으로도 색상에 변화를 줄 수 있습니다. (e.g. `@color/foo`를 `res/values/colors.xml`과 `res/values-night/colors.xml`에 각각 정의)  
  
하지만, theme attribute를 쓰는걸 추천합니다.  
  
color 계층의 변화를 주려면 **의미있는 이름(semantic name)** 을 부여해야 합니다. 즉, `@color/white`라고 이름지은 색을 `-night` 설정의 어두운 색으로 제공하지는 않을 겁니다 — 요렇게 하면 좀 헷갈릴 겁니다.

대신에 `@color/background`같은 semantic name을 쓰고 싶을 겁니다. 그런데, 이렇게 하면 색깔 선언과 값 제공이 같이 있다는 문제가 있습니다. 따라서 요건 theme별로 변화를 줄 수 없죠.  
   
  
또한, `@colors`에 변화를 주려면 보다 많은 색을 만들어야 합니다. 다른 상황이라서 실제 색이 같더라도 새로운 이름을 부여한다면(i.e. background는 아니지만 같은 색), 그래도 colors 파일에 entry를 추가해야 합니다.  
  
  
theme attribute를 사용함으로써 semantic color의 선언을 실제 값 제공 부분과 분리할 수 있습니다. 호출부도 깔끔해지고 실제 색은 theme에 의해서 달라지죠 (`?attr/` 구문 사용).  
  
  
색 선언을 문자 그대로의 값(literally named values)으로 유지함으로써, 사용할 색상들을 미리 정의하고 theme 레벨에서 변화를 주는게 편리해 집니다.

> Define a palette of colors used by your app and vary them at the theme level

이 접근의 부가적인 이점은 이 색상들을 참조하는 layout과 style들의 재사용성이 좋아진다는 것입니다. theme은 overlaid 혹은 varied 될 수 있기에, 색상 변화를 위한 별도의 layout이나 style을 추가로 만들 필요가 없게 되지요 — 다른 theme에서 같은 layout을 사용할 수 있습니다.

<br/>

---

## Always?

theme에 의해 색상 변화를 주고 싶지 않은 상황도 있기에, "always\* refer to colors via theme attributes" 에 별표를 붙였습니다.

예를 들어, [Material Design guidelines](https://material.io/design/color/dark-theme.html#ui-application) 에서 light theme과 dark theme에서 같은 brand color를 사용하는 경우를 보여줍니다.

![always?](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcox2Dh%2FbtqYIjUVBkf%2FkkdawgkP5kncPzY0BoGCek%2Fimg.png)

이런 드문 경우에 대해서는, color resource를 바로 참조하는 것도 괜찮습니다.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<FloatingActionButton …
  app:backgroundTint="@color/owl_pink_500"/>
```

<br/>

---

## State of the art

layout/style에서 theme attribute를 사용하지 않을 수 있는 또 다른 상황은 [ColorStateList](https://developer.android.com/reference/android/content/res/ColorStateList)들을 사용할 때 입니다.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<View …
  android:background="@color/primary_20"/>
```

요것도 `primary_20`이 `ColorStateList`(내부적으로 theme attribute를 참조)라면 괜찮습니다 (아래 코드 참조). ColorStateList가 보통은 상태(pressed, disabled etc)에 따라 다른 색상을 나타내기 위해 사용되지만, 테마 작업에도 유용합니다.

알파 값을 지정할 수 있지요.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<selector …
  <item android:alpha="0.20" android:color="?attr/colorPrimary" />
</selector>
```

이런 종류의 single-item-`ColorStateList` (i.e. 한 가지 색만 존재함)는 유지해야할 color 리소스를 줄이는데 도움이 됩니다. primary color와 알파 값이 다른 별도의 색을 따로 정의하지 않고도 (per configuration!), 현재 theme의 colorPrimary이 뭐든지 간에 이를 대체할 수 있습니다. 만약 primary color가 바뀌면, 그냥 한 군데만 바꾸면 됩니다. 변경되어야할 모든 부분들을 추적할 필요가 없는거죠.

요게 유용하지만, 알아둬야할 몇 가지 경고사항이 있습니다.

1. 정의된 색도 알파 값을 가지면, 두 알파 값들은 조합됩니다. e.g. 50% 알파를 50% 불투명한 흰색에 적용하면 25% 흰색이 됩니다.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<selector …
  <item android:alpha="0.50" android:color="#80ffffff" />
</selector>
```

그래서, theme color는 완전 불투명한(fully opaque) 색으로 지정하고, 알파 값 조정은 `ColorStateList`로 하는 게 좋습니다.

2. alpha component는 API 23에서 추가되었습니다. min SDK가 더 낮다면, 이 동작의 백포트를 위해서 [AppCompatResources.getColorStateList](https://developer.android.com/reference/androidx/appcompat/content/res/AppCompatResources.html#getColorStateList(android.content.Context,%20int))를 사용하셔야 합니다. (추가로 항상 `android:alpha` namespace를 사용해야 함. `app:alpha` namespace 말고)

3. 가끔 아래 코드처럼 color를 drawable 자리에 사용하기도 합니다.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<View …
  android:background="@color/foo"/>
```

`View`의 backgroud는 drawable이고, 이 편법은 해당 color를 `ColorDrawable`로 변환합니다. 하지만 `ColorStateList`는 `Drawable`로 변환할 방법이 없었습니다. (API 29에서 요 이슈 해결을 위해 `ColorStateListDrawable`이 도입됨)

요 제약사항은 아래처럼 우회할 수 있습니다.

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<View …
  android:background="@drawable/a_solid_white_rectangle_shape_drawable"
  app:backgroundTint="@color/some_color_state_list"/>
```

background tint가 해당 view 에서 요구하는 state들을 지원하는지 잘 확인해야 합니다. (예: disabled 되었을 때 바뀌어야 하는지)

<br/>

---

## Enforcement

theme attribute와 ColorStateList를 사용해야 한다고 납득했다고 하더라도, 코드 전체 혹은 팀 전체에 강제하려면 어떻게 해야 할까요? 코드 리뷰를 꼼꼼히 하려고 노력할 수 있겠지만, 잘 되지 않죠. 더 좋은 접근법은 요걸 확인할 수 있는 툴을 사용하는 것입니다. 아래 글은 literal color 사용을 어떻게 lint check에 추가하는지 설명하고 있고, 이번 글에서 다룬 내용들에도 적용될 수 있습니다.

[https://proandroiddev.com/making-android-lint-theme-aware-6285737b13bc](https://proandroiddev.com/making-android-lint-theme-aware-6285737b13bc)

![enforcement](https://miro.medium.com/max/720/1*LR0C_Cn5IUzyiWVRfvA-tg.png)

<br/>

---

## Be Indirect

theme attribute와 ColorStateList를 사용해서 color를 theme으로 뽑아 내는 것은, layout과 style을 더욱 더 유연하게 합니다. 재사용성을 높이고, 코드 베이스를 간결하고 유지보수가 용이하게 유지시켜 주죠.

다음 포스트에서는 theme 사용과 상호작용을 더 알아봅시다.

[Android Styling: themes overlay](https://medium.com/androiddevelopers/android-styling-themes-overlay-1ffd57745207)  
![themes overlay](https://miro.medium.com/max/720/1*_oRkdiiwzxsiEKUW-KSRXg.png)

<br/>

---

