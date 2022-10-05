---
title: "'Graphics Pipeline Overview - Vulkan Game Engine Tutorial 02' 정리"
date: 2021-10-14T23:30:00+09:00
summary: "graphics pipeline 설명, shader 코드, LvePipeline 클래스 작성"
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 3
# featuredImage: /images/lve/i5-2_cover.jpg
tags:
- vulkan
---

[Youtube Link](https://www.youtube.com/watch?v=_riranMmtvI&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

{{< youtube _riranMmtvI >}}

## 1. 주요 내용

- graphics pipeline 설명
- vertex shader, fragment shader 작성(만) 해보기. SPIR-V로 컴파일
- `LvePipeline` 클래스 작성 : 컴파일된 spv 파일 읽어서 사이즈 출력

## 2. Graphics Pipeline 설명

컴퓨터가 그림 한 장을 어떻게 그리는지 자세히 들여다 봐 봅시다.


삼각형 하나만 생각해 봅시다.

[53s](https://www.youtube.com/watch?v=_riranMmtvI&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=53s)  

![triangle](/images/lve/i2_triangle.jpg)


컴퓨터 입장에서 꼭지점(Vertex) 정보 (여기서는 숫자 6개) 가 있으면,  
고 정보 기반으로 어떤 픽셀이 삼각형에 포함되는지 확인해서,  
해당 픽셀에 색칠을 할 겁니다.

바로 요게 Graphics Pipeline 이 하는 일입니다.

[193s](https://www.youtube.com/watch?v=_riranMmtvI&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=193s)  

![graphics pipeline](/images/lve/i2_graphics_pipeline.jpg)

Graphic Pipeline은 인풋 정보들을 가지고, FrameBuffer를 채우기 위한 일련의 과정입니다.  
여러 stage 들을 거치는데, 공장의 컨베이어 벨트를 생각하시면 적절합니다.

위 그림에서 녹색은 Fixed Function이고, 보라색은 Programmable 합니다.  
요 Programmable한 Vertex Shader, Fragment Shader 를 통해서 GPU한테 추가로 많은 일을 시킬 수 있습니다.  

각 단계에 대해서 아직은 잘 몰라도 되지만, 간단한 설명은 [Vulkan-Tutorial.com](https://vulkan-tutorial.com/Drawing_a_triangle/Graphics_pipeline_basics/Introduction) 을 참고해도 좋을 것 같습니다.

[226s](https://www.youtube.com/watch?v=_riranMmtvI&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=226s)  

![cpu vs gpu](/images/lve/i2_cpu_vs_gpu.jpg)

왜 이런 복잡한 과정을 거칠까 하는 생각이 들 수 있습니다..  
이것은 CPU와 GPU의 동작 차이에 기인합니다.  
GPU는 단순하고 반복적인 계산에 특화되어 있습니다. 한꺼번에 수천개의 vertex를 처리할 수 있죠.  
대신에 제한된 방법으로 제한된 용도로만 사용할 수 있죠.  

---

## 3. 실습 코드 수정

shaders 폴더에 Vertex Shader와 Fragment Shader를 만들어 봅시다.

### 3.1. vertex shader 작성

[409s](https://www.youtube.com/watch?v=_riranMmtvI&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=409s)

![vertex shader](/images/lve/i2_vertex_shader2.jpg)


Vertex Shader는 NDC(Normalized Device Coordinates, 정규화 장치 좌표계) 기준으로 위치 데이터 출력합니다.  
\[-1, -1\] ~ \[1, 1\] 안에 들어오는 게 화면에 그려진다고 보면 됩니다. 화면 중앙이 (0, 0)이 되겠죠.  
OpenGL 이랑 비교하면 y축 방향 바뀌었다고 합니다.

NDC를 사용하는 것은 다양한 화면 또는 실행창 크기을 하나의 공통 좌표계에 매핑하기 위함입니다.  


`simple_shader.vert`
```glsl
#version 450

vec2 positions[3] = vec2[](
  vec2(0.0, -0.5),
  vec2(0.5, 0.5),
  vec2(-0.5, 0.5)
);

void main() {
  gl_Position = vec4(positions[gl_VertexIndex], 0.0, 1.0);
}
```

- `gl_Position`: 결과 저장할 변수. built-in output variable
- `gl_VertexIndex` 현재 index. built-in variable

요 Vertex Shader 는 각 Vertex 마다 불립니다.

### 3.2. fragment shader 작성

[591s](https://www.youtube.com/watch?v=_riranMmtvI&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=591s)

![fragment](/images/lve/i2_frag.jpg)

fragment shader는 삼각형 안쪽에 해당하는 각 픽셀에 대해서 불립니다.  
아마도 해당 픽셀의 중앙이 삼각형 안쪽에 포함되는지 여부로 갈리는 것 같습니다.

### 3.3. SPIR-V 로 컴파일

작성한 Shader를 Vulkan에 사용하기 위해서는 Standard Portable Intermediate Representation V (SPIR-V) 형식으로 컴파일 해서 전달해야 합니다.

Vulkan SDK 의 glslc.exe 컴파일러를 사용해서 컴파일 해야 합니다.

  
두 파일을 편하게 컴파일 하기 위해, `compile.bat` 파일을 준비합니다.

### 3.4. LvePipeline 클래스 작성

일단은 컴파일된 spv 파일을 읽어들여서 사이즈 출력하는 정도의 동작만 합니다.

---

## 4. 클래스 다이어그램

![클래스 다이어그램](/images/lve/i2_class-diagram.png)

앱이 시작되면, LvePipeline 생성자에서 shader 파일들을 읽어서 사이즈 출력합니다.
