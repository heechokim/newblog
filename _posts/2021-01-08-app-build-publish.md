---
layout: post
title:  "[안드로이드] Android 앱 출시하기"
date:   2021-01-08 18:34:10 +0700
categories: [안드로이드]
---

> __참고 레퍼런스__
>
> ✍🏻 [Android Developer 도큐먼트 - 앱 출시 overview](https://developer.android.com/studio/publish)
>
> ✍🏻 [Android Developer 도큐먼트 - 출시 준비](https://developer.android.com/studio/publish/preparing)
>
> ✍🏻 [Android Developer 도큐먼트 - 앱 버전 지정하기](https://developer.android.com/studio/publish/versioning)

## 1️⃣ What is 앱 출시(Publish, Release)?

* Android 앱을 사용자에게 제공할 수 있는 일반적인 방법 => 앱 출시하기!

* 일반적으로는 [Google Play](https://developer.android.com/distribute/google-play)에 Android 앱 출시한다.

    * Google Play 외에도 [원 스토어](https://m.onestore.co.kr/mobilepoc/main/main.omp?NaPm=ct%3Dkjnwcj0o%7Cci%3Dcheckout%7Ctr%3Dds%7Ctrx%3D%7Chk%3D1c5df61768c378ecf28bc88abde4bb1fb03b6845) 등 Android 앱을 출시할 수 있는 마켓들이 존재한다.

* Google Play에 Android 앱을 출시하기 전, [확인하면 좋을 체크 리스트](https://m.onestore.co.kr/mobilepoc/main/main.omp?NaPm=ct%3Dkjnwcj0o%7Cci%3Dcheckout%7Ctr%3Dds%7Ctrx%3D%7Chk%3D1c5df61768c378ecf28bc88abde4bb1fb03b6845)를 확인하는 것이 좋다.

* 앱을 출시하기 위한 프로세스는 크게 2가지로 나뉘고, 각 프로세스에는 여러 단계가 포함되어 있다.

    1. __출시를 위한 앱 준비하기__

    2. __사용자에게 앱 배포하기__

* 아래 내용을 통해 각 프로세스에 대해 알아보자!

## 2️⃣ 앱 출시 프로세스 1단계 - 출시를 위한 앱 준비하기

1. __출시를 위해 앱의 여러가지 구성 확인하기__

    * manifest 파일에 [android:debugable](https://developer.android.com/guide/topics/manifest/application-element#debug) 속성이 존재한다면 제거하거나 false로 설정하기

        * android:debugable 속성에 true 속성값 설정을 하면 앱이 유저 모드에서 실행되고 있어도 debug가 가능하다.

    * manifest 파일에 android:versionName, android:versionCode, android:icon, android:label 속성 추가하기

    * Log 호출 코드 최대한 삭제하기

    * 프로젝트 디렉터리 정리하기

        * [Android 프로젝트 관리](https://developer.android.com/studio/projects#ApplicationProjects) 에서 설명하는 디렉터리 구조를 따르고 있는지 점검해야 한다.

        * 추가로 디렉터리 별 어떤 항목을 점검해야 하는지는 [여기](https://developer.android.com/studio/publish/preparing#clean-up-your-project-directories)를 참고하자.

    * 좋은 패키지 이름 선택하기

        * 패키지 이름은 Google Play에 앱을 출시할 때, 앱의 고유 ID로 사용되므로 의미있게 짓는 것이 좋다.
    
        * 또한 패키지 이름은 앱이 사용자에게 출시된 이후에는 절대로 변경할 수 없기 떄문에 패키지 이름을 대충 짓는 것은 지양해야 한다.

        * [pakage 이름에 관한 참고 문서](https://developer.android.com/guide/topics/manifest/manifest-element#package)

    * 앱 버전 설정하기

        * 앱 버전 설정에 관해서는 [이 블로그의 다른 포스팅 - 앱 버전 설정하기](https://choheeis.github.io/newblog//articles/2021-01/app-version)에서 확인하자.

    * 다양한 기기와 호환 가능한지 점검하기

        * [Android Developer 도큐먼트 - 화면 호환성 overview](https://developer.android.com/guide/practices/screens_support#screen-independence)를 참고하여 여러 화면에 대응할 수 있는 앱을 개발하는 것이 좋다.(사용자는 다양한 화면 크기의 디바이스를 사용하기 때문!)

    * Gradle을 사용하여 앱을 빌드하는 프로젝트라면 release 빌드 타입으로 설정하기

        * [이 블로그의 다른 포스팅 - 앱 빌드](https://choheeis.github.io/newblog//articles/2021-01/app-build-run) 에서 debug, release 빌드 타입에 관해 알 수 있다!

2. __출시(release) 버전의 App 빌드하고 서명하기(App Signing)__

    * release 빌드 타입으로 앱을 빌드하고 이에 서명해야 한다.

    * 앱 빌드에 관해서는 [이 블로그의 다른 포스팅 - 앱 빌드](https://choheeis.github.io/newblog//articles/2021-01/app-build-run) 을 참고하자.

    * 앱 서명에 관해서는 [이 블로그의 다른 포스팅 - 앱 서명](https://choheeis.github.io/newblog//articles/2021-01/app-signing)을 참고하자.

3. __출시할 수 있도록 앱에 사용되는 리소스 최종 점검하기__

    * 이미지, 멀티미디어 파일, 그래픽 등 앱에 사용되는 모든 리소스가 앱 자체 리소스에 포함되거나 프로덕션 서버 저장소에 저장되어 있는지 꼼꼼히 확인해야 한다.

4. __앱이 의존하는 원격 서버나 외부 서비스 점검하기__

    * 앱이 의존하고 있는 원격 서버나 외부 서비스(AWS, Firebase 등)를 최종적으로 점검해야 한다.

    * 특히 앱이 서버에 접속하는 경우 테스트 URL이 아닌 프로덕션용 URL을 통해 접속하는지 확인해야 한다.

5. __출시 버전의 앱을 테스트 기기에 배포 후, 최종 테스트하기__

    * 앱을 Google Play나 마켓에 배포하기 전에, 1대 이상의 디바이스 기기와 테블릿에서 출시 버전의 앱을 철저히 테스트해야 한다.(생성한 APK를 테스트 기기에 설치)

    * UI 요소의 크기가 올바르게 보이는지 확인

    * 앱 성능과 배터리 효율이 허용 수준인지 확인

    * [테스트할 사항에 관한 참고 문서](https://developer.android.com/docs/quality-guidelines/core-app-quality)

6. __Google Play나 마켓에 출시하기__

    * 앱 출시를 위해 Google Play나 마켓 자체에서 요구하는 여러 프로세스들이 존재할 수 있다.

    * 예를 들어, 앱 아이콘 준비/앱 홍보 문구/앱 스크린샷 준비 등

    * [Google Play 출시 시 등록 정보에 관한 참고 문서](https://support.google.com/googleplay/android-developer/answer/9866151?visit_id=637458015748945882-3309950274&rd=1#zippy=%2C%EA%B3%A0%ED%95%B4%EC%83%81%EB%8F%84-%EC%95%84%EC%9D%B4%EC%BD%98)

    * [Google Play 필터링에 관한 참고 문서](https://developer.android.com/google/play/filters)

    * [Google Play 라이선스에 관한 참고 문서](https://developer.android.com/google/play/licensing)

* __그 외 준비물__

    * 앱 개발자와 앱 사용자 간의 계약 각서인 [최종 사용자 라이선스 계약서(EULA)](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%EC%82%AC%EC%9A%A9%EA%B6%8C_%EB%8F%99%EC%9D%98) 를 준비하는 것이 좋다.

    * EULA는 개발자의 인력, 조직, 지적 재산을 보호하는데 도움이 되고, 앱에 포함하여 EULA를 제공하는 것이 좋다.

    
## 3️⃣ 앱 출시 프로세스 2단계 - 사용자에게 앱 배포하기 

* __개발자__

    * 보통 Google Play같은 앱 마켓에 앱을 배포한다.

    * 자체 웹 사이트에 앱을 배포할 수도 있고, 사용자에게 직접 앱을 전송하여 배포할 수도 있다.

* __사용자__

    * 보통 Google Play같은 앱 마켓에 존재하는 앱을 설치한다.

* __Google Play에 앱 출시하기__

    * 자세한 내용은 [Google Play](https://developer.android.com/distribute/google-play)를 참고하면 된다.

* __웹 사이트에 앱 출시하기__

    * Google Play나 오픈 마켓에 앱을 출시하지 못하는 경우도 있을 수 있다.

    * 예를 들면, 회사 내부에서만 사용하는 앱!

    * 이런 경우, 비공개 웹 사이트나 회사 내부 사이트에 앱을 출시할 수 있다.

    * 출시 버전의 앱 APK를 웹 사이트에 호스팅하고, 사용자에게 다운로드 링크를 제공하면 된다.

    * 사용자가 다운로드 링크에 접속하면 사용자의 디바이스 기기에 APK 파일이 설치된다.

## 4️⃣ 앱 출시 프로세스 1단계(출시를 위한 앱 준비하기)과정 더 자세히 알아보기!

* 참고자료 --> [Android Developer 도큐먼트 - 앱 출시 준비하기](https://developer.android.com/studio/publish/preparing)

* 출시를 위한 앱을 준비하려면 다음과 같은 것들을 준비해야 한다.

    1. __앱을 출시 버전으로 구성하기__ : 

    2. __구성된 출시 버전으로 앱 빌드하기__ : 

    3. __앱 테스트하기__

> 출시를 위한 앱 구성부터 이어서 읽고, 적합한 위치에 내용 껴넣기