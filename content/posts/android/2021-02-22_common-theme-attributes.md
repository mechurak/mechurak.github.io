---
title: "'Android styling: common theme attributes' 번역"
date: 2021-02-22T01:01:00+09:00
summary: "theme을 잘 활용합시다."
categories: [ android ]
# featuredImage: /images/logo/android.png
tags:
- android
- theme
---

스터디 하면서 샘플 프로젝트 따라 해보다가, 조금 된 예제를 따라하다보니 뭔가 theme 관련해서 마음대로 안되는 상황이 왔습니다.
  

찾다보니 요 시리즈를 알아두면 되겠다 싶네요.

아래 포스트를 번역한 내용입니다.(안드로이드 공식 블로그인듯 합니다.)

[Android styling: common theme attributes](https://medium.com/androiddevelopers/android-styling-common-theme-attributes-8f7c50c9eaba)

![android.png](/images/logo/android.png)

<br/>

---

## Android styling: common theme attributes

이전 포스트에서 themes와 styles의 차이점과 themes를 이용해서 유연한 style와 layout을 작성하는 방법을 알아보았습니다.  
구체적으로 theme attributes를 사용해서 리소스에 접근하는 걸 추천했습니다. 이렇게 함으로써 실제 값을 다르게 할 수 있습니다. (e.g. in dark theme).  
즉, 아직 layout이나 style에서 리소스를 직접 접근해서 쓰고 있다면(혹은, 하드 코딩된 값을 쓰고 있다면ㅠㅜ), theme attribute를 쓰는 걸 고려하시기 바랍니다.  

```diff
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<ConstraintLayout ...
-  android:foreground="@drawable/some_ripple"
-  android:background="@color/blue" />
+  android:foreground="?attr/selectableItemBackground"
+  android:background="?attr/colorPrimarySurface" />
```
[gist link](https://gist.github.com/nickbutcher/7975a81a31afa7bebfc17d2e825168d3#file-common_theme_attrs_prefer-xml-diff)

그럼 어떤 theme attribute들이 있을까요? 이번 포스트에서 알아둬야할 자주 쓰이는 theme attribute들을 알아봅시다. Material, AppCompat, platform에서 선언된 애들입니다. 전체 리스트는 아니지만(전체 리스트를 확인하려면 attrs 파일을 확인해보시길 바랍니다.), 제가 항상 쓰는 attribute들입니다.

<br/>

---

## Colors

이 색깔들은 [Material color system](https://material.io/design/color/)에서 왔습니다. 앱 전반에서 쓸 수 있는 의미있는 이름을 부여하고 있죠. ([theme attrs 로 구현되어 있음](https://material.io/develop/android/theming/color))

![colors](https://miro.medium.com/max/720/0*MbdH1jqXiP1jr1lQ)

- `?attr/colorPrimary` 앱의 브랜드 색상
- `?attr/colorSecondary` 앱의 두 번째 브랜드 색상. 보통 브랜드 색상의 보색임
- `?attr/colorOn[Primary, Secondary, Surface etc]` 각 색상에 대비되는 색
- `?attr/color[Primary, Secondary]Variant` 각 색상에서 약간의 변화
- `?attr/colorSurface` 컴포넌트들(e.g. cards, sheets, menus)의 바탕을 위한 색
- `?android:attr/colorBackground` 화면의 바탕색
- `?attr/colorPrimarySurface` Light themes에서는 `colorPrimary`, Dark theme 에서는 `colorSurface`.
- `?attr/colorError` 에러 표시를 위한 색

<br/>

추가로 도움이 되는 색
- `?attr/colorControlNormal` icons/controls이 보통 상태일때 적용되는 색
- `?attr/colorControlActivated` icons/controls이 활성 상태일때 적용되는 색 (e.g. checked).
- `?attr/colorControlHighlight` control 강조를 위해 적용되는 색 (e.g. ripples, list selectors).
- `?android:attr/textColorPrimary` 가장 눈에 띄는 글자 색
- `?android:attr/textColorSecondary` 두번 째 글자 색

<br/>

---

## Dimens

- `?attr/listPreferredItemHeight` 리스트 아이템의 (최소) 높이
- `?attr/actionBarSize` 툴바의 높이

<br/>

---

## Drawables

- `?attr/selectableItemBackground` ripple/highlight 효과가 필요한 아이템 (전경에도 유용함!!)
- `?attr/selectableItemBackgroundBorderless` ripple 효과 필요한 경계선 없는 아이템.
- `?attr/dividerVertical` 요소들을 수직으로 나눌 때 사용
- `?attr/dividerHorizontal` 요소들을 수평으로 나눌 때 사용

<br/>

---

## TextAppearances

Material Design에는 [Type scale](https://material.io/design/typography/the-type-system.html#type-scale)이 정의되어 있습니다. 앱 전반에서 사용할 텍스트 스타일들의 집합이고, 각 항목들은 theme attribute로 [제공](https://material.io/develop/android/theming/typography)됩니다.

다른 폰트로 scale 을 만들려면 [Material type scale generator](https://material.io/design/typography/the-type-system.html#type-scale)를 확인해 보십시오.

![TextAppearances](https://miro.medium.com/max/720/0*vJ5UClMficVLP60q)

- `?attr/textAppearanceHeadline1` defaults to light 96sp text.
- `?attr/textAppearanceHeadline2` defaults to light 60sp text.
- `?attr/textAppearanceHeadline3` defaults to regular 48sp text.
- `?attr/textAppearanceHeadline4` defaults to regular 34sp text.
- `?attr/textAppearanceHeadline5` defaults to regular 24sp text.
- `?attr/textAppearanceHeadline6` defaults to medium 20sp text.
- `?attr/textAppearanceSubtitle1` defaults to regular 16sp text.
- `?attr/textAppearanceSubtitle2` defaults to medium 14sp text.
- `?attr/textAppearanceBody1` defaults to regular 16sp text.
- `?attr/textAppearanceBody2` defaults to regular 14sp text.
- `?attr/textAppearanceCaption` defaults to regular 12sp text.
- `?attr/textAppearanceButton` defaults to medium all caps 14sp text.
- `?attr/textAppearanceOverline` defaults to regular all caps 10sp text.

<br/>

---

## Shape

Material Design은 [shape system](https://material.io/design/shape)을 사용합니다. [theme attrs](https://material.io/develop/android/theming/shape)로 구현되어 있고, small, medium, large 컴포넌트들을 위해 각각 정의되어 있습니다.

직접 만든 컴포넌트에 shape appearance를 적용해야 하면, MaterialShapeDrawable을 활용하면 좋을 겁니다.

![shape](https://miro.medium.com/max/720/0*t-pKXoZJ8mKrinAx)

- `?attr/shapeAppearanceSmallComponent` used for Buttons, Chips, Text Fields etc. Defaults to rounded 4dp corners.
- `?attr/shapeAppearanceMediumComponent` used for Cards, Dialogs, Date Pickers etc. Defaults to rounded 4dp corners.
- `?attr/shapeAppearanceLargeComponent` used for Bottom Sheets etc. Defaults to rounded 0dp corners (i.e. square!)

<br/>

---

## Button Styles

Material 에는 세 가지([Contained](https://material.io/components/buttons#contained-button), [Text](https://material.io/components/buttons#text-button), [Outlined](https://material.io/components/buttons#outlined-button)) 타입의 버튼이 있습니다. [MDC(Material Componets)](https://github.com/material-components/material-components-android)는 `style`을 지정할 수 있는 theme attrs를 제공합니다.

![button styles](https://miro.medium.com/max/720/1*M5BxQsmZmMt6zcBtTuW3kA.png)

- `?attr/materialButtonStyle` defaults to contained (or just omit the style).
- `?attr/borderlessButtonStyle` for a text style button.
- `?attr/materialButtonOutlinedStyle` for outlined style.

<br/>

---

## Floats

- `?android:attr/disabledAlpha` Default disabled alpha for widgets.
- `?android:attr/primaryContentAlpha` The alpha applied to the foreground elements.
- `?android:attr/secondaryContentAlpha` The alpha applied to secondary elements.

<br/>

---

## App vs Android namespace

어떤 attribute는 `?android:attr/foo`를 참조하고 어떤 건 `?attr/bar` 요렇게 되어 있습니다. 앞의 것은 Android 플랫폼에 정의되어 있어서 `android`를 붙여야 합니다. (layout에서 view attribute를 `android:id`로 접근하는 것과 비슷).

안 붙은 녀석들은 앱에 포함되는 static library(i.e. AppCompat or MDC)에 정의되어 있어서, namespace가 필요 없습니다. (layout에서 `app:baz`를 사용하는 것과 비슷)

어떤 element들은 플랫폼과 라이브러리 양쪽에 정의되어 있습니다(e.g. `colorPrimary`). 이런 경우에는 모든 API level에서 사용할 수 있는 non-platform 버전을 사용하십시오. 백포팅을 위해서 그렇게 라이브러리에 중복 선언되어 있는 것입니다. 위쪽에서도 non-platform 버전들을 언급했습니다.

> prefer non-platform attributes which can be used on all API levels  

<br/>

---

## More Resources

thee attributes 전체 리스트는 아래에서 확인하세요.

- [Android platform](https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/res/res/values/attrs.xml)
- [AppCompat](https://github.com/aosp-mirror/platform_frameworks_support/blob/master/v7/appcompat/res/values/attrs.xml)

Material Design Components:

- [Color](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/color/res/values/attrs.xml)
- [Shape](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/shape/res/values/attrs.xml)
- [Type](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/typography/res/values/attrs.xml)

<br/>

---

## Do It Yourself

theme으로 변화를 주고 싶은 항목이 theme attribute에 없는 경우도 있습니다. 걱정 마세요... 직접 만드시면 됩니다!

Google I/O app에서 컨퍼런스 세션 리스트를 두 화면에서 보여주는 예제입니다.

![Do It Yourself](https://miro.medium.com/max/720/0*hur4DTCAixE_Qqv4)

거의 비슷한데 왼쪽에는 시간이 들어갈 공간이 필요하고 오른쪽에는 그렇지 않습니다. 구현을 위해서 theme attribute을 이용했습니다. layout 하나로 두 화면을 나타낼 수 있었죠.

1. [attrs.xml](https://github.com/google/iosched/blob/89df01ebc19d9a46495baac4690c2ebfa74946dc/mobile/src/main/res/values/attrs.xml#L41)에 theme attribute 정의

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<attr name="sessionListKeyline" format="dimension" />
```
[gist](https://gist.github.com/nickbutcher/05fa9dcf1e344dab77edc06e68dc85c0#file-common_theme_attrs_custom_attr-xml)

<br/>

2. 다른 테마에 [다른 값](https://github.com/google/iosched/blob/89df01ebc19d9a46495baac4690c2ebfa74946dc/mobile/src/main/res/values/themes.xml#L51) 제공

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<style name="Theme.IOSched.Schedule">
  …
  <item name="sessionListKeyline">72dp</item>
</style>

<style name="Theme.IOSched.Speaker">
  …
  <item name="sessionListKeyline">16dp</item>
</style>
```
[gist](https://gist.github.com/nickbutcher/f34af81ac45d2db42e88ed44ff055f79#file-common_theme_attrs_custom_attr_values-xml)

<br/>

3. 두 화면에서 같이 사용하는 (각각 위 테마 중 하나를 사용) 하나의 layout에서 해당 theme attr를 사용

```xml
<!-- Copyright 2019 Google LLC.    
   SPDX-License-Identifier: Apache-2.0 -->
<Guideline …
  app:layout_constraintGuide_begin="?attr/sessionListKeyline" />
```
[gist](https://gist.github.com/nickbutcher/0402c4f66a7c41935245f3566aee5239#file-common_theme_attrs_custom_attr_usage-xml)


---

## Question (mark) everything

어떤 theme attribute들이 사용 가능한지 알아둬야, layout, style, drawable을 작성할 때 사용할 수 있습니다. theme attribute를 사용하면 테마 적용(like dark theme)이 쉬워지고, 더 유연하고 유지보수가 용이한 코드를 작성할 수 있습니다.

더 깊이 알아보기 위해서, 다음 포스트로 가시지요!

[Android Styling: prefer theme attributes](https://medium.com/androiddevelopers/android-styling-prefer-theme-attributes-412caa748774)
![Android Styling: prefer theme attributes](https://miro.medium.com/max/720/1*_oRkdiiwzxsiEKUW-KSRXg.png)

---
