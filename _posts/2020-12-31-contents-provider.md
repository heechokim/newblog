---
layout: post
title:  "[안드로이드] Content Provider(4대 컴포넌트 - 컨텐츠 제공자)"
date:   2020-12-31 18:34:10 +0700
categories: [안드로이드]
---

## 1️⃣ What is Content Provider?

✍🏻 [Android Developer 도큐먼트 - 콘텐츠 제공자](https://developer.android.com/guide/topics/providers/content-providers) 를 참고하여 작성합니다.

* Content Provider는 안드로이드 앱을 구성하는 4대 컴포넌트 중 하나이다.

    * 4대 컴포넌트에 대한 설명은 [이 블로그의 다른 포스팅 - 안드로이드 4대 컴포넌트](https://choheeis.github.io/newblog//articles/2020-12/android-components)를 읽어보면 알 수 있을 것이다!

* __Content Provider(콘텐츠 제공자)의 역할은?__

    * Android 앱이 __자체적으로 저장하고 있는 데이터__, __다른 앱이 저장하고 있는 데이터__ 에 대한 __엑세스__ 권한을 관리하도록 돕는다.

        * Android 앱이 저장하는 데이터에 대한 설명은 [여기](https://developer.android.com/training/data-storage) 에서 추가로 확인할 수 있다.

    * 다른 앱과 데이터를 공유할 방법을 제공한다.

    * 데이터를 캡슐화하고, 데이터 보안에 대한 매커니즘을 제공한다.

    * 콘텐츠 제공자는 한 프로세스의 데이터를 다른 프로세스에서 실행되는 코드와 연결할 수 있게 해주는 표준 인터페이스이다.

* __Content Provider을 구현하면 얻을 수 있는 장점__

    * 다른 앱에서 우리 앱에 저장된 데이터를 __안전하게__ 수정하거나 엑세스할 수 있다.

    * <img width="450" alt="01" src="https://user-images.githubusercontent.com/31889335/103402432-7974be80-4b90-11eb-81dd-745299614a31.png">

    * 위 그림을 보면 다른 앱이 우리 앱의 데이터에 접근할 수 있도록 __우리 앱에서 content provider를 구현한 모습__ 을 볼 수 있다.

        * 다른 앱이 직접 우리 앱 데이터 저장소에 엑세스하는 것이 아니라 content provider에 먼저 엑세스해야 한다.

        * 우리 앱의 content provider가 우리 앱의 데이터에 엑세스하여 다른 앱에 전달하게 되므로 __안전하다.__

        * content provider가 중간에 끼어있음으로써 앱 데이터를 바로 노출하지 않게 되므로 __데이터를 캡슐화__ 하게 되는 것과 같고, __데이터 보안__ 을 유지할 수 있게 된다.

        * 주로 content provider는 우리 앱에 구현하지만 다른 앱에서 사용하도록 구현되기 때문에 다른 앱 입장에서는 우리 앱 데이터에 엑세스하기 위한 API 로 보일 수 있다.

    * 즉, 우리 앱 기능 중 __다른 앱에 우리 앱 데이터를 공유해야 하는 기능__ 이 있다면 content provider를 구현하면 된다.

    * 다른 앱과 데이터를 공유해야 하는 기능이 필요 없다면 content provider를 구현할 필요가 없을까?
    
    * content provider는 __nice abstraction(좋은 추상화) 기능__ 을 제공하기도 한다. 따라서 데이터 공유 기능을 위한 content provider 가 아니더라도 추상화 기능을 하는 존재로 content provider를 구현할 수 있다.

    * 예를 들어, content provider를 추상화 계층으로써 사용하면 우리 앱 데이터에 의존하고 있는 다른 앱들에 영향을 주지 않고, 우리 앱 데이터 저장소 구현을 수정할 수 있다.

    * <img width="770" alt="02" src="https://user-images.githubusercontent.com/31889335/103403825-dbcfbe00-4b94-11eb-9cf8-52331662f71d.png">

    * 위 그림은 다른 앱이 우리 앱 데이터를 공유하고 있어 우리 앱에 의존 상태인 두 앱의 모습이다.

    * 만약 우리 앱 저장소로 사용하고 있던 SQLite를 다른 저장소로 바꿔야 하는 상황이 발생해 우리 앱 데이터를 다른 저장소로 이주시켜야 한다면?

    * 이럴 경우, content provider가 다른 앱과 우리 앱 데이터 사이에 추상 계층으로써 존재하고 있다면 아무런 문제 없이 다른 저장소로 이주할 수 있다.

    * 즉, 앱 저장소를 바꿔도 우리 앱에 의존하고 있던 다른 앱에는 아무런 영향이 미치지 않는 것이다.