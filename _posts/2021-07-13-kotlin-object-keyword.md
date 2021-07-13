---
layout: post
title:  "[Kotlin] 코틀린의 object 키워드 잘 알고 사용하기(feat. companion object)"
date:   2021-07-13 18:34:10 +0700
categories: [kotlin]
---

> __참고 자료__  
> [Kotlin Document - Object expressions and declarations](https://kotlinlang.org/docs/object-declarations.html)  
> [Youtube Android Developers - Object - Kotlin Vocabulary](https://www.youtube.com/watch?v=KUk6k865Vgg)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. object expressions 와 object declarations 구분하기](#1)  
> [2. What is object expressions?](#2)



<br>

## 0️⃣ 프롤로그<a id="0"></a>

코틀린으로 코드를 작성하다보면(저는 안드로이드 앱 개발을 코틀린으로 합니다아) __object__ 라는 키워드를 사용하게 되는 몇 가지 경우가 있다!  
하지만 그동안 대충 구글링하여 짧은 순간 습득한 지식으로 이해한 후 사용했었는데..(으이구~) 좀 제대로 알고 사용하려는 목적으로 공부해본다..ㅎㅎ  
(아! 그리고 object 키워드에 대해서 공부하게 된 가장 큰 목적은 companion object를 제대로 이해해보고자 시작하게 된 것이다..ㅋㅋㅋ)

## 1️⃣ object expressions 와 object declarations 구분하기<a id="1"></a>

* [kotlin 공식 문서의 Object expressions and declarations](https://kotlinlang.org/docs/object-declarations.html)을 읽어보니 __Object expressions__ 와 __Object declarations__ 로 나누어서 설명하고 있다.

* 먼저 이 두 용어를 한글 번역해보면 Object expression은 "객체 식" 이고 Object declaration은 "객체 선언" 이다.(객체 선언은 대충 감이 잡히지만 객체 식은 뭘까? 알게 되겠지..)

* __object expressions와 object declarations의 공통점__

  * 예를 들어, 개발을 하다가.. A 클래스의 모습에서 아주 살짝만 수정된 모습인 B 클래스의 객체를 생성해야 할 경우가 있을 수도 있다. 이런 경우, B 클래스를 명시적으로 새로 작성하는 건 비효율적일 수 있다. 왜? 거의 비슷한 코드를 하나 더 작성하는 것이니까! 그럼 B 클래스를 새로 작성하지 않고 B 클래스의 객체를 생성할 수 있을까?

  * 코틀린은 딱! 이런 상황에서 사용할 수 있도록 object expressions와 object declarations라는 것을 제공한다! (일단, 이 두 개가 뭔진 모르겠지만 요런 상황에서 사용할 수 있다는 공통점이 있구나~ 라고 생각하고 넘어가자)

## 2️⃣ What is object expressions?<a id="2"></a>

* 코틀린에서 지원해주는 __object expressions라는 것은 object 키워드를 사용하여 익명 클래스의 객체를 생성해주는 것이다.__ 바로 아래 코드를 보면 무슨 말인지 한 방에 이해될 것! (익명 클래스는 class 키워드를 사용하여 명시적으로 이름과 함께 선언된 클래스가 아닌, 이름 없는 클래스를 말한다.)

* ~~~kotlin
  fun main() {
      val yourName = object {
          val LastName = "Kim"
          val FirstName = "Chohee"

          // object expressions는 내부적으로 코틀린의 Any 클래스를 default 부모 클래스로 상속하고 있기 때문에 toString() 메소드를 오버라이딩할 수 있다!
          override fun toString() = "Your name is $LastName $FirstName"
      }

      println(yourName.toString())
  }
  ~~~
  
* (위 코드 참고) object 키워드 뒤에 나오는 코드를 보면 멤버 변수가 LastName과 FirstName이고, 멤버 메소드로 toString() 를 가지는 익명 클래스가 작성되어 있다! 이런 식으로 익명 클래스를 작성해주고 그 앞에 object 키워드를 붙여주면 해당 익명 클래스의 __객체가 바로 생성된다.__ 따라서 __object expressions라는 것은 익명 클래스를 객체화해주는 것!__ 이라고 쉽게 이해하면 될 듯하다.

* <img width="700" alt="01" src="https://user-images.githubusercontent.com/31889335/125470357-c2ffd2d5-b7a3-437b-b3ff-a7b465de7cf2.png">

* (위 그림 참고) 다시 한 번 보자. 위 코드는 object 키워드를 익명 클래스 앞에 붙여줌으로써 해당 익명 클래스의 객체를 바로 생성하고, 생성한 객체를 변수 yourName에 할당한 코드이다. 위 코드의 main 함수를 실행해보면 Your name is Kim Chohee가 출력된다!

* 이렇게 특정 코드 라인에서 익명 클래스를 작성하고 이를 바로 객체로 만드는 경우는, 해당 익명 클래스를 프로젝트 내에서 여러번 재사용하지 않고 딱 한 번만 사용해야 하는 경우에 유용할 것이다.(ㅇㅈ!)
 
* 그리고 object expressions의 한글 번역이 "객체 식"이었던 것을 기억해보자. 프로그래밍 언어 분야에서 "expression" 라는 것은 "수식" 이라는 뜻을 가지고 있고, 숫자와 연산자로 이루어지는 1+2와 같은 수식 외에도 함수 호출식, 변수 이름 같은 식별자, 배열의 할당 연산자([])와 같은 것도 모두 "수식"에 포함된다. 여기서 중요한 것은 프로그래밍 언어 분야에서 "수식"이라고 불리는 것들은 모양과 형태가 다를 수 있지만 모두 하나의 단일 값이 될 수 있는 것을 의미한다. 따라서 익명 클래스를 객체화하는 행위를 object expressions 라고 이름 붙인 것도 익명 클래스를 객체화하여 하나의 단일한 값으로 만들 수 있기 때문일 것이라고 추측한다 ㅎ.ㅎ

* 위 코드 주석을 보면 object expressions는 기본적으로 [Any](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-any/)라는 코틀린 클래스를 상속하고 있다고 언급했다! 좀 더 확장해서.. Any 클래스 말고도 다른 클래스나 다른 인터페이스를 상속한 익명 클래스도 object expressions로 객체화할 수 있다.

* ~~~kotlin
  fun main() {
      // Person 인터페이스를 상속하는 익명 클래스를 object expressions 사용해서 객체화시키기
      val yourName = object: Person {
          val LastName = "당신은"
          val FirstName = "천재!"

          // Person 인터페이스에 정의된 getYourName 구현
          override fun getYourName(): String = "Your name is $LastName $FirstName"
      }

      println(yourName.getYourName())
  }

  interface Person {
      fun getYourName(): String
  }
  ~~~
  
* (위 코드 참고) 이번에는 Person 이라는 인터페이스를 하나 만들고 getYourName() 이라는 메소드를 정의한 후, Person 인터페이스를 상속하는 익명 클래스를 object expressions로 객체화해보았다. main 함수를 실행시키면 Your name is 당신은 천재!가 출력된다.

* 보통 안드로이드 앱 개발에서, 위와 같이 부모 클래스나 인터페이스를 상속하는 익명 클래스를 object expressions로 객체화하는 경우는 아래와 같다.

* <img width="650" alt="02" src="https://user-images.githubusercontent.com/31889335/125475693-b3260e6d-0d4f-4f2f-b50c-5703c9f40a2c.png">

* (위 코드 참고) XXOnXXXListener() 라는 메소드를 호출할 때 인자로 특정 인터페이스를 구현한 객체를 넘겨줘야 하는 경우가 많은데 보통 이럴 때 object expressions을 사용한다. (람다로 대체할 수도 있긴 하지만..) XXOnXXXListener() 메소드를 호출하는 코드라인에서 인자로 전달하려는 목적으로 단 한번만 구현하면 되기 때문이기도 하고, 해당 인터페이스를 구현한 클래스를 더 이상 프로젝트 다른 곳에서 재사용하지도 않기 때문에 익명 클래스를 만들어 구현하는 것! (예를 들어, 텍스트 뷰를 클릭한 결과로 실행되어야 하는 작업은 모든 텍스트 뷰에서 동일하지 않을 것이기 때문에.. 각각 다른 텍스트 뷰의 클릭 리스너를 호출하는 순간 순간마다 각기 다른 작업을 실행하는 코드를 매번 구현해야 하기 때문이다!)

# 끝 아님~

이어서 작성 중입니다 :)
