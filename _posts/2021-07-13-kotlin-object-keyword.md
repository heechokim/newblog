---
layout: post
title:  "[Kotlin] 코틀린의 object 키워드 잘 알고 사용하기(feat. companion object)"
categories: [kotlin]
---

> __참고 자료__  
> [Kotlin Document - Object expressions and declarations](https://kotlinlang.org/docs/object-declarations.html)  
> [Youtube Android Developers - Object - Kotlin Vocabulary](https://www.youtube.com/watch?v=KUk6k865Vgg)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. object expression VS object declaration](#1)  
> [2. object expression이 뭐예요?](#2) 
> [3. object expression 더 알아보기](#3)   
> [3. What is object declarations?](#3)



<br>

## ✍🏻 프롤로그<a id="0"></a>

코틀린으로 코드를 작성하다보면(저는 안드로이드 앱 개발을 코틀린으로 합니다) __object__ 라는 키워드를 사용하게 되는 몇 가지 경우가 있다!  
하지만 그동안 대충 구글링하여 짧은 순간 습득한 지식으로 이해한 후 사용했었는데..(으이구~) 좀 제대로 알고 사용하려는 목적으로 공부해본다..ㅎㅎ  
(아! 그리고 object 키워드에 대해서 공부하게 된 가장 큰 목적은 companion object를 제대로 이해해보고자 시작하게 된 것이다..ㅋㅋㅋ)

## ✅ object expression VS object declaration<a id="1"></a>

- `object` 키워드 사용법을 알려면 object expression 이랑 object declaration 라는 걸 구분할 수 있어야 함
- `object` 키워드를 이 두 가지 모두에서 사용하기 때문
- 용어 한글 번역
  - object expression = 객체 표현식
  - object declaration = 객체 선언
- object expression와 object declaration의 공통점
  - 개발을 하다 보면 A 클래스의 모습에서 아주 살짝만 수정된 모습인 B 클래스를 작성해야 하는 경우가 있을 수 있다. 이런 경우, B 클래스를 만들면 A 클래스와 거의 비슷한 클래스를 하나 더 작성하는 꼴이 되어 비효율적이다.
  - B 클래스를 새로 작성하지 않고 B 클래스의 객체를 생성할 수 있을까?
  - 코틀린은 딱 이런 상황에서 사용할 수 있도록 object expression와 object declaration라는 것을 제공한다.

## ✅ object expression이 뭐예요?<a id="2"></a>

- object expression(객체 표현식)는 `object` 키워드를 사용하여 익명 클래스의 객체를 생성할 때 사용   
- `익명 클래스` = 클래스를 작성할 때 클래스 이름이 명시적으로 작성되어 있지 않은 클래스. 즉, 이름이 없는 클래스

<br>

- 바로 아래 코드를 보면 무슨 말인지 한 방에 이해됨
- ~~~kotlin
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
- `object` 키워드 뒤에 나오는 코드를 보면 LastName, FirstName라는 멤버 변수가 선언되어 있고, toString()라는 멤버 메소드를 가지는 익명 클래스가 작성되어 있음
- 이런 식으로 `익명 클래스`를 작성하고 그 앞에 `object` 키워드를 붙여주면 그 시점에서 해당 익명 클래스의 __객체가 바로 생성됨__ 
- 객체가 바로 생성된다 = 해당 클래스의 생성자 함수 호출

<br>

- <img width="700" alt="01" src="https://user-images.githubusercontent.com/31889335/125470357-c2ffd2d5-b7a3-437b-b3ff-a7b465de7cf2.png">
- (위 그림 참고) `object` 키워드를 `익명 클래스` 앞에 붙여줌으로써 익명 클래스의 객체를 `object` 키워드 작성 시점에서 바로 생성함
- 생성한 객체가 yourName 변수에 할당됨
- 위 코드의 main 함수를 실행해보면 Your name is Kim Chohee가 출력됨

<br>

- 정리
  - object expression = `object` 키워드 뒤에 익명 클래스를 선언하면 그 시점에서 바로 객체화해주는 것
  - 이렇게 특정 코드 라인에서 익명 클래스를 작성하고 바로 객체로 만드는 경우는, 해당 익명 클래스를 프로젝트 내에서 여러번 재사용하지 않고 딱 한 번만 사용해야 하는 경우에 유용
  - obejct expression에서 사용되는 `object` 키워드는 한글 해석한 그대로 `객체` 라고 생각하기 = 키워드 작성 시점에서 바로 객체로 생성되므로!

<br>

- 생각
  - 왜 이름이 object expression 일까?
  - object expression = 객체 표현식 인 것을 떠올려보자
  - 프로그래밍 언어 분야에서 "표현식" = "수식" 임
  - 프로그래밍 언어에서 "수식" = 1+2와 같은 수식 / 함수 호출식 / 변수 이름 등 포함
  - 프로그래밍 언어에서 "수식" = 모양과 형태가 다를 수 있지만 모두 하나의 단일 값이 될 수 있는 것을 의미
  - 따라서 익명 클래스를 객체화하는 행위인 object expression도 익명 클래스를 객체화하여 하나의 단일 값으로 만들 수 있기 때문에 object "expression" 라는 이름이라고 추측해 봄

## ✅ object expression 더 알아보기<a id="3"></a>

- object expression은 기본적으로 [Any](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-any/)라는 코틀린 클래스를 상속함
- Any 클래스 말고도 다른 클래스나 다른 인터페이스를 상속한 익명 클래스도 object expression로 객체화 가능

- 아래 코드 봐보기
- ~~~kotlin
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
- 위 코드 처럼 Person 이라는 인터페이스를 하나 만듬
- Person 인터페이스를 상속하는 익명 클래스를 object expressions로 객체화 가능
- main 함수 실행시키면 Your name is 당신은 천재!가 출력됨

- 안드로이드 앱 개발에서 언제 object expression를 쓰면 좋을까?
  - <img width="650" alt="02" src="https://user-images.githubusercontent.com/31889335/125475693-b3260e6d-0d4f-4f2f-b50c-5703c9f40a2c.png">
  - XXOnXXXListener() 라는 메소드를 호출할 때 많이 사용함
  - XXOnXXXListener() 메소드는 인자로 특정 인터페이스를 구현한 클래스의 객체를 넘겨줘야 하는 경우가 많음
  - 위 코드에서는 View.OnClickListener라는 인터페이스를 구현한 클래스의 객체를 setOnClickListener() 메소드의 인자로 넘겨야 함
  - 보통 이럴 때 object expressions을 사용해서(람다로 대체할 수도 있긴 하지만..) 인자로 전달해야 하는 객체를 생성
  - 특정 인터페이스를 구현한 클래스를 더 이상 프로젝트 다른 곳에서 재사용하지 않기 때문에 익명 클래스를 만들어 구현하는 것!
  - 예를 들어, 텍스트 뷰를 클릭한 결과로 실행되어야 하는 작업은 모든 텍스트 뷰에서 동일하지 않을 것이다. 각각 다른 텍스트 뷰의 클릭 리스너를 호출하는 순간 순간마다 각기 다른 작업을 실행하는 코드를 매번 구현해야 하기 때문

## 3️⃣ What is object declarations?<a id="3"></a>

* 코틀린에서 지원하는 __object declarations는 소프트웨어 디자인 패턴 중 하나인 [싱글톤 패턴](https://en.wikipedia.org/wiki/Singleton_pattern)을 만드는 작업을 쉽게 할 수 있도록 도와주는 것__ 이다.

* 싱글톤 패턴을 간단하게만 설명하자면.. 객체 지향 언어에서는 필요한 클래스들을 만들어놓고 실제로 사용할 때는 클래스에서 정의한 것을 토대로 객체라는 것을 만들어서 메모리에 할당한다. (즉, 객체는 클래스의 인스턴스(instance)이다.) 이 때, 똑같은 클래스를 토대로 한 객체를 여러 개 생성한다면 각 객체는 모두 따로 따로 메모리 공간을 차지하게 된다.(즉, 생성된 객체 수 만큼 따로 따로 메모리에 할당된다는 것!) 하지만 싱글톤 패턴으로 클래스를 만들게 되면 해당 클래스의 객체는 여러 번 생성할 수 없고 단 한 번만 생성할 수 있다. 따라서 메모리에 한 번만 할당되게 되는 것이다.

* 코틀린에서는 싱글톤 패턴이 적용되는 클래스를 쉽게 정의할 수 있도록 지원하고 있는데 이 때 사용되는 키워드가 __object__ 이고, 이러한 행위를 __object declaration__ 이라고 부른다. 앞서 설명한 object expressions도 object 키워드를 사용하기 때문에 조금 헷갈릴 수 있지만, 서로 이름이 다른 것처럼 역할도 다르다! (object expression과 object declaration 구분하기!)

* ~~~kotlin
  // 코틀린에서 싱글톤 패턴이 적용된 클래스 만들기
  object MySingleTon {
      fun printName() {
          println("Kimchohee")
      }
  }
  ~~~
  
* (위 코드 참고) class 키워드를 사용하여 클래스를 정의하는 모습과 비슷하지만 class 키워드 대신 object라는 키워드를 붙여서 클래스를 생성해주면 싱글톤 패턴이 적용된 클래스가 생성된다!

* ~~~java
  // 자바에서 싱글톤 패턴이 적용된 클래스 만들기
  public class MySingleTon {
      private static MySingleTon INSTANCE;
      
      private MySingleTon() { }
      
      public static MySingleTon getInstance() {
          if (INSTANCE == null) {
              INSTANCE = new MySingleTon();
          }
          return INSTANCE;
      }
      
      public void printName() {
          System.out.println("Kimchohee");
      }
  }
  ~~~
  
* (위 코드 참고) 자바로 싱글톤 패턴을 만들기 위해서는 위와 같은 코드를 작성해야 한다. INSTANCE라는 static 변수를 만들어 놓고 null 체크를 하는 방식이다. 만약 INSTANCE가 null이 아니면(즉, 단 한 번이라도 객체가 생성된 적이 있으면) 기존의 객체를 반환해줌으로써 새로운 객체가 여러 번 생성되는 것을 막는 방식으로 싱글톤 패턴을 구현한다.(사실 자바에서 싱글톤 패턴을 구현하는 방법은 다양하다! 위 코드는 다양한 방법 중 null 체크를 통해 싱글톤 패턴을 구현하는 방법이다.)

* 하지만 코틀린에서는 object 키워드를 사용해서 클래스를 정의하기만 하면 바로 해당 클래스는 싱글톤 패턴이 적용되도록 코틀린 언어 내부적으로 구현되어 있다! 따라서 자바보다 코틀린에서 더 쉽게 싱글톤 패턴을 구현할 수 있는 것이다!!

* 딱 봐도 싱글톤 패턴을 만들 때 사용하는 object 키워드는 "수식"이 아니라 "(클래스)정의"를 하는 모습을 하고 있으므로 object declaration이라는 이름으로 부르는 것 같다. 

* ~~~kotlin
  // 코틀린에서 MySingleTon 클래스의 객체 사용하기
  fun main() {
      val mySingleTon = MySingleTon
      mySingleTon.printName()// Kimchohee 출력됨
  }
  
  // 자바에서 MySingleTon 클래스의 객체 사용하기
  public static void main(String[] args){
      MySingleTon mySingleTon = MySingleTon.getInstance();
      mySingleTon.printName(); // Kimchohee 출력됨
  }
  ~~~
  
* (위 코드 참고) 코틀린에서 싱글톤 패턴이 적용된 클래스의 객체를 생성하고 사용하기 위해서는 해당 클래스 이름 자체를 사용하면 된다.

* ~~~kotlin
  object MySingleTon: SuperClass() {
      override superExample() { 
          //...
      }

      fun printName() {
          println("Kimchohee")
      }
  }
  ~~~
  
* (위 코드 참고) object 키워드를 사용하여 싱글톤 패턴이 적용된 클래스를 만들 때는 부모 클래스 상속도 가능하다.


# 끝 아님~

계속 이어서 작성 중입니다 :)  
  asgdgasdf
git asdfsdafstest 중
