# git clone --depth=1, 히스토리는 필요 없을 때


다른 프로젝트를 살펴볼 때, 사실 히스토리는 관심 없는 경우가 많다. 이 때, `--depth=1` 옵션으로, 히스토리는 하나로 퉁쳐서 clone 할 수 있다.
<!--more-->

<br/>

## 0. 핵심요약

{{< admonition >}}
- 기존 히스토리는 필요 없다면 `git clone` 할 때, `--depth=1` 을 넣자.
- `--branch <name>` 옵션으로 특정 브랜치나 TAG 지정 가능
{{< /admonition >}}

<br/>

## 1. 사용 예시

유니티 샘플 프로젝트를 찾아보다가 아래 프로젝트를 살펴보고 싶어졌다.  

{{< image src="https://media.githubusercontent.com/media/Verasl/BoatAttack/release/2019.3/Assets/Textures/UI/welcome-title.png" caption="출처:[{GitHub - Unity} Boat Attack](https://github.com/Unity-Technologies/BoatAttack)">}}

프로젝트도 커다랗고, 커밋도 250개 이상이다.  
내가 저 히스토리를 살펴 볼 상황은 이유는 없어 보인다.  

이럴때 clone 할 때 `--depth=1` 옵션을 사용하자.

```bash
git clone --depth=1 https://github.com/Unity-Technologies/BoatAttack.git
```

<br/>

위 커맨드는 기본 브랜치 (main 이나 master?)를 가져온다.  

혹, 특정 브랜치에 관심 있는 상황이면 `--branch <name>` 도 같이 사용하면 된다.  
(`<name>` 자리에 브랜치 이름도 되고, TAG도 됨.)

```bash
git clone --depth=1 --branch 21.2/arm-test https://github.com/Unity-Technologies/BoatAttack.git BoatAttackArmBranch
```

<br/>

---

## 99. Reference
- [{Git 공식문서} git clone, `--depth` 옵션](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---depthltdepthgt)
- [{Stackoverflow} How to clone git repository with specific revision/changeset?](https://stackoverflow.com/a/51771769)

<br/>

---
