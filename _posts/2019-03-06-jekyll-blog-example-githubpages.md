---
layout: post
title: "지킬(Jekyll)을 사용하여 Github pages와 연계한 나만의 블로그 만들기"
tags: [Jekyll, Blog, Example, Github pages, 지킬, 블로그, 예제, 깃헙 페이지]
comments: true
---

> 이 포스트는 MacOS 기준으로 작성되었습니다.
이 포스트에서는 Jekyll을 사용하여 Github Pages와 연계한 블로그를 만드는 과정을 설명합니다.
다양한 도메인 지식을 가진 독자들을 위해 기본 지식을 조금씩 담아 서술하였습니다.
>
> #### 전체적인 구성은 다음과 같습니다.
> * 지킬 설치하기
> * 깃헙 레포지토리와 연동하기
> * 나만의 테마를 적용시켜서 꾸미기
> * 커스텀 도메인 연결하기


## 1. 지킬(Jekyll) 설치하기

#### 지킬은 정적 페이지 생성 도구로써, 마크다운 형식으로 페이지를 생성해주는 도구입니다. 지킬을 사용하여 블로그를 만들고 포스트를 만들어 관리할 수 있게 해줍니다.

먼저 지킬을 설치합니다.
터미널을 열어 해당 명령어를 입력합니다.

```
sudo gem install jekyll
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

## 2. 깃헙 레포지토리(Github Repository) 생성 및 연결

깃헙 레포지토리를 생성합니다. 이름은 자신의 깃헙 계정이름(`username`)을 붙여
`leeheejin.github.io`와 같이 생성합니다.
![2019-03-06-1](https://user-images.githubusercontent.com/9789023/53867918-297e4c80-4038-11e9-8d42-830cd8ac44f5.png)
#### 왜 이런 이름으로 생성해야 하나요?
> 다음과 같은 이유 때문입니다. [링크](#)

사전에 만든 지킬 블로그 디렉토리를 자신의 레포지토리에 연결합시다.

먼저 깃과 연결할 준비를 해줍니다.
깃 사용을 위해 초기화를 합시다.
```
git init
```

그 뒤 해당 디렉토리 안에 있는 모든 파일들을 깃에 업로드 하겠다는 명렁어를 입력합니다.
*정확히는 이 명령어는 마지막 버전으로부터 변경이 있는 모든 파일을 의미합니다.*
```
git add .
```

그리고 간단하게 해당 커밋에 대한 설명을 적습니다. `"init"` 부분이 설명을 의미하는 메시지입니다.
```
git commit -m "init"
```

위 명령어들로 **어떤 파일들을 업로드 할 것인지**(`git add .`), 
**지금의 변경점이 무엇을 위한 변경인지**(`git commit -m "init"`)
를 결정했으니 이제 **어디에 업로드를 할 것인지**를 결정합니다.

```
git remote add origin https://github.com/Leeheejin/leeheejin.github.io.git
```

이 명령어는 위에서 만든 `leeheejin.github.io` 레포지토리에 지금 `git add .` 를 통해 
업로드하고자 결정한 파일들을 해당 저장소에 업로드 하겠다는 의미입니다.
독자 분들은 

```
git remote add origin https://github.com/Leeheejin/leeheejin.github.io.git
```

에서 

```
git remote add origin https://github.com/[github 사용자명]/[github 사용자명].github.io 으로.git
```

와 같은 식으로 변경하여서 명령어를 실행하시면 되겠습니다.

이후,

```
git push -u origin master
```

명령어를 통해 해당 저장소의 [master branch](#) 에 푸쉬할 수 있습니다.

해당 명령어를 실행하시면 다음과 같은 화면을 보실 수 있습니다.

```
Username for 'https://github.com':
```

그러면 독자분들의 깃헙 유저명을 적으시면 됩니다.

```
Username for 'https://github.com': leeheejin
Password for 'https://leeheejin@github.com':
```

이후 비밀번호를 입력한 뒤에

```
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 12 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (11/11), 3.88 KiB | 3.88 MiB/s, done.
Total 11 (delta 0), reused 0 (delta 0)
To https://github.com/Leeheejin/leeheejin.github.io.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

와 같이 성공적으로 파일들이 원격 저장소에 업로드 된 것을 확인할 수 있습니다.

![2019-03-06-3](https://user-images.githubusercontent.com/9789023/53945237-85aea280-4104-11e9-8166-6cee58f6fcfe.png)

이제 `https://localhost:4000/` 에서만 제공되던 블로그를 `https://leeheejin.github.io` 로 접속할 수 있습니다!

## 3. 지킬(Jekyll)에 테마(Theme) 적용하기

블로그를 만들고 배포하였다면 이제는 꾸며볼 차례입니다.
여러 무료로 공개된 테마중에서 마음에 드는 것을 선택하여 테마를 적용하는 방법에 대해서 알아봅시다.

*이 포스트에서는 [kiko-now](https://github.com/barryclark/jekyll-now) 를 사용하였습니다.*

![2019-03-06-4](https://user-images.githubusercontent.com/9789023/53951873-c7464a00-4112-11e9-82b1-7d92aa61d3ee.png)

해당 레포지토리에서 오른쪽 상단의 **Fork** 버튼을 클릭하여 자신의 레포지토리로 퍼올 수 있습니다.

퍼온 레포지토리에서 **Clone or download** 버튼을 눌러 해당 레포지토리의 파일들을 내려받고 

![2019-03-06-5](https://user-images.githubusercontent.com/9789023/53950729-2d7d9d80-4110-11e9-8163-69d40cb8b8c4.png)

내려받은 파일들을 자신의 `[github 사용자명].github.io` 디렉토리에 모두 붙여넣기 합니다.
> <mark> 주의! </mark> 파일들을 붙여넣을때 기존 디렉토리에 존재하던 파일들을 모두 삭제해야 합니다.

![2019-03-06-6](https://user-images.githubusercontent.com/9789023/53951074-007dba80-4111-11e9-9ccd-0935bb586523.png)

파일을 옮겨 붙이고 나면 디렉토리에는 위 이미지와 같은 파일들이 존재할 것입니다.

이제 실행해 봅시다!

`https://[github 사용자명].github.io` 에 접속하면
테마가 적용된 블로그를 확인하실 수 있습니다.

![2019-03-06-7](https://user-images.githubusercontent.com/9789023/53951221-53577200-4111-11e9-8b1f-04f17ec36086.png)

> 저는 추가로 프로필 이미지, 블로그 이름, 메뉴 이름들을 변경해서 적용했습니다.
> * _config.yml 파일에서 `name:` ,`description:`, `avatar:` 항목을 수정하면 됩니다. :)

## 4. 커스텀 도메인(Custom Domain) 연결하기

이제 커스텀 도메인을 연결해 봅시다.

커스텀 도메인을 연결하기 위해서는 **도메인을 가지고 계셔야 합니다.**

저는 [godaddy](https://kr.godaddy.com/offers/domains/godaddy-b?isc=gofkkr06&countryview=1&currencyType=krw&gclid=Cj0KCQiAn4PkBRCDARIsAGHmH3evMjXWM3x7N_EWCSSfOZbWgvPIf5VagNGHgZRxjwJ1Y9aM3rM9ZVwaAimREALw_wcB&gclsrc=aw.ds)
에서 [welsigogi.dev](https://welsigogi.dev) 도메인을 구입하였습니다. 해당 도메인을 가지고 블로그에 연결해 보려고 합니다.

먼제 godaddy에 접속하여 도메인을 사용하기 위한 설정을 해 줄 겁니다.

![2019-03-06-9](https://user-images.githubusercontent.com/9789023/53952599-8c451600-4114-11e9-9a1c-4f58a630efb8.png)

사용하고자 하는 도메인의 관리 페이지에 들어갑니다. DNS 관리 페이지에서 **레코드** 카테고리와 하단의 **도메인** 카테고리를 수정할겁니다.
 
![2019-03-06-10](https://user-images.githubusercontent.com/9789023/53952793-25742c80-4115-11e9-8881-92736234c910.png)

위 이미지는 수정을 마친 최종 화면입니다.
**레코드** 카테고리에서 유형이 **A**인 항목을 찾아 값을 `185.199.108.153`로 업데이트합니다.
그리고 유형이 **CNAME** 항목을 변경합니다. 이름은 **www** 입니다.해당 부분을 커스텀 도메인의 주소로 바꾸고 값을 `[github 사용자명].github.io`로 바꿉니다.
그리고 하단의 **추가** 버튼을 눌러 **A** 유형으로 세개를 만듭니다. 값 부분에 각각 `185.199.109.153`, `185.199.110.153`, `185.199.111.153` 으로 만드시면 됩니다.
TTL은 위 이미지와 똑같이 하시면 됩니다.

그리고,
하단의 **도메인** 부분에서 **추가**를 눌러 다음과 같이 작성해줍니다.

![2019-03-06-11](https://user-images.githubusercontent.com/9789023/53953275-41c49900-4116-11e9-9cee-9e386bee055e.png)

> `leeheejin.github.io` 값을 독자분들은 `[github 사용자명].github.io`로 변경해서 넣으시면 됩니다.

Godaddy에서의 설정은 이것으로 끝입니다. 이제는 깃헙 레포지토리에서 설정을 해 주어야 합니다.
 
자신의 `[github 사용자명].github.io` 레포지토리의 **Settings** 에서 하단으로 내려오면 GitHub Pages 항목을 보실 수 있습니다.

![2019-03-06-8](https://user-images.githubusercontent.com/9789023/53952162-784ce480-4113-11e9-8b7c-771520da4103.png)

> 현재 이미지에서는 이미 도메인을 연결하였기 때문에 *Your site is published at https://welsigogi.dev/* 메시지가 플로팅 되어있습니다.

Custom domain 에서 본인이 사용할 도메인을 적고 **save** 버튼을 눌러 저장하고 Enforce HTTPS 항목에 체크를 해 줍니다.

이것으로 이제 커스텀 도메인으로 블로그에 접속할 수 있습니다!
