---
layout: post
title: "카카오톡 봇 JS를 사용하여 나만의 봇 만들기"
tags: [KakaoTalkBot, Bot, KakaoTalk, 카카오톡 봇, 봇, 카카오톡]
comments: true
---

> 이 포스트는 MacOS 기준으로 작성되었습니다.
>
> 이 포스트에서는 카카오톡 봇 JS 어플리케이션을 사용하여 필요 기능을 만들게 된 과정과 결과물을 설명합니다.
>
> 다양한 도메인 지식을 가진 독자들을 위해 기본 지식을 조금씩 담아 서술하였습니다.

#### 전체적인 구성은 다음과 같습니다.
* 시작 동기
* 개선점
* Getting started
* Example

## 1. 왜 시작하였나.
```
카카오톡을 이용하는 시간이 많아짐에 따라 카카오톡을 이용하면서 간단하게 필요한 기능을 
카카오톡 어플리케이션을 실행하는 동안 수행하고 싶다고 느꼈습니다.
```

평소 카카오톡을 이용하면서 자주 사용하려고 했던 기능이나 있게 된다면 시간을 절약시키거나
궁금증을 해결할 수 있는(?) 요소는 무엇이 있을지를 고민했습니다. 대부분은 카카오톡 안에서 해결할 수 있었지만
엄청 자주 필요로 하는 기능같은 경우에는 매번 확인할 때마다 카카오톡에서 추가적으로 **몇번의 터치**나 **작업**이 필요했습니다. 
그래서 이러한 불편함을 해소하고자 최소한으로 해당 기능을 사용할 수 있는 기능이 있으면 좋지 않을까 란 생각으로 시작했습니다.

먼저 인싸 친구들을 위한 `네이버 실시간 검색 순위`,

내가 원하는 지역의 `날씨 및 미세먼지 정보`

정도가 적절하다고 생각했습니다.

---

## 2. 약간의 귀찮음으로 시작된 작업

```
카카오톡 봇을 작동시키는 앱에서 사용되는 JS(javascript)에 대한 도메인 지식이 적었고,
처음엔 너무 귀찮았습니다.(..) 전 그저 필요한 기능들을 사용하고자 하는 마음이 처음엔 강했었기 때문에
오픈된 소스를 가지고 기능을 구성하면서 차근차근 시작했습니다.
```

#### 재..재밌다.

몇개의 소스들을 찾아보고 갖다 붙이면서 대략적으로 어떻게 하면 원하는 기능을 구현할 수 있을지에 대한
감을 얻게 되었습니다. 예제를 보며 구현하다 보니 파싱이 생각보다 쉬웠습니다. 물론 재미도 있었습니다.

---

## 3. 날씨, 실검 기능만을 가진 Miner 에서 Miner++ 로

```
처음의 오픈소스를 뒤적거리면서 필요한 기능을 붙이다가 직접 기능을 만들어 보고자 하는 욕심이 생겼습니다.
그래서 직접 만들어 보기로 했습니다.
추가로 정의한 필요 기능은 다음과 같습니다.
```

웹툰 `덴마 업데이트 알람`

그리고 다이어터들을 위한 `음식 칼로리 계산기`,

지난날 뜨겁게 데였던 `가상화폐 시세 검색기`(...)

---

### 그래서 소스는?

