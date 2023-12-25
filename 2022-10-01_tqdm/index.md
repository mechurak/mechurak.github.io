# 진행상황 표시에 사용하는 tqdm 모듈, enumerate, zip, generator 와 함께 사용


시간이 좀 걸리는 loop 를 돌 때, 해당 iterable 을 `tqdm`으로 감싸기만 하면 progress bar를 예쁘게 보여준다.  
`enuerate`, `zip`과 같이 사용할 때는 길이를 확인할 수 있는 안쪽의 list 에 `tqdm`을 적용해야 하고, `generator`와 같이 사용할 때는 `total`을 같이 넘겨주면 된다.
<!--more-->

{{< figure src="/images/logo/ml.jpg" >}}


## 1. 기본 사용법
iterable을 tqdm으로 감싸기만 하면 끝.[^1]

```python
from tqdm import tqdm

for i in tqdm(range(10000)):
    ...
```

    76%|████████████████████████████         | 7568/10000 [00:33<00:10, 229.00it/s]


<br/>

## 2. notebook 에서 사용
`tqdm.notebook` 에서 import 하고 동일하게 tqdm 사용  
`tqdm_notebook()`이 따로 있던 때도 있었지만, 지금은 `tqdm()` 함수를 쓰면 됨[^2]

```python
from tqdm.notebook import tqdm

for image_path in tqdm(image_paths):
    ...
```
Kaggle, Colab 에서 요렇게 사용하면 색깔도 예쁘게 칠해 줌.
<img src="/images/ml/tqdm_output.jpg"/>

<br/>

## 3. enumerate, zip 과 함께 사용
tqdm 내부적으로 감싼 녀석의 `__len__()` 을 확인하는데, `enumerate`, `zip`은 그게 없음.[^3]  
=> 안쪽의 list를 tqdm으로 감싼다.

<br/>

아래 코드에서 `enumerate` 이나, `zip`을 `tqdm`으로 감싸는 것이 아니라, 더 안쪽의 `paths`를 감쌈.  
(제일 안쪽의 `paths` 는 `list`라 길이를 알 수 있음.)  

```python
...
for (i, (path, label)) in enumerate(zip(tqdm(paths), labels)):
    image = cv2.imread(path)
    ...
```

zip은 둘 중에 어느 쪽을 tqdm으로 감싸도 무관.  

<br/>

## 4. generator 와 함께 사용
[generator]({{< ref "../python/2020-12-17_generator.md" >}})가 몇 개 값을 줄 건지 알고 있다면, 그 값을 `total` 인자로 넘김.[^4]  

```python
from tqdm import tqdm

length = 1000000
generator = (3 * n for n in range(length))  # just doing something random
for n in tqdm(generator, total=length):
    pass
```

[^1]: [tqdm 공식 문서](https://tqdm.github.io/)  
    > Instantly make your loops show a smart progress meter - just wrap any iterable with `tqdm(iterable)`, and you're done!

[^2]: [tqdm_notebook은 이제 tqdm으로 사용하세요, 네이버 블로그](https://m.blog.naver.com/kiddwannabe/221815973023)
    > TqdmDeprecationWarning: This function will be removed in tqdm==5.0.0  
    > Please use `tqdm.notebook.tqdm` instead of `tqdm.tqdm_notebook`

[^3]: [tqdm을 enumerate/zip과 함께 사용하기, 티스토리 블로그](https://beausty23.tistory.com/207)

[^4]: [tqdm show progress for a generator I know the length of, Stack Overflow](https://stackoverflow.com/a/42205097/16111308)
