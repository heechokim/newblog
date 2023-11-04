---
layout: post
title:  "[Kotlin] 1️⃣1️⃣ 코틀린에 존재하는 키워드(keyword)들과 연산자(operation)들"
date:   2020-11-25 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ 코틀린에 존재하는 키워드들 모아보기~!

✍🏻 [kotlin 도큐먼트 - keywords and operations](https://kotlinlang.org/docs/reference/keyword-reference.html) 를 참고하여 작성합니다.

이 포스팅은 코틀린 언어에 존재하는 여러 가지 키워드들과 연산자들을 한 곳에 모아서 보기 위한 목적을 갖는다!

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

* __as__

    as 가 사용되는 경우는 두 가지 경우가 있다.

    1. __[자료형 type 변환(casting) 목적으로 사용되는 경우](https://kotlinlang.org/docs/reference/typecasts.html#unsafe-cast-operator)__

        자료형 type 지정을 위해 가장 일반적으로 사용되는 연산자는 __:__ 연산자이다. 하지만 만약 타입을 바꾸고 싶다면?
        
        __as__, __as?__ 연산자가 자료형 type 변환에 사용되는 연산자이다.

        as 연산자는 __unsafe(불안전한) 자료형 변환__ 을 위해 사용되는 연산자이다.

        __unsafe type cast 연산자란?__

        as, as? 와 같이 type cast에 사용되는 연산자들 중 as 연산자는 특별한 기능을 가지고 있다.
        
        만약 해당 자료형 변환이 불가능하다면 exception(예외)을 throw(던지다) 발생시킨다. 이러한 as 연산자를 unsafe type cast 연산자라고 부른다.

        따라서 만약 개발자가 자료형 변환이 불가능한 경우를 염두해두고 as를 사용하여 type cast를 수행하면 컴파일시 컴파일 에러는 발생하지 않는다.
        
        하지만 런타임에서 에러를 발생시킬 수 있다.

        더 자세히 알아보자.

        ~~~kotlin
        val x: String = y as String
        ~~~

        위 코드는 변수 y에 담긴 데이터를 String 자료형으로 변환시킨 후, 변수 x에 대입하는 코드이다. 만약 y가 nullable한 변수여서 null 값을 저장하고 있다면?

        null은 String 자료형으로 변환할 수 없으므로 위 코드는 as 연산자에 의해 예외를 발생시킨다.

        하지만 아래 코드와 같은 경우는 어떨까?

        ~~~kotlin
        val x: String? = y as String?
        ~~~

        y를 String? 타입으로 변환시켜 x에 대입하는 코드인데 String? 는 nullable(null을 허용)한 자료형이므로 y가 null이여도 String? 타입으로 변환 가능하다.

        따라서 위 코드는 변환이 가능한 경우라 예외를 발생시키지 않는다.

    2. __[import 구문에서 사용되는 경우](https://kotlinlang.org/docs/reference/packages.html#imports)__

        > [여기](https://kotlinlang.org/docs/reference/packages.html#imports) 를 보면 알 수 있을 것이다. 나중에 import 구문에서 사용되는 as를 알아야할 필요가 있을 때 다시 알아보고 정리하자!

* __[as?](https://kotlinlang.org/docs/reference/typecasts.html#safe-nullable-cast-operator)__

    __as?__ 연산자는 바로 위에서 알아본 __as 연산자__ 와 비슷하지만 기능이 조금 다른 연산자이다.

    __safe type cast(안전한 타입 변환)__ 시에 사용되는 연산자이다.

    ~~~kotlin
    val x: String = y as String
    ~~~

    as 연산자에 대해 안다면 위 코드는 y가 null 일 때 exception을 발생시킨다는 것을 알고 있을 것이다.

    하지만 exception이 발생된다는 것은 해당 exception이 발생했을 때 예외 처리를 해주는 추가 코드가 필요하므로 위와 같은 사소한 변수 대입시에 예외가 발생한다면 골치 아플 수 있다.

    굳이 예외를 발생시키지 않을 수도 있을까? as? 연산자를 사용하면 된다!

    ~~~kotlin
    // 1. as 연산자에 의해 exception 발생
    val x:String = y as String

    // 2. as? 연산자에 의해 null 반환
    val x: String? = y as? String
    ~~~

    위 첫 번째 코드에서는 y가 null일 때 null을 String 자료형으로 변환할 수 없으므로 exception이 발생해야 한다.
    발생하지 않는다. 대신 null을 return한다. y가 null이면 x에 null이 대입되는 것이다.
