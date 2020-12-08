---
layout: post
title:  "[Kotlin] 9️⃣ 코틀린의 Array들"
date:   2020-10-19 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ Array가 있고, List도 있다?

코틀린 공부를 하다보니 ArrayList 같은 List 형태의 배열도 있고, IntArray, BooleanArray 등의 Array 형의 배열도 있다는 것을 알게 되었다.

사실 List와 Array 중 "배열" 이라고 불러야 하는 것은 Array이고, List는 "배열"이 아니라 "리스트"라고 불러야 한다.

먼저 [이전 포스팅 - 코틀린의 List들](https://choheeis.github.io/newblog//articles/2020-10/kotlinList) 포스팅을 보면 코틀린 표준 라이브러리 안의 여러 패키지 중 kotlin.collections 라는 이름의 패키지에 List 형태의 자료구조들이 구현되어 있다는 것을 알 수 있을 것이다.

이러한 List 형태의 자료구조와 Array 형태의 자료구조는 엄연히 다른 자료구조이다.

코틀린에는 Array 형태의 자료구조들이 존재한다.

Array 형태의 자료구조들도 코틀린 표준 라이브러리 안에 구현되어 있다. 하지만 List 형태의 자료구조들과는 다른 곳인 __kotlin__ 이라는 이름의 패키지 안에 구현되어 있다!

[kotlin 표준 라이브러리에 있는 패키지들](https://kotlinlang.org/api/latest/jvm/stdlib/#packages) 을 보면

<img width="733" alt="01" src="https://user-images.githubusercontent.com/31889335/96465188-a38e3400-1263-11eb-8d60-aae7451acf61.png">

위와 같이 kotlin 이라는 이름의 패키지도 있고, kotlin.collecions 라는 이름의 패키지도 있는 것을 알 수 있다.

여기서 [kotlin](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/) 이라는 패키지는 __코틀린의 core 함수들과 core 자료형(type)들이 구현__ 되어 있는 곳이다!

즉, 이곳에 Boolean, Int, Char 등의 자료형들도 구현되어 있고, 이 외의 여러 core 함수들이 구현되어 있다는 것이다.

또 바로 이곳에 앞으로 알아보아야 할 Array 형태의 자료구조들도 구현되어 있다!

<img width="241" alt="02" src="https://user-images.githubusercontent.com/31889335/96466731-1cda5680-1265-11eb-928c-afe19b5ee4e7.png">
<img width="217" alt="03" src="https://user-images.githubusercontent.com/31889335/96466739-1e0b8380-1265-11eb-9f82-0da694303abc.png">
<img width="220" alt="04" src="https://user-images.githubusercontent.com/31889335/96466743-1f3cb080-1265-11eb-844d-7d75845625d1.png">
<img width="233" alt="05" src="https://user-images.githubusercontent.com/31889335/96466751-1fd54700-1265-11eb-91bf-3411566639b2.png">
<img width="215" alt="06" src="https://user-images.githubusercontent.com/31889335/96466756-21067400-1265-11eb-9878-cb99ec9075c5.png">
<img width="206" alt="07" src="https://user-images.githubusercontent.com/31889335/96466760-219f0a80-1265-11eb-91d8-47c4d0275677.png">
<img width="220" alt="08" src="https://user-images.githubusercontent.com/31889335/96466765-2237a100-1265-11eb-921c-8220d1c49e72.png">
<img width="224" alt="09" src="https://user-images.githubusercontent.com/31889335/96466766-22d03780-1265-11eb-9c95-072512ba031e.png">

위 그림들을 보면 다양한 자료형의 Array 들이 kotlin 패키지 안에 구현되어 있음을 알 수 있다!

## 2️⃣ Array 라는 자료구조

* 자료구조 Array의 모습

    일단 [BooleanArray](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/) 를 예로 들어 Array 형태의 자료구조 모습에 대해서 알아보자.

    BooleanArray는 리스트가 아니라 배열이다. 더 자세히 말하자면 배열에 저장될 데이터들의 타입이 boolean 인 배열이다.

    BooleanArray 외의 IntArray, CharArray 등 다른 이름의 Array들도 원리는 이와 똑같다.

    __배열__ 이라는 자료구조에 대해서는 [이 블로그의 다른 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 의 배열 부분에서 알 수 있을 것이다.

* Array 자료구조 선언하기

    > 아래 내용들은 [여기](https://kotlinlang.org/docs/reference/basic-types.html#arrays) 를 참고하여 작성한 것!

    1. __첫 번째 방법 ) 확장 함수 사용하기__

        ~~~kotlin
        var arrayTest = intArrayOf(1, 2, 3)
        ~~~

        위 코드와 같이 intArrayOf() 나 arrayOfNulls() 등 Array를 선언할 때 사용할 수 있는 여러 확장 함수들이 존재한다.

        위 코드는 배열 [1, 2, 3] 의 모습으로 생성되었다.

    2. __두 번째 방법 ) 생성자 함수 사용하기__

        ~~~kotlin
        var arrayTest = IntArray(5){ i -> i }
        ~~~

        확장 함수 말고 Array 클래스의 생성자 함수를 사용하여 Array를 생성하는 방법이 있다.

        이 방법의 좋은 점은 배열의 크기를 정해놓고, 함수를 통해 배열의 각 원소에 저장될 데이터를 조작할 수 있다는 점이다.

        위 코드의 i 는 0 부터 4까지 증가하기 때문에 [0, 1, 2, 3, 4] 의 모습으로 배열이 생성된다.

        위 코드처럼 생성자 함수를 사용할 때 함수를 사용할 수 있는 이유에 대해 알아보자.

        <img width="724" alt="10" src="https://user-images.githubusercontent.com/31889335/96468086-a2aad180-1266-11eb-9af1-f465c4188548.png">

        BooleanArray의 [Init 방법](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/-init-.html) 을 보면 두 번째 인자에 함수가 들어갈 수 있는 것을 알 수 있다.

        코틀린은 함수형 프로그래밍 언어이기 때문에 어떤 함수의 인자에 또 다른 함수가 들어갈 수 있는데 만약 마지막 인자로 함수가 들어오면 위 코드와 같이 () 밖으로 {} 를 뺄 수 있다. 이에 대한 내용은 [이 블로그의 다른 포스팅 - 고차함수와 람다표현식](https://choheeis.github.io/newblog//articles/2020-12/kotlinHigherOrderFunctionAndLambda) 포스팅을 보면 알 수 있을 것이다.

        따라서 위 코드에서 IntArray 형 배열의 초기화를 IntArray(5){ i -> i } 이라고 작성한 것이다.

* Array 를 조작할 때 사용되는 함수들

    1. get(인덱스)

        배열 안에서 해당 인덱스에 해당하는 데이터를 return 하는 함수

    2. set(인덱스, 데이터)

        배열 안에서 해당 인덱스의 데이터를 set 함수의 두 번째 인자에 있는 데이터로 수정해주는 함수

    3. [] 연산자로 바로 index에 해당하는 값을 get하거나 set할 수 있음 (인텔리제이에서 get과 set을 사용하면 [] 연산자로 바꾸라고 나옴!)


## 3️⃣ Array를 List로 바꾸기

코틀린에는 자료구조를 바꿔주는 함수가 다양하게 존재한다. 주로, __to~()__ 의 이름을 가진다.

* [toList()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-list.html) 함수 사용

* [toMutableList()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-mutable-list.html) 함수 사용

## 4️⃣ Array에 포함된 원소 중 가장 큰 값/작은 값 구하기

kotlin.collections 패키지에 구현되어 있는 __[maxOrNull()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/max-or-null.html)__ 메소드를 사용하면 된다.

가장 작은 값은 __[minOrNull()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/min-or-null.html)__ 메소드를 사용하면 된다.

