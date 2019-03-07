---
layout: post
title: "지킬(Jekyll)을 사용하여 Github pages와 연계한 나만의 블로그 만들기"
tags: [Jekyll, Blog, Example, Github pages, 지킬, 블로그, 예제, 깃헙 페이지]
comments: true
---

> 이 포스트에서는 Jekyll을 사용하여 Github pages와 연계한 블로그를 만드는 과정을 소개합니다.
> 이 포스트는 MacOS 기준으로 작성되었습니다.
---

## 1. 지킬(Jekyll) 설치하기

#### 지킬은 정적 페이지 생성 도구로써, 마크다운 형식으로 페이지 제너레이트 해주는 도구입니다.

먼저 지킬을 설치합니다.

```
$ sudo gem install jekyll
```

지킬을 실행하기 위해 디렉토리를 하나 만듭니다.

```
mkdir myblog
```

해당 디렉토리로 이동합니다.
```
cd myblog
```

해당 디렉토리 위치에서 아래와 같은 명령어를 실행하면 지킬이 제공하는 기본 테마를 포함한 페이지가 생성됩니다.
`jekyll new [github 사용자명].github.io`와 같은 형식으로 생성해 주시면 되겠습니다.

```
jekyll new leeheejin.github.io
```

위 명령어를 실행하고 나면
현재 디렉토리에서 `[github 사용자명].github.io`라는 이름의 디렉토리가 생성된 것을 확인할 수 있습니다.
이렇게 만든 블로그를 실행시키기 위해서는 `[github 사용자명].github.io` 디렉토리로 이동한 후에 아래의 명령어를 입력합니다.

```
cd leeheejin.github.io
```

이어서,

```
jekyll serve
```
명령어를 실행합니다.

해당 명령어를 실행하면 `https://localhost:4000/` 에 접속하여 기본 테마로 이루어진 블로그를 확인 할 수 있습니다.
![2019-03-06-2](https://user-images.githubusercontent.com/9789023/53942597-85130d80-40fe-11e9-8ec3-5cffab0a2135.png)

---

## 2. 깃헙 레포지토리 생성 (Create Github Repository)

깃헙 레포지토리를 생성합니다. 이름은 자신의 깃헙 계정이름(`username`)을 붙여 `leeheejin.github.io`와 같이 생성합니다.
![2019-03-06-1](https://user-images.githubusercontent.com/9789023/53867918-297e4c80-4038-11e9-8d42-830cd8ac44f5.png)
#### 왜 이런 이름으로 생성해야 하나요?
> 다음과 같은 이유 때문입니다. [링크](#)

