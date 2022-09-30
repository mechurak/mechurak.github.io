---
title: "Colab에서 Google Drive 연결 및 custom module 사용"
date: 2022-07-18T23:00:00+09:00
summary: "Google Drive에 있는 custom module을 Colab에서 import 해보자"
categories: [ ml ]
# featuredImage: /images/logo/ml.jpg
tags:
- colab
- google_drive
---

자신이 만든 python 모듈을 Colab에서 사용할 수 있다.  
Google Drive 에 `.py` 파일 혹은 패키지(폴더)를 올려둔 뒤,
Google Drive를 mount 하고, 해당 폴더를 `sys.path` 에 추가하면 된다.

<br/>

---

## 1. Colab에 Google Drive 연결

### (option 1) 파일 패널의 마운트 버튼 이용 (최초에만 확인 필요)

![google_drive_mount_1.jpg](/images/ml/google_drive_mount_1.jpg)
1. `폴더` 버튼
2. `마운트` 버튼
3. `확인` 하면, `sample_data` 위에 `drive` 폴더 보임

<br/>

![google_drive_mount_2.jpg](/images/ml/google_drive_mount_2.jpg)

### (option 2) 코드로 mount (실행시마다 인증 필요)
```python
from google.colab import drive
drive.mount('/content/gdrive')
```
단, 실행시마다 인증해야함.

<br/>

---

## 2. PYTHONPATH 에 custom module 위치 추가

- 해당 폴더의 경로 확인
    - 폴더에 오른쪽 버튼 클릭해서 `경로 복사` 버튼
- 해당 폴더를 `sys.path` 에 추가함
    - `parent` 폴더 밑에 `my_awosome_package` 폴더가 있다면, `parent` 폴더를 추가

```python
import sys
sys.path.append('/content/drive/MyDrive/awesome_work/parent/')
```

<br/>

---

## 3. 기타

해당 custom module이 자주 변할 예정이라면, Github 에 올려두고,  
Google Drive 에 해당 repo를 pull 하는 별도의 Colab 노트북을 준비해두는 것도 좋아 보인다.

<br/>

---

## 99. Reference

- [How to import custom modules in google colab? | stackoverflow](https://stackoverflow.com/a/52744016)

<br/>

---