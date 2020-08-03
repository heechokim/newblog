---
layout: post
title:  "[안드로이드] 👨‍👩‍👧 Coordinator Layout"
date:   2020-07-23 18:34:10 +0700
categories: [안드로이드]
---

# [Coordinator Layout]

[Sun Flower](https://github.com/android/sunflower) 앱을 클론코딩 하던 중 fragment_view_pager.xml 이라는 파일에서 CoordinatorLayout이라는 뷰를 사용하는 모습을 보게 되었다.

나는 기존에 대부분의 뷰를 ConstraintLayout을 사용하여 작성하였기 때문에 CoordinatorLayout에 대한 공부가 필요했다.

<br>

## 1️⃣ CoordinatorLayout이 어디에 명시되어 있나?

가장 먼저 구글 developer 사이트에서 [CoordinatorLayout](https://developer.android.com/jetpack/androidx/releases/coordinatorlayout) 을 검색해봤다.

<img width="690" alt="01" src="https://user-images.githubusercontent.com/31889335/89144105-b9a32700-d587-11ea-831c-5f1e7f54e2f0.png">

검색해보니 CoordinatorLayout은 Jetpack에 속해있는 UI 컴포넌트였다!

이 문서를 읽어보면 간단한 설명이 적혀있는데 CoordinatorLayout은 __AppBarLayout 및 FloatingActionButton과 같은 최상위 위젯을 배치할 때 사용되는 Layout__ 이라고 설명되어 있다.

이러한 CoordinatorLayout에 대한 자세한 내용은 다음에 살펴보고 이제 CoordinatorLayout을 프로젝트에서 사용하려면 어떻게 해야하는지 알아보자.

<br>

## 2️⃣ CoordinatorLayout 종속성 추가하기

CoordinatorLayout은 Jetpack에 속해있는 라이브러리이기 때문에 종속성 추가를 선언해줘야 한다.

> [Jetpack에 관련하여 작성한 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-05/jetpack) 을 보면 Jetpack이 무엇인지, 종속성 추가는 어떻게 해야 하는 것인지 알 수 있을 것이다.

다음과 같이 앱 또는 모듈의 build.gradle 파일에 CoordinatorLayout 종속 항목을 추가하면 된다.

~~~kotlin
dependencies {
        implementation "androidx.coordinatorlayout:coordinatorlayout:1.1.0"
    }
~~~

<br>

## 3️⃣ 그래서 CoordinatorLayout이 도대체 뭘까? (클래스 관점)

앞에서는 CoordinatorLayout의 release에 관련된 문서를 읽어봤다면 이번에는 [CoordinatorLayout이라는 위젯 자체에 대한 문서](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout) 를 읽어보자.

<img width="670" alt="02" src="https://user-images.githubusercontent.com/31889335/89145307-59ae7f80-d58b-11ea-8027-ee44ba862f3f.png">

일단 CoordinatorLayout 클래스에 대해서 살펴보면 위와 같은 구조로 되어 있다.

CoordinatorLayout 클래스는 부모 클래스로 ViewGroup을 상속하고, NestedScrollingParent2, NestedScrollingParent3 라는 인터페이스를 구현하고 있다.

위 그림에서 클래스 상속 계층도를 보면 기존의 android ViewGroup 클래스 하위 계층에 Jetpack 패키지인 androidx 로 묶여있는 CoordinatorLayout이 연결된 것을 볼 수 있을 것이다.

<br>

## 4️⃣ 그래서 CoordinatorLayout이 도대체 뭘까? (역할 관점)

> [android developer 사이트 - CoordinatorLayout](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout) 문서와 [장범석님의 개발일지 - CoordinatorLayout 기본 동작 원리](http://dktfrmaster.blogspot.com/2018/03/coordinatorlayout.html) 를 참고하여 작성함.

CoordinatorLayout은 한 마디로 매우 강력한 기능을 가진 __FrameLayout__ 이라고 할 수 있다.

CoordinatorLayout은 다음과 같은 두 경우에 사용되도록 만들어졌다.

1. 애플리케이션에서 최상위의 decor View로써 사용
2. 자식뷰들간의 특정한 상호작용을 지원하는 컨테이너로써 사용

2번의 경우 설명을 조금 더 자세히 할 수 있다.

CoordinatorLayout는 자신을 부모로 가지고 있는 자식뷰에 __Behavior__ 라는 것을 지정할 수 있는데 이것을 지정함으로써 하나의 부모 안에서 여러 다른 상호작용을 지원할 수 있고, 자식뷰들간에도 서로 상호작용할 수 있게 된다.

또한 CoordinatorLayout을 부모로 가지고 있는 자식뷰들은 __anchor__ 라는 것을 갖게 된다. 이 anchor는 일반적으로 다른 뷰를 가리키며 뷰의 id 값을 작성함으로써 anchor를 갖게 된다.

anchor은 반드시 CoordinatorLayout의 자식뷰들 중 하나여야 한다.

<br>

## 5️⃣ CoordinatorLayout과 연관된 클래스들

<img width="889" alt="03" src="https://user-images.githubusercontent.com/31889335/89146392-84e69e00-d58e-11ea-82db-7d066f921d1c.png">

androidx.coordinatorlayout.widget 패키지로 묶인 클래스는 위와 같이 총 4가지가 있다.

그 중 맨 첫번째 클래스인 CoordinatorLayout에 대해 위에서 살펴보았고, 다른 클래스들도 찾아보면 좋을 것 같다.

<br>





