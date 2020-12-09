---
layout: post
title:  "[Firebase] Firebase Cloud Messaging(FCM) 사용기"
date:   2020-11-21 18:34:10 +0700
categories: [Firebase]
---

## 0️⃣ Firebase Cloud Messaging 이란!

✍🏻 [FCM 도큐먼트](https://firebase.google.com/docs/cloud-messaging?hl=ko)를 참고하여 작성합니다.

줄여서 __FCM__ 이라고도 부르는 Firebase Cloud Messaging은 Firebase에서 제공하는 __무료 메시지 전송 라이브러리__ 이다.

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

FCM이 제공하는 __메시지 전송__ 기능은 아래 그림과 같은 4개의 컴포넌트 세트에 의존하여 작동되는 아키텍처를 갖는다.

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

    FCM 백엔드에서 메시지 전송 요청을 수락한다고 곧바로 실기기에 메시지가 전송되는 것은 아니다.
    
    FCM 백엔드와 실기기 사이에 세 번째 컴포넌트인 __전송 레이어(layer)__ 가 끼워져 있기 때문이다.
    
    전송 레이어에서는 플랫폼 별 메시지 요청 구성을 정의하고, 기기로 타겟팅된 메시지를 라우팅(안드로이드 기기에 보낼지, iOS 기기에 보낼지, Web에 보낼지 등을 처리)한 후, 메시지 전송을 처리한다.

    여기서 알아두어야 할 것은 전송 레이어는 FCM에서 제공하는 서비스가 아닌 FCM 외부에 존재하는 서비스라는 것이다.

4. __네 번째 컴포넌트 - 사용자 기기의 FCM SDK__ 

    <img width="627" alt="05" src="https://user-images.githubusercontent.com/31889335/100587246-a9cc0180-3333-11eb-8967-6d85861471a2.png">

    사용자 기기에 알림이 표시되거나 앱의 포그라운드/백그라운드 및 앱 상태 관련 로직에 따라 메시지가 처리된다.

## 3️⃣ FCM 작동 흐름 정리

FCM 서비스를 사용하여 메시지 전송 기능을 구현하는 과정의 작동 원리를 순차적으로 다시 한 번 이해해보자.

1. __FCM에서 보낸 메시지를 수신할 클라이언트 앱(여기선 device로 표현)을 등록한다.__

    FCM에서 보낸 메시지를 받을 클라이언트 앱의 인스턴스를 Firebase 프로젝트에 등록해야 한다.

    > 등록 방법은 아래에 정리하였다.

    클라이언트 앱의 인스턴스를 등록하면 해당 클라이언트 앱의 고유 등록 토큰을 얻게 된다.

2. __메시지 전송 및 수신을 4단계에 거쳐 처리한다.__

    1. 메시지 전송 요청이 알림 작성기나 신뢰할 수 있는 서버 환경에서 작성되면 이 요청이 FCM 백엔드로 전송된다.

    2. FCM 백엔드는 메시지 요청을 수신하여 허락하고, 메시지 ID 같은 메타데이터를 생성하여 플랫폼 별 전송 레이어에 전달한다.

    3. 플랫폼 별 전송 레이어를 통해 온라인 상태(네트워크에 연결된 상태)에 있는 기기에 메시지가 전송된다.

    4. 앞서 등록했던 클라이언트 앱이 설치된 기기에 메시지 및 알림이 보여진다.

## 4️⃣ FCM을 사용하여 Android 앱에 메시지 전송하기(구현 과정)

✍🏻 [Firebase 도큐먼트 - Android에서 Firebase 사용하기](https://firebase.google.com/docs/android/setup) 문서를 참고하여 작성합니다.

FCM 서비스를 사용하기 위해서 가장 먼저 해야할 일은 __Android 프로젝트에서 Firebase(Firebase Cloud Messaging 라이브러리가 아니라 Firebase 자체)를 사용하기 위한 세팅 작업__ 이다.

FCM은 Firebase가 지원하는 라이브러리 중 하나일 뿐이기 때문에 FCM 을 사용하려면 Firebase 자체를 사용하기 위한 작업을 해야하는 것이다.

1. __기본 설정 맞춰주기__

    Firebase를 Android 프로젝트에서 사용하려면 꼭 맞춰줘야 하는 몇 가지 설정이 있다.

    * Android Studio 설치 및 최신 버전으로 업데이트하기

    * Android Studio로 생성한 프로젝트가 다음 요구 사항들을 충족하는지 확인하기

        * 프로젝트가 Android API Level 16(Jelly Bean, 젤리빈) 이상을 타겟팅하는가?

        * 프로젝트가 Build Tool인 Gradle 버전이 4.1 이상을 사용하는가?

        * 프로젝트에서 사용하는 com.android.tools.build:gradle 라는 라이브러리의 버전이 3.2.1 이상인가?

            > Project 수준의 build.gradle 파일에서 확인할 수 있다.

        * 프로젝트의 compileSdkVersion 이 28 이상인가?
    
    * Google 계정으로 [Firebase Console에 로그인](https://console.firebase.google.com/)하기

2. __Android 클라이언트 앱을 Firebase에 연결하기__

    기본 설정을 맞춰주었다면 Android 클라이언트 앱을 Firebase에 연결시키는 작업을 해야한다.

    Android 클라이언트 앱을 Firebase에 연결시키는 작업은 아래의 순서대로 진행하면 된다.

    1. __Firebase 프로젝트 만들기__

        먼저 Android 클라이언트 앱과 연결할 Firebase 프로젝트를 만들어야 한다.

        [Firebase Console](https://console.firebase.google.com/) 에서 __프로젝트 추가__ 를 클릭한 후, 하라는 대로 하면 된다!

        Firebase 프로젝트를 생성하면 해당 프로젝트만의 고유 프로젝트 ID가 부여된다.

    2. __Firebase 프로젝트에 클라이언트 앱 등록하기__

        앞서 생성한 Firebase 프로젝트에 Android 클라이언트 앱을 등록해야 한다.

        앱 등록이라는 것은 보통 Firebase 프로젝트에 [앱을 추가하는 것](https://firebase.google.com/docs/projects/learn-more#adding_apps_to_a_project) 을 의미한다.

        이제 Firebase 프로젝트에 앱을 추가해보자.

        1. [Firebase Console](https://console.firebase.google.com/) 사이트에서 앞서 생성한 프로젝트를 클릭해 해당 프로젝트의 대시보드로 접속하기

            <img width="961" alt="06" src="https://user-images.githubusercontent.com/31889335/100844565-5f778b80-34bf-11eb-8219-7fd22d4d2742.png">

            위와 같은 사이트가 프로젝트의 대시보드이다.

        2. 위 그림에서 빨간 박스로 표시해둔 앱 추가하기 버튼 중 Android 아이콘을 선택하여 앱 추가 과정을 진행하기

            [Android 패키지 이름](https://developer.android.com/studio/build/application-id)을 작성해야 하는 과정이 있다.
            
            Android 디바이스 기기와 Google Play 스토어에서는 이 Android 패키지 이름을 통해 Android 클라이언트 앱을 고유하게 식별한다.

            즉, Android 패키지 이름 = 어플리케이션 ID 이다.

            Android 패키지 이름은 Android 프로젝트의 모듈 수준의 build.gradle 파일에서 찾을 수 있다.

            <img width="732" alt="07" src="https://user-images.githubusercontent.com/31889335/100848741-e2e7ab80-34c4-11eb-912a-16649a93815f.png">

            Android 패키지 이름 작성 외에 디버그 서명 인증서 SHA-1 을 작성하는 과정이 있는데 이것은 Firebase에서 지원하는 로그인 기능을 구현할 때 필요하므로 FCM에서는 설정하지 않아도 된다.

            google-services.json 파일을 Android 프로젝트에 추가하는 과정이 있는데 이 파일이 무엇인지는 [Firebase 구성파일 도큐먼트](https://firebase.google.com/docs/projects/learn-more#config-files-objects)를 읽어보면 알 수 있다.

            이 파일을 추가할 때 반드시 google-services.json 라는 이름 그대로 추가되어야 하고, google-services(2).json 같은 이름으로 추가되지는 않았는지 주의해야 한다.

            나머지는 하라는 대로 따라하면 문제 없을 것이다.

    여기까지 하면 Firebase 프로젝트에 Android 클라이언트 앱 등록이 완료된다!

이제 FCM 라이브러리를 사용하기 위한 본격적인 작업을 해보자!

✍🏻 [FCM 도큐먼트 - Android에서 구현하기](https://firebase.google.com/docs/cloud-messaging/android/client) 를 참고하여 작성합니다.

1. __FCM 라이브러리 종속성 추가하기__

    앞서 Firebase 프로젝트에 Android 클라이언트 앱을 등록하는 과정 중 앱 수준 build.gradle 파일에 [Firebase Android Bom](https://firebase.google.com/docs/android/learn-more#bom) 종속성을 추가해서 모든 Firebase 라이브러리 버전을 관리하는 작업이 있었을 것이다.

    ~~~kotlin
    // 앱 수준의 buile.gradle 파일
    dependencies {
        // Firebase Bom 종속성 추가(버전 관리용)
        implementation platform('com.google.firebase:firebase-bom:26.1.0')

        // Firebase가 제공해주는 여러 라이브러리 중 자신이 사용할 라이브러리를 버전 없이 추가해주기
        implementation 'com.google.firebase:firebase-analytics-ktx'
    implementation 'com.google.firebase:firebase-messaging-ktx'
        }
    ~~~

    우리는 Firebase에서 제공해주는 라이브러리 중 FCM 라이브러리를 사용할 것이므로 위와 같이 firebase-analytics와 firebase-messaging 라이브러리 종속성을 추가하자.

    FCM은 Google Analytics 와 함께 사용하는 것이 나중에 최적화하기 좋다.(구글 애널리틱스는 구글에서 제공하는 데이터 분석 서비스로, FCM과 함께 사용할 경우 메시지 전송 횟수 및 사용자 행동 분석 등을 분석할 수 있다.)

2. __앱 manifest 파일 수정하기__

    Android 프로젝트의 manifest 파일에 [FirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService?hl=ko) 클래스를 확장하는 service를 추가해야 한다.

    이 service를 추가하는 이유는 포그라운드에서 앱의 알림을 수신할 수 있도록 하기 위함 뿐만 아니라 백그라운드에서도 앱의 알림을 수신할 수 있도록 하기 위해서이다.

    [manifest 샘플 github](https://github.com/firebase/quickstart-android/blob/afa250e1c45ec45f633cfd94878ed3a655d1525e/messaging/app/src/main/AndroidManifest.xml) 코드를 참고하여 작성해보자.

    ~~~xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.fcm">

        <application
            ...
            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>

            <!-- FCM service 추가(코틀린, 자바에 따라 조금 다를 수 있음) -->
            <service
                android:name="여기는 일단 비워두자. 바로 다음 단계에서 채워줄 것임!"
                android:exported="false">
                <intent-filter>
                    <action android:name="com.google.firebase.MESSAGING_EVENT" />
                </intent-filter>
            </service>
        </application>

    </manifest>
    ~~~

    서비스의 name 속성을 지정해주기 위해 MyFirebaseMessagingService 라는 이름의 클래스 파일을 만들어야 한다.(파일 이름은 자유롭게 수정 가능) 

    <img width="543" alt="08" src="https://user-images.githubusercontent.com/31889335/100853707-686e5a00-34cb-11eb-954f-10bfa1738e60.png">
    
    생성한 클래스 파일을 [MyFirebaseMessagingService 샘플 github](https://github.com/firebase/quickstart-android/blob/afa250e1c45ec45f633cfd94878ed3a655d1525e/messaging/app/src/main/java/com/google/firebase/quickstart/fcm/kotlin/MyFirebaseMessagingService.kt) 코드를 참고하여 추가로 작성해보자.

    FirebaseMessagingService 클래스를 상속받아야 하며 [FCM 도큐먼트 - 메시지 수신](https://firebase.google.com/docs/cloud-messaging/android/receive?hl=ko) 를 참고하여 재정의해야 하는 메소드들을 확인 후, 각 상황에 맞는 코드를 재정의 함수 안에 작성하면 된다.

    일단 지금은 MyFirebaseMessagingService 클래스 파일에 어떤 함수들을 재정의해야 하는지, 어떤 코드들을 작성해야 하는지 등은 넘어가자.(뒤에 나오는 '포그라운드 상태의 앱에서 메시지 받기 부분'에서 다시 설명한다.)

    이제 다시 manifest 파일로 돌아와서 비워두었던 android:name 속성을 채워주자. MyFirebaseMessagingService 클래스 파일의 위치를 속성값으로 작성해주면 된다.

    ~~~xml
    // 예시
    <service
        android:name=".MyFirebaseMessagingService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT" />
        </intent-filter>
    </service>
    ~~~

    클래스 파일을 패키지 안에 넣었다면 위 코드와 달라질 수 있다! (예 - fcmUtil 이라는 패키지를 생성하고 그 안에 넣었다면 .fcmUtil.MyFirebaseMessagingService 가 속성값이 된다.)

3. __알림 아이콘 및 색상과 관련된 메타 데이터 추가하기__

    알림 아이콘 및 알림 색상 설정과 관련된 메타 데이터를 manifest 파일의 \<application> 태그 안에 추가로 작성할 수 있다.

    [알림 아이콘 및 색상 관련 메타 데이터 샘플 github 코드](https://github.com/firebase/quickstart-android/blob/afa250e1c45ec45f633cfd94878ed3a655d1525e/messaging/app/src/main/AndroidManifest.xml#L13-L22) 를 참고하여 작성해보자.

    아이콘 drawable과 색상은 마음대로 수정 가능하다.

    ~~~xml
    <!--  메타 데이터 추가 예시 -->
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.fcm">

        <application
            ...

            <!-- Set custom default icon (메타 데이터 추가) -->
            <meta-data
                android:name="com.google.firebase.messaging.default_notification_icon"
                android:resource="@drawable/ic_launcher_foreground" />

            <!-- Set color used with incoming notification messages (메타 데이터 추가) -->
            <meta-data
                android:name="com.google.firebase.messaging.default_notification_color"
                android:resource="@android:color/holo_blue_dark" />

            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>

            <!-- FCM service -->
            <service
                android:name=".MyFirebaseMessagingService"
                android:exported="false">
                <intent-filter>
                    <action android:name="com.google.firebase.MESSAGING_EVENT" />
                </intent-filter>
            </service>
        </application>

    </manifest>
    ~~~

4. __알림 채널과 관련된 메타 데이터 추가하기__

    [Android Developer 도큐먼트 - 알림 채널](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#ManageChannels) 문서를 보면 알림 채널이 무엇인지 알 수 있을 것이다.

    Android 8.0(API 레벨 26)부터는 모든 알림을 채널에 할당해야 하며, 그러지 않으면 알림이 표시되지 않는다.

    알림 채널을 따로 만들지 않으면 FCM 기본 채널로 설정된다. 알림 채널을 따로 만드려면 [Android Developer 도큐먼트 - 알림 채널 만들기](https://developer.android.com/training/notify-user/channels) 를 참고하여 만들 수 있다.

    FCM 서비스에서 보내는 알림도 알림 채널에 할당하기 위해 manifest 파일에 아래와 같은 메다 데이터를 추가해야 한다.

    [알림 채널 메타 데이터 샘플 github 코드](https://github.com/firebase/quickstart-android/blob/afa250e1c45ec45f633cfd94878ed3a655d1525e/messaging/app/src/main/AndroidManifest.xml#L25-L27) 를 참고하여 추가해보자.

    ~~~xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.fcm">

        <application
            ...

            <!-- Set custom default icon (메타 데이터 추가) -->
            <meta-data
                android:name="com.google.firebase.messaging.default_notification_icon"
                android:resource="@drawable/ic_launcher_foreground" />

            <!-- Set color used with incoming notification messages (메타 데이터 추가) -->
            <meta-data
                android:name="com.google.firebase.messaging.default_notification_color"
                android:resource="@android:color/holo_blue_dark" />

            <!-- Set color used with incoming notification messages (알림 채널 메타 데이터 추가) -->
            <meta-data
                android:name="com.google.firebase.messaging.default_notification_channel_id"
                android:value="@string/default_notification_channel_id" />

            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>

            <!-- FCM service -->
            <service
                android:name=".MyFirebaseMessagingService"
                android:exported="false">
                <intent-filter>
                    <action android:name="com.google.firebase.MESSAGING_EVENT" />
                </intent-filter>
            </service>
        </application>

    </manifest>
    ~~~

    추가로 res/values/strings.xml 파일에 아래와 같이 FCM 기본 채널 id를 추가해줘야 한다.

    ~~~xml
    <resources>
        <string name="default_notification_channel_id" translatable="false">fcm_default_channel</string>
    </resources>
    ~~~

    만약 알림 채널을 따로 만들었다면 해당 알림 채널 객체의 id로 위 부분을 바꿔주면 된다.

5. __등록 토큰에 Access(접근)하기__

    __3️⃣ FCM 작동 흐름 정리__ 부분에서 알아본 내용 중 Firebase 프로젝트에 Android 클라이언트 앱을 등록하면 해당 클라이언트 앱만의 고유 등록 토큰을 얻게 됨을 알아봤었다.

    FCM SDK가 앱을 처음 실행할 때 해당 클라이언트 앱의 고유 등록 토큰을 생성하기 때문에 얻을 수 있는 것이다.

    > __FCM SDK__ 는 앞서 FCM 아키텍처에 포함된 컴포넌트 4가지 중 가장 마지막 컴포넌트였다.

    등록 토큰은 한 번 생성되면 고정되는 것이 아니라 

    * 새 기기에서 앱을 복원한 경우

    * 사용자가 앱을 삭제하거나 재설치한 경우

    * 사용자가 앱 데이터를 소거한 경우

    위와 같은 3가지 경우에 등록 토큰이 계속 바뀌면서 업데이트 될 수 있다.

    따라서 등록 토큰이 업데이트 되었을지도 모르기 때문에 가장 최근의 등록 토큰을 가져와 사용해야 한다.

    이제부터 등록 토큰을 관리하는 방법에 대해서 알아보자.

    1. __가장 마지막으로 업데이트된 현재 등록 토큰 가져오기__

        [FirebaseMessaging.getInstance().getToken()](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging#getToken()) 을 호출함으로써 가장 최신의 등록 토큰을 가져오거나 확인할 수 있다.

        ~~~kotlin
        FirebaseMessaging.getInstance().token.addOnCompleteListener(OnCompleteListener { task ->
            if (!task.isSuccessful) {
                Log.w(TAG, "Fetching FCM registration token failed", task.exception)
                return@OnCompleteListener
            }

            // Get new FCM registration token
            val token = task.result

            // Log and toast
            val msg = getString(R.string.msg_token_fmt, token)
            Log.d(TAG, msg)
            Toast.makeText(baseContext, msg, Toast.LENGTH_SHORT).show()
        })
        ~~~

    2. __토큰 모니터링하기__

        토큰이 변경되어 새로 업데이트될 때마다 FirebaseCloudMessaging() 클래스의 public 메소드인 onNewToken() 콜백 메소드가 호출된다.

        ~~~kotlin
        // MyFirebaseCloudMessagingService 클래스 파일에서 재정의하기
        override fun onNewToken(token: String) {
            Log.d(TAG, "Refreshed token: $token")

            // If you want to send messages to this application instance or
            // manage this apps subscriptions on the server side, send the
            // FCM registration token to your app server.

            // 서버에 토큰 변경을 알리는 함수 호출 예시 코드
            sendRegistrationToServer(token)
        }
        ~~~

    이렇게 Android 클라이언트 앱 고유 등록 토큰에 대해 알아보고, 관리하는 방법도 알아보았다.

6. __Google Play 서비스 확인하기__

    [Google Play 서비스](https://support.google.com/googleplay/answer/9037938?hl=ko) 에 대해서 알아보고 만약 자신의 클라이언트 앱에서 Google Play 서비스를 사용하고 있다면 아래 과정을 진행해야 한다.

    Google Play 서비스는 간단하게 Google에서 지원하는 서비스들을 앱에 연결해주는 녀석이다. 예를 들어 Google 로그인, Google Analitics 등의 Google 서비스를 사용하는 앱이라면 이 Google Play 서비스가 앱과 서비스를 연결해주는 것이다. 

    Google Play 스토어와는 별개의 것이다.

    <img width="680" alt="09" src="https://user-images.githubusercontent.com/31889335/100860955-84c2c480-34d4-11eb-9998-58c6b88ceea1.png">

    위와 같은 녀석이 Google Play 서비스인데 Android 폰을 사용하면 기본적으로 설치되어 있을 것이다.

    하지만 사용자가 폰에 설치되어 있던 Google Play 서비스 앱을 직접 삭제했을 수 있고, 사용자의 기기와 호환이 안되는 버전의 Google Play 서비스 앱을 가지고 있을 수도 있다.

    따라서 Android 클라이언트 앱은 항상 Google Play 서비스 앱의 APK가 폰에 존재하는지, 기기와 호환되는 버전의 APK 인지를 확인한 후, Google Play 서비스 기능에 엑세스해야 한다.

    FCM 라이브러리도 위에서 Google Analytics 라이브러리 종속성을 추가함으로써 Google 서비스를 사용하고 있는 것이다. 따라서 Android Play Service가 현재 실행가능한 상태인지 확인해야 한다.

    가장 먼저 실행되는 Launch Activity의 onCreate() 메소드나 onResume() 메소드에서 확인 작업을 하는 것이 좋다.

    __GoogleApiAvailability.getInstance().isGooglePlayServicesAvailable()__ 을 호출하여 Google Play Service가 실행 가능한지 확인하는 작업이 가능하다.

    > onResume() 에서 확인 작업을 실행하면 사용자가 '뒤로' 버튼 같은 다른 수단을 통해 실행 중인 앱으로 다시 돌아왔을 때 확인 작업을 계속 수행할 수 있다.
    > 
    > 예를 들어, Google Play 서비스 앱이 없어 설치 한 후 다시 Android 클라이언트 앱에 돌아왔을 때 확인 작업을 이어서 할 수 있는 것이다.

    기기에 호환되는 Google Play 서비스 앱이 없으면 __[GoogleApiAvailability](https://developers.google.com/android/reference/com/google/android/gms/common/GoogleApiAvailability).getInstance().isGooglePlayServicesAvailable()__ 을 호출하여 사용자가 구글 플레이 스토어에서 다운 받게끔 할 수도 있다.

7. __자동 초기화 방지하기__

    등록 토큰이 생성되거나 변경되면 FCM 라이브러리는 식별자와 구성 데이터를 Firebase 프로젝트에 자동으로 업로드(초기화)한다.

    만약 토큰이 자동으로 생성되는 것과 Firebase 프로젝트에 업로드되는 과정을 강제로 막고 싶다면 manifest 파일에 아래와 같은 자동 초기화 기능과 애널리틱스 수집에 관한 메타 데이터를 모두 추가해야 한다.

    ~~~xml
    <meta-data
        android:name="firebase_messaging_auto_init_enabled"
        android:value="false" />

    <meta-data
        android:name="firebase_analytics_collection_enabled"
        android:value="false" />
    ~~~

    FCM 자동 초기화를 중지했다가 다시 시작하려면 

    ~~~kotlin
    // mainActivity.kt
    Firebase.messaging.isAutoInitEnabled = true
    ~~~

    위와 같은 코드를 추가해야 한다.

    애널리틱스 수집을 중지했다가 다시 시작하려면

    ~~~kotlin
    setAnalyticsCollectionEnabled(true);
    ~~~

    위와 같은 코드를 추가해야 한다.

    이 코드들을 추가하면 앱을 시작해도 이 설정이 유지된다.

여기까지 하면 대망의 FCM 을 사용해 Android 클라이언트 앱에 메시지 전송할 __준비 단계ㅋㅋ__ 가 끝난 것이다!

이제 알림 작성기를 통해 FCM 설정이 완료된 Android 클라이언트 앱에 메시지를 보내보자.

<img width="238" alt="10" src="https://user-images.githubusercontent.com/31889335/100863011-42e74d80-34d7-11eb-8058-5f9fe30de7d0.png">

또는 [FCM 도큐먼트 - Android 카테고리](https://firebase.google.com/docs/cloud-messaging) 에서 위와 같은 여러 전송 관련 문서를 읽고 메시지를 보내볼 수도 있을 것이다!

## 5️⃣ 알림 작성기를 통해 메시지 요청하여 백그라운드 상태에 있는 앱에서 알림 받기

✍🏻 [FCM 도큐먼트 - 백그라운드 앱에 테스트 메시지 보내기](https://firebase.google.com/docs/cloud-messaging/android/first-message) 를 참고하여 작성합니다.

<img width="885" alt="11" src="https://user-images.githubusercontent.com/31889335/100883674-f2321d80-34f3-11eb-99fc-afcc0dec0d9f.png">

위 순서대로 진행하면 테스트 메시지가 보내질 것이다.

단, Android 클라이언트 앱이 __백그라운드__ 에 있을 때만 메시지가 전송된다.

> __Android 클라이너트 앱이 백그라운드에 있다?__
> 
> 앱이 실행 조차 안 되어 있는 상황이거나 다른 앱이 가장 위에서 실행되고 있어 홈 버튼을 눌러야 보이는 상황을 말한다.

이 때, 주의할 점이 있다.

안드로이드 버전 파이 이상부터는 배터리 관리에 관한 설정으로 앱이 백그라운드 상태에 있으면 알림을 받지 않도록 사용자가 직접 설정할 수 있게 되었다.

[FCM 도큐먼트 - 백그라운드 제한 확인하기](https://firebase.google.com/docs/cloud-messaging/android/receive?hl=ko#restricted) 를 참고하여 자신의 클라이언트 앱이 백그라운드 제한을 받았는지 받지 않았는지 확인하고 제한을 해제해야 FCM에서 보낸 알림을 백그라운드 상태의 앱에서 받을 수 있다.

<img width="280" alt="12" src="https://user-images.githubusercontent.com/31889335/100885323-db8cc600-34f5-11eb-9301-8f4ade9c7d29.png">

백그라운드에서 알림 받기 성공!

## 6️⃣ 알림 작성기를 통해 메시지 요청하여 포그라운드 상태에 있는 앱에서 알림 받기

✍🏻 [FCM 도큐먼트 - 포그라운드 앱에 테스트 메시지 보내기](https://firebase.google.com/docs/cloud-messaging/android/receive?hl=ko) 를 참고하여 작성합니다.

FCM은 Android 클라이언트 앱이 백그라운드(background) 상태이냐 포그라운드(foreground) 상태이냐에 따라 동작 방식이 다르다.

앱이 포그라운드 상태에 있다는 것은 앞서 알아본 백그라운드 상태와 반대로, 사용자가 현재 해당 앱을 실행시켜 눈으로 보면서 사용하고 있는 상태를 말한다.

백그라운드 상태에 있을 때는 FCM 알림이 작업 표시줄로 왔지만 포그라운드 상태에 있을 때는 FCM 알림이 다른 곳으로 온다.

포그라운드 상태에 있는 앱에서는 FirebaseMessagingService 클래스를 상속받도록 작성했던 MyFirebaseMessagingService 클래스 파일의 __onMessageReceived()__ 콜백 메소드가 호출됨으로써 알림이 온다.

따라서 포그라운드 상태에서 받은 알림을 처리하려면 FirebaseMessagingService 클래스의 public 메소드인 __onMessageReceived()__ 라는 콜백 메소드를 재정의해야 한다.

onMessageReceived() 콜백 메소드에서는 메시지가 수신되면서 매개 변수로 전달된 [RemoteMessage](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/RemoteMessage) 객체를 사용하여 여러 작업을 할 수 있다.

[onMessageReceived() 재정의 샘플 github 코드](https://github.com/firebase/quickstart-android/blob/afa250e1c45ec45f633cfd94878ed3a655d1525e/messaging/app/src/main/java/com/google/firebase/quickstart/fcm/kotlin/MyFirebaseMessagingService.kt#L26-L61) 를 참고하여 작성해보자.

~~~kotlin
class MyFirebaseMessagingService : FirebaseMessagingService() {

    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        super.onMessageReceived(remoteMessage)
        // TODO(developer): Handle FCM messages here.
        // Not getting messages here? See why this may be: https://goo.gl/39bRNJ
        // 앱이 foreground 상태에 있을 때 FCM 알림을 받았다면 onMessageReceived() 콜백 메소드가 호출됨으로써 FCM 알림이 대신된다.
        Log.d("onMessageReceived 콜백 호출됨", "From: ${remoteMessage.from}")

        // 메시지 유형이 데이터 메시지일 경우
        // Check if message contains a data payload.
        var fcmBody: String = ""
        if (remoteMessage.data.isNotEmpty()) {
            Log.d(TAG, "Message data payload: ${remoteMessage.data}")
            fcmBody = remoteMessage.data.get("Body").toString()
        }

        // 메시지 유형이 알림 메시지일 경우
        // Check if message contains a notification payload.
        // Set FCM title, body to android notification
        var notificationInfo: Map<String, String> = mapOf()
        remoteMessage.notification?.let {
            notificationInfo = mapOf(
                "title" to it.title.toString(),
                "body" to it.body.toString()
            )
            sendNotification(notificationInfo)
        }
    }

    override fun onDeletedMessages() {
        super.onDeletedMessages()
        // 이 메소드가 호출되었을 때 실행해야 할 작업이 존재하면 여기에 작성하기
    }

    override fun onNewToken(p0: String) {
        super.onNewToken(p0)

        /** 변경된 토큰 가져오기 및 확인하기 */
        FirebaseMessaging.getInstance().token.addOnCompleteListener(OnCompleteListener { task ->
            if (!task.isSuccessful) {
                Log.w(TAG, "Fetching FCM registration token failed", task.exception)
                return@OnCompleteListener
            }

            // Get new FCM registration token
            val token = task.result
            Log.d("newFCMToken", token.toString())
        })
    }

    /**
     * 푸시 메시지의 세부 설정을 하고, 안드로이드 앱에 푸시 메시지를 보내는 메소드
     * 
     * onMessagedReceived() 콜백 메소드에서 FCM이 보낸 메시지의 title, body 등을 알아와 sendNotification()의 매개변수로 넘기면 됨
     * 
     * @param messageBody FCM message body received.
     */
    private fun sendNotification(messageBody: Map<String, String>) {
        val intent = Intent(this, MainActivity::class.java)
        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
        val pendingIntent = PendingIntent.getActivity(this, 0 /* Request code */, intent,
            PendingIntent.FLAG_ONE_SHOT)

        val channelId = getString(R.string.default_notification_channel_id)
        val defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)

        // icon, color는 메타 데이터에서 설정한 것으로 설정해주면 된다.
        val notificationBuilder = NotificationCompat.Builder(this, channelId)
            .setSmallIcon(R.drawable.ic_launcher_foreground)
            .setContentTitle(messageBody["title"]))
            .setContentText(messageBody["body"])
            .setAutoCancel(true)
            .setSound(defaultSoundUri)
            .setContentIntent(pendingIntent)

        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager

        // Since android Oreo notification channel is needed.
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(channelId,
                "Channel human readable title",
                NotificationManager.IMPORTANCE_DEFAULT)
            notificationManager.createNotificationChannel(channel)
        }

        notificationManager.notify(0 /* ID of notification */, notificationBuilder.build())
    }
}
~~~

위와 같이 onMessageReceived() 콜백 메소드를 재정의했으면 __onDeletedMessages()__ 라는 콜백 메소드도 재정의해주면 좋다.

이 메소드는 FCM이 보낸 메시지가 여러 이유로 인해 전송되지 못할 때 호출되는 콜백 메소드이다.

예를 들어, 특정 기기가 대기 중인 메시지가 100개 이상이거나 기기가 한 달 이상 FCM에 연결되지 않았을 경우 FCM 메시지 전송이 실패할 수 있다.

만약 이 콜백 메소드가 호출된다면 해당 앱 인스턴스를 앱 서버와 전체 동기화 해주는 작업이 필요하다.

<img width="283" alt="13" src="https://user-images.githubusercontent.com/31889335/100893307-928d3f80-34fe-11eb-9d52-102b532137de.png">

onMessageReceived 의 remoteMessage 객체가 가지고 있는 data, notification 등이 무엇인지 더 알고 싶다면 FCM 서버 측 설정에 대해 조금 알 필요가 있다.

[FCM 도큐먼트 - 서버 측 FCM 설정 요청 형식](https://firebase.google.com/docs/cloud-messaging/server#xmpp-request) 부분을 보면 FCM 알림 메시지를 클라이언트 앱에 보내기 위해 서버 측에서 어떤 요청 형식을 작성하는지 알 수 있을 것이다.

# 끝!

이렇게 FCM을 사용해서 메시지 전송하기 구현을 완료했습니다!

이게 FCM 구현의 전부가 아니지만 대략적인 flow를 알게되었다면 좋겠습니다. 

포스팅을 하면서 진행한 샘플 프로젝트 github 코드는 [여기](https://github.com/choheeis/Android_YoungChaYoungCha/tree/master/FCM)에서 볼 수 있습니다 😃