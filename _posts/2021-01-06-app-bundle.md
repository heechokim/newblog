---
layout: post
title:  "[안드로이드] App Bundle이 뭘까?"
date:   2021-01-06 18:34:10 +0700
categories: [안드로이드]
---

> __참고 레퍼런스__
>
> ✍🏻 [Android Developer 도큐먼트 - app bundle 간략 소개](https://developer.android.com/platform/technology/app-bundle)
>
> ✍🏻 [Android Developer 도큐먼트 - About App Bundle](https://developer.android.com/guide/app-bundle)
>
> ✍🏻 [Android Developers Youtube 채널 - Building your first app bundle](https://www.youtube.com/watch?v=IPLhLu0kvYw)

## 1️⃣ What is App Bundle?

* __Android 앱 빌드 및 출시를 위한 새로운 체재(format)다.__

    * 기존에는 배포 준비가 된 APK 파일을 Google Play Console에 업로드하는 방식으로 앱 출시를 했음

    * 기존의 빌드 및 출시 방법이 App Bundle을 사용한 방식으로 새롭게 바뀜

    * Android App Bundle은 앱의 모든 컴파인된 코드, 리소스 뿐 아니라 APK 파일 생성 및 서명까지 Google Play에 맡기는 방식.

    * 일단 여기까지만 간단히 인지하고, 왜 App Bundle이 등장했는지 알아보자.

* __Android 앱의 APK 크기가 설치 전환율에 미치는 영향__

    * 👉🏻 [참고 문서](https://medium.com/googleplaydev/shrinking-apks-growing-installs-5d3fcba23ce2)

    * Google Play에 존재하는 앱은 평균 퀄리티가 점점 좋아지는 추세이다.

    * 다양한 기능, 좋은 해상도를 갖는 이미지, 더 나은 그래픽 등이 앱에 추가되면서 APK 크기는 커져만 가고 있다.

    * <img width="676" alt="01" src="https://user-images.githubusercontent.com/31889335/103724714-59be1a00-5018-11eb-9d8c-3c6a0cf962c4.png">

    * APK 크기는 사용자의 앱 설치율에 영향을 미친다는 연구 진행.

    * 연구 결과 : __APK 크기가 작을수록 사용자의 앱 설치율이 높다!__

    * 상세 결과 : APK 크기가 6MB 증가할 때마다 설치 전환율이 1% 감소한다.

    * 왜 APK 크기가 크면 설치율이 낮을까?

        * 사람들은 APK 크기가 크면 아래와 같은 문제들이 발생할 것임을 고려하기 때문.

        * 사람들은 앱 설치시 다운로드해야 하는 데이터 양과 이에 비례하는 cost를 고려한다.

        * 사람들은 앱을 다운받는데 걸리는 시간을 따진다. (당장 몇 초 이내로 다운받아지길 원하는 사람도 있음)

        * 앱 다운로드 중에 발생하는 네트워크 연결 문제를 고려한다.

    * 사용자들의 APK 크기 선호도와 설치율 관계가 나라마다 다를까?
        
        * 다르다.

        * 예를 들어, 인도와 인도네시아 사람들 중 50% 정도의 인원은 와이파이에 연결할 수 없다.

        * 즉, APK 를 다운로드 하는데 드는 비용을 모두 지불해야 한다..🙀

        * <img width="694" alt="02" src="https://user-images.githubusercontent.com/31889335/103725945-0b5e4a80-501b-11eb-91eb-22a8ddee9fe2.png">

        * > 이어서 작성하기!

* __왜 App Bundle이 등장함ㅁ?__

## 2️⃣ 왜 사용해야 하나?