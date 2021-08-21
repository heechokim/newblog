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
> [1. object expressions 와 object declarations 구분하기](#1)  
> [2. What is object expressions?](#2)  
> [3. What is object declarations?](#3)



<br>

## ✍🏻 프롤로그<a id="0"></a>

코틀린으로 코드를 작성하다보면(저는 안드로이드 앱 개발을 코틀린으로 합니다) __object__ 라는 키워드를 사용하게 되는 몇 가지 경우가 있다!  
하지만 그동안 대충 구글링하여 짧은 순간 습득한 지식으로 이해한 후 사용했었는데..(으이구~) 좀 제대로 알고 사용하려는 목적으로 공부해본다..ㅎㅎ  
(아! 그리고 object 키워드에 대해서 공부하게 된 가장 큰 목적은 companion object를 제대로 이해해보고자 시작하게 된 것이다..ㅋㅋㅋ)

## ✅ object expressions 와 object declarations 구분<a id="1"></a>

* [코틀린 공식 문서](https://kotlinlang.org/docs/object-declarations.html)을 읽어보니 object 키워드에 대한 설명을 \<Object expressions\> 와 \<Object declarations\> 두 가지로 나누어서 설명하고 있음
* 한글 번역해보면 \<Object expression\>은 \<객체 표현식\> 이고 \<Object declaration\>은 \<객체 선언\>
* __\<object expressions\>와 \<object declarations\>의 공통점__
  * 개발을 하다 보면.. A 클래스의 모습에서 아주 살짝만 수정된 모습인 B 클래스를 작성해야 하는 경우가 있을 수 있다. 이런 경우, B 클래스를 새로 생성하게 되면 거의 비슷한 클래스를 하나 더 작성하는 꼴이 되고, 비효율적이다. B 클래스를 새로 작성하지 않고 B 클래스의 객체를 생성할 수 있을까?
  * 코틀린은 딱 이런 상황에서 사용할 수 있도록 object expressions와 object declarations라는 것을 제공한다.

## ✅ What is object expressions?<a id="2"></a>

* object expression은 `object` 키워드를 사용하여 익명 클래스의 객체를 생성해주는 녀석이다.
* 익명 클래스 : 클래스를 작성할 때 클래스 이름이 명시적으로 작성되어 있지 않은 클래스. 즉, 이름이 없는 클래스
* 바로 아래 코드를 보면 무슨 말인지 한 방에 이해될 것이다.
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
* (위 코드 참고) object 키워드 뒤에 나오는 코드를 보면 멤버 변수가 LastName과 FirstName이고, 멤버 메소드로 toString() 를 가지는 익명 클래스가 작성되어 있음을 알 수 있음. 이런 식으로 익명 클래스를 작성해주고 그 앞에 object 키워드를 붙여주면 그 시점에서 해당 익명 클래스의 __객체가 바로 생성됨.__ 

* 즉, __object expressions라는 것은 익명 클래스를 선언한 시점에서 바로 객체화해주는 것!__ 이라고 쉽게 이해하면 될 듯?

* <img width="700" alt="01" src="https://user-images.githubusercontent.com/31889335/125470357-c2ffd2d5-b7a3-437b-b3ff-a7b465de7cf2.png">

* (위 그림 참고) 다시 한 번 보자. 위 코드는 object 키워드를 익명 클래스 앞에 붙여줌으로써 해당 익명 클래스의 객체를 바로 생성하고, 생성한 객체를 변수 yourName에 할당한 코드임. 위 코드의 main 함수를 실행해보면 Your name is Kim Chohee가 출력됨!

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
