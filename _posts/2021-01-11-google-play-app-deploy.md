---
layout: post
title:  "[안드로이드] Google Play에 앱 출시하기"
date:   2021-01-11 18:34:10 +0700
categories: [안드로이드]
---

> __참고 레퍼런스__
>
> ✍🏻[Google Play - 앱 만들기](https://support.google.com/googleplay/android-developer/answer/9859152?hl=ko&ref_topic=7072031)
>
> ✍🏻[Google Play - 대시보드에서 앱 설정하기](https://support.google.com/googleplay/android-developer/answer/9859454#zippy=)
>
> ✍🏻[Google Play - 버전 준비 및 출시하기](https://support.google.com/googleplay/android-developer/answer/9859348)



## 1️⃣ Google Play에 앱 출시하는 방법

* 보통 Android 앱 개발자들은 Google Play에 자신의 앱을 출시하여 사용자들에게 앱을 제공한다.

* __Google Play에 앱을 출시하기 전, 준비해야 하는 것들이 있다.__

    * [이 블로그의 다른 포스팅 - 앱 출시하기](https://choheeis.github.io/newblog//articles/2021-01/app-build-publish)

    * [이 블로그의 다른 포스팅 - 앱 서명하기](https://choheeis.github.io/newblog//articles/2021-01/app-signing) 

    * 위 2개의 포스팅을 먼저 읽고, 서명한 APK나 App Bundle을 준비해야 한다.

* __Google Play에 신규 앱을 출시하는 경우__

    1. __업로드 키 생성하고, 앱에 서명하기__

        * [이 블로그의 다른 포스팅 - 앱 서명하기](https://choheeis.github.io/newblog//articles/2021-01/app-signing) 를 참고하여 업로드 키를 생성하고, App Bundle에 서명해야 한다.

        * __중요 )__ 2021년 8월부터는 Google Play에 신규 앱을 출시할 경우, Android App Bundle로 앱을 출시해야 한다.

            또한 App Bundle 크기가 150MB 이상인 앱일 경우, [Play Asset Delievery](https://developer.android.com/guide/app-bundle/asset-delivery)나 [Play Feature Delievery](https://developer.android.com/guide/app-bundle/play-feature-delivery)를 사용해야 한다.

    2. __앱 만들기__

        * [Google Play Console](https://play.google.com/console/about/)로 접속하여 Google Play 개발자 계정으로 로그인 한 후, 앱 만들기를 진행해야 한다.

        * 해당 개발자 계정으로 만든 앱이 아직 없을 경우, 아래와 같이 첫 앱을 만들 수 있는 UI를 볼 수 있다.

        * <img width="451" alt="01" src="https://user-images.githubusercontent.com/31889335/104141241-23f2aa00-53f9-11eb-9473-4c9092912d45.png">

        * 첫 앱을 이미 만들어 놓은 경우에는 Google Play Console 어딘가에 새로운 앱을 추가로 만들 수 있는 UI가 있을 것이다^^

        * 앱 만들기를 클릭한 후, 아래와 같이 만들 앱의 세부 정보를 입력한다.

        * <img width="926" alt="02" src="https://user-images.githubusercontent.com/31889335/104141246-25bc6d80-53f9-11eb-8b7c-c8f1bc90b4bf.png">

        * 앱 만들기를 완료하면 해당 앱에 관한 대시보드가 생성된다. 대시보드를 통해 해당 앱에 설정, 출시, 테스트 등 앱을 출시하기 위해 반드시 준비해야 하는 다양한 작업을 진행할 수 있다.

        * <img width="1303" alt="03" src="https://user-images.githubusercontent.com/31889335/104141249-27863100-53f9-11eb-91b5-94412b4e094b.png">

    3. __앱 설정하기__

        * 앱을 만든 후에는 해당 앱에 필요한 세부 설정을 진행해야 한다.

        * 앱 설정은 대시보드에서 할 수 있고, 아래와 같이 크게 __앱 콘텐츠에 관한 정보 입력__ 과 __앱이 분류 및 표시되는 방식 관리__ 에 대한 설정을 해야 한다.

        * <img width="770" alt="04" src="https://user-images.githubusercontent.com/31889335/104141448-36b9ae80-53fa-11eb-8654-84718b90c204.png">

            * __앱 엑세스 권한 설정__ : 사용자가 해당 앱의 모든 기능에 엑세스할 수 있는지, 부분 기능만 엑세스할 수 있는지에 관한 설정

            * __광고__ : 해당 앱이 광고를 포함하고 있는지에 관한 설정

            * __콘텐츠 등급__ : 해당 앱에 포함된 콘텐츠에 관한 국가별 콘텐츠 등급 설정(한국 예시 - 만 3세 이상 허용)

            * __타겟층__ : 해당 앱의 타켓 유저 연령대 층 설정

            * __뉴스 앱__ : 해당 앱이 뉴스 앱인지에 관한 설정

            * __앱 카테고리 선택 및 연락처 세부정보 제공__ : Google Play Store의 어느 카테고리에 앱이 포함되기 원하는지, 사용자에게 보여주는 개발자 정보 등 설정

            * __스토어 등록 정보 설정__ : 앱 아이콘, 스크린샷, 그래픽이미지, 테블릿샷 등에 관한 설정

        * 앱 세부 설정을 아래와 같이 모두 완료해야 앱을 출시할 수 있다.

        * <img width="752" alt="05" src="https://user-images.githubusercontent.com/31889335/104142435-eee95600-53fe-11eb-974b-65ed1cd978ac.png">

    2. __버전 준비하기__

        * Google Play에서는 '버전'이라는 기능을 제공한다.

        * 버전을 사용하면 App Bundle이나 APK를 관리할 수 있고, 특정 트랙으로 앱을 출시할 수도 있다.

    > 이어서 작성해야 함!

* __Google Play에 이미 출시되어 있는 기존 앱을 
