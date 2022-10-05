---
title: "'Vulkan Game Engine Tutorial' 정리 시작"
date: 2021-10-12T23:30:00+09:00
summary: "Vulkan 스터디를 위해서는, Brendan Galea의 요 유튜브 시리즈가 그나마 친절한 것 같습니다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 1
# featuredImage: /images/lve/i5-2_cover.jpg
tags:
- vulkan
---

Vulkan 을 조금은 알아야 해서 스터디 할 자료를 찾아보는데,  
Brendan Galea의 요 유튜브 시리즈가 그나마 친절한 것 같습니다.

[{{< figure src="/images/lve/i0_playlist.png" title="Vulkan (c++) Game Engine Tutorials" >}}](https://www.youtube.com/watch?v=Y9U9IE0gVHA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

[공식 페이지](https://www.vulkan.org/learn#vulkan-tutorials)에도 소개되어 있으니, 어느 정도 인정받는 튜토리얼로 보입니다.  
[제작자 github](https://github.com/blurrypiano/littleVulkanEngine)에 강좌별 대략적인 내용과 소스 diff 도 제공하고 있습니다.  

열심히 따라하면서 이해만 잘~~ 하면ㅠㅜ 될 듯 하네요. 

<br/>

저도 영상 내용을 블로그에 정리해 보면서, Vulkan에 대한 이해도를 높여 보고자 합니다.

---

첫 번째 영상은 강의 소개 느낌입니다.

## 00 - Starting Point

[Youtube link](https://www.youtube.com/watch?v=Y9U9IE0gVHA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

{{< youtube Y9U9IE0gVHA >}}

- 간략한 Vulkan 소개
- 강의 소개
  - 간단한 게임 엔진을 만들어 보면서, Vulkan을 이해해보는 강의 
  - 당장 게임을 만들고 싶은 거면, 유니티나 언리얼을 보시라
- 프로젝트 셋업은 [Vulkan-Tutorial.com 의 개발 환경 세팅](https://vulkan-tutorial.com/Development_environment)을 사용

## 1. Development environment

저의 개발 환경의 아래와 같습니다.

- Window 10
- Visual Studio 2019 Community 버전 (Unity 설치하면 같이 깔리는)

여기에 Vulkan SDK, GLFW, GLM 준비가 필요합니다.

[Vulkan-Tutorial.com 의 개발 환경 세팅](https://vulkan-tutorial.com/Development_environment)을 보면서 차근차근 따라 합시다.

### 1.1. Vulkan SDK 설치

[https://vulkan.lunarg.com/sdk/home](https://vulkan.lunarg.com/sdk/home) 에서 SDK Installer 설치 후,

잘 동작하는지 확인을 위해, Bin 폴더의 `vkcube.exe` 실행해보기

### 1.2. GLFW 다운

플랫폼에 따른 윈도우 관련 처리를 위해 필요함.  
  
[https://www.glfw.org/download.html](https://www.glfw.org/download.html) 에서 64-bit Windows binaries 다운  
적당한 곳에 압축 해제 (ex. 내문서 - Visual Studio 2019 밑, `C:\Users\sshim\Documents\Visual Studio 2019\Libraries`)

### 1.3. GLM 다운

linear algebra(선형대수)를 위해 필요.  
header only 라이브러리.  
  
[https://github.com/g-truc/glm/releases/](https://github.com/g-truc/glm/releases/) 에서 최신 버전 다운

적당한 곳에 압축 해제 (ex. 내문서 - Visual Studio 2019 밑, `C:\Users\sshim\Documents\Visual Studio 2019\Libraries`)

### 1.4. Visual Studio 프로젝트 셋업

1. `Windows Desktop Wizard` 로 새 프로젝트 생성  
  - Application Type : Console  
  - Empty Project 체크  
2. main.cpp 작성  
3. project properties 수정해서 라이브러리와 헤더 폴더 추가  
  - Configuraiton, Platform을 All 로 해두고 수정  
  - C++ -> General -> Additional Include Directories 에 폴더 추가  
    . vulkan, glfw 은 include 폴더  
    . glm 은 glm 폴더  
  - Linker -> General -> Additional Library Directories 에 폴더 추가  
    . vulkan 은 Lib 폴더  
    . glfw 은 lib-vc2019 폴더  
  - Linker -> Input -> Additional Dependencies 에 라이브러리 추가  
    . vulkan-1.lib  
    . glfw3.lib  
  - C++ -> Language -> C++ Language Standard 에서 C++17 선택  
4. platform 을 x64로 바꾸고 Run

## 99. Reference

- [유튜브 playlist](https://www.youtube.com/watch?v=Y9U9IE0gVHA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)
- [저자의 강의 내용 github](https://github.com/blurrypiano/littleVulkanEngine)
- [Vulkan-Tutorial.com 개발 환경 세팅](https://vulkan-tutorial.com/Development_environment)