---
layout: post
title:  "[Kotlin] 9️⃣ 코틀린의 Array들"
date:   2020-10-19 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ Array가 있고, List도 있다?

코틀린 공부를 하다보니 ArrayList 같은 List 형태의 배열도 있고, IntArray, BooleanArray 등의 Array 형의 배열도 있다는 것을 알게 되었다.

먼저 [이전 포스팅 - 코틀린의 List들](https://choheeis.github.io/newblog//articles/2020-10/kotlinList) 포스팅을 보면 코틀린 표준 라이브러리 안의 여러 패키지 중 kotlin.collections 라는 이름의 패키지에 List 형태의 자료구조들이 구현되어 있다는 것을 알 수 있을 것이다.

코틀린에는 이 List 형태의 자료구조들과는 또 다르게 생긴 Array 형태의 자료구조들이 존재한다.

Array 형태의 자료구조들도 코틀린 표준 라이브러리 안에 구현되어 있다. 하지만 List 형태의 자료구조들과는 다른 곳인 __kotlin__ 이라는 이름의 패키지 안에 구현되어 있다!

[kotlin 표준 라이브러리에 있는 패키지들](https://kotlinlang.org/api/latest/jvm/stdlib/#packages) 을 보면

<img width="733" alt="01" src="https://user-images.githubusercontent.com/31889335/96465188-a38e3400-1263-11eb-8d60-aae7451acf61.png">

위와 같이 kotlin 이라는 이름의 패키지도 있고, kotlin.collecions 라는 이름의 패키지도 있는 것을 알 수 있다.

여기서 [kotlin](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/) 이라는 패키지는 __코틀린의 core 함수들과 자료형(type)들이 구현__ 되어 있는 곳이다!

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

일단 [BooleanArray](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/) 를 예로 들어 Array 형태의 자료구조에 대해서 알아보자.

BooleanArray는 Java의 __boolean[]__ 과 같다. 즉, 배열이고 배열에 저장될 데이터들의 타입이 boolean 이라는 것이다.

BooleanArray 외의 다른 자료형들의 Array들도 이와 똑같다.

이러한 형태의 자료구조를 사용할 때 사용할 수 있는 함수들은 [BooleanArray 문서](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/) 를 보면 알 수 있듯이 각각의 문서를 보면 된다.

__여기서 알아두면 좋은 점__ 은 이러한 Array 형태의 자료구조를 초기화 할 때의 방법인데

~~~kotlin
fun main() {
    // 크기가 3인 배열에 숫자 0, 1, 2 가 차례로 저장되며 초기화됨
    // it 은 index 0 부터 순차대로 증가한다.
    val numbers = IntArray(3){ it }

    numbers.forEach {
        println(it)
    }
}
~~~

위와 같은 코드이다.

위 코드의 출력 결과는

<img width="30" alt="11" src="https://user-images.githubusercontent.com/31889335/96469008-b145b880-1267-11eb-9267-6c95bc2c218b.png">

이와 같다.

<img width="724" alt="10" src="https://user-images.githubusercontent.com/31889335/96468086-a2aad180-1266-11eb-9af1-f465c4188548.png">

BooleanArray의 [Init 방법](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/-init-.html) 을 보면 두 번째 인자에 함수가 들어갈 수 있는 것을 알 수 있다.

코틀린은 함수형 프로그래밍 언어이기 때문에 어떤 함수의 인자에 또 다른 함수가 들어갈 수 있는데 만약 마지막 인자로 함수가 들어오면 위 코드와 같이 () 밖으로 {} 를 뺄 수 있다. 이에 대한 내용은 [이전 포스팅 - 코틀린 언어 압축 스터디](https://choheeis.github.io/newblog//articles/2020-07/KotlinZip) 포스팅의 고차 함수 부분을 보면 알 수 있을 것이다.

따라서 위 코드에서 IntArray 형 배열의 초기화를 IntArray(3){ it } 이라고 작성한 것이다.