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

위와 같이 navigation 컴포넌트를 구성하는 것에는 정말 많은 것들이 있기 때문에 가장 먼저 개요(Overview)부터 읽어볼 것이다.

<br>

## 1️⃣ 2018년 5월 경 Android Platform에 Navigation 이라는 것이 등장하다!

[Navigation의 릴리즈 문서](https://developer.android.com/jetpack/androidx/releases/navigation?hl=ko#1.0.0-alpha01) 를 보면 최초 등장이 2018년 5월 8일이라고 되어있다.

약 2년 전에 등장한 Navigation은 이제 Android app 개발에 있어 중요한 존재가 되었다.

## 2️⃣ Navigation이 뭐 하는 애인가?

Navigation 에 대해서 간단하게 알아보기 위해 [Navigation 유튜브 공식 영상](https://youtu.be/Y0Cs2MQxyIs) 를 먼저 봐보자!

'Navigation' 이라는 단어는 사용자가 앱 내의 여러 콘텐츠를 여기 저기 왔다 갔다 탐색할 수 있도록 해주는 상호작용을 의미하는 뜻으로 사용된다.

Android Jetpack의 Navigation은 이러한 Navigation의 의미에 맞게 사용자가 앱 내를 자유롭게 탐색할 수 있게 구현하는 것을 도와주는 라이브러리이다.

<br>

## 3️⃣ Navigation의 구성 요소

Android Jetpack의 Navigation을 사용하여 탐색이 용이한 앱을 구현하려면 Navigation에 속하는 3가지 구성 요소를 알고 있어야 한다.

아래 설명할 3가지 개념들을 보면 이해가 될듯 말듯 할 것이다. 그래도 일단 읽어보자!

<br>

1. __Navigation Graph(탐색 그래프)__

    Android Jetpack의 Navigation이 가지고 있는 첫 번째 구성 요소는 Navigation Graph 이다.

    Navigation Graph는 탐색과 관련된 모든 정보를 모아 놓은 것이다.
    
    조금 더 자세히 설명하면 탐색과 관련된 모든 정보를 하나의 Navigation Graph라는 xml 파일에 모아 놓음으로써 destination 이라고 부르는 사용자가 탐색 가능한 앱 내의 모든 개별 컨텐츠를 표시하고, 사용자가 앱 내에서 탐색할 수 있는 모든 경로를 나타내는 역할을 담당한다.

    <br>

2. __NavHost__

    Android Jetpack의 Navigation이 가지고 있는 두 번째 구성 요소는 NavHost 이다.

    NavHost는 Navigation Graph에서 destination이라고 불리는 개별 컨텐츠를 표시하는 빈 컨테이너이다.

    개별 컨텐츠(=destination)은 NavHostFragment라는 것이 포함되어 개별 컨텐츠의 프래그먼트를 표시한다.

    <br>

3. __NavController__

    Android Jetpack의 Navigation이 가지고 있는 세 번째 구성 요소는 NavController 이다.

    NavController는 NavHost에서 앱 탐색을 관리하는 객체이다.

    즉, 앱 내에서 사용자가 개별 컨텐츠(=destination)들을 탐색하며 이동할 때 NavHost에서 destination(=개별 컨텐츠)의 전환을 조율한다.

    <br>

이 세 가지 구성 요소의 관계는 다음과 같다.

__사용자가 앱을 탐색하는 동안 NavGraph에서 특정 경로를 따라 이동할지, 특정 destination으로 직접 이동할지 NavController에게 알려준다.__

__그러면 NavController가 NavHost에 적절한 대상을 표시하게 된다.__

> 탐색 원리부터 다시 읽고 시작하기

