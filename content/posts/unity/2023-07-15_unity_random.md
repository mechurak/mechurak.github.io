---
title: "Unity에서 Random - Random.value, Random.Range()"
date: 2023-07-15T07:30:00+09:00
categories: [ Unity ]
tags:
- unity
- random
---

Unity에서 Random 값이 필요할 때, `Random.value`, `Random.Range()`를 사용할 수 있음
<!--more-->

<br/>

{{< image src="/images/logo/unity.png" caption="Unity 로고" width=50% >}}

<br/>

## 핵심요약
{{< admonition >}}
- `Random.value` 로 `[0.0f, 1.0f]` 사이의 랜덤 값
- `Random.Range(0, 11)` 로 원하는 정수 영역 정해서 랜덤 값. 뒤쪽은 영역에서 제외 `[0, 11)`
- `Random.Range(0.0f, 10.0f)` 로 원하는 실수 영역 정해서 랜덤 값. 뒤쪽은 영역에 포함 `[0.0f, 10.0f]`
{{< /admonition >}}
<br/>

---

## 코드 확인

```cs
float randomValue = Random.value;  // 0.0f ~ 1.0f
Debug.Log($"randomValue: {randomValue}");
// randomValue: 0.1424287

int randomRangeInt = Random.Range(0, 11);  // 0 ~ 10 (뒤쪽 숫자는 범위에 미포함)
Debug.Log($"randomRangeInt: {randomRangeInt}");
// randomRangeInt: 2

float randomRangeFloat = Random.Range(0.0f, 10.0f);  // 0.0f ~ 10.0f (뒤쪽 숫자도 범위에 포함)
Debug.Log($"randomRangeFloat: {randomRangeFloat}");
// randomRangeFloat: 5.182066
```

<br/>

---

## 더 알아보기
상황에 따라 편하게 활용할 수 있도록 다른 방식도 제공하고 있다.

```cs
Vector2 randomInsideUnitCircle = Random.insideUnitCircle;   // 반지름 1.0f 인 원 안쪽
Debug.Log($"randomInsideUnitCircle: {randomInsideUnitCircle}");
// randomInsideUnitCircle: (0.50, 0.35)

Vector3 randomInsideUnitSphere = Random.insideUnitSphere;  // 반지름 1.0f 인 구 안쪽
Debug.Log($"randomInsideUnitSphere: {randomInsideUnitSphere}");
// randomInsideUnitSphere: (0.09, -0.34, -0.34)

Vector3 randomOnUnitSphere = Random.onUnitSphere;  // 반지름 1.0f 인 구 표면
Debug.Log($"randomOnUnitSphere: {randomOnUnitSphere}");
// randomOnUnitSphere: (-0.72, -0.24, -0.65)

Quaternion randomRotation = Random.rotation;
Debug.Log($"randomRotation: {randomRotation}, euler: {randomRotation.eulerAngles}");
// randomRotation: (-0.77415, 0.22743, -0.08703, 0.58429), euler: (300.11, 127.02, 244.78)

Color randomColor = Random.ColorHSV();  // 파라미터로 hue, saturation, value, alpha 각각 범위 지정도 가능
Debug.Log($"randomColor: {randomColor}");
// randomColor: RGBA(0.575, 0.294, 0.374, 1.000)
```

<br/>

---

## 활용 예시
- 랜덤 위치에 오브젝트 생성
- https://docs.unity3d.com/ScriptReference/Random.Range.html
```cs
public GameObject prefab;

// Click the "Instantiate!" button and a new `prefab` will be instantiated
// somewhere within -10.0 and 10.0 (inclusive) on the x-z plane
void OnGUI()
{
    if (GUI.Button(new Rect(10, 10, 100, 50), "Instantiate!"))
    {
        var position = new Vector3(Random.Range(-10.0f, 10.0f), 0, Random.Range(-10.0f, 10.0f));
        Instantiate(prefab, position, Quaternion.identity);
    }
}
```

<br/>

- 속도는 10으로 일정하게, 방향 랜덤하게
- https://docs.unity3d.com/ScriptReference/Random-onUnitSphere.html
```cs
void Start()
{
    // Sets the rigidbody velocity to 10 and in a random direction.
    GetComponent<Rigidbody>().velocity = Random.onUnitSphere * 10;
}
```

<br/>

- 매터리얼의 색깔 랜덤하게
- https://docs.unity3d.com/ScriptReference/Random.ColorHSV.html
```cs
void OnMouseDown()
{
    // Pick a random, saturated and not-too-dark color
    // 파라미터들 : hueMin, hueMax, saturationMin, saturationMax, valueMin, valueMax
    GetComponent<Renderer>().material.color = Random.ColorHSV(0f, 1f, 1f, 1f, 0.5f, 1f);
}
```

<br/>

---

## Reference
- [{Unity Manual} Important Classes - Random](https://docs.unity3d.com/Manual/class-Random.html)
- [{Unity ScriptReference} Random](https://docs.unity3d.com/ScriptReference/Random.html)
