---
title: "'Fragment Interpolation - Vulkan Game Engine Tutorial 07' 정리"
date: 2021-10-27T23:30:00+09:00
summary: "vertex 에 색을 추가해서, 삼각형을 예쁘게 칠해 봅시다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 9
# featuredImage: /images/lve/i7_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i7_cover.jpg)
[Youtube Link](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- Fragment Interpolation 설명
- Vertex 데이터에 색깔 추가해서, fragment shader 에서 사용

<br/>

---

## 2. vertex shader 에서 fragment shader 로 데이터 전달
​
vertex shader 에서, fragment shader 로 데이터를 넘길 수 있습니다.
​
[36s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=36)
![i7_transfer.jpg](/images/lve/i7_transfer.jpg)
vertex shader 에서 `out`, fragment shader 에서는 `in` 으로 지정합니다.  
양쪽의 location 일치해야 하고, 변수명은 상관 없습니다.
​
<br/>

vertex shader 는 vertex(점)별로 호출되고,  
resterization을 거친 후, fragment shader 는 픽셀별로 호출됩니다.
​
[59s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=59)
![i7_question.jpg](/images/lve/i7_question.jpg)
​
그렇다면 삼각형 중간에 있는 픽셀들의 인풋 값들은 어떻게 만들어지는 걸까요?  
답은 linear interpolation 입니다.
​
<br/>

---
​
## 3. Linear Interpolation

![i7_linear.jpg](/images/lve/i7_linear.jpg)
점 몇개만 찍혀 있을 때, 중간 값들을 어떻게 유추해야 할까요?  
심플하게 점 사이를 직선으로 이어버리는 방법이 linear interpolation 입니다.

<br/>

[111s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=111)
![i7_simple_linear.jpg](/images/lve/i7_simple_linear.jpg)​​
직선에서의 예시입니다.

<br/>

[143s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=143)
![i7_multi.jpg](/images/lve/i7_multi.jpg)​​
2-dimensions 에서도 똑같이 계산 됩니다.

<br/>

[166s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=166)
![i7_color.jpg](/images/lve/i7_color.jpg)​
position 말고도 어떤 타입이라도 interpolation 가능합니다. 위 그림처럼 color 두요.
​
<br/>

---
​
## 4. Barycentric Coordinates
​
삼각형에서는 어떻게 interpolation 할까요?
​
![i7_bary_centric.jpg](/images/lve/i7_bary_centric.jpg)​
​
Barycentric Coordinates 를 이용해서 interpolation 수행한다고 합니다.  
gpu 가 알아서 잘 할겁니다.
​
[255s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=255)
![i7_bary_centric2.jpg](/images/lve/i7_bary_centric2.jpg)​
​
2차원으로 생각하면, 변수가 t 하나 였던 게, 베타, 감마 두 개로 늘어났다고 생각하면 된다고 합니다.  
3차원이면, 변수가 하나 더 늘어나구요.

<br/>
<br/>

다시 pipeline으로 돌아가 봅시다.  ​
각 픽셀의 위치 따라 다른 알파, 베타, 감마값으로 interpolation 된 값이 Fragment shader 의 인풋으로 들어갑니다.

[320s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=320)
![i7_pipeline.jpg](/images/lve/i7_pipeline.jpg)​

​
<br/>

---
​
## 5. SIMD model
​
이번 강좌의 결과에서, vertex shader 는 3번 불리고,​
![i7_vertex3.jpg](/images/lve/i7_vertex3.jpg)​
​
fragment shader 는 60000번 다른 인풋으로 불려야 합니다.
![i7_frag_times.jpg](/images/lve/i7_frag_times.jpg)​

<br/>

​요 때, interpolation 은 gpu 하드웨어에 의해서 매우 빠르게 수행됩니다.  ​
SIMD model (Single Instruction Multiple Data) 이라는 gpu의 작동 방식 때문이라고 합니다.
​
[721s](https://youtu.be/ngoZZkMuCOM?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=721)
![i7_simd.jpg](/images/lve/i7_simd.jpg)​
​
같은 instruction을 여러 점에 대해서 동시에 수행하는 거죠.  
유연성 희생한 대신, 요런 동작에 최적화되어 있습니다.
​
<br/>

---
​
## 6. 구현

### 6.1. simple_shader.vert

```glsl
layout(location = 0) in vec2 position;  
layout(location = 1) in vec3 color;  
​
layout(location = 0) out vec3 fragColor;  // in, out 간에는 location 무관함
​
void main() {  
 gl_Position = vec4(position, 0.0, 1.0);  
 fragColor = color;  // input 으로 들어온 color 를 fragment shader에서 사용할 수 있도록 out 변수에 넘김
}
```
- `in`, `out` 간에는 location 무관함
- `in` 으로 들어온 color 를 fragment shader에서 사용할 수 있도록 `out` 의 변수에 넘김
​
### 6.2. simple_shader.frag

```glsl
layout (location = 0) in vec3 fragColor;  // 데이터 형 맞춰야 함
layout (location = 0) out vec4 outColor;  
​
void main() {  
 outColor = vec4(fragColor, 1.0);  // 하드코딩 대신 넘어온 값 사용
}
```

- `in` 으로 들어온 데이터가, vertex shader 에서 `out` 으로 내보낸 데이터와 형식 및 location 맞아야 함
- 하드코딩 대신 넘어온 컬러 값 사용
​
### 6.3. LveModel::Vertex 수정

- LveModel::Vertex 에 color 추가
- getAttributeDescriptions() 에 color 관련 정보 추가
  - offset 은 `offsetof()` 매크로로 계산
​
### 6.4. FirstApp 수정​

- Vertex data 에 color 값들 추가
​
### 6.5. 결과
​
![i7_result.png](/images/lve/i7_result.png)
​
<br/>

---
​
## 7. 다이어그램
​
![i7_diagram.png](/images/lve/i7_diagram.png)


<br/>

---