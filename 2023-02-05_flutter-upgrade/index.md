# Flutter 버전 업그레이드


Flutter 3.7 이 발표 되었단다. 기왕이면 최신 버전을 쓰자. 뭔가 좋아 졌겠지.
<!--more-->

{{< figure src="/images/logo/flutter.png" >}}

<br/>
<br/>

## 핵심 요약

```bash
# 현재 채널 및 버전 확인
> flutter --version

# 채널 변경 (optional)
> flutter channel stable

# 업그레이드
> flutter upgrade

# 작업 프로젝트의 패키지 업그레이드 (좀 오래 걸리니 -v 옵션 추가해서 로그 출력)
> flutter -v pub get
```

<br/>

---

## 현재 채널 및 버전 확인

`flutter doctor` 혹은 `flutter --version` 으로 현재 사용하고 채널과 flutter 버전을 확인할 수 있다.

```bash
> flutter doctor

# Output
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.3.9, on Microsoft Windows [Version 10.0.19045.2486], locale ko-KR)
[!] Visual Studio - develop for Windows (Visual Studio Community 2019 16.11.4)
    X Visual Studio is missing necessary components. Please re-run the Visual Studio installer for the "Desktop development with C++" workload, and include these components:
        MSVC v142 - VS 2019 C++ x64/x86 build tools
         - If there are multiple build tool versions available, install the latest
        C++ CMake tools for Windows
        Windows 10 SDK
[√] Android Studio (version 2021.3)
[√] VS Code (version 1.75.0)
[√] Connected device (3 available)
[√] HTTP Host Availability
```
<br/>

나의 경우, `stable` 채널을 쓰고 있었고, Flutter 3.3.9 버전을 사용하고 있었다.  
3.7이 나왔다고 하고, 특별히 안 올릴 이유가 없으니, 사용 Flutter 버전을 올리기로 했다.

혹, `stable` 채널이 아니라, `beta` 채널을 사용 중이면, `flutter channel stable` 명령으로 채널을 변경할 수 있다.

<br/>

---

## 업그레이드

그냥 `flutter upgrade` 명령만 내리면 끝.  
사용하고 있는 채널의 최신 버전으로 flutter를 업그레이드 한다.

```bash
> flutter upgrade

# Output
Upgrading Flutter to 3.7.1 from 3.3.9 in C:\flutter...
Checking Dart SDK version... 
Downloading Dart SDK from Flutter engine 800594f1f4a6674010a6f1603c07a919b4d7ebd7... 
Expanding downloaded archive...
Building flutter tool... 
Running pub upgrade... 

Upgrading engine...
Downloading android-arm-profile/windows-x64 tools...             2,185ms
Downloading android-arm-release/windows-x64 tools...               505ms

# ...

Flutter 3.7.1 • channel stable • https://github.com/flutter/flutter.git
Framework • revision 7048ed95a5 (3 days ago) • 2023-02-01 09:07:31 -0800
Engine • revision 800594f1f4
Tools • Dart 2.19.1 • DevTools 2.20.1

Running flutter doctor...
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.7.1, on Microsoft Windows [Version 10.0.19045.2486], locale ko-KR)
```

<br/>

---

## 작업 중인 프로젝트 pug get
Flutter 업그레이드 후, 작업 중인 프로젝트를 빌드 하는데 `pug get` 에서 반응이 없었다.  
괜히 빨리 업그레이드 했나 후회하고 있었는데, 알고보니 첫 빌드가 그냥 오래 걸린 것이었다. (한 3분 정도는 걸렸던 것 같다.)

자세한 로그를 출력하면서 진행하도록 `flutter -v pug get` 을 한 번 해주도록 하자.

```bash
> flutter -v pug get
```

<br/>

---

## Flutter 3.7 주요 수정 사항
[What’s new in Flutter 3.7 블로그](https://medium.com/flutter/whats-new-in-flutter-3-7-38cbea71133c)에 따르면, Material 3 update 랑 iOS 개선이 큰 축으로 보인다.  

<br/>

[Material 3](https://flutter.github.io/samples/web/material_3_demo/#/) 샘플에서 Material 2 와 Material 3 를 토글해가면서 확인해 볼 수 있다.

{{< figure src="https://miro.medium.com/max/4800/1*AsyYVtFMXY0iS6gLz_O4BA.webp" >}}

<br/>

---

## Reference

- [{Flutter 공식 문서} Upgrading Flutter](https://docs.flutter.dev/development/tools/sdk/upgrading)
- [{Medium 블로그} What’s new in Flutter 3.7](https://medium.com/flutter/whats-new-in-flutter-3-7-38cbea71133c)
- [{Flutter Samples} Material 3](https://flutter.github.io/samples/web/material_3_demo/#/)

<br/>

---
