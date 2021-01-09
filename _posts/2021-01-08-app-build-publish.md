---
layout: post
title:  "[안드로이드] App Build & Publish(앱 빌드, 출시)"
date:   2021-01-08 18:34:10 +0700
categories: [안드로이드]
---

> __참고 레퍼런스__
>
> ✍🏻 [Android Developer 도큐먼트 - Publish App](https://developer.android.com/studio/publish)
>
> ✍🏻 [Android Developer 도큐먼트 - 출시 준비](https://developer.android.com/studio/publish/preparing?hl=ko)

## 1️⃣ What is 앱 출시(Publish, Release)?

* Android 앱을 사용자에게 제공할 수 있는 일반적인 방법 => 앱 출시하기!

* 일반적으로는 Google Play에 Android 앱 출시한다.

    * Google Play 외에도 [원 스토어](https://m.onestore.co.kr/mobilepoc/main/main.omp?NaPm=ct%3Dkjnwcj0o%7Cci%3Dcheckout%7Ctr%3Dds%7Ctrx%3D%7Chk%3D1c5df61768c378ecf28bc88abde4bb1fb03b6845) 등 Android 앱을 출시할 수 있는 마켓들이 존재한다.

* Google Play에 Android 앱을 출시하기 위해서는 [확인하면 좋을 체크 리스트](https://m.onestore.co.kr/mobilepoc/main/main.omp?NaPm=ct%3Dkjnwcj0o%7Cci%3Dcheckout%7Ctr%3Dds%7Ctrx%3D%7Chk%3D1c5df61768c378ecf28bc88abde4bb1fb03b6845) 확인을 권장하고 있다.

* 앱을 출시하기 위한 프로세스는 크게 2가지(출시를 위한 앱 준비, 사용자에게 앱 출시하기)로 나뉘고, 각 프로세스에는 여러 단계가 포함되어 있다.

## 2️⃣ 앱 출시 프로세스 1단계 - 출시를 위한 앱 준비하기

1. __출시를 위한 앱 구성(Configuration) 설정하기__

    * Log 호출 코드 최대한 삭제하기

    * manifest 파일에서 [android:debugable](https://developer.android.com/guide/topics/manifest/application-element#debug) 속성 제거하기

        * android:debugable 속성에 true 속성값 설정을 하면 앱이 유저 모드에서 실행되고 있어도 debug가 가능하다.

    * manifest 파일에서 android:versionName, android:versionCode 속성 추가하기

2. __출시(release) 버전의 App 빌드 및 서명하기(app signing)__

    * release 빌드 type이 설정된 Gradle build 파일을 사용하여 앱 출시 버전을 빌드하고 이에 서명할 수 있다.

    * 앱 빌드에 관해서는 [Android Developer 도큐먼트 - 앱 빌드 및 실행](https://developer.android.com/studio/run) 을 참고하자.

    > 이어서!