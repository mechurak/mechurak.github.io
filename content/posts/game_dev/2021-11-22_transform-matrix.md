---
title: "'2D Transformations - Vulkan Game Engine Tutorial 10' 정리"
date: 2021-11-22T23:30:00+09:00
summary: "transformation matrix를 이용해, scale과 rotation을 적용해 봅시다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 10
# featuredImage: /images/lve/i10_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i10_cover.jpg)
[Youtube Link](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- transformation matrix 을 통한 rotation, scale 설명
- PushConstant 에 transform matrix 추가해서 shader에서 사용
- LveGameObject 클래스 추가
  - 화면에 표시되는 객체

<br/>

---

## 2. Transformation Matrix 설명


### 2.1. 변환 행렬을 통한 vertex의 이동
모델의 rotation, scale 변경을 매트릭스로 표현할 수 있습니다.  
x축, y축을 그대로 늘리거나 돌리거나 하는 느낌으로 이해할 수 있는 것 같습니다.  

[40s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=40)
![i10_example_from.webp](/images/lve/i10_example_from.webp)

매트릭스는 열 기준으로 봐야 하며, (a, c) 가 x축, (b, d)가 y축을 의미합니다.


현재 모델의 각 점(v0 ~ v3)에 transform matrix를 적용하면, 아래 그림처럼 점들이 이동합니다.


[44s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=44)
![i10_example_to.webp](/images/lve/i10_example_to.webp)
기준이 되는 x축을 요렇게 잡아당기는 느낌입니다.
여기서는 i벡터(1, 0)를 (a, c) 로 옮겼습니다. (j는 그냥 둠)
기준 축이 (1, 0)에서 (a, c)로 바뀌면서, 사각형이 따라서 커지고 기울어짐.

<br/>

### 2.2. Game Object
게임 화면에 표시되서 독립적으로 존재하는 객체를 Game Object라고 부릅니다.
[181s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=181)
![i10_game_object_example.jpg](/images/lve/i10_game_object_example.jpg)

[245s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=245)
![i10_game_object.jpg](/images/lve/i10_game_object.jpg)

Game Object 를 구현하는 여러가지 접근법이 있는데, 우리는 [ECS(Entity Component System)](https://austinmorlan.com/posts/entity_component_system/) 이라는 걸 아주 간략화해서 구현할 겁니다.

<br/>

### 2.3. scale matrix

[669s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=669)
![i10_scale.jpg](/images/lve/i10_scale.jpg)

모델의 크기를 늘리기 위한 매트릭스 입니다.  
매트릭스를 만들 때, 열 방향으로 짝 지어서 만들어 줘야 합니다.

```scale_matrix.cpp
glm::mat2 scaleMat{ {scale.x, .0f}, {.0f, scale.y} };
```
<br/>

### 2.4. rotation matrix

회전을 위한 매트릭스를 만듭니다.

[745s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=745)
![i10_rotation.jpg](/images/lve/i10_rotation.jpg)

[775s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=775)
![i10_rotation_matrix.jpg](/images/lve/i10_rotation_matrix.jpg)

ɵ 라디안 만큼 돌리고 싶을 때, 코사인, 사인으로 위와 같이 매트릭스를 구성합니다.  
움.. 그런가 부다 합시다 ㅠㅜㅋ


[839s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=839)
![i10_y_direction.jpg](/images/lve/i10_y_direction.jpg)

vulkan 에서는 y축이 보통의 수학과는 반대로 아래쪽이 플러스입니다.
플러스 90도 만큼 회전시켰을 때, 고등학교때를 생각해보면 왼쪽으로 돌아가야 하는데,
vulakn 에서는 y축 플러스 방향이 아래쪽이라, 오른쪽으로 90도 돌았습니다.


### 2.5. matrix 적용 순서

[100s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=100)
![i10_order.jpg](/images/lve/i10_order.jpg)
매트릭스 곱하기은 교환법칙이 성립하지 않습니다.

아래 그림에서도 scale 먼저 하고 rotate 한 것과, rotate 먼저 하고 scale 한 것은 결과가 다릅니다.
(스케일 먼저 하고, 그 다음에 로테이션하는 게 이해가 쉬움)

![i10_multipy_order.jpg](/images/lve/i10_multipy_order.jpg)

cpp에서나 shader 코드에서 먼저 적용해야 하는 매트릭스가 오른쪽에 와야 합니다. (column major order)

```cpp
glm::mat2 mat2() {
  const float s = glm::sin(rotation);
  const float c = glm::cos(rotation);
  glm::mat2 rotMatrix{ {c, s}, {-s, c} };
  glm::mat2 scaleMat{ {scale.x, .0f}, {.0f, scale.y} };
  return rotMatrix * scaleMat;  // scale 먼저 한 후, rotate
};
```

<br/>

---


## 3. 코드 수정
### 3.1. simple_shader.vert
push constant 에 transform 추가

```glsl
layout(location = 1) in vec3 color;

layout(push_constant) uniform Push {
  mat2 transform;  // tranform 매트릭스
  vec2 offset;
  vec3 color;
} push;

void main() {
  gl_Position = vec4(push.transform * position + push.offset, 0.0, 1.0); // 포지션에 transform 매트릭스 적용
}
```

<br/>

### 3.2. LveGameObejct 추가

- 게임상의 오브젝트를 표현하는 객체  
- id와 모델을 가짐  
- 모델의 position, scale, rotation 을 나타내는 Transform2dComponent 가짐  

<br/>

### 3.3. FirstApp 클래스 수정
**(1) SimplePushCosntantData 구조체에 transform 추가**
. shader 의 push_constant 와 맞춰줌

```cpp
struct SimplePushConstantData {
  glm::mat2 transform{1.f};  // Identity Matrix 로 초기화
  glm::vec2 offset;
  alignas(16) glm::vec3 color;
};
```

<br/>

**(2) LveModel 대신 LveGameObject 들을 가지도록 함**
. loadGameObjects() 메소드에서 GameObject 하나 만들고 모델 및 속성 값들 설정한 후, 내부 멤버 변수(vector)에 추가

<br/>

**(3) renderGameObjects() 메소드 추가**
. recordCommandBuffer() 내부에서 호출해서, 모든 GameObject 들을 그리도록 함.
. 각 GameObject 별로, push_const 업데이트 -> 모델 bind -> draw


<br/>

### 3.4. 다이어그램
![i10_diagram.png](/images/lve/i10_diagram.png)

### 3.5. 결과 화면
![i10_result.png](/images/lve/i10_result.png)
삼각형이 오른쪽으로 회전합니다.

<br/>

---

## 4. Summary

[974s](https://youtu.be/gxUcgc88tD4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=974)
![i10_summary.jpg](/images/lve/i10_summary.jpg)
- transformation matrix 의 각 열은 i벡터, j벡터가 어디로 가야하는 알려 줍니다.
- transformation은 곱하기로 같이 적용될 수 있습니다.
- 매트릭스 곱하기는 결합법칙은 성립하지만, 교환법칙은 성립하지 않습니다.

<br/>

---