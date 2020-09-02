---
layout: post
title:  "[안드로이드] 🗺 navigation이라는 것이 있다!"
date:   2020-08-24 18:34:10 +0700
categories: [안드로이드]
---

# [🗺 navigation이라는 것이 있다!]

sunFlower 앱을 클론 코딩하면서 jetpack에 속한 정말 다양한 라이브러리들을 사용해보기 시작했다.

그 중 가장 내용도 많고, 기본이 될 것만 같은 라이브러리인 navigation에 대해서 공부해보자.

이번에도 어김없이 [Android developer - navigation](https://developer.android.com/guide/navigation) 문서를 읽어볼 것이고,

<img width="283" alt="01" src="https://user-images.githubusercontent.com/31889335/91011749-d62efe00-e61f-11ea-8f40-e98dbbebea75.png">

위와 같이 navigation 라는 컴포넌트의 구성 하위에는 정말 많은 문서들이 있다.

가장 먼저 개요(Overview)부터 읽어볼 것이다.

<br>

## 1️⃣ 2018년 5월 경 Android Platform에 Navigation 이라는 것이 등장하다!

[Navigation의 릴리즈 문서](https://developer.android.com/jetpack/androidx/releases/navigation?hl=ko#1.0.0-alpha01) 를 보면 최초 등장이 2018년 5월 8일이라고 되어있다.

약 2년 전에 등장한 Navigation은 이제 Android app 개발에 있어 중요한 존재가 되었다.

Navigation이 대체 뭘까?

## 2️⃣ Navigation이 뭐 하는 애인가?

Navigation 에 대해서 간단하게 알아보기 위해 [Navigation 유튜브 공식 영상](https://youtu.be/Y0Cs2MQxyIs) 를 먼저 봐보자!

Android Platform에서 사용하는 'Navigation' 이라는 단어는 __사용자가 앱 내의 여러 콘텐츠를 여기 저기 왔다 갔다하며 탐색하는 동안 일어나는 상호작용__ 이라는 뜻으로 사용된다.

Android Jetpack의 Navigation은 이러한 Navigation의 의미에 맞게 사용자가 앱 내를 자유롭게 탐색할 수 있는 앱을 구현할 때 도움이 되는 라이브러리이다.

## 3️⃣ 사용자가 앱 내의 여러 콘텐츠를 탐색하는게 그렇게 중요해?

[안드로이드 아키텍처에 관한 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-05/AndroidArchitecture) 을 보면 __모바일 앱 사용자 환경__ 이라는 개념에 대해 공부한 내용이 있다. 이 내용을 다시 한 번 보고 오자!

보고 왔다면 모바일 앱 사용자 환경이 끊어지지 않도록 앱을 개발하는 것이 중요하다는 것을 알게 되었을 것이다.

이러한 모바일 앱 사용자 환경은 대부분 사용자가 앱 내 다양한 화면 및 콘텐츠를 탐색하는 흐름에 의해 만들어지기 때문에 앱 개발에 있어서 __사용자의 앱 내 탐색__ 을 고려하는 것은 매우 중요하다!

그렇기 때문에 Navigation 이라는 탐색을 위한 라이브러리도 출시된 것이 아닐까?

## 4️⃣ Navigation의 구성 요소

Android Jetpack의 Navigation을 사용하여 탐색이 용이한 앱을 구현하려면 Navigation에 속하는 3가지 구성 요소를 알고 있어야 한다.

아래에서 설명할 3가지 개념들을 보면 이해가 될 듯 말 듯 할 것이다... 그래도 일단 읽어보자!

1. __Navigation Graph(탐색 그래프)__

    Android Jetpack의 Navigation이 가지고 있는 첫 번째 구성 요소는 Navigation Graph 이다.

    Navigation Graph는 탐색과 관련된 모든 정보를 모아 놓은 것이다.
    
    조금 더 자세히 설명하면 탐색과 관련된 모든 정보를 하나의 Navigation Graph라는 xml 파일에 모아 놓는다.
    
    이 파일 안에서 destination 이라고 부르는 사용자가 탐색 가능한 앱 내의 모든 개별 컨텐츠를 표시하고, 사용자가 앱 내에서 탐색할 수 있는 모든 경로를 나타낸다.

2. __NavHost__

    Android Jetpack의 Navigation이 가지고 있는 두 번째 구성 요소는 NavHost 이다.

    NavHost는 Navigation Graph에서 destination이라고 불리는 개별 컨텐츠를 표시하는 빈 컨테이너이다.

    이 개별 컨텐츠(=destination)에는 NavHostFragment라는 것이 포함되어 개별 컨텐츠의 프래그먼트를 표시한다.

3. __NavController__

    Android Jetpack의 Navigation이 가지고 있는 세 번째 구성 요소는 NavController 이다.

    NavController는 앱 탐색을 직접 관리하는 객체이다.

    즉, 앱 내에서 사용자가 개별 컨텐츠(=destination)들을 탐색하며 이동할 때 NavHost에서 destination(=개별 컨텐츠)의 전환을 NavController를 통해 조율한다.

이 세 가지 구성 요소의 관계는 다음과 같다.

__사용자가 앱을 탐색하는 동안 NavGraph에서 특정 경로를 따라 이동할지, 특정 destination으로 직접 이동할지 NavController에게 알려준다.__

__그러면 NavController가 NavHost에 적절한 대상을 표시하게 된다.__

## 5️⃣ 사용자가 앱 내부를 탐색할 때 어떤 순서로 탐색할까?

> 여기부터는 [Android developer - navigation principles](https://developer.android.com/guide/navigation/navigation-principles) 문서를 읽고 작성한 내용이다.

로그인 화면 같은 조건부 화면을 제외하고 사용자가 앱 내부를 탐색할 때 어떤 순서로 탐색하는지 생각해보자.

<img width="850" alt="02" src="https://user-images.githubusercontent.com/31889335/91959795-168b2c00-ed44-11ea-9de8-f59b3d50e651.png">

위 그림처럼 가장 먼저 사용자는 런처(launcher)에서 앱 아이콘을 터치하여 앱을 실행시킬 것이다.

그 다음, 로그인 화면 같은 조건부 화면이 바로 나올 수도 있지만 사용자 탐색 흐름을 고려할 때는 조건부 화면은 고려하지 않을 것이다.

따라서 앱을 실행시킨 후에는 첫 번째 파란색 화살표가 가리키는 화면 같은 메인 화면을 탐색하게 될 것이다. (위 그림은 SunFlower라는 앱이고, 해당 앱에서 메인 화면은 My garden 이라는 화면에서 보이는 List Screen 이다.)

그 다음으로 사용자는 메인 화면에서 세부 화면으로 들어가며 탐색할 것이다.

사용자는 세부 화면에서 다시 메인 화면으로 돌아오기 위해 위 그림의 빨간색 화살표가 가리키는 것 처럼 back 버튼이나 back 화살표 버튼을 클릭하여 탐색한 경로 그대로 뒤돌아 나오게 된다.

## 6️⃣ stack 으로 표현되는 탐색 상태

위와 같은 흐름으로 사용자가 탐색할 때 Android는 사용자의 화면 탐색 히스토리를 __Stack__ 이라는 자료구조 형태로 저장한다.

간단히 살펴보자면 다음 그림과 같다.

<img width="769" alt="03" src="https://user-images.githubusercontent.com/31889335/91965755-e778b880-ed4b-11ea-825f-36a1962b85b6.png">

스택의 가장 상단에 있는 화면이 현재 보여지고 있는 화면이다.

따라서 사용자가 back 버튼을 누르면 스택 상단에 있던 화면이 pop 되어 사라지고, 한 단계 바로 아래에 있던 화면이 스택의 최상단이 되면서 사용자에게 이전 화면이 보이게 되는 것이다.

이에 대한 더 자세한 내용은 [Android developer - activity stack](https://developer.android.com/guide/components/activities/tasks-and-back-stack) 부분에서 볼 수 있다!

> 탐색 시작하기부터 이어서 읽고 포스팅 이어하기!