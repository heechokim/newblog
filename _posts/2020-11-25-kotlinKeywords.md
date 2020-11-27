---
layout: post
title:  "[Kotlin] 1️⃣1️⃣ 코틀린에 존재하는 키워드(keyword)들"
date:   2020-11-25 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ 코틀린에 존재하는 키워드들 모아보기~!

✍🏻 [kotlin keyword 도큐먼트](https://kotlinlang.org/docs/reference/keyword-reference.html) 를 참고하여 작성합니다.

이 포스팅은 코틀린 언어에 존재하는 여러 가지 키워드들을 한 곳에 모아서 보기 위한 목적을 갖는다!

따라서 자세한 설명보다는 간단한 설명이 주를 이룰 것이다.

또한 공부하면서 그때 그때 알게 된 것들을 천천히 추가하며 완성할 예정이다.(이 포스팅 내용을 마무리 하는데 오~래 걸릴 수 있음!)

* __?:__

    이 키워드는 어떤 변수에 값을 저장할 때 두 값 중 하나를 선택하여 저장할 수 있도록 해준다.
    
    어떠한 변수를 조건에 따라 다른 값으로 초기화 해줘야 할 때 if-else 문을 작성하여 코드가 길어질텐데 이를 간단하게 줄여주는 키워드이다.

    __?:__ 연산자의 왼쪽 value값이 null이면 오른쪽 value 값으로 변수를 초기화한다.

    ~~~kotlin
    val test: Int = if(b != null) b.length else -1
    ~~~

    라는 코드는

    ~~~kotlin
    val test: Int = b?.length ?: -1
    ~~~

    과 동일한 코드가 된다.

* __super__

    이 키워드는 sub class에서 super class에 존재하는 public 메소드나 public 속성을 호출할 때 사용한다.

    자세한 내용은 [이 블로그의 다른 포스팅 - 안드로이드 개발에서 사용되는 코틀린 언어 패턴](https://choheeis.github.io/newblog//articles/2020-07/kotlinPattern) 에서 확인할 수 있다.

* __lateinit__

    이 키워드는 