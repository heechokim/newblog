---
layout: post
title:  "[Kotlin] 1️⃣0️⃣ 코틀린의 Map들"
date:   2020-11-25 18:34:10 +0700
categories: [kotlin]
---

## 0️⃣ 코틀린에서 Map 자료구조를 사용하려면?

코틀린에서 Map 자료구조는 kotlin 표준 라이브러리(kotlin stdlib) 안의 __kotlin.collections__ 라는 패키지 안에 구현되어 있음을 [Collection에 대한 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-10/kotlinCollection) 을 보면 알 수 있을 것이다.

따라서 코틀린에서 Map 자료구조를 사용하려면 kotlin 표준 라이브러리에 있는 것들을 가져다 사용하면 된다!

## 1️⃣ Map 이라는 자료구조

[Map](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/)은 kotlin.collections 패키지에 구현되어 있는 자료구조 중 하나이다.

Map 이라는 자료구조는 일반적으로 __key__ 와 __value__ 의 쌍으로 데이터를 저장하는 구조를 구현한 자료구조이다.

어떤 자료구조들보다 key를 통해서 value를 빠르게 찾아올 수 있어 효율적일 때가 많다.

단, value를 찾기 위해서 key는 반드시 __중복되면 안된다.__

코틀린의 Map은 신기한 점이 있다. __value의 값으로 함수가 들어갈 수 있다__ 는 점이다!

이렇게 간단하다면 간단한 자료구조인 Map을 코틀린에서 어떻게 조작할 수 있는지 알아보자.

## 2️⃣ Map을 조작할 수 있는 코틀린 함수들

✍🏻 [Map Specific Operations 도큐먼트](https://kotlinlang.org/docs/reference/map-operations.html) 를 참고하여 작성합니다.

* __Map 자료구조 선언하기 --> mapOf() 사용__

    Map 자료구조를 사용하려면 코드로 Map 자료형의 변수를 선언해야 한다.

    Map 자료형 변수 선언시 [mapOf()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-of.html)라는 메소드를 사용하면 된다.

    단, mapOf()는 read-only(읽기 전용, 수정 불가)한 Map 자료형을 선언할 때만 사용된다.

    만약 수정 가능한 Map 자료형을 선언하려면 MutableMap에 대해 알아보면 된다.

    mapOf() 함수를 사용하여 () 안에 key-value 쌍의 데이터를 __배열의 형태로__ 한꺼번에 여러개 선언할 수 있다.

    __to__ 라는 표현을 사용해서 key와 value를 매칭해준다.

    <img width="576" alt="01" src="https://user-images.githubusercontent.com/31889335/100115993-8e39a480-2eb6-11eb-8453-1d4b7acd848f.png">

    위 코드의 출력 결과는

    <img width="182" alt="02" src="https://user-images.githubusercontent.com/31889335/100116084-ac9fa000-2eb6-11eb-9b95-90d11ff6cfea.png">

    위와 같다.

    출력 결과를 분석해보면 map이라는 이름의 배열에 Map 형태의 자료구조가 총 3개 저장된 모습이다.

    mapOf() 함수는 배열에 Map을 저장하기 때문에 __순서__ 를 가지고 있고, 만약 동일한 key를 가진 Map이 한 배열에 여러 개 저장되어 있으면 그 중 가장 마지막 원소로 저장된 Map의 key만 유효하게 된다.

* __key를 통해 value값 찾기__

    Map 자료구조의 장점은 key를 통해 해당 value를 빠르게 찾을 수 있다는 점이라고 했다.

    <img width="575" alt="03" src="https://user-images.githubusercontent.com/31889335/100117407-1a989700-2eb8-11eb-9a4f-89e1615c68a2.png">

    출력 결과는

    <img width="48" alt="04" src="https://user-images.githubusercontent.com/31889335/100117155-dc9b7300-2eb7-11eb-977b-b2c4af854cac.png">

    위와 같다.

    위 코드를 보면 알 수 있듯이 __get()__ 함수를 사용하여 ()안에 key를 지정해줌으로써 해당 value 값을 가져올 수 있다.

    마찬가지로 간단하게 __[key]__ 표현을 사용하여 해당 value 값을 가져올 수도 있다.

    추가로 key를 통해 value를 찾는 기능이 몇 가지 더 있다.

    * __[getValue()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/get-value.html)__ : 해당 key가 존재하지 않으면 exception을 던진다.

    * __[getOrElse()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/get-or-else.html)__ : 해당 key가 존재하지 않으면 작성해준 lambda 함수를 실행한다.

    * __[getOrDefault()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/get-or-default.html)__ : 해당 key가 존재하지 않으면 지정해준 값을 리턴한다.