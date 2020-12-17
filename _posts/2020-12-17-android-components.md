---
layout: post
title:  "[안드로이드] 안드로이드 4대 컴포넌트(Android Componenents)"
date:   2020-12-17 18:34:10 +0700
categories: [안드로이드]
---

## 0️⃣ Android 앱에 대한 간략한 설명(소개)

✍🏻 [Android Developer 도큐먼트 - 앱 기본 요소](https://developer.android.com/guide/components/fundamentals.html#Components) 를 참고하여 작성합니다.

안드로이드 4대 컴포넌트를 알아보기 전에, __안드로이드 앱에 대한 간략한 소개__ 를 먼저 해보려고 한다.

1. __Android 앱을 개발할 때 사용할 수 있는 프로그래밍 언어__

    Kotlin, Java, C++ 언어를 사용해서 개발할 수 있다.

2. __Android로 구동되는 기기에 앱을 설치할 때__

    사용자는 자신의 기기에 Android 앱을 설치하여 사용하게 된다.

    기기에 앱을 설치할 때는 __APK(Android Package)__ 라는 파일이 필요한데 이 파일은 앱의 모든 콘텐츠가 들어있는 파일이며 __Android SDK 도구에 의해 앱의 모든 코드와 리소스 파일이 컴파일__ 된 후, 하나의 APK 파일이 만들어진다.

    따라서 어떠한 앱을 기기에 설치하고 싶다면 APK 파일을 다운받아야 한다.

    APK 파일의 이름은 확장자가 apk인 __~~~.apk__ 로 만들어진다.

3. __Android 앱의 보안 모델__

    각각의 Android 앱은 자체적인 [보안 샌드박스](https://source.android.com/security/app-sandbox?hl=ko) 안에 속해있어서 몇 가지 보안 기능을 통해 보호된다.

    > 안드로이드 보안 샌드박스에 대해서 더 공부가 필요하다!

    먼저 안드로이드 OS는 __멀티 유저 Linux 시스템__ 을 지원하는 운영체제이다. 여기서 유저라는 것은 각 Android 앱을 말한다. 
    
    즉, Android OS로 구동되는 기기에 여러가지 다양한 앱이 설치되면 각각의 앱을 한 명의 유저로 처리하기 때문에 독립된 사용자 ID(UID)가 앱마다 부여된다.

    이 사용자 ID는 Android OS 시스템만 알고 있을 뿐, 앱 자체는 인식하지 못한다.

    Android OS는 이 UID를 통해 앱 안의 모든 파일에 해당 앱에 관한 사용자 ID를 가지고 있는 유저만 접근할 수 있도록 접근 권한을 설정한다.

    __따라서 여러 개의 서로 다른 앱들이 각자가 저장하고 있는 리소스나 데이터에 서로 접근하지 못하도록 보호할 수 있게 된다.__

    또 Android는 __프로세스 가상머신__ 이라는 것을 사용해서 앱을 보호하고 있다.

    먼저 각 Android 앱이 실행되면 메모리에 별도의 프로세스가 생성되고, 이 프로세스는 __자체적인 가상 머신(VM)__ 을 가지고 있다.

    각 앱은 별도의 프로세스로 메모리 다른 공간에 올라갈 뿐더러 각 프로세스에 가상 머신이 존재하기 때문에 __서로 다른 앱의 코드는 분리된 환경에서 실행__ 되게 된다.

    > 안드로이드 프로세스 가상 머신은 ART라는 것을 더 찾아본 후, 별도록 포스팅하자!
    
    즉 Android 앱 보안 모델에 대해 정리해보자면, UID와 프로세스 가상머신을 통해 __Android 시스템은 각 앱이 자신의 구성요소에만 접근할 수 있도록 하고, 그 이상은 허용되지 않도록 작동시킨다__ 라는 보안 원칙을 가지고 있는 것이다.

    이런 Android 시스템에 의해 Android 앱은 시스템에서 접근 권한을 받지 못한 부분에는 접근할 수 없다.

    하지만 Android 시스템이 이러한 접근 권한을 영원히 허용하지 않는 것은 아니고, __다양한 방법으로 접근 권한을 얻을 수 있게 되어 있다.__

    예를 들어, 두 개의 앱이 같은 __Linux 사용자 ID를 공유__ 하도록 설정할 수 있다. 이 경우에는 두 개의 앱이 서로의 파일과 리소스에 접근할 수 있게 된다.

    또한, Linux 사용자 ID를 공유하는 앱들을 별도의 프로세스가 아닌 하나의 프로세스에서 실행시켜 같은 VM을 공유하도록 설정해줄 수도 있다. 

    이 외에도 Android 앱은 사용자의 연락처, 카메라, 앨범 등 기기의 여러 데이터에 접근할 수 있도록 권한 요청을 할 수도 있다. 이런 종류의 권한 요청은 [메니페스트 파일에서 시스템 권한 사용](https://developer.android.com/guide/topics/permissions/overview) 을 명시해줘야 한다.

## 1️⃣ Android 앱을 구성하고 있는 4가지 컴포넌트

이제 본격적으로 Android 앱을 구성하고 있는 4가지 컴포넌트에 대해 알아보자.

__Activity__, __Service__, __Broadcast Receiver__, __Content Provider__ 이 Android 앱을 구성하는 4가지 컴포넌트이다.

각 컴포넌트는 사용자가 앱에 들어올 수 있는 __진입점__ 이 된다.

또한 각 컴포넌트는 뚜렷한 역할을 수행하고 있고, 각자의 수명주기를 가지고 있다. 따라서 자신만의 생성 및 소멸 방식을 정의하고 있다.

구체적으로 각 컴포넌트에 대해 알아보자.

* __Activity__

    Activity는 __사용자와 앱이 상호작용하기 위한 진입점__ 이다. 따라서 Activity는 사용자 인터페이스(UI)를 포함한 화면 하나를 의미한다.

    대부분의 앱은 단 한 개의 Activity가 아닌 여러 Activity로 이루어져 있을 것이다.

    예를 들어 이메일 앱이라면 이메일을 작성하는 Activity, 보낸 이메일 목록을 확인하는 Activity, 받은 이메일 목록을 확인하는 Activity 등이 함께 작동하여 이메일 앱을 구성하고 있을 것이다.

    그렇지만 각각의 __Activity는 사실 독립되어 있다.__

    따라서 Android 시스템에서 허용할 경우, 다른 앱이 이메일 앱의 Acitivity 중 하나에 접근할 수도 있다.

    Activity는 Android 시스템과 Android 앱 사이에서 아래와 같은 상호작용을 돕는 역할을 한다.

    * 사용자 화면에 현재 표시되고 있는 Acitivity를 추적하여 해당 Activity를 실행하고 있는 프로세스를 시스템에서 계속 실행하도록 한다.

    * 사용자가 다시 찾을 만한 Activity(중단된 Activity)를 실행하고 있는 프로세스를 종료시키지 않고 유지한다. 프로세스 유지 우선순위 중 더 높은 우선순위를 부여한다.

    Activity를 구현할 때는 [Activity](https://developer.android.com/reference/android/app/Activity) 라는 클래스를 상속한 SubClass를 작성함으로써 구현할 수 있다.

* __Service__

    Service는 __여러 가지 이유로 인해 앱을 백그라운드 상태에서 계속 실행시키기 위한 다목적 진입점__ 이다.

    Service는 Activity와 다르게 사용자 인터페이스(UI)를 제공하지 않는다.

    Service는 사용자 인터페이스를 제공하지 않으면서 앱이 백그라운드 상태에 있을 때도 계속 실행할 수 있기 때문에 __사용자가 앱에서 나간 후에도 백그라운드에서 음악을 재생하거나 사용자와 상호작용하고 있는 UI를 차단하지 않으면서 네트워크를 통해 데이터를 다운받거나 가져와야 할 때__ 사용할 수 있다.

    Service를 구현할 때는 [Service](https://developer.android.com/reference/android/app/Service) 라는 클래스를 상속한 SubClass를 작성함으로써 구현할 수 있다.

* __Broadcast Receiver__

    Broadcast Receiver는 __사용자가 만드는 플로우 밖에서 Android 시스템이 앱에 알려야 하는 사건이 생길 때, 이벤트를 앱에 전달함으로써 앱에 진입__ 할 수 있는 진입점이다.

    Broadcast라는 용어를 한글로 번역하면 __'방송'__ 이라는 뜻이고 Broadcast Receiver는 __'방송을 수신하는 사람'__ 을 의미한다.
    
    Android 시스템이 앱에 알려야 하는 사건을 '방송' 이라고 생각하면 되고 이 방송을 수신하는 것이 Broadcast라고 생각하면 된다.

    대부분의 'Broadcast(방송, 앱에 알려야하는 사건)'은 Android 시스템에서 발생한다. 

    예를 들어, 배터리가 부족하거나 사진을 캡쳐했을 때 Android 시스템은 Broadcast를 발생시킨다.

    이 '방송'을 수신하는 Broadcast Receiver에 해당 이벤트를 수신했을 때 어떤 작업을 실행해야 하는지 작성해줄 수 있다.

    Broadcast Receiver는 사용자 인터페이스를 제공하지 않지만 __상태표시줄 알림__ 을 제공한다.

    <img width="380" alt="01" src="https://user-images.githubusercontent.com/31889335/102481191-baef6f00-40a4-11eb-9ba0-552cb594a74e.png">

    따라서 상태표시줄 알림을 사용하여 사용자에게 Broadcast 이벤트가 발생했음으로 알릴 수 있다.

    Broadcast Receiver를 구현할 때는 [BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver) 라는 클래스를 상속하는 SubClass를 작성함으로써 구현할 수 있다.

* __Content Provider__

    Content Provider는 __Android 앱 간 데이터를 공유할 수 있도록 해주는 녀석__ 이다.

    예를 들어, 전화번후부 앱이 저장하고 있는 지인들의 연락처를 다른 앱에서 사용해야 한다던지, 갤러리 앱이 저장하고 있는 사진들을 다른 앱에서 사용해야 한다던지 할 때 Content Provider를 사용할 수 있다.

    다른 앱은 Content Provider를 통해 해당 데이터를 가져오거나 Content Provider가 허용할 경우, 해당 데이터를 수정할 수도 있다.

    <img width="575" alt="02" src="https://user-images.githubusercontent.com/31889335/102482729-030f9100-40a7-11eb-9f3d-d441cb1d77f2.png">

    > 그림 출처 : https://www.codeproject.com/Articles/818578/An-Absolute-Beginner-s-Guide-to-Building-and-Acces

    Content Provider를 구현할 때는 [Content Provider](https://developer.android.com/reference/android/content/ContentProvider) 라는 클래스를 상속하는 SubClass를 작성함으로써 구현할 수 있다.

> 이어서 더 작성하기!

