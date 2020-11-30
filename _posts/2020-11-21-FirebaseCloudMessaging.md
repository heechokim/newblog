---
layout: post
title:  "[Firebase] Firebase Cloud Messaging 사용기"
date:   2020-11-21 18:34:10 +0700
categories: [Firebase]
---

## 0️⃣ Firebase Cloud Messaging 이란!

✍🏻 [FCM 도큐먼트](https://firebase.google.com/docs/cloud-messaging?hl=ko)를 참고하여 작성합니다.

줄여서 __FCM__ 이라고도 부르는 Firebase Cloud Messaging은 Firebase에서 제공하는 __무료 메시지 전송 플랫폼__ 이다.

Android, iOS, Web, C++, Unity 등의 플랫폼 개발에서 메시지 전송 기능을 구현할 때 사용할 수 있다.

[FCM에 대해 소개하는 유튜브 영상](https://youtu.be/sioEY4tWmLI)도 한 번 시청하자^^

그럼 이제 FCM을 사용하면 정확히 어떤 기능들을 구현할 수 있는지 알아보자.

## 1️⃣ FCM이 제공하는 주요 기능 3가지!

✍🏻 [FCM 도큐먼트 - 주요 기능](https://firebase.google.com/docs/cloud-messaging?hl=ko#key-capabilities)을 참고하여 작성합니다.

FCM을 사용하여 구현할 수 있는 기능에는 총 3가지가 존재한다.

1. __알림 메시지 전송 or 데이터 메시지 전송 기능__

    앱이나 웹, 게임 등을 이용하는 사용자에게 알림 메시지를 전송할 수 있다.

    또는 사용자에게 데이터 메시지를 전송하고 애플리케이션 코드에서 임의로 처리할 수 있다.

    알림 메시지와 데이터 메시지 등 메시지 유형에 관해서는 [여기](https://firebase.google.com/docs/cloud-messaging/concept-options?hl=ko#notifications_and_data_messages)에서 알아볼 수 있으니 여기서는 일단 넘어가도록 하자.

2. __다양한 메시지를 전송받을 기기 타겟팅 기능__

    단일 기기, 그룹 기기, 특정 주제를 구독한 기기 등 3가지 타겟팅 방식으로 클라이언트 앱에 메시지를 배포할 수 있다.

3. __클라이언트 앱에서 메시지 전송 기능__

    메시지를 전송 받은 클라이언트 기기에서 다시 서버로 확인 및 채팅 등의 메시지를 보낼 수 있다.

이렇게 FCM에서 제공하는 3가지 기능에 대해 간략하게 알아봤다!

## 2️⃣ FCM 작동 원리 및 아키텍처

✍🏻 [FCM 도큐먼트 - 아키텍처](https://firebase.google.com/docs/cloud-messaging/fcm-architecture?hl=ko)를 참고하여 작성합니다.

FCM이 제공하는 __메시지 전송__ 기능은 아래 그림과 같은 4개의 컴포넌트 세트에 의존하여 작동된다.

<img width="669" alt="01" src="https://user-images.githubusercontent.com/31889335/99872085-94175780-2c22-11eb-848d-37ac47c3df07.png">

4개의 컴포넌트 각각에 대해 알아보고 FCM 작동 원리에 대해 알아보자.

1. __첫 번째 컴포넌트 - 알림 작성기__

    <img width="150" alt="02" src="https://user-images.githubusercontent.com/31889335/99872137-fa03df00-2c22-11eb-9013-75101fd6a004.png">

    FCM 메시지 전송 기능을 작동시키는 아키텍처의 첫 번째 컴포넌트는 __메시지 전송 요청을 작성하거나 구현하는 도구(이하 알림 작성기)__ 이다.

    알림 작성기에는 2가지 종류가 있는데 하나는 [Firebase Notifications Console](https://console.firebase.google.com/) 이라는 GUI 기반의 웹 사이트이고, 다른 하나는 Firebase Admin SDK 또는 Firebase 서버 프로토콜을 지원하는 서버 환경이 다른 하나이다.

    위 두 가지 도구 중 하나를 사용하여 메시지 전송 요청을 작성하거나 구현하는 작업이 FCM 메시지 전송 서비스 아키텍처의 첫 컴포넌트인 것이다.

    특히, [모든 유형의 메시지](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) 전송을 완벽하게 자동화하려면 Firebase Admin SDK 또는 Firebase 서버 프로토콜을 지원하는 서버 환경에서 메시지 전송 요청을 구현해야 한다.

    예를 들어 어떤 프로젝트에 서버를 담당하는 백앤드 개발자가 존재한다면 [FCM 도큐먼트 - 서버 환경](https://firebase.google.com/docs/cloud-messaging/server) 문서를 참고하여 서버를 구축해달라고 요청해야 한다.

2. __두 번째 컴포넌트 - FCM 백엔드__

    <img width="365" alt="03" src="https://user-images.githubusercontent.com/31889335/100586211-278f0d80-3332-11eb-8a77-cf1a3bd31aca.png">

    첫 번째 컴포넌트였던 알림 작성기에서 작성한 메시지 topic 이나 instance를 두 번째 컴포넌트인 FCM 백엔드로 전달한다.

    그럼 FCM 백엔드에서 메시지 전송 요청을 수락하고, 메시지 ID와 같은 메시지 메타데이터를 생성하고, topic을 통해 메시지 [팬아웃](https://ko.wikipedia.org/wiki/%ED%8C%AC_%EC%95%84%EC%9B%83) 을 수행한다.

3. __세 번째 컴포넌트 - 플랫폼 수준의 전송 레이어__

    <img width="559" alt="04" src="https://user-images.githubusercontent.com/31889335/100586678-dd5a5c00-3332-11eb-8616-d372e7fc1987.png">

    FCM 백엔드에서 수락된 메시지 전송 요청의 결과로 곧바로 실기기에 메시지가 전송되는 것이 아니다.
    
    FCM 백엔드와 실기기 중간에 세 번째 컴포넌트인 __전송 레이어(layer)__ 를 끼워 플랫폼 별 메시지 요청 구성을 정의하고, 기기로 타겟팅된 메시지를 라우팅(안드로이드 기기에 보낼지, iOS 기기에 보낼지, Web에 보낼지 등을 처리)한 후, 메시지 전송을 처리한다.

    여기서 알아두어야 할 것은 전송 레이어는 FCM에서 제공하는 서비스가 아닌 FCM 외부에 존재하는 서비스라는 것이다.

4. __네 번째 컴포넌트 - 사용자 기기의 FCM SDK__ 

    <img width="627" alt="05" src="https://user-images.githubusercontent.com/31889335/100587246-a9cc0180-3333-11eb-8967-6d85861471a2.png">

    사용자 기기에 알림이 표시되거나 앱의 포그라운드/백그라운드 및 앱 상태 관련 로직에 따라 메시지가 처리된다.

## 3️⃣ FCM 작동 흐름 정리

FCM 서비스를 사용하여 메시지 전송 기능을 구현하는 과정의 작동 원리를 순차적으로 다시 한 번 이해해보자.

1. __FCM에서 보낸 메시지를 수신할 클라이언트 앱(여기선 device로 표현)을 등록한다.__

    FCM에서 보낸 메시지를 받을 클라이언트 앱의 인스턴스를 등록해야 한다.

    클라이언트 앱의 인스턴스를 등록하면 해당 클라이언트 앱의 고유 등록 토큰을 얻게 된다.

2. __메시지 전송 및 수신을 4단계에 거쳐 처리한다.__

    1. 메시지 전송 요청이 알림 작성기나 신뢰할 수 있는 서버 환경에서 작성되면 이 요청이 FCM 백엔드로 전송된다.

    2. FCM 백엔드는 메시지 요청을 수신하여 허락하고, 메시지 ID 같은 메타데이터를 생성하여 플랫폼 별 전송 레이어에 전달한다.

    3. 플랫폼 별 전송 레이어를 통해 온라인 상태(네트워크에 연결된 상태)에 있는 기기에 메시지가 전송된다.

    4. 앞서 등록했던 클라이언트 앱이 설치된 기기에 메시지 및 알림이 나타난다.

## 4️⃣ FCM을 사용하여 Android 앱에 메시지 전송하기(구현 과정)

✍🏻 [FCM 도큐먼트 - Android에서 구현하기](https://firebase.google.com/docs/cloud-messaging/android/client) 문서를 참고하여 작성합니다.