소스는 오픈소스의 경우 이 [블로그](http://blog.naver.com/PostView.nhn?blogId=dt3141592&logNo=221459185203&parentCategoryNo=&categoryNo=35&viewDate=&isShowPopularPosts=true&from=search)와 이 [레포지토리](https://github.com/DarkTornado/KakaoTalkBot-Examples)의 도움을 받았어요.

직접 작성한 소스는 3가지 함수로 아래와 같습니다.

```javascript

function getCoinPrice(pos) {

    var data = Utils.getWebText("https://m.search.daum.net/kakao?w=tot&DA=SH1&q="
    +pos+
    "%20%EA%B0%80%EA%B2%A9");
    data = data.split('txt_price">')[1];
    data = data.split("<")[0].trim();

    var result = "[현재 "+pos+" 시세]\n\n"+data+"원 입니다.";

    return result;
}

```

```javascript

function getCal(pos) {
    var data = Utils.getWebText("http://www.dietshin.com/"
    +"calorie/calorie_search.asp?keyword="+pos);
    data = data.replace(/(<([^>]+)>)/g, "");
    data = data.replace(/ /gi, "");
    data = data.split("음식명\n칼로리")[1];
    data = data.split("kcal")[0];
    data = data.trim();

    var result = data+"kcal 입니다.";

    if (result.length > pos.length + 30) {
        result = "칼로리 정보를 찾지 못했습니다.\n\n부엉."
    }
    return result;
}

```

```javascript

function getLastDenma() {
    var data = Utils.getWebText("https://comic.naver.com/webtoon/"
    +"list.nhn?titleId=119874&weekday=tue");
    data = data.split(",'119874',")[1];
    data = data.split(")")[0];

    return data;
}

```

덴마 알람의 경우 쓰레드에서 해당 탐색을 돌려야 했습니다.
그래서 전역으로 쓰레드를 돌리다 보니까 모든 채팅방에서 알람 기능을 원하지 않아도
작동하는 현상을 확인했고, 이것은 명백한 `사이드 이펙트`라고 생각했습니다.
또한, 쓰레드가 내 관리 하에서 올바르게 돌아가고 있다는 확신이 필요했습니다.
그래서 스레드를 제어할 수 있는 수단으로 전역 변수에 쓰레드가 돌아가고 있는
해당 채팅방 이름을 담아 그 방에만 업데이트 알람 메시지를 던져 주고
또 알람을 켜고 끌 수 있게 조절하기로 했습니다.

```javascript

        if (roomList.indexOf(room.toString()) < 0) {
            roomList.push(room.toString());
            java.lang.Thread({
                run : function() {
                    replier.reply("덴마 알람을 켰습니다.\n\n부엉.")
                    var nowDenma = getLastDenma();
                    while (roomList.indexOf(room.toString()) > -1) {
                        java.lang.Thread.sleep(10000); // 10000ms = 10s
                        var lastDenma = getLastDenma();
                        if (lastDenma !== nowDenma) {
                            replier.reply("덴경대 집합!\n\n부엉.")
                            nowDenma = lastDenma;
                        }
                    }
                }
            }).start();
        }
        else {
            replier.reply("덴마 알람은 이미 켜져있습니다.\n\n부엉.")
        }

  ...
  
          if (roomList.indexOf(room.toString()) > -1) {

            delete roomList[roomList.indexOf(room.toString())];

            replier.reply("덴마 알람을 껐습니다.\n\n부엉.")
        }
        else {
            replier.reply("덴마 알람이 꺼진 상태입니다.\n\n부엉.")
        }
```

---

## 4. 나도 해보고 싶어요, getting started

### 카카오톡 봇을 만들기 위해 필요한 것들

* 안드로이드 기기

* 카카오톡 봇 JS
![kakaotalk-bot](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/kakaotalkbot.jpeg?raw=true)

* Wear OS by Google
![google](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/google.jpeg?raw=true)

---

### 카카오톡 봇 JS 실행하기

카카오톡 봇 JS 를 실행하면 아래와 같은 화면을 보실 수 있습니다.

![getting-started-01](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/getting-started-01.jpeg?raw=true)

실행하게 되면 보시는 이미지와 같이 어플리케이션 화면에서
빨갛게 표시된 <mark> 알림 읽기 권한을 확인할 수 없습니다. </mark>
라는 문구를 확인하실 수 있습니다. 
알림 읽기 권한을 줘야 카카오톡 봇이 알림에 마치 실제 사용자처럼 반응할 수 있기 때문에 권한을 줍시다.

`권한 설정하기` 버튼을 누르게 되면 

![getting-started-02](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/getting-started-02.jpeg?raw=true)

이런 화면을 볼 수 있는데 **카카오톡 봇** 과 **Wear OS by Google** 의 오른쪽 체크박스에 체크를 해 줍니다.

---

### 카카오톡 봇 JS 소스 적용하기

다음은 봇의 소스를 변경하는 방법입니다.
카카오톡 봇 JS 어플리케이션의 맨 오른쪽 항목에 **에디터** 라는 항목에 봇 소스 코드를 적을 수 있습니다.
**에디터** 항목을 누르면 볼 수 있는 화면은 아래와 비슷합니다. `(화면이 다르다고 놀라지 마세요. 본 이미지는 이미 소스가 작성된 작성자 기준의 화면입니다!)`

![getting-started-03](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/getting-started-03.jpeg?raw=true)

여기다가 소스 코드를 적고 오른쪽 하단의 **플로피 디스켓** 모양을 누르면 소스를 저장할 수 있습니다.
소스는 항상 잘 저장합시다!


![getting-started-04](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/getting-started-04.jpeg?raw=true)

그 뒤 메인 화면에서 **스크립트 리로드** 버튼을 눌러 현재의 소스를 반영할 수 있습니다.
<mark> 참! 봇 작동 항목에 슬라이드 바를 활성화 해주는 것을 잊으면 안됩니다. </mark>

---

## 5. 실행 예제

> 각 명령어 실행 이미지

1. 덴마 업데이트 알람

    (1) 실행
    ![denmaOn](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/denmaOn.png?raw=true)
    
    (2) 종료
    ![denmaOff](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/denmaOff.png?raw=true)

아쉽게도 작성일 당시 덴마가 업데이트 되지 않아서 덴경대 집합 메시지를 미네르가 보내는걸 보여드릴 수가 없었습니다.(...)

2. 음식 칼로리 계산기
![cal](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/cal.png?raw=true)

3. 가상화폐 시세 검색기
![bitcoin](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/bitcoin.png?raw=true)

4. 실시간 급상승 검색어
![pop](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/pop.png?raw=true)

5. 지역 날씨 및 미세먼지
![weather](https://github.com/Leeheejin/leeheejin.github.io/blob/master/images/2019-06-14/weather.png?raw=true)

---
