---
layout: post
title:  "[Firebase] Firebase Cloud Messaging 사용기"
date:   2020-11-21 18:34:10 +0700
categories: [Firebase]
---

## 0️⃣ Firebase Cloud Messaging 이란!

✍🏻 [FCM 도큐먼트](https://firebase.google.com/docs/cloud-messaging?hl=ko)를 참고하여 작성합니다.

줄여서 __FCM__ 이라고도 부르는 Firebase Cloud Messaging은 Firebase에서 제공하는 __무료 메시지 전송 플랫폼__ 이다.

Android, iOS, Web, C++, Unity 등의 개발에서 메시지 전송 기능을 구현할 때 사용할 수 있다.

[Firebase에 대해 소개하는 유튜브 영상](https://youtu.be/sioEY4tWmLI)도 한 번 시청하자^^

그럼 이제 FCM으로 정확히 어떤 기능들을 구현할 수 있는지 알아보자.

## 1️⃣ FCM이 제공하는 주요 기능 3가지!

✍🏻 [FCM 도큐먼트 - 주요 기능](https://firebase.google.com/docs/cloud-messaging?hl=ko#key-capabilities)를 참고하여 작성합니다.

FCM을 사용하여 구현할 수 있는 기능을 총 3가지가 존재한다.

1. __알림 메시지 전송 or 데이터 메시지 전송__

    앱이나 웹, 게임 등을 이용하는 사용자에게 알림 메시지를 전송할 수 있다.

    또는 사용자에게 데이터 메시지를 전송하고 애플리케이션 코드에서 임의로 처리할 수 있다.

    알림 메시지와 데이터 메시지 등 메시지 유형에 관해서는 [여기](https://firebase.google.com/docs/cloud-messaging/concept-options?hl=ko#notifications_and_data_messages)에서 알아볼 수 있으니 여기서는 일단 넘어가도록 하자.

2. __다양한 메시지를 전송받을 기기 타겟팅__

    단일 기기, 그룹 기기, 특정 주제를 구독한 기기 등 3가지 타겟팅 방식으로 클라이언트 앱에 메시지를 배포할 수 있다.

3. __클라이언트 앱에서 메시지 전송__

    메시지를 전송 받은 클라이언트 기기에서 다시 서버로 확인, 채팅 등의 메시지를 보낼 수 있다.

이렇게 FCM에서 제공하는 3가지 기능에 대해 간략하게 알아봤다!

## 2️⃣ FCM 작동 원리 및 아키텍처

✍🏻 [FCM 도큐먼트 - 아키텍처](https://firebase.google.com/docs/cloud-messaging/fcm-architecture?hl=ko)를 참고하여 작성합니다.

FCM이 제공하는 __메시지 전송__ 기능은 아래 그림과 같은 4개의 컴포넌트 세트에 의존하여 작동된다.

<img width="669" alt="01" src="https://user-images.githubusercontent.com/31889335/99872085-94175780-2c22-11eb-848d-37ac47c3df07.png">

4개의 컴포넌트 각각에 대해 알아보고 어떻게 이어지는지 알아보자.

1. __메시지 요청을 작성하거나 구현하는 도구__

    <img width="150" alt="02" src="https://user-images.githubusercontent.com/31889335/99872137-fa03df00-2c22-11eb-9013-75101fd6a004.png">

    FCM 메시지 전송 기능을 작동시키는 아키텍처의 첫 번째 컴포넌트는 __메시지 요청을 작성하거나 구현하는 도구__ 이다.

    이 도구에는 2가지 종류의 도구가 존재하는데 하나는 [Firebase Notifications Console](https://console.firebase.google.com/) 이라는 GUI 기반의 사이트이고, 다른 하나는 Firebase Admin SDK 또는 Firebase 서버 프로토콜을 지원하는 서버 환경이 두 번째 도구이다.

    위 두 가지 도구를 사용하여 메시지 요청을 작성하거나 구현하는 것이 FCM 메시지 기능의 첫 컴포넌트인 것이다.

    특히, [모든 유형의 메시지](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages)를 완벽하게 자동화하려면  



    