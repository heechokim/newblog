---
layout: post
title:  "[안드로이드] Activity LifeCycle(수명주기)가 뭐예요?"
date:   2021-01-03 18:34:10 +0700
categories: [안드로이드]
---

> __참고 레퍼런스__
>
> ✍🏻 [Android Developer 도큐먼트 - Activity 소개](https://developer.android.com/guide/components/activities/intro-activities#mtal)
>
> ✍🏻 [Android Developer 도큐먼트 - Activity LifeCycle 소개](https://developer.android.com/guide/components/activities/activity-lifecycle)
>
> ✍🏻 [Youtube MindOrks 채널 - Activity LifeCycle](https://www.youtube.com/watch?v=RiFui-i-s-o)

## 1️⃣ What is Activity LifeCycle(수명 주기)?

* __LifeCycle에 대해 알아보기 전에 [이 블로그의 다른 포스팅 - activity](https://choheeis.github.io/newblog//articles/2020-12/activity)에 대해서 먼저 읽자!__

    * Activity는 수명 주기라는 것을 가지고 있고, 수명 주기에 따라 __여러 상태__ 를 띈다는 것을 알게 될 것이다.

    * Activity의 상태간 전환을 위한 여러 __콜백 메소드__ 가 존재하고, 이 콜백 메소드들이 호출됨에 의해 상태가 전환됨도 알게 되었을 것이다.

* __생명 주기에 이해하기 전, 알아야할 것!__

    * 사용자는 앱을 탐색하고, 앱에서 나가고, 앱을 다시 들어오고,,, 사용자는 이렇게 앱을 사용하며 여러 가지 활동을 하게 된다.
    
    * 사용자가 여러 가지 활동을 하는 동안 앱의 Activity는 서로 다른 상태를 가지게 된다.

    * 또, 사용자의 활동에 맞게 이 상태들이 계속 전환된다.

    * Activity를 구현할 때 상속해야 하는 [Activity](https://developer.android.com/reference/android/app/Activity) 클래스는 






