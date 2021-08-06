---
layout: post
title:  "[안드로이드] 안드로이드 앱 빌드 실행 과정(feat. Build란?)"
date:   2020-07-27 18:34:10 +0700
categories: [안드로이드]
---

> __참고 자료__  
> [Android Developer - 빌드 구성](https://developer.android.com/studio/build)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. Android의 Build란?](#1)  
> [2. Android의 Build를 관리하는 Gradle](#2)
> [3. What is Build Configuration(빌드 구성)?](#3)  
> [4. 안드로이드 앱이 빌드 되는 과정은 어떠할까?](#4)
> [5. 커스텀 Build Configuration 을 위해 알아야하는 개념들](#5)

## 0️⃣ 프롤로그<a id="0"></a>

안드로이드 스터디를 하면서 [SunFlower](https://github.com/android/sunflower) 라는 앱을 클론코딩하던 중 jetpack 라이브러리인 CoordinatorLayout을 사용하기 위해 app 수준의 build.gradle 파일에 종속성 추가를 하려던 순간이였다...!  
기존의 나는 라이브러리 버전을 직접 명시하여 종속성을 추가했었는데 SunFlower 앱은 아래와 같이 라이브러리의 버전을 직접 명시하지 않고, __$rootProject.~__ 라는 변수로 명시하는 것을 보게 되었다.  
<img width="721" alt="01" src="https://user-images.githubusercontent.com/31889335/88516249-f15e1c00-d027-11ea-882f-0dc9ef761c23.png">

이 변수들이 무엇이고 어디에 선언 되어 있는지 찾아보니 Project 수준의 build.gradle 파일에 다음과 같이 정의되어 있었다.  
<img width="365" alt="02" src="https://user-images.githubusercontent.com/31889335/88516257-f58a3980-d027-11ea-9d89-e645b3a54ee8.png">

이 모습을 보고 이런식으로도 라이브러리 버전을 관리할 수 있다는 점을 깨달았고, 동시에 ext 같은 블록 키워드가 무엇이며 build.gradle 파일이 앱 빌드 시 어떻게 사용되는지 등이 궁금해졌다.  
따라서 [Android Developer - 빌드 구성](https://developer.android.com/studio/build) 문서를 읽어보기로 하였다! 

<br>

## 1️⃣ Android의 Build란?<a id="1"></a>

* Android에서 Build란, 앱의 소스 코드와 리소스들을 __컴파일__ 한 후, __APK(Android Application Package) 또는 App Bundle__ 로 만드는 과정이다. 빌드 결과로 만들어진 APK로 QA 테스트를 하거나, 또는 APK나 App Bundle에 서명을 한 후 Google Play Store에 배포할 수 있다.

## 2️⃣ Android의 Build를 관리하는 Gradle<a id="2"></a>

* Android 앱 빌드는 build toolkit(빌드 도구)인 [Gradle](https://gradle.org/)이라는 것을 사용하여 수행된다. Gradle은 Android 빌드 프로세스를 관리하고 자동화한다. 더불어 Gradle에서 제공하는 Android 플러그인을 사용하면 앱을 빌드하고 테스트하는데 필요한 특정 프로세스나 특정 구성(Configuration)을 정의할 수 있고, 이 플러그인이 Gradle과 함께 작동하여 더욱 커스텀화된 Android 빌드 시스템을 만들 수 있다.

* Gradle의 중요한 특징 중 하나는 Gradle은 안드로이드 앱만을 빌드해주는 시스템이 아니라는 것이다. Gradle은 자바/C++/파이썬 등 다양한 언어로 작성하는 어떤 플랫폼에서든 해당 빌드 시스템을 자동화해주고 관리해주는 시스템이다. 

* 더 중요한 점은 Android 빌드 시스템은 Android Studio와 독립적인 관계라는 것이다. '안드로이드 앱을 빌드하기 위해서는 Android Studio가 필요하다!' 는 잘못된 생각이다. Android Studio는 코드의 편집을 담당할 뿐, 안드로이드 앱 빌드는 Gradle을 통해 수행된다. 따라서 Gradle과 Android Studio는 독립적인 관계이다. 그렇기 때문에 안드로이드 앱을 Android Studio가 아닌, 컴퓨터 커맨드 라인에서 gradle 명령어를 통해 빌드할 수도 있다. 더불어 Android Studio와 Gradle이 서로 연관되어 있지 않기 때문에 Android Studio의 버전 업데이트와 Gradle 버전 업데이트는 따로 해야 한다.

## 3️⃣ What is Build Configuration(빌드 구성)?<a id="3"></a>

* Gradle로 수행되는 안드로이드 빌드 시스템은 빌드 프로세스를 커스텀으로 구성할 수 있다. 예를 들어 최종 배포용 APK를 생성하기 위한 빌드와 개발 중 테스트용 APK를 생성하기 위한 빌드 프로세스를 다르게 하고 싶을 수도 있다. 구체적으로는 최종 배포용 APK의 앱 아이콘과 테스트용 APK의 앱 아이콘('Dev용' 이라는 단어가 앱 아이콘에 들어가 있다고 가정) 을 다르게 하여 앱을 빌드하고 싶다면 어떻게 해야할까? Gradle에서 제공하는 Android 플러그인을 사용하면 상황에 따라 다른 빌드 프로세스가 수행되도록 빌드 시스템을 커스텀 구성할 수 있다. 이렇게 빌드 시스템을 커스텀으로 구성하는 과정에서 앱의 핵심 소스 코드들은 수정할 필요가 없다는 점이 특징이다. 

## 4️⃣ 안드로이드 앱이 빌드 되는 과정은 어떠할까?<a id="4"></a>

* 안드로이드 앱 빌드 과정(Build Process)에는 안드로이드 앱 프로젝트를 APK나 App Bundle로 만들기 위한 여러 가지 tool이나 과정들이 포함되어 있다. 기본적인 안드로이드 앱 빌드 과정을 알아보자!

* <img width="469" alt="03" src="https://user-images.githubusercontent.com/31889335/88516160-ce336c80-d027-11ea-90e8-3e6c57d8c3ab.png">

* (위 그림 참고) 안드로이드 앱 빌드 과정은 기본적으로 위와 같은 순서로 진행된다. 가장 먼저 컴파일러가 프로젝트의 소스 코드를 DEX(Dalvik Executable) 파일로 변환하고, 리소스들을 컴파일한다. DEX 파일은 안드로이드 운영체제가 탑재된 디바이스에서 실행할 수 있는 바이트 코드 파일이다. 그 다음, APK 패키저(Packager)가 DEX 파일과 컴파일된 리소스를 합쳐서 APK나 App Bundle로 만든다. 이렇게 만들어진 APK를 디바이스에 설치하거나 앱을 Google Play Store 같은 앱 스토어에 출시하려면 APK 또는 App Bundle(이하 AAB)에 서명을 해야한다. APK 패키저는 debug 키스토어나 release 키스토어를 사용하여 서명하는 작업까지 담당하고 있다. 만약 앱을 테스트 또는 개발용 APK나 ABB로 빌드하는 거라면 debug 키스토어에 저장되어 있는 키를 사용하여 서명되고, 앱을 배포용 APK나 ABB로 빌드하는 거라면 release 키스토어에 저장되어 있는 키를 사용하여 서명된다. Android 프로젝트를 처음 생성할 때 기본적으로 debug 키스토어가 구성된다. 하지만 release 키스토어는 필요할 때 개발자가 직접 생성해야 한다. 

* 이러한 안드로이드 앱 빌드 과정의 최종 목적을 한 마디로 표현하자면, __안드로이드 앱 프로젝트를 컴파일하여 디바이스에 설치할 수 있는 debug APK 또는 release APK로 만드는 과정__ 이라고 할 수 있다.(이 포스팅에서 App Bundle에 대해 설명하진 않지만, 간략하게만 설명하자면 App Bundle은 디바이스에 설치할 수 있는 것이 아니다. App Bundle은 Google Play Store에 배포할 때 사용하고 APK의 사이즈를 줄이기 위한 목적으로 등장한 새로운 배포물 형식이다.)

## 5️⃣ 커스텀 Build Configuration 을 위해 알아야하는 개념들<a id="5"></a>

* 위 설명들에서 Gradle과 Gradle에서 제공하는 Android 플러그인을 사용하면 커스텀 빌드 구성을 할 수 있다고 언급했다. 커스텀 빌드 구성을 하기 위해 알아야할 몇 가지 개념들을 알아보자.

* __1. Build Types(빌드 유형)__ 

  * Build Type은 Gradle이 Android 빌드 과정을 수행할 때 사용하도록 하는 특정 세팅들을 정의해놓은 것이다. 예를 들어, debug 빌드 타입을 정의한다고 가정하면 debug 옵션이나 debug 서명 키 같은 세팅들을 해줄 수 있다. 또 release 빌드 타입을 정의한다고 하면 앱 난독화 세팅이나 release 서명 키 같은 세팅들을 해줄 수 있다. 안드로이드 앱 빌드를 수행하기 위해서는 무조건 한 개 이상의 빌드 타입이 정의되어 있어야 하는데 Android Studio에서 첫 프로젝트 생성시 기본적으로 debug와 release 빌드 타입을 생성해놓는다. 

* __2. Product Flavors(제품 버전)__
  
  * Product Flavors는 하나의 앱에 무료 버전이나 유료 버전이 있는 것처럼, 앱이 하나의 제품으로 배포될 때 서로 다른 제품 버전(앱 버전이 아님)이 필요할 경우 정의하는 것이다. Product Flavor을 사용하면 제품 버전에 따라 서로 다른 소스 코드와 서로 다른 리소스로 앱을 구성할 수 있다. 이 때, 각 버전에 상관 없이 공통으로 사용되어야 하는 코드는 서로 공유할 수도 있다. 안드로이드 앱 빌드 과정을 수행하기 위해 Product Flavor은 필수는 아니다.(선택임) 'Flavor'을 한글 번역하면 '맛' 이라는 뜻이 되는 것처럼 제품 맛을 다르게 하고 싶을 경우 사용한다고 이해해도 될 것 같다^.^

* __3. Build Variants(빌드 변형)__

  * Build Variants는 앞에서 설명한 Build Type과 Product Flavor을 섞은, 말그대로 '빌드 변형' 이다. 예를 들어, 빌드 타입에 debug와 release가 정의되어 있고, 제품 버전에 free(무료)와 paid(유료)가 정의되어 있을 때 만들 수 있는 Build Variant는 'debugFree', 'debugPaid', 'releaseFree', 'releasePaid' 이렇게 4가지 이다. 따라서 무료 버전의 앱을 debug용으로 빌드하고 싶을 때는 debugFree 빌드 변형을 사용하여 빌드하면 된다.

* __4. Manifest entries(메니페스트 항목)__

  * 여기서부터 이어서~~~





안드로이드 스튜디오는 새 프로젝트를 생성할 때 아래 그림과 같은 파일들을 자동으로 생성함으로써 프로젝트 구조를 생성하고 각 파일들을 기본값들로 채운다.

<img width="281" alt="09" src="https://user-images.githubusercontent.com/31889335/92091803-371eb900-ee0c-11ea-8801-a26fcd56fb13.png">

<img width="349" alt="04" src="https://user-images.githubusercontent.com/31889335/88511815-76453780-d020-11ea-9c06-82d9930cd398.png">

위 그림을 보면 앱의 Project 수준의 일부분으로 build.gradle 파일이 있는 것을 볼 수 있다.

위 그림에서 회색 부분을 __root project__ 부분이라고 한다.

<br>

## 2️⃣ settings.gradle 파일은 뭘까?

<img width="345" alt="05" src="https://user-images.githubusercontent.com/31889335/88512234-33379400-d021-11ea-9b94-fb5954ab108c.png">

위 그림에서 앱의 Project 수준에 존재하는 __settings.gradle__ 파일은 무엇일까?

이 파일은 앱을 빌드할 때 포함해야 하는 모듈을 gradle에게 알려주는 역할을 한다.

여기서 모듈은 위 그림에서 초록색 부분을 말한다.

따라서 이 파일은 대부분 간단하며

~~~kotlin
include ':app' 
~~~

이라는 코드만 포함하는 경우가 많다.

하지만 만약 다중 모듈 프로젝트라면 최종 빌드에 들어가야 하는 모듈들을 이곳에 모두 명시해줘야 한다.

<br>

## 3️⃣ 최상위 build.gradle 파일은 뭘까?

<img width="349" alt="06" src="https://user-images.githubusercontent.com/31889335/88512707-f029f080-d021-11ea-81b7-50adb7e902b1.png">

위 그림에서 root project 부분에 있는 build.gradle 파일을 최상위 build.gradle 파일이라고 한다.

이 파일은 프로젝트의 모든 모듈에 적용되는 빌드 구성을 적용하는 역할을 한다.

기본적으로 최상위 build.gradle 파일은 __buildscript__ 블록을 사용하여 프로젝트의 모든 모듈에 공통되는 gradle 저장소와 종속 항목들을 정의한다.

~~~kotlin
// buildscript 블록 안에는 모든 모듈에 적용되는 저장소들과 gradle 종속성이 명시되어야 한다.
buildscript {

    // 저장소라는 것은 gradle 종속성을 검색하고 다운로드할 곳들을 의미한다.
    // gradle은 jcenter라는 remote 저장소가 이미 제공한다.
    repositories {
        google()
        jcenter()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:3.6.0'
    }
}

// allprojects 블록은 프로젝트 내의 모든 모듈에 포함되어야 하는 것들을 정의하는 곳이다.
allprojects {
   repositories {
       google()
       jcenter()
   }
}
~~~

<br>

## 4️⃣ 최상위 build.gradle 파일을 구성할 때 팁!

여러 모듈이 포함된 Android 프로젝트인 경우 최상위 build.gradle 파일에서 특정 속성들을 모두 정의하고 이를 각 모듈에서 공유하는 것이 유용하다.

이를 위해 다음과 같이 최상위 build.gradle 파일에 __ext__ 블록을 만들고 그 안에 속성들을 정의하여 여러 모듈에서 공유하면 된다.

~~~kotlin
buildscript {...}

allprojects {...}

ext {

    compileSdkVersion = 28
    supportLibVersion = "28.0.0"
    ...
}
...
~~~

이런 다음 각 모듈에서 위에서 정의한 속성들을 사용하려면 각 모듈의 build.gradle 파일에서 아래와 같이 사용하면 된다.

~~~kotlin
android {
  compileSdkVersion rootProject.ext.compileSdkVersion
  ...
}
...
dependencies {
    // ${rootProject.ext.속성이름} 을 사용하고, 이를 위해 작은따옴표가 아닌 큰따옴표를 사용해야 한다.
    implementation "com.android.support:appcompat-v7:${rootProject.ext.supportLibVersion}"
    ...
}
~~~

<br>

## 4️⃣ module 수준에 있는 build.gradle 파일은 뭘까?

<img width="348" alt="07" src="https://user-images.githubusercontent.com/31889335/88514294-c3c3a380-d024-11ea-9c28-3403bb998957.png">

이번에는 root project 부분이 아닌 Module 부분(초록색 부분)에 존재하는 build.gradle 파일에 대해서 알아보자.

이 파일은 특정 모듈에 적용되는 빌드 설정을 구성할 수 있도록 해준다.

이 파일은 주로 다음과 같이 구성된다.

~~~kotlin

// 맨 처음으로 해당 모듈에 적용되는 안드로이드 플러그인이 명시된다.
apply plugin: 'com.android.application'

// android 블록 안에서 이 모듈에 적용되는 모든 빌드 구성들을 명시한다.
android {

  // compileSdkVersion은 이 프로젝트의 소스코드를 컴파일할 Android API Level을 말한다. gradle은 이 컴파일 버전을 사용하여 앱을 빌드한다.
  compileSdkVersion 28

  // buildToolsVersion은 sdk 빌드 툴 버전을 명시한다.
  buildToolsVersion "29.0.2"

  defaultConfig {

    applicationId 'com.example.myapp'

    minSdkVersion 15

    targetSdkVersion 28

    // 앱 버전 코드를 명시한다.
    versionCode 1

    // 유저에게 친숙한 버전명을 명시한다.
    versionName "1.0"
  }

  buildTypes {

    release {
        minifyEnabled true // Enables code shrinking for the release build type.
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
  }

  flavorDimensions "tier"
  productFlavors {
    free {
      dimension "tier"
      applicationId 'com.example.myapp.free'
    }

    paid {
      dimension "tier"
      applicationId 'com.example.myapp.paid'
    }
  }

  splits {

    density {

      enable false

      exclude "ldpi", "tvdpi", "xxxhdpi", "400dpi", "560dpi"
    }
  }
}

// 라이브러리들의 종속성을 명시한다.
dependencies {
    implementation project(":lib")
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation fileTree(dir: 'libs', include: ['*.jar'])
}
~~~

여기까지 안드로이드 빌드 시스템에 포함되는 주요 빌드 파일들을 살펴보았다! 

이제부터 프로젝트에 라이브러리 종속성을 추가할 때 라이브러리 버전은 최상위 build.gradle 파일에 ext 블록안에 명시하면 좋을 것 같다!! 😃



