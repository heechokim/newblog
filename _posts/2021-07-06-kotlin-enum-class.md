---
layout: post
title:  "[Kotlin] 코틀린 Enum Class란?"
date:   2021-07-06 18:34:10 +0700
categories: [kotlin]
---

> __참고 자료__  
> [Kotlin Document - Enum Classes](https://kotlinlang.org/docs/enum-classes.html)  
> [Youtube 테크과학! DiMo - Kotlin 강좌 #25 - Data Class와 Enum Class](https://www.youtube.com/watch?v=SKosPXHLT5Q)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. What is Enum Class?](#1)  
> [2. Enum Class만의 특징](#2)


<br>

## 0️⃣ 프롤로그<a id="0"></a>

프롤로그까지는 아니지만...  요즘 회사 다니면서 느낀점... 저녁시간과 주말이 정말 소중한 시간이였다..!! 퇴근하면 부족한 부분 공부하거나 좋아하는 책들 읽으면서 시간을 보내는데 이 시간이 너무 짧게만 느껴진다.  
그치만 이제 곧.. 캡디 개발도 해야하고 이사갈 집도 구하러 다녀야할텐데 ,, 미래의 나에게 맡기자..(ㅎㅎ?) 

## 1️⃣ What is Enum Class?<a id="1"></a>

* 이 포스팅은 코틀린 버전 v1.5.20 기준에서 공부한 Enum Class 설명이다. 

* Enum Class의 __"Enum"__ 은 Enumerated(='열거'라는 뜻) type의 줄임말이다. 즉, Enum class는 열거형 타입의 클래스이다. 유튜브 몇 개 봐보니 한국말로는 이늄 클래스 또는 이넘 클래스로 읽는 듯하다.

* 코틀린에서는 일반 Class 외에 Data Class라는 것과 Enum Class라는 것을 따로 제공하는데 이 두 클래스는 일반 클래스가 제공하지 않는 특정한 용도의 기능들을 각각 제공한다.(두 클래스 중 Enum Class를 알아보자~)

* ~~~kotlin
  enum class Store {
      COKE, SNACK, ICECREAM, MILK, WATER
  }
  ~~~

* (위 코드 참고) Enum Class의 가장 기본적인 사용법은 위와 같다. 뭔진 몰라도 5개의 상수?들이 한줄로 쭈루룩 열거되고 있는 모습이다. 'Enum = 열거'에 딱 맞는 모습이다.

* Enum Class에 선언된 5가지 상수들은 사실 __클래스 Store의 객체(object)__ 이다. 즉, 위 5개의 상수같이 보이는 것들은 모두 Store 클래스의 객체를 선언한 것이다. 이 때, 각 객체 이름을 Store 외의 이름으로 자유롭게 지을 수 있다. Enum Class에 객체를 선언할 때는 상수를 선언할 때처럼 모든 문자를 대문자로 쓰고, 여러 개의 객체를 선언할 때는 콤마(,)를 찍어 선언하는 것이 규칙이다.

* 따라서 위 Store Enum Class 안에서는 이미 Store 클래스의 객체 5개가 선언되어 있는 것이다.

* ~~~kotlin
  enum class Store(val price: Int) {
      // 가게에서 파는 5가지 상품에 가격 부여하여 객체 초기화
      COKE(1800),
      SNACK(2000),
      ICECREAM(900),
      MILK(1500),
      WATER(600)
  }
  ~~~

* (위 코드 참고) Enum Class에 선언된 것들은 해당 Enum Class의 객체이므로 enum class에 생성자를 만들어주고, 각 객체 선언 시 초기화를 해줄 수도 있다.

## 2️⃣ Enum Class만의 특징<a id="2"></a>

* ~~~kotlin
  enum class Store(val price: Int) {
      COKE(1800),
      SNACK(2000),
      ICECREAM(900),
      MILK(1500),
      WATER(600); // 새미콜론 찍기

      fun isExpensive(standardPrice: Int): Boolean = this.price > standardPrice // 가격 비교 대상이 객체 자기 자신의 가격이므로 this.price임
  }
  ~~~

* (위 코드 참고) Enum Class는 일반 클래스의 특징도 가지고 있기 때문에 멤버 객체 외에 멤버 메소드도 선언할 수 있다. 대신 이런 경우에는 멤버 객체 선언부 마지막에 새미콜론(;)을 찍어 멤버 객체 선언부와 멤버 메소드 선언부를 분리시켜줘야 한다.

* ~~~kotlin
   fun main() {
      var selectedItem = Store.ICECREAM
      if(selectedItem.isExpensive(1000)) {
          println("1000원 밖에 없음.. 비싸ㅠ")
      } else {
          println("싸네! 살래~")
      }

      selectedItem = Store.COKE
      if(selectedItem.isExpensive(1000)) {
          println("1000원 밖에 없음.. 비싸ㅠ")
      } else {
          println("싸네! 살래~")
      }
  }
  ~~~
  
* (위 코드 참고) main() 함수를 위와 같이 작성하여 실행해보면 첫 번째 출력은 싸다고 나오고 두 번째 출력은 비싸다고 나온다.

* ~~~kotlin
  enum class Store {
    MILK {
        override fun buy() = println("우유 샀음")
    },
    ICECREAM {
        override fun buy() = println("아이스크림 샀음")
    }; // 새미콜론 찍기

    abstract fun buy() // 추상 메소드(정의만 된 상태의 메소드이므로 어딘가에서 구현이 필요)
  }
  ~~~
  
* (위 코드 참고) Enum Class의 또 다른 특징은 Enum Class에 선언된 객체는 자신만의 익명 클래스(anonymous class)로 선언할 수도 있다는 점이다. 익명 클래스는 자신만의 메소드를 가질 수도 있고 위 코드와 같이 메소드를 오버라이딩할 수도 있다. 

* ~~~kotlin
  fun main() {
    val milk = Store.MILK
    milk.buy()

    val icecream = Store.ICECREAM
    icecream.buy()
  }
  ~~~
  
* (위 코드 참고) main 함수를 위와 같이 작성하고 실행시키면 첫 번째 줄에는 우유 샀음이 출력되고 두 번째 줄에는 아이스크림 샀음이 출력된다.

# 끝 아님 ~

https://kotlinlang.org/docs/enum-classes.html#implementing-interfaces-in-enum-classes 여기부터 이어서 공부하기!
