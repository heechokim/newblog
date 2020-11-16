---
layout: post
title:  "[안드로이드] 👨‍👩‍👧 CoordinatorLayout이 뭔가!"
date:   2020-07-23 18:34:10 +0700
categories: [안드로이드]
---

[Sun Flower](https://github.com/android/sunflower) 앱을 클론코딩 하던 중 CoordinatorLayout이라는 뷰를 사용하는 xml 파일을 보게 되었다.

나는 기존에 대부분의 뷰를 ConstraintLayout을 사용하여 작성하였기 때문에 CoordinatorLayout에 대한 공부가 필요했다.

<br>

## 1️⃣ CoordinatorLayout이 뭘까?

가장 먼저 구글 developer 사이트에서 [CoordinatorLayout 릴리즈 문서](https://developer.android.com/jetpack/androidx/releases/coordinatorlayout) 을 검색해봤다.

<img width="690" alt="01" src="https://user-images.githubusercontent.com/31889335/89144105-b9a32700-d587-11ea-831c-5f1e7f54e2f0.png">

검색해보니 CoordinatorLayout은 Jetpack에 있는 UI 컴포넌트 중 하나였다!

조금 더 자세히는 2018년에 alpha 버전이 출시되어 2019년에 정식으로 1.1.0 버전이 출시된 UI 컴포넌트 라이브러리이다.

[CoordinatorLayout 가이드 문서](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout) 를 읽어보면 

<img width="535" alt="04" src="https://user-images.githubusercontent.com/31889335/97545607-9e24ac80-1a0e-11eb-81b6-8bf3b823fd24.png">

CoordinatorLayout은 FrameLayout을 상속받아 구현된 레이아웃이라는 것을 알 수 있다.

보통 CoordinatorLayout은 아래 두 가지 경우에 사용하기 위해 의도되었는데!

1. __화면의 최상위 부분을 꾸미거나 크롬 레이아웃을 사용하기 위해 사용된다.__

2. __하나 이상의 자식 뷰와 어떤 특별한 상호작용을 하기 위한 컨테이너로 사용된다.__

여기서 화면의 최상위 부분이라는 것은 안드로이드 화면에 배치되는 최상위 위젯을 의미하는데 __AppbarLayout__ 이나 __FloatingActionButton__ 이 최상위 위젯에 속한다.

또한 하나 이상의 자식 뷰와 특별한 상호작용이라는 말의 뜻은 __리사이클러뷰를 위로 스크롤하면 AppBar가 사라지고, 아래로 스크롤하면 AppBar가 나타나는 효과__ 를 생각해보면 될 것 같다!

![CoordinatorLayout_Example](https://user-images.githubusercontent.com/31889335/97552451-9fa6a280-1a17-11eb-8963-02dfdea1f96f.gif)

위 애니메이션 효과는 CoordinatorLayout의 자식뷰인 RecyclerView와 AppBarLayout가 상호작용 한 결과이다.

RecyclerView를 위로 스크롤하면 AppBar가 사라지는 상호작용이 일어났다고 생각하면 된다.

이와 같이 두 가지 경우에 사용하길 의도하여 출시된 레이아웃이 CoordinatorLayout이다 😃

<br>

## 2️⃣ CoordinatorLayout 사용 준비하기

CoordinatorLayout은 Jetpack에 속해있는 라이브러리이기 때문에 사용하기 위해서는 프로젝트에 종속성 추가를 해줘야 한다.

> [Jetpack에 관련하여 작성한 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-05/jetpack) 을 보면 Jetpack이 무엇인지, 종속성 추가는 어떻게 해야 하는 것인지 알 수 있을 것이다.

다음과 같이 앱 또는 모듈의 build.gradle 파일에 CoordinatorLayout 종속 항목을 추가하면 된다. 

~~~kotlin
dependencies {
        implementation "androidx.coordinatorlayout:coordinatorlayout:버전"
    }
~~~

## 3️⃣ CoordinatorLayout 사용하기

CoordinatorLayout을 사용하여 위에서 보여준 __리사이클러뷰를 위로 스크롤하면 AppBar가 나오고, 아래로 스크롤하면 AppBar가 사라지는__ 영상 속 화면을 구현해보자!

[이 블로그의 다른 포스팅 - AppBar 구현하기](https://choheeis.github.io/newblog//articles/2020-10/AppBar) 에서 구현 과정을 알아보면 된다.

# 끝!

이 포스팅을 하며 CoordinatorLayout의 존재 이유에 대해 알게 되었다! AppBar를 구현하며 조금 더 자세한 사용법을 알아보자~! 😀