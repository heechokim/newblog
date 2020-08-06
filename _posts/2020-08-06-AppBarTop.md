---
layout: post
title:  "[SunFlower] 🚥 AppBar에 대해서 알아보자"
date:   2020-08-06 18:34:10 +0700
categories: [sunflower]
---

# [🚥 AppBar에 대해서 알아보자]

## 1️⃣ AppBar는 안드로이드 화면에서 어디일까?

안드로이드에서 AppBar는 화면에서 다음과 같이 StatusBar 바로 하단에 위치한다. 

<img width="552" alt="15" src="https://user-images.githubusercontent.com/31889335/89493764-d9d51f00-d7ef-11ea-8724-7a1811eda32d.png">

AppBar를 사용하지 않는 화면도 많지만 사용하는 화면도 많다!

또한 AppBar는 위 그림과 같이 화면의 위쪽에 위치할 수도 있지만

<img width="376" alt="16" src="https://user-images.githubusercontent.com/31889335/89493925-1a349d00-d7f0-11ea-860d-d85b0c2b25f3.png">

이와 같이 화면의 아래쪽에 위치할 수도 있다.

<br>

## 2️⃣ AppBar는 Material Design에 속한다!

AppBar는 Material Design에 속해있으므로 [Material Design 공식 사이트 - AppBar:Top](https://material.io/components/app-bars-top) 문서를 읽어보면서 AppBar의 구체적인 역할과 사용법을 알아보자!

<br>

## 3️⃣ 화면에서 AppBar의 역할

AppBar의 역할은 다양하다. 브랜딩을 위해서 사용될 수도 있고, 화면의 제목을 나타내기 위해 사용될 수도 있으며 navigation이나 다양한 action들을 위해서 사용되기도 한다.

<br>

## 4️⃣ AppBar에는 두 가지 타입이 있다

1. Regular 타입

    <img width="369" alt="17" src="https://user-images.githubusercontent.com/31889335/89494454-2a994780-d7f1-11ea-9b0b-a727a977e266.png">

    위 그림처럼 화면의 상단에 보이는 AppBar를 Regular 타입의 AppBar라고 한다.

    <br>

2. Contextual action bar 타입

    <img width="371" alt="18" src="https://user-images.githubusercontent.com/31889335/89494556-5f0d0380-d7f1-11ea-9a94-0e6f7abc46b0.png">

    위 그림과 같은 AppBar를 Contextual action bar 타입의 AppBar라고 한다. 
    
    > --> [regular 타입의 AppBar가 contextual action bar 타입으로 바뀌는 과정](https://kstatic.googleusercontent.com/files/6a059a3f7a6cc28b726e024bb118edf1673498e294176409b10fcbf460e9c63be22440a0a35f988f05701dd92f6ef9ab641fc421be11599f64c8e5520a2a0b0b)

    Contextual action bar 타입은 AppBar 아래에 보이는 사진들이나 선택 가능한 아이템들을 선택하는 기능을 구현해야 할 때 선택에 따른 action을 제공한다.

    <br>

## 5️⃣ AppBar의 여러가지 모습

[Material Design 공식 사이트 - AppBar:Top](https://material.io/components/app-bars-top) 문서를 보면 위에서 알아본 AppBar로 보여질 수 있는 여러 모습들을 볼 수 있다.

예를 들면, [화면 Scroll 할 때 AppBar 사라지게 하기](https://kstatic.googleusercontent.com/files/60e734664455ce42d79267bdc7f7dd20239ea2086102e302e0d8c3e255b639faddbbbeaf75ceb9ac3fb2f1f14451f937427eabaf6aa028bcd4a3bd80504e68b2) 나 [화면 Scroll 할 때 AppBar 그림자 생기게 하기](https://kstatic.googleusercontent.com/files/009157b0504e76610fbaa8478ca2915099427f000019b27bd920a42f36a9dddf2cde3fb61fc23c45ff956a183ab65e6e84a1af83c629765f57fe25ab2e092da9) 등 AppBar로 만들 수 있는 여러 모습들이 어떤 것들이 있는지 알 수 있을 것이다!

<br>

## 6️⃣ AppBar 사용법

위에서 알아본 AppBar를 사용하기 위해 실제로 어떤 코드를 입력해야 할까?

[AppBar:Top - Android](https://material.io/develop/android/components/app-bars-top) 문서를 보면 AppBar의 xml 태그와 여러 함수들을 볼 수 있을 것이다.

1. __Material Design 종속성을 module.app 수준의 build.gradle 파일에 추가하기__

    Material Design에 속해있는 AppBar를 사용하기 위해서는 Material Design 종속성을 프로젝트에 추가해야 한다.

    종속성을 추가하는 법은 [Material Design에 대한 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-04/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EB%A8%B8%ED%84%B0%EB%A6%AC%EC%96%BC-%EB%94%94%EC%9E%90%EC%9D%B81) 을 읽어보면 알 수 있다!

    <br>

2. __menu의 item들 정의해주기__

    만약 AppBar 내에 navigation icon 기능이나 다른 action item들을 사용하기 원한다면 

    <img width="640" alt="19" src="https://user-images.githubusercontent.com/31889335/89496905-350a1000-d7f6-11ea-96cf-3c5a06a92d2a.png">

    이와 같이 menu 파일안에 각 item들을 정의해주어야 한다.

    이 부분에 대해서는 [AppBar:Top - Android](https://material.io/develop/android/components/app-bars-top) 문서의 Content descriptions 부분을 자세히 읽어보자!

    <br>

3. __AppBar 타입을 정하고 타입별 사용법 적용하기__

    앞서 정리한 내용 중 AppBar의 타입에는 Regular 타입과 Contextual action bar 타입이 존재한다는 내용이 있었다.

    <img width="334" alt="20" src="https://user-images.githubusercontent.com/31889335/89497140-b661a280-d7f6-11ea-88ad-14c32935eb74.png">

    Regular 타입의 AppBar를 구현할 때는 주로 위 그림에서 볼 수 있는 4가지의 API들을 사용하게 된다.

    <img width="369" alt="17" src="https://user-images.githubusercontent.com/31889335/89497341-1f491a80-d7f7-11ea-821a-c15452192710.png">

    위 AppBar와 같이 navigation icon, 화면 제목, 2개의 action item, 메뉴(=overflow menu) 를 갖는 AppBar를 구현하는 방법에 대해서는 [AppBar:Top - Android](https://material.io/develop/android/components/app-bars-top) 문서의 Regular top app bar examples 밑의 코드들을 보면 된다.

    또한 AppBar를 구성하는 4가지 API에 대한 xml 속성들도 알 수 있을 것이다.

    여기서부터는 문서를 직접 보고 코드를 작성하는 것이 더 효율적일 것 같다!

    Contextual action bar 타입의 AppBar를 구현하는 방법도 이 문서의 아래쪽에 있다!

    <br>

# 끝!



    




