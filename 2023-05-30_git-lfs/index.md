# git lfs 설치 및 기본 세팅


텍스트가 아닌 바이너리 파일들은 텍스트 상의 diff가 무의미해서 git lfs(Large File Storage) 로 따로 관리하게 된다. git lfs 를 알아보자.
<!--more-->

<br/>

## 0. 핵심요약

{{< admonition >}}
- Windows 의 경우에는 [git for windows](https://gitforwindows.org/) 에 `git-lfs`도 포함되어 있음
- `git clone` 할 때 특별한 오류 없으면, 잘 설치되어 있는 것
- `git lfs track *.fbx` 이런 느낌으로 lfs로 관리할 확장자 지정
- `.gitattributes` 파일로 관리됨
{{< /admonition >}}

<br/>

## 1. 잘 설치 되어 있는지 확인

git-lfs GitHub의 [README](https://github.com/git-lfs/git-lfs#on-windows)에 보면, Windows 의 경우에는 git-lfs도 기본으로 같이 설치 된다.
{{< image src="/images/dev_common/git-lfs-windows.png" caption="git for windows 설치 중 기본으로 체크되어 있음" >}}

<br/>

또한, 프로젝트 별로 설치해야 하는 것이 아니라, system 에 한 번만 설치하면 된다고 한다. ([{GitHub Issue} git lfs install only once: per project or system-wide?](https://github.com/git-lfs/git-lfs/issues/3087#issuecomment-983667014))


<br/>

혹시나 설치했는지 가물가물 하다면 아무 위치에서나 아래 커맨드를 쳐보면 된다. (git repo 가 아닌 위치도 무관해 보임)

```bash
git lfs install
```

<br/>

아래 내용이 출력될 것이다.

```bash
Updated Git hooks.  # git repo 가 아니면 요건 안 나오는 듯 
Git LFS initialized.
```

<br/>

Git LFS가 잘 초기화 되었단다.

<br/>

---

## 2. 내 repo 에서 git lfs 세팅

내 repo에 git-lfs를 세팅해야 한다면, 아래 커맨드로 git lfs 가 관리할 파일들을 명시해 주면 된다.

```bash
git lfs track *.fbx
```

요렇게 하면 `.gitattributes` 파일이 생기고, 명시한 확장자들이 씌여 있을 것이다.

<br/>

여러 확장자를 같이 명시하려면 아래처럼 가능하다.

```bash
git lfs track "*.exe" "*.psd" "*.png" "*.exr" "*.zip" "*.tiff" "*.tif" "*.raw" "*.fbx" "*.jpg" "*.wav" "*.mp3" "*.ogg" "*.obj" "*.aiff" "*.tga" 
```

<br/>

---

## 99. Reference
- [{GitHub} Git Large File Storage](https://github.com/git-lfs/git-lfs#on-windows)
- [{GitHub Issue} git lfs install only once: per project or system-wide?](https://github.com/git-lfs/git-lfs/issues/3087#issuecomment-983667014)
- [{Stackoverflow} Git LFS - how to track multiple file types with one command](https://stackoverflow.com/a/46334726/16111308)

<br/>

---
