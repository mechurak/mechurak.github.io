---
title: "'Android styling: themes vs styles' 번역"
date: 2021-02-18T01:01:00+09:00
summary: "theme을 잘 활용합시다."
categories: [ android ]
# featuredImage: /images/logo/android.png
tags:
- android
- theme
---

스터디 하면서 샘플 프로젝트 따라 해보다가, 조금 된 예제를 따라하다보니 뭔가 theme 관련해서 마음대로 안되는 상황이 왔습니다.

  
찾다보니 요 시리즈를 알아두면 되겠다 싶네요.

아래 포스트를 번역한 내용입니다. (안드로이드 공식 블로그인듯 합니다.)  
[Android styling: themes vs styles](https://medium.com/androiddevelopers/android-styling-themes-vs-styles-ebe05f917578#)  

![android.png](/images/logo/android.png)

---

[Android styling system](https://developer.android.com/guide/topics/ui/look-and-feel/themes) 은 앱 디자인 작업을 위한 강력한 방법을 제공하지만, 잘못 사용하기 쉽습니다. 이를 적절히 사용하면 themes 와 styles 을 쉽게 유지할 수 있고, branding updates 가 덜 부담스러우며, 다크 모드를 지원하는 것이 간단해 질 수 있습니다. 이 글은 [Chris Banes](https://medium.com/@chrisbanes)와 저자([Nick Butcher](https://medium.com/@crafty))가 앞으로 연재할 시리즈의 첫 포스트입니다. 머리 쥐어 뜯지 않아도 스타일리쉬한 앱을 만들 수 있도록, 안드로이드 스타일링의 미스터리를 이 시리즈에서 풀어 보겠습니다.

이 포스트에서는, 스타일링 시스템의 구성 요소인 themes 와 styles 를 살펴보겠습니다.

<br/>

## Theme != Style

themes 와 styles 둘 다 `<style>`구문을 사용하지만 매우 다른 용도로 사용됩니다. 둘 다 key 가 attributes 이고 value 가 resource 인 key-value 저장소로 생각할 수 있습니다. 각각을 살펴봅시다.

<br/>

## What's in a style?

style 은 view attribute values의 collection 입니다. `Map<view attribute, resource>`로 생각하실 수 있습니다. 다시 말하면 key 는 모두 view attributes 입니다. widget 이 선언하고 당신이 layout 파일에 세팅할 수도 있는 그 attribute 입니다. Styles 은 단일 타입의 widget 에 적용됩니다. 다른 widget은 다른 attributes 셋을 지원하기 때문입니다.

> Styles are a collection of view attributes; specific to a single type of widget

<br/>

[원본 gist](https://gist.github.com/nickbutcher/209ed867f74ac5cd251f68dba7e0da18#file-themes_vs_styles_style-xml)
```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<style name="Widget.Plaid.Button.InlineAction" parent="…">
  <item name="android:gravity">center_horizontal</item>
  <item name="android:textAppearance">@style/TextAppearance.CommentAuthor</item>
  <item name="android:drawablePadding">@dimen/spacing_micro</item>
</style>
```

<br/>

아래에서 보듯이 style의 각 key는 layout 에서 설정할 수 있는 attribute들입니다.

[원본 gist](https://gist.github.com/nickbutcher/e904e2e3008de4285f56540539332bf8#file-themes_vs_styles_view_attrs-xml)
```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<Button …
  android:gravity="center_horizontal"
  android:textAppearance="@style/TextAppearance.CommentAuthor"
  android:drawablePadding="@dimen/spacing_micro"/>
```

이것들을 style 로 추출하면 여러 view 에서 쉽게 재사용할 수 있고, 유지보수도 용이합니다.

<br/>

## Usage

Styles 은 layout의 각 view 에서 사용될 수 있습니다.

[원본 gist](https://gist.github.com/nickbutcher/e111751dc60c0a72c2550bee28ced9b3#file-themes_vs_styles_style_usage-xml)

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<Button …
  style="@style/Widget.Plaid.Button.InlineAction"/>
```

View 는 하나의 style만 적용합니다. - 웹에서 css 가 여러 class들을 적용할 수 있는 것과 대조됩니다.

<br/>

## Scope

view 에 적용된 style 은 바로 그 view 에만 적용되고, 자식들에게는 적용되지 않습니다. 예를 들어 3개의 버튼을 가진 `ViewGroup`이 있을 때, `InlineAction` style을 `ViewGroup`에 적용하더라도 자식 버튼들에는 해당 style이 적용되지 않습니다.  
style로부터 제공된 값들은 layout에서 직접 설정한 값들과 결합됩니다. ([스타일링 우선순위](https://medium.com/androiddevelopers/whats-your-text-s-appearance-f3a1729192d)에 따라)

<br/>

## What's a theme?

theme은 styles이나 layouts등에서 참조할 수 있는 이름을 붙인 resources의 모음입니다. 의미있는 이름을 Android resources에 제공함으로써, 나중에 참조할 수 있습니다. 예를 들어 `colorPrimary`는 해당 색을 위한 의미있는 이름입니다.

[원본 gist](https://gist.github.com/nickbutcher/09471e60710519ad78b50df783601497#file-themes_vs_styles_theme-xml)

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<style name="Theme.Plaid" parent="…">
  <item name="colorPrimary">@color/teal_500</item>
  <item name="colorSecondary">@color/pink_200</item>
  <item name="android:windowBackground">@color/white</item>
</style>
```

이 이름 붙인 resources를 theme attributes라고 하고, 고로 theme 은 `Map<theme attribute, resource>`라고 볼 수 있습니다.

theme attributes는 view attributes와 다릅니다. 특정 view 타입에 한정된 속성이 아니고, 앱 전반에서 사용할 수 있는 의미있는 이름의 포인터이기 때문입니다.

theme은 이름 붙인 resources의 실제 값을 제공합니다. 위의 예제에서 `colorPrimary` attribute는 이 theme의 기본 색상이 teal(청록색)임을 명시합니다.

resource를 theme으로 추상화함으로써, 다른 theme 에서 다른 값(예를 들면, `colorPrimary`\=orange)을 제공할 수 있습니다.

> Themes are a collection of named resources, useful broadly across an app


<br/>

theme 은 interface와 비슷합니다. interface로 프로그래밍하면 실제 구현 클래스로의 의존성이 없어지고 다른 동작을 하는 구현 클래스도 쉽게 제공할 수 있습니다. themes도 비슷한 역할을 합니다. layout과 style을 작성할때 theme attributes 를 이용함로써, 다른 실제 값을 제공하면 다른 테마가 적용되도록 할 수 습니다.

아래는 러프한 pseudo-code 입니다.

[원본 gist](https://gist.github.com/nickbutcher/60b03553e278ed6f4c35d26cc2c59a39#file-themes_vs_styles_theme_psuedo_interface-kt)

```java
/* Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 */
interface ColorPalette {
  @ColorInt val colorPrimary
  @ColorInt val colorSecondary
}

class MyView(colors: ColorPalette) {
  fab.backgroundTint = colors.colorPrimary
}
```

아래처럼 따로 확장하지 않고 `MyView`가 그려지는 방식을 다르게 할 수 있습니다.

[원본 gist](https://gist.github.com/nickbutcher/e491315b05af92c8bb0816ae6848364e#file-themes_vs_styles_theme_psuedo_interface_usage-kt)

```kotlin
/* Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 */
val lightPalette = object : ColorPalette { … }
val darkPalette = object : ColorPalette { … }
val view = MyView(if (isDarkTheme) darkPalette else lightPalette)
```

<br/>

## Usage

`Activity`, `View`, `ViewGroup` 처럼 `Context`를 가지거나 나타내는 곳에 theme을 적용할 수 있습니다.

[원본 gist](https://gist.github.com/nickbutcher/d5db6c5329c390999739fa9c352cb4a0#file-themes_vs_styles_theme_usage-xml)
```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->

<!-- AndroidManifest.xml -->
<application …
  android:theme="@style/Theme.Plaid">
<activity …
  android:theme="@style/Theme.Plaid.About"/>

<!-- layout/foo.xml -->
<ConstraintLayout …
  android:theme="@style/Theme.Plaid.Foo">
```

`Context`를 [`ContextThemeWrapper`](https://developer.android.com/reference/android/view/ContextThemeWrapper.html#ContextThemeWrapper(android.content.Context,%20int))로 감싸고 이후에 [inflate](https://developer.android.com/reference/android/view/LayoutInflater.html#from(android.content.Context)) 하는 등의 방식으로 코드에서 theme 을 지정할 수도 있습니다.

<br/>

themes 의 강력함은 어떻게 사용하느냐에 달려 있습니다. theme attributes를 참조함으로써 더 유연한 widgets을 만들 수 있습니다. 다른 themes은 다른 실제 값을 제공합니다. 예를 들어, ViewGroup에 배경색을 지정하고 싶은 상황이라고 해봅시다.

[원본 gist](https://gist.github.com/nickbutcher/b5bc28be5c8cdd297b5ae59de1780fc5#file-themes_vs_styles_theme_attr_usage-xml)

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<ViewGroup …
  android:background="?attr/colorSurface">
```

실제 색(`#ffffff`나 `@color`리소스)을 지정하는 대신 `?attr/themeAttributeName`구문을 사용함으로써 theme에게 위임할 수 있습니다. 이 구문은 해당 semantic attribute를 위한 값을 theme에게 질의한다는 의미입니다. 이렇게 간접적으로 접근함으로써 (거의 같은데 색깔 몇개만 다른) 별도의 layout이나 style을 만들지 않고도, 다른 동작(e.g. light, dark themes 에서 다른 배경색 제공)을 제공할 수 있습니다.

> Use the `?attr/themeAttributeName` syntax to query the theme for the value of this semantic attribute

<br/>

## Scope

[`Theme`](https://developer.android.com/reference/android/content/res/Resources.Theme.html)은 [`Context`](https://developer.android.com/reference/android/content/Context)의 속성으로 접근되며, `Context`이거나 가진 `Activity`, `View`, `ViewGroup`에서 획득이 가능 합니다.

이 객체들은 tree 구조로 존재합니다(`Activity`가 `ViewGroup`들을 포함하고, `ViewGroup`은 `View`들을 포함 등). tree 의 특정 노드에 theme을 적용하면 자손 노드에도 해당 theme이 적용됩니다. 예를 들어, `ViewGroup`에 theme을 적용하면 해당 `ViewGroup`에 속한 `View`들 모두에 해당 theme이 적용됩니다. (styles은 하나의 view에만 적용되는 것과 대조됨)

[원본 gist](https://gist.github.com/nickbutcher/20860aef17375f16a86ee0ac2ca5e729#file-themes_vs_styles_theme_cascade-xml)

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<ViewGroup …
  android:theme="@style/Theme.App.SomeTheme">
  <!-- SomeTheme also applies to all child views. -->
</ViewGroup>
```

이건 정말 정말 유용합니다. light 스크린에 특정 부분만 dark theme을 적용하고 싶다고 생각해 봅시다. 이 동작은 [여기서](https://medium.com/androiddevelopers/android-styling-themes-overlay-1ffd57745207) 다룹니다.

이 동작은 layout inflation time에만 적용됩니다. `Context`가 `setTheme` 메소드를 제공하고, `Theme`이 `applyStyle` 메소드를 제공하지만, 이 메소드들은 inflation 전에 불려야 합니다. inflation 이후에 새로운 theme을 적용하고 style을 적용하는 것은 이미 존재하는 view를 업데이트 하지 않습니다.

<br/>

## Separate Concerns

styles와 themes의 다른 역할과 상호작용을 이해함는 것은 스타일링 리소스들 관리에 도움이 됩니다.

예를 들어 파란색 테마를 가진 상황에서, Pro 버전 몇 화면은 보라색 테마를 가져야 하고, [dark themes](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme) 도 제공하고 싶다고 합시다.

styles 만으로 접근한다면 Pro/non-Pro 와 light/dark 조합을 위한 4개의 styles 이 필요합니다. style 은 특정 타입의 view(`Button`, `Switch` 등)에 한정적이므로, 각각의 view 타입에 대해서 각각의 조합이 필요한 상황이 되어 버립니다.

![without theme](https://miro.medium.com/max/2000/0*aKRwEx8Z0SCfN8hy)

styles와 themes를 같이 사용하면, theme에 의해서 대체되는 부분을 고립시킬 수 있어서 각 view 타입에 대해서 하나의 style만 있으면 됩니다. 위의 예제에서 다른 `colorPrimary` 값을 제공하는 4개의 themes를 만들고, styles에서 각 theme으로부터 값을 가져오게 할 수 있습니다.

이 접근이 styles과 themes의 상호작용을 고려해야해서 더 복잡해 보일지도 모릅니다. 하지만 themes별로 변경되는 부분을 고립시키는 효과가 있습니다. 만약 파란색에서 오렌지색으로 테마를 변경한다고 하면, 여러 파일에서 styling 전반에 걸친 수정을 하는 게 아니라, 한 곳만 수정하면 됩니다. 또한 styles 이 많아지는 것을 막을 수 있습니다. 이상적으로는 각 view 타입별로 약간의 styles만 있으면 됩니다. theming을 활용하지 않는다면 styles.xml이 무지막지하게 많아져서 관리하는게 고통이 될 겁니다.

다음에는 많이 사용되는 theme attributes를 알아보고, 직접 만드는 방법도 알아보겠습니다.

[Android styling: common theme attributes](https://medium.com/androiddevelopers/android-styling-common-theme-attributes-8f7c50c9eaba)
