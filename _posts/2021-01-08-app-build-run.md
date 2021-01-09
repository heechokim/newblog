---
layout: post
title:  "[안드로이드] App Build & Run(앱 빌드 & 실행)"
date:   2021-01-08 18:34:10 +0700
categories: [안드로이드]
---

> __참고 레퍼런스__
>
> ✍🏻 [Android Developer 도큐먼트 - 앱 빌드 overview](https://developer.android.com/studio/run)

## 1️⃣ Android Studio에서 앱을 빌드하고 실행하기

* <img width="400" alt="01" src="https://user-images.githubusercontent.com/31889335/104087885-5f309400-52a6-11eb-91e1-df264a978197.png">

* 위 그림처럼 Android Studio에서 왼쪽에는 app, 오른쪽에는 앱을 실행하려는 기기를 선택한다.

* 빨간 박스로 표시한 Run버튼을 클릭하여 앱을 실행시킨다.(위에서 선택한 실행 기기로 실행된다.)

## 2️⃣ 앱 실행 및 디버그에 관한 구성(Configuration) 변경하기

* 앱을 처음 실행할 시, Android Studio는 기본으로 설정되어 있는 앱 실행 구성을 사용해서 앱을 실행시킨다.

* 개발자는 앱 실행/디버그 구성을 변경할 수 있다.

* 앱 실행/디버그 구성을 변경하려면 Android Studio에서 Run > Edit Configurations를 클릭하면 된다.

* <img width="450" alt="02" src="https://user-images.githubusercontent.com/31889335/104013674-1d93e080-51f5-11eb-8be7-e712aaad3717.png">

* <img width="1079" alt="04" src="https://user-images.githubusercontent.com/31889335/104014380-42d51e80-51f6-11eb-80a9-72df6e08df78.png">

* __앱 실행/디버그 구성(App run configuration)에서 커스텀할 수 있는 항목들__

    * 앱 배포(Deploy)시, APK로 배포할지, App Bundle로 배포할지 설정

    * 실행할 모듈 설정

    * 배포할 패키지 설정

    * 시작할 Activity 설정

    * 타겟 디바이스 설정

    * Emulator 세팅 설정

    * logcat 옵션 설정

    * 등등
    
    * 추가 참고자료 --> [Android Developer 도큐먼트 - 실행/디버그 구성 생성 및 수정하기](https://developer.android.com/studio/run/rundebugconfig)

* __기본으로 설정되어 있는 앱 실행/디버그 구성 항목들__

    * 빌드 시 APK파일로 빌드

    * 기본으로 설정되어 있는 Activity로 시작(AndroidManifest.xml 파일에 launch로 설정되어 있는 Activity)

* 기본 앱 실행 구성이 자신의 프로젝트에 맞지 않을 경우 맞춤 변경 가능하다.

## 3️⃣ build variant 바꾸기

* variant = '여러 가지의, 변체, 다른' 의 의미를 갖는 단어

* Gradle은 하나의 프로젝트에서 여러 가지 버전의 APK를 빌드할 수 있는 기능을 제공한다.

* 예를 들어, 개발 중일 경우에 한해 앱을 __debug 버전__ 으로 빌드하여 Run(실행)할 수 있다.

* 안드로이드 프로젝트를 처음 생성하면 Gradle은 기본적으로 debug 버전, release 버전 두 가지 build type을 생성한다.

* Android Studio에서 Build > Select Build Variant를 클릭하면 build type을 변경해서 앱을 실행할 수 있는 창이 나타난다.

* <img width="316" alt="03" src="https://user-images.githubusercontent.com/31889335/104014256-1b7e5180-51f6-11eb-945d-3bc90d8a22de.png">

* <img width="398" alt="06" src="https://user-images.githubusercontent.com/31889335/104014899-22f22a80-51f7-11eb-9bb3-a8fc5fa8e230.png">

* 위 그림과 같이 Build Variants를 선택하는 창에는 Module과 Active Buile Variant라는 두 개의 항목이 있다.

* 기본적으로 Active Build Variant으로 설정할 수 있는 값은 위 그림과 같이 __debug__ 와 __release__ 가 존재하고, 선택한 빌드 타입이 해당 모듈의 빌드 버전으로 지정된다.

* 이곳에서 Active Build Variant로 선택된 빌드 타입으로 앱이 빌드되고, run을 클릭하면 이 빌드 타입으로 앱이 실행된다.

* 🌟 만약 개발하던 앱이 Google Play나 다른 마켓에 출시해야하는 순간이 온다면, Active Build Variant을 release 빌드 타입으로 설정해서 앱을 빌드해야 한다.

* 앱의 기능이나 앱 요구사항이 다른 다양한 경우가 존재하고, 경우에 따라 다르게 빌드를 하고 싶다면 debug, release 타입 별도의 구성 설정도 할 수 있다. [여기](https://developer.android.com/studio/build/build-variants?hl=ko) 참고

* <img width="815" alt="07" src="https://user-images.githubusercontent.com/31889335/104015667-4e294980-51f8-11eb-89d2-f3e07606b2fb.png">

* 위 그림처럼 buildTypes 구문을 사용하여 각 빌드 type별로 다른 설정을 해둘 수 있다.

## 4️⃣ 프로젝트 빌드하기

* 앞 내용들은 __앱 빌드__ 에 관한 내용이였다.

* 개발할 때, Run을 클릭하면 앱이 빌드되어 노트북에 연결한 실제 기기 or 애뮬레이터 기기에 앱이 설치된다.

* 🌟 하지만 만약 Google Play나 다른 마켓에 앱을 출시하려면 앞서 했던 앱 빌드와는 조금 다른 앱 빌드를 해줘야 한다.

* __출시를 위한 앱 빌드__

    1. 먼저 위에서 알아보았던 build variant를 원하는 빌드 타입으로 선택해야 한다.

    2. Android Studio의 build 메뉴의 옵션들을 사용해서 앱을 빌드해야 한다.

    * <img width="302" alt="08" src="https://user-images.githubusercontent.com/31889335/104016762-48ccfe80-51fa-11eb-83c5-77783ec75e2c.png">

    * 개발할 때 자주 사용되는 옵션만 아래서 알아보자.

    * __Clean Project__ : 중간 빌드 파일/캐싱된 빌드 파일을 모두 삭제

    * __Rebuild Project__ : Active Build Variant로 선택된 build type에 관해 Clean Project를 실행한 후, APK를 생성

    * <img width="430" alt="09" src="https://user-images.githubusercontent.com/31889335/104018040-892d7c00-51fc-11eb-9f13-1e69c2b73eb2.png">

    * __위 옵션의 Build APK(s)__ : Active Build Variant로 선택된 build type에 관해 현재 프로젝트 내 모든 모듈의 APK를 빌드

        * 빌드 완료 후, 아래와 같은 알림을 통해 빌드된 APK파일이 저장된 위치를 알 수 있고, [APK Analyzer](https://developer.android.com/studio/build/apk-analyzer)를 통해 APK 파일을 분석할 수 있다.

        * <img width="380" alt="10" src="https://user-images.githubusercontent.com/31889335/104087545-da447b00-52a3-11eb-8bab-ae98ae603972.png">

        * <img width="1345" alt="11" src="https://user-images.githubusercontent.com/31889335/104087524-acf7cd00-52a3-11eb-80f6-cbf185bd626c.png">

        * debug 빌드 타입으로 APK를 빌드하면 기본적으로 디버그 키로 APK가 서명된다.

        * release 빌드 타입으로 APK를 빌드하면 APK 서명이 자동으로 되지 않고, 개발자가 [수동으로 서명](https://developer.android.com/studio/publish/app-signing)해야 한다.

        * APK에 수동으로 서명하기 위해서 Build > Generate Signed Bundle/APK 를 선택해서 수동으로 서명하는 방법도 있다.

            <img width="294" alt="14" src="https://user-images.githubusercontent.com/31889335/104087748-4d9abc80-52a5-11eb-9e0a-6a17a478bd9b.png">

        * Android Studio는 빌드된 APK를 프로젝트의 project-name/module-name/build/outputs/apk 경로에 저장한다.

    * __위 옵션의 Build Bundle(s)__ : Active Build Variant로 선택된 build 타입에 관해 현재 프로젝트 내 모든 모듈의 App Bundle을 빌드

        * 빌드 완료 후, 아래와 같은 알림을 통해 빌드된 App Bundle파일이 저장된 위치를 알 수 있고, APK Analyzer를 통해 App Bundle 파일을 분석할 수 있다.

        * <img width="377" alt="13" src="https://user-images.githubusercontent.com/31889335/104087687-e4b34480-52a4-11eb-81ff-735c375f892d.png">

        * <img width="1356" alt="12" src="https://user-images.githubusercontent.com/31889335/104087662-bdf50e00-52a4-11eb-8d61-2e4e27081d69.png">

        * debug 빌드 타입으로 App Bundle을 빌드하면 기본적으로 App Bundle이 디버그 키로 서명되고, [bundletool](https://developer.android.com/studio/command-line/bundletool)을 사용해서 해당 App Bundle의 앱을 연결된 기기에 배포(설치)할 수 있다.

        * release 빌드 타입으로 App Bundle을 빌드하면 자동으로 App Bundle에 서명되지 않고, 개발자의 수동 서명이 필요하다.

        * App Bundle에 수동으로 서명하기 위해서 [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)을 사용하여 수동으로 서명하거나, Android Studio에서 Buile > Generate Signed Bundle/APK 선택하여 수동 서명할 수 있다.

        * Android Studio는 App Bundle을 프로젝트의 project-name/module-name/build/outputs/bundle 경로에 저장한다.

# 끝!