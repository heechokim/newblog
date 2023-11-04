---
layout: post
title:  "[안드로이드] 안드로이드 프로젝트 구조(Android Project Structure)"
date:   2021-07-05 18:34:10 +0700
categories: [안드로이드]
---

> __참고 자료__  
> [Android Developers Document - 프로젝트 개요](https://developer.android.com/studio/projects?hl=ko)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. 안드로이드 스튜디오에서 프로젝트 구조 보는 두 가지 방법](#1)  
> [2. 안드로이드 프로젝트를 이해하려면 모듈을 알아야 한다](#2)
<br>

## 0️⃣ 프롤로그<a id="0"></a>

지인 개발자가 개발한 안드로이드 프로젝트를 구경하던 중,, 프로젝트 구조가 신기하게 생긴 것을 보게되었다! 프로젝트 구조에서 app 폴더와 같은 depth에 다른 폴더들이 있는 구조였다. 찾아보니 __라이브러리 모듈__ 이라는 것이었고, 이참에 안드로이드 프로젝트 구조를 공부해서 남겨본다 :)  
<img width="292" alt="스크린샷 2021-07-05 오후 9 10 54" src="https://user-images.githubusercontent.com/31889335/124469604-88edf200-ddd5-11eb-8190-dcec8cf496e0.png">

## 1️⃣ 안드로이드 스튜디오에서 프로젝트 구조 보는 두 가지 방법<a id="1"></a>

* Android Studio에서 프로젝트 구조를 볼 수 있는 방법은 두 가지이다. __Android 뷰로 보기__ 와 __Project 뷰로 보기__ 이다.

* __Android 뷰로 보기__

  * ![image](https://user-images.githubusercontent.com/31889335/124470178-42e55e00-ddd6-11eb-9b74-ec18f0196eac.png)

  * (위 그림 참고) 하늘색 박스 부분을 Android로 선택해주면, Android 뷰로 프로젝트 구조를 확인할 수 있다. 어떤 프로젝트를 Android Studio로 실행하면 기본적으로 해당 프로젝트의 구조를 Android 뷰로 보여준다.

  * <img width="292" alt="스크린샷 2021-07-05 오후 9 10 54" src="https://user-images.githubusercontent.com/31889335/124471474-db301280-ddd7-11eb-8c48-814b7064e1a2.png" />    

  * (위 그림 참고) Android 뷰로 프로젝트 구조를 보게 되면, 해당 프로젝트가 컴퓨터에 연결된 디스크에 실제로 저장되는 구조(계층)와는 다르게 보인다. 하지만 가장 상위 폴더가 모듈별로 보여지기 때문에 프로젝트 구조를 한 눈에 보기 쉬운 장점이 있다.

  * Android 뷰로 프로젝트 구조를 볼 때의 특징은 프로젝트의 모든 빌드 관련 구성 파일을 __Gradle Scripts__ 라는 최상위 폴더로 묶어서 보여준다는 것이다. 하지만 실제 디스크 상에서는 이렇게 묶여있지 않다.

  * <img width="292" alt="" src="https://user-images.githubusercontent.com/31889335/124473428-4844a780-ddda-11eb-829f-2a48464b6266.png" />

  * (위 그림 참고) 또한 앱 런처 아이콘의 여러 밀도별 대체 리소스가 ic_launcher.png 그룹에 묶여서 보여지는 것처럼, Android 뷰에서는 하나의 파일당 여러 대체 리소스 파일이 존재할 경우 하나의 그룹에 넣어서 보여준다.

* __Project 뷰로 보기__

  * <img width="239" alt="03" src="https://user-images.githubusercontent.com/31889335/124474975-23513400-dddc-11eb-9189-7298220c9e64.png">

  * (위 그림 참고) 하늘색 박스 부분을 Project로 선택해주면, Project 뷰로 프로젝트 구조를 확인할 수 있다. Android 뷰에서 편의상 숨겨진 모든 파일을 모두 보여주고, 실제 디스크에 저장되는 프로젝트 구조로 보여준다.

  * Project 뷰에서의 빌드 구성 파일은 하나의 폴더로 묶여있지 않다. App의 빌드 구성 파일은 App 폴더 안에 있으며, 프로젝트 전체의 빌드 구성 파일은 App 폴더와 같은 depth에 위치한다.

## 2️⃣ 안드로이드 프로젝트를 이해하려면 모듈을 알아야 한다<a id="2"></a>

* __What is 모듈(Module)?__

  * 모듈은 소스 파일과 빌드 구성 파일 등으로 구성된 모음집(Collection)이다. 따라서 Project 뷰로 프로젝트 구조를 보았을 때, app 폴더 하위에는 소스 파일과 빌드 구성 파일이 모두 포함되어 있었으므로 app 폴더도 하나의 모듈이 된다.
  
  * 이러한 모듈을 통해 하나의 프로젝트를 기능 단위로, 여러 개로, __분할__ 할 수 있게 된다.

  * 하나의 프로젝트에는 여러 개의 모듈이 포함될 수 있고, 어떤 하나의 모듈이 다른 모듈을 종속 항목으로 사용할 수도 있다. 또한 각 모듈은 개별적으로 빌드하고 테스트, 디버그할 수 있다!

  * 특히, Android Studio에서는 3가지의 서로 다른 Module Type(유형)을 지원한다. -> app 모듈, feature 모듈, library 모듈

* __app 모듈__

  * app 모듈은 앱의 소스 코드, 리소스 파일, 빌드 구성 파일, Android Manifest 파일 등을 포함하고 있는 모듈이다. Android Studio에서 새로운 프로젝트를 만들면 기본적으로 app 모듈이 자동으로 생성된다.

* __feature 모듈__

  * feature 모듈은 Google Play에서 지원하는 [Play Feature Delivery](https://developer.android.com/guide/app-bundle/play-feature-delivery?hl=ko) 를 사용할 수 있는 모듈이다.
   
  * Play Feature Delivery를 간단히 설명해보자면.. 예를 들어, 결제 기능이 있는 앱에 feature 모듈로서 결제 모듈을 만들어 놓았다고 하자. 이 앱을 사용하는 어떤 사용자는 앱을 사용하긴 하지만 유료로 무언가를 결제하지는 않는 성향의 사용자일 수 있다. 또 다른 사용자는 이 앱에서 유료로 무언가를 결제까지 하는 사용자일 수 있다. 이런 경우, Play Feature Delivery를 활용하면 첫 번째 사용자에게는 결제 모듈을 제공하지 않고, 두 번째 사용자에게만 결제 모듈을 제공한다. 즉, 첫 번째 사용자는 사용하지도 않을 결제 모듈을 다운받게 되는 불상사를 당하지 않게 되어 다운로드 받을 총 apk 크기를 줄이는 효과를 얻을 수 있다!

* __library 모듈__

  * library 모듈은 구조적으로 app 모듈과 동일한 모듈이다. 그러나 사용 목적이 다른데, library 모듈은 app 모듈에서 종속 항목으로 사용하기 위한 모듈이기도 하고 프로젝트1과 프로젝트2에서 공통으로 사용해야 하는 코드를 library 모듈로 만들어서 어떤 프로젝트에서든지 재사용할 수 있는 코드를 제공하기 위한 모듈이기도 하다.

  * library 모듈의 중요한 특징은 빌드될 때 APK로 만들어지는 것이 아니라 code archive 파일로 만들어지기 때문에 library 모듈을 안드로이드 기기에 설치할 수는 없다.

  * Android 프로젝트에 적용할 수 있는 library 모듈은 또 두 가지 종류로 나뉘는데 Android Library와 Java Library이다. 이 둘에 대해서는 나중에 조금 더 자세히 알아보기로 하고 일단 넘어가자 ㅎㅎ..

*   ~~~gradle
    dependencies {
        implementation(project(":my-library-module"))
    }
    ~~~

* (위 코드 참고) 위에서 알아본 __모듈__ 이라는 것을 __하위 프로젝트(sub-project)__ 라고도 부른다. 따라서 Gradle 설정에서도 모듈을 프로젝트로 취급한다. 예를 들어, 라이브러리 모듈을 생성하고 app 모듈의 종속 항목으로 추가할 때는 위와 같은 코드를 작성한다.

# 끝 ~

이렇게 공부해보니.. 지인의 프로젝트 구조에는 여러 개의 library 모듈이 포함되어 있는 것이였다! :)  
마지막 수정일 : 2021년 7월 5일
