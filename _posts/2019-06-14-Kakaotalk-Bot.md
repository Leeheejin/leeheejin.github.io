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

## 0. 전체적인 구성은 다음과 같습니다.
* 시작 동기
* 개선점
* Getting started
* Full code

## 1. 왜 시작하였나.
```
카카오톡을 이용하는 시간이 많아짐에 따라 카카오톡을 이용하면서 간단하게 필요한 기능을 
카카오톡 어플리케이션을 실행하는 동안 수행하고 싶다고 느꼈습니다.
```

평소 카카오톡을 이용하면서 자주 사용하려고 했던 기능이나 있게 된다면 시간을 절약시키거나
궁금증을 해결할 수 있는(?) 요소는 무엇이 있을지를 고민했어요. 대부분은 카카오톡 안에서 해결할 수 있었지만
엄청 자주 필요로 하는 기능같은 경우에는 카카오톡에서 추가적으로 **몇번의 터치**나 **작업**이 필요했죠. 
그래서 이러한 불편함을 해소하고자 최소한으로 해당 기능을 사용할 수 있는 기능이 있으면 좋지 않을까 란 생각으로 시작했습니다.

---

먼저 인싸 친구들을 위한 `네이버 실시간 검색 순위`,

내가 원하는 지역의 `날씨 및 미세먼지 정보`

정도가 적절하다고 생각했어요.

## 2. 약간의 귀찮음으로 시작된 작업

```
카카오톡 봇을 작동시키는 앱에서 사용되는 JS(javascript)에 대한 도메인 지식이 적었고,
처음엔 너무 귀찮았어요(..) 난 그저 필요한 기능들을 사용하고자 하는 마음이 처음엔 강했었기 때문에
오픈된 소스를 가지고 기능을 구성하면서 차근차근 시작했습니다.
```

#### 재..재밌다.

몇개의 소스들을 찾아보고 갖다 붙이면서 대략적으로 어떻게 하면 원하는 기능을 구현할 수 있을지에 대한
감을 얻게 되었어요. 파싱하는 방법이 생각보다 쉬웠죠. 그리고 재미도 있었어요.

## 3. 날씨, 실검 기능만을 가진 Miner 에서 Miner++ 로

```
처음의 오픈소스를 뒤적거리면서 필요한 기능을 붙이다가 직접 필요한 기능을 만들어 보고자 하는 욕심이 생겼어요.
그래서 직접 만들어 보기로 했습니다.
추가로 정의한 필요 기능은 다음과 같아요.
```

웹툰 `덴마 업데이트 알람`

그리고 다이어터들을 위한 `음식 칼로리 계산기`,

지난날 뜨겁게 데였던 `가상화폐 시세 검색기`(...)


### 그래서 소스는?

소스는 오픈소스의 경우 이 [블로그](#)와 이 [블로그](#)의 도움을 받았어요.

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
    var data = Utils.getWebText("http://www.dietshin.com/calorie/calorie_search.asp?"
    +"keyword="+pos);
    data = data.replace(/(<([^>]+)>)/g, "");
    data = data.replace(/ /gi, "");
    data = data.split("음식명\n칼로리")[1];
    data = data.split("kcal")[0];
    data = data.trim();

    var result = data+"kcal 입니다.";

    if (result.length > 40) {
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
작동하는 현상을 확인했고, 이것은 명백한 `사이드 이펙트`라고 생각했어요.
또한, 쓰레드가 내 관리 하에서 올바르게 돌아가고 있다는 확신이 필요했고요.
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

### 실행 예제

> 각 명령어 실행 이미지

1. 덴마 업데이트 알람
![](#)
2. 음식 칼로리 계산기
![](#)
3. 가상화폐 시세 검색기
![](#)

---

### 나도 해보고 싶어요, getting started

Miner의 현재 버전의 소스는 오픈되어 있지 않지만 포스팅에서 소개한 내용은 공개하기로 했습니다.

> 카카오톡 봇을 만들기 위해 필요한 것들
> 1. 안드로이드 기기
> 2. 카카오톡 봇 JS
![](#)
> 3. 구글 스마트 알람 캐쳐
![](#)

#### 전체 소스

```javascript

function response(room, msg, sender, isGroupChat, replier, imageDB) {

    run = true;

    msg = msg.trim();

    var test = 0
    // 디버깅 모드용
    // 0 정상작동
    // 1 업데이트
    // 2 일부 기능 제한
    var blockApi = '날씨실검시세';
    // API 종류
    // 날씨 - 해당 지역 날씨 정보, 미세먼지 정보 반환
    // 실검 - 네이버 현재 실시간 검색어 Top 10 반환
    // 시세 - 가상화폐 현재 가격을 반환
    // 칼로리 - 해당 음식 칼로리 정보 반환
    // 덴마 알람 - 웹툰 덴마의 업데이트시, 덴경대 집합 반환

    const kiman = ['후우','후','살고싶다','두고봐','기만','연봉','도태'];

    if (test === 1 && msg.includes('미네르')) {
        replier.reply("상태 : 미네르가 현재 업데이트 중입니다.\n\n " +
            "현재 버전 : Ver 0.2 released")
    }

    else if (test === 2) {
        if (blockApi.includes(msg.substring(5, 6))) {
            replier.reply("현재 해당 기능은 버그 수정 중입니다.\n\n" +
                "사용이 제한되었습니다. 빠른 시일 내에 조치하겠습니다.\n\n" +
                "감사합니다.")
        }
    }

    else if (msg.includes('미네르 날씨')) {
        var result = getWeatherInfo(msg.substring(6));
        replier.reply(result);
    }

    else if (msg.includes('미네르 실검')) {
        var result = getPopSearch();
        replier.reply(result);
    }

    else if (msg.includes('미네르 시세')) {
        var result = getCoinPrice(msg.substring(7));
        replier.reply(result);
    }

    else if (msg.includes('미네르 칼로리')) {
        var result = getCal(msg.substring(7));
        replier.reply(result);
    }

    else if (msg === '미네르 나대신 사과해') {
        replier.reply("사과도 혼자서 못하는\n\n"
        +sender+"님 대신에\n\n제가 사과드려요\n\n부엉.")
    }

    else if (msg === '미네르 탕수육 부엉?') {
        replier.reply("아뇨.(단호)")
    }

    else if (msg.includes('미네르 민트초코')) {
        replier.reply("민트초코는 사랑입니다.\n\n부엉.")
    }

    else if (msg === '미네르 안녕' || msg === '미네르 인사') {
        replier.reply("안녕하세요.\n\n"+sender+"님.\n\n만나서 반가워요\n\n부엉.")
    }

    else if(msg === '미네르 위로해줘') {
        replier.reply(sender+"님은 충분히 노력하셨어요."
        +"\n\n더 이상 아파하지 않아도 돼요.\n\n부엉.")
    }

    else if (msg === '미네르 덴마 알람 켜줘') {

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
    }

    else if (msg === '미네르 덴마 알람 꺼줘') {

        if (roomList.indexOf(room.toString()) > -1) {

            delete roomList[roomList.indexOf(room.toString())];

            replier.reply("덴마 알람을 껐습니다.\n\n부엉.")
        }
        else {
            replier.reply("덴마 알람이 꺼진 상태입니다.\n\n부엉.")
        }
    }

    else if (msg.includes('미네르 명령')) {

        replier.reply("미네르의 명령어 목록입니다.\n\n" +
            "미네르 날씨 [지역명]\n해당 지역의 날씨 정보를 알려줍니다.\n\n" +
            "미네르 실검\n실시간 검색어 순위를 알려줍니다.\n\n" +
            "미네르 시세 [가상화폐명]\n해당 가상화폐의 가격을 한화로 알려줍니다.\n\n" +
            "미네르 칼로리 [음식명]\n해당 음식의 칼로리 정보를 알려줍니다.\n\n" +
            "미네르 덴마 알람 [켜줘],[꺼줘]\n덴마 업데이트 알람을 켜고 끌 수 있습니다."
            +"\n\n이상입니다. 부엉.")
    }


    if (sender === "Hbh") {
        var once = 0

        for (var i = 0; i < kiman.length; i++) {
            if (msg.includes(kiman[i]) && once === 0) {
                replier.reply('[알림]\n\n기만이 감지되었습니다.');
                once = 1;
            }
        }
    }

    // 랩해줘
    // 음식 칼로리 필요 운동량
    // 레시피
}

function getWeatherInfo(pos) {

    var data = Utils.getWebText("https://m.search.naver.com/search.naver?query="
    +pos+"%20날씨");
    data = data.replace(/<[^>]+>/g,"");
    data = data.split("월간")[1];
    data = data.split("시간별 예보")[0];
    data = data.trim();
    data = data.split("\n");

    var results = [];
    results[0] = data[0];
    results[1] = data[3].replace("온도","온도: ").trim()+" °C";
    results[2] = data[4].replace("온도","온도: ").trim()+" °C";
    results[3] = data[9].replace("먼지","먼지:").trim();
    results[4] = data[13].replace("습도","습도:").trim()+" %";

    var result = "["+pos+" 날씨 정보]\n\n상태 : "+results.join("\n");

    if (result.length < 40 || result.length > 80) {
        result = "날씨 정보를 찾지 못했습니다.\n\n부엉."
    }

    return result;
}

function getCoinPrice(pos) {

    var data = Utils.getWebText("https://m.search.daum.net/kakao?w=tot&DA=SH1&q="
    +pos+"%20%EA%B0%80%EA%B2%A9");
    data = data.split('txt_price">')[1];
    data = data.split("<")[0].trim();

    var result = "[현재 "+pos+" 시세]\n\n"+data+"원 입니다.";

    return result;
}

function getPopSearch() {

    var u = Utils.getWebText("https://www.naver.com")
    var a = u.split("급상승 검색어 검색어")
    var b = a[1].split("11")
    var c = b[0].replace(/(<([^>]+)>)/g,"")
    c = c.replace(/\n\n\n/g,"\n")
    .replace(/(?![0-9]+) /g,"")
    .trim()
    .replace(/(?=(\D))\b/g,".")

    return "[실시간 급상승 검색어]\n "+c;
}

function getCal(pos) {
    var data = Utils.getWebText("http://www.dietshin.com/calorie/calorie_search.asp?"
    +"keyword="+pos);
    data = data.replace(/(<([^>]+)>)/g, "");
    data = data.replace(/ /gi, "");
    data = data.split("음식명\n칼로리")[1];
    data = data.split("kcal")[0];
    data = data.trim();

    var result = data+"kcal 입니다.";

    if (result.length > 40) {
        result = "칼로리 정보를 찾지 못했습니다.\n\n부엉."
    }
    return result;
}

function getLastDenma() {
    var data = Utils.getWebText("https://comic.naver.com/webtoon/"
    +"list.nhn?titleId=119874&weekday=tue");
    data = data.split(",'119874',")[1];
    data = data.split(")")[0];

    return data;
}
//이 아래 6가지 메소드는 스크립트 액티비티에서 사용하는 메소드들
function onCreate(savedInstanceState, activity) {}
function onStart(activity) {}
function onResume(activity) {}
function onPause(activity) {}
function onStop(activity) {}
function onDestroy(activity) {}

denmaUpdate = 0;
threadCheck = false;
roomList = [];

```
