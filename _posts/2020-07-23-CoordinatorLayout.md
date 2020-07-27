---
layout: post
title:  "[안드로이드] Coordinator Layout(포스팅 완료 안됨)"
date:   2020-07-23 18:34:10 +0700
categories: [안드로이드]
---

## Coordinator Layout이 뭐냐!
---

* __CoordinatorLayout 은 어디에 있는 라이브러리일까?__

    Coordinator Layout은 

    <img width="244" alt="04" src="https://user-images.githubusercontent.com/31889335/88266149-a67f9400-cd09-11ea-9060-3a93f3b52a09.png">

    androidx 패키지안에 포함되어 있는 것으로 보아 Jetpack에 포함되어 있는 라이브러리라는 것을 알 수 있다!

    <br>

* __CoordinatorLayout이 뭘까?__

    <img width="672" alt="스크린샷 2020-07-27 오후 2 38 03" src="https://user-images.githubusercontent.com/31889335/88507207-dcc55800-d016-11ea-8cf3-a166fe60d339.png">

    [Coordinator Layout 문서](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout) 에 따르면 CoordinatorLayout은 __ViewGroup__ 클래스를 상속받고 있고, NestedScrollingParent2, NestedScrollingParent3 이라는 인터페이스를 구현하는 __클래스__ 이다.

    <br>

* __CoordinatorLayout은 언제 사용될까? 왜 사용될까?__

    [Coordinator Layout 문서](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout) 에 따르면 CoordinatorLayout은 아주 강력한 __FrameLayout__ 이라고 한다.

    FrameLayout의 일종인 CoordinatorLayout은 주로 두 가지 경우에 사용되도록 만들어진 클래스이다.

    1. 하나 또는 여러 개의 child 뷰와 어떤 특별한 상호작용을 하기 위한 컨테이너로 사용되는 경우

    2. chrome 레이아웃이나 

    <br>

* __CoordinatorLayout이 가지는 특징은 무엇일까?__

    CoordinatorLayout만이 가지는 특징이 한 가지 있다!

    CoordinatorLayout안의 child 뷰에 대한 __동작(behavior)__ 을 지정할 수 있도록 되어 있기 때문에 단 하나의 부모 layout으로도 여러 다양한 상호작용을 제공할 수 있다.

    CoordinatorLayout안의 child 뷰는 __anchor__ 기능을 가지고 있다.

    CoordinatorLayout은 jetpack 라이브러리이기 때문에 https://developer.android.com/jetpack/androidx/releases/coordinatorlayout?hl=ko 를 보고 종속성을 추가해줘야 한다.

    > 이 포스팅 다시다시!