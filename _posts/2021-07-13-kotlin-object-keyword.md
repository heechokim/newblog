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
> [4. object declaration가 뭐예요?](#4)  
> [5. object declaration 더 알아보기](#5)  
> [6. companion object가 뭐예요?](#6)  
> [7. companion object 더 알아보기](#7)



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
  - 코틀린은 딱 이런 상황에서 사용할 수 있도록 object expression와 object declaration라는 것을 지원함

## ✅ object expression이 뭐예요?<a id="2"></a>

- object expression(객체 표현식)는 __`object` 키워드를 사용하여 익명 클래스의 객체를 생성할 때 사용__
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

<br>

- Any 클래스 말고도 다른 클래스나 다른 인터페이스를 상속한 익명 클래스도 object expression로 객체화 가능
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
  - 위 코드처럼 Person 이라는 인터페이스를 하나 만듬 
  - Person 인터페이스를 상속하는 익명 클래스를 object expressions로 객체화 가능
  - main 함수 실행시키면 Your name is 당신은 천재!가 출력됨

<br>

- 안드로이드 앱 개발에서 언제 object expression를 쓰면 좋을까?
  - <img width="650" alt="02" src="https://user-images.githubusercontent.com/31889335/125475693-b3260e6d-0d4f-4f2f-b50c-5703c9f40a2c.png">
  - XXOnXXXListener() 라는 메소드를 호출할 때 많이 사용함
  - XXOnXXXListener() 메소드는 인자로 특정 인터페이스를 구현한 클래스의 객체를 넘겨줘야 하는 경우가 많음
  - 위 코드에서는 View.OnClickListener라는 인터페이스를 구현한 클래스의 객체를 setOnClickListener() 메소드의 인자로 넘겨야 함
  - 보통 이럴 때 object expressions을 사용해서(람다로 대체할 수도 있긴 하지만..) 인자로 전달해야 하는 객체를 생성
  - 특정 인터페이스를 구현한 클래스를 더 이상 프로젝트 다른 곳에서 재사용하지 않기 때문에 익명 클래스를 만들어 구현하는 것!
  - 예를 들어, 텍스트 뷰를 클릭한 결과로 실행되어야 하는 작업은 모든 텍스트 뷰에서 동일하지 않을 것이다. 각각 다른 텍스트 뷰의 클릭 리스너를 호출하는 순간 순간마다 각기 다른 작업을 실행하는 코드를 매번 구현해야 하기 때문

## ✅ object declaration가 뭐예요?<a id="4"></a>

- object declaration는 __소프트웨어 디자인 패턴 중 [싱글톤 패턴](https://en.wikipedia.org/wiki/Singleton_pattern)을 만드는 작업을 할 때 사용__
- 싱글톤 패턴
  - 객체 지향 언어에서는 필요한 `클래스` 들을 만들어놓고 실제로 사용할 때는 클래스를 토대로 만들어지는 `객체` 라는 것을 생성해서 메모리에 할당함
  - 이 때, 똑같은 클래스를 토대로 하여 만들어지는 객체를 이곳 저곳에서 여러번 생성한다면?
    - 여러 번 생성된 객체는 모두 따로 따로 메모리 공간을 차지하게 됨
    - 즉, 생성된 객체 수 만큼 따로 따로 메모리에 할당됨
  - 하지만 `싱글톤 패턴` 으로 클래스를 만들어 놓으면 해당 클래스를 토대로 생성되는 객체는 여러 번 생성할 수 없음
  - 따라서 딱 한 번만 객체로 생성되어 메모리에 할당됨
  - 싱글톤 패턴이 적용된 클래스의 객체를 여러 곳에서 사용하더라도 이 객체는 항상 같은 메모리 주소를 바라봄

<br>

- `싱글톤 패턴` 이 적용된 클래스 만드는 기존 방법(자바)
- ~~~java
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
  - 자바에서는 싱글톤 패턴이 적용된 클래스를 만들기 위해서 위와 같은 코드를 작성해야 했음
  - INSTANCE라는 static 변수를 만들어 놓고 이 변수의 값을 null 체크하는 방식
  - 만약 INSTANCE가 null이 아니면(즉, 단 한 번이라도 객체가 생성된 적이 있으면) 기존의 생성되어 있던 객체를 반환해줌으로써 새로운 객체가 여러 번 생성되는 것을 막는 방식
  - 하지만 이 방식은 thread-safe 하지 않았음
    - 쓰레드 A에서 위 코드의 if (INSTANCE == null) 코드줄을 실행하고 있다고 해보자.
    - 쓰레드 A가 이 코드줄을 실행할 시점에서 INSTANCE는 null 이였다고 가정
    - 이 때 갑자기 운영체제에 의해 쓰레드 B에게 제어권이 넘어가게 되었다고 하면 쓰레드 A는 다음 코드줄을 실행하지 못하고 실행을 멈추게 된다.
    - 즉, INSTANE = new MySingleTon(); 을 실행하지 못하고 이 줄에서 멈춤
    - 그런데 쓰레드 B에서도 MySingleTon 클래스의 코드 실행하는 작업이 실행된다면? 
    - ![image](https://user-images.githubusercontent.com/31889335/130348819-b55a69f1-b353-47d0-89b6-95c6859f5fa7.png)
    - 즉, 두 개의 쓰레드가 동시에 MySingleTon 클래스에 접근하게 될 경우임
    - 쓰레드 A가 멈춘 동안 쓰레드 B가 INSTANCE null 체크를 하는 순간에도 INSTANCE는 null이다.(쓰레드 A에서 new MySingleTon(); 이 실행되지 못했으므로)
    - 쓰레드 B에서 INSTANCE = new MySingleTon(); 이 실행되어 INSTANCE에 MySingleTon 클래스의 객체가 할당됨
    - 이 후 다시 쓰레드 A에게 제어권이 돌아오면 쓰레드 A는 실행 멈춤되었던 코드 줄을 실행하기 시작함
    - 쓰레드 A에 의해서 INSTANCE = new MySingleTon(); 이 또 실행되어 결국 두 개의 메모리 주소에 MySingleTon 클래스의 객체가 할당됨
    - 이는 싱글톤 패턴이 아님 (메모리에 한 번만 할당되어야 하기 때문)
    - thread-safe를 보장하기 위해 위 코드에 쓰레드 여러 개에서 더블 체크하는 경우를 방지하는 코드가 더 추가되게 됨
    - ![image](https://user-images.githubusercontent.com/31889335/130349171-b3964d08-3c1b-46c9-be82-69aa6bf1ef39.png)
    - 이 외에도 여러 가지 문제를 방지하기 위한 코드가 붙여져 싱글톤 패턴 적용된 클래스를 자바로 작성하는 방법이 복잡해짐
  - 자바에서 싱글톤 패턴 클래스 하나 작성하는데 긴 코드를 작성해야 했고, 싱글톤 패턴을 만들 때 마다 반복적으로 작성해야 하는 불편함 발생

<br>

- `싱글톤 패턴` 이 적용된 클래스 만드는 쉬운 방법(코틀린)
- ~~~kotlin
  // 코틀린에서 싱글톤 패턴이 적용된 클래스 만들기
  object MySingleTon {
      fun printName() {
          println("Kimchohee")
      }
  }
  ~~~
  - 코틀린에서는 `싱글톤 패턴` 이 적용된 클래스를 자바보다 더 쉽게 만들 수 있도록 지원해줌
  - `object` 키워드를 사용하면 되고, 이러한 행위를 object decalaration라고 부름
  - `class` 키워드를 사용하여 클래스를 정의하는 모습과 비슷하지만 class 키워드 대신 `object` 키워드를 붙여서 클래스를 생성해주면 싱글톤 패턴이 적용된 클래스가 생성됨
  - 코틀린 언어 내부적으로 이렇게 클래스 생성 시 thread-safe한 싱글톤 패턴이 되도록 처리 다 되어있음
  - 자바보다 훨씬 간단
  - 이렇게 클래스 작성하면 작성 시점에 바로 해당 클래스의 객체를 단 한 개 생성해놓음

<br>

- 정리
  - object declaration = 싱글톤 패턴 적용된 클래스 만들 때 사용하는 것
  - obejct declaration에서 사용되는 `object` 키워드는 한글 해석한 그대로 `객체` 라고 생각하기
    - òbject` 키워드를 붙여 생성한 클래스는 싱글톤 패턴이 적용된 클래스라 딱 한 번만 객체화 되기 때문
    - 또 클래스 작성 시점에 바로 해당 클래스의 객체를 단 한 개 생성하기 때문

<br>

- 생각
  - 왜 이름이 object declaration 일까?
  - declaration = 선언 인 것을 떠올려보자
  - 사용법이 클래스를 "선언"하는 모습과 비슷하기 때문에 object "declaration" 이 아닐까 추측

## ✅ object declaration 더 알아보기<a id="5"></a>

* `싱글톤 패턴` 이 적용된 클래스의 객체 사용법
* ~~~kotlin
  // 코틀린에서 MySingleTon 클래스의 객체 사용하기
  fun main() {
      val mySingleTon = MySingleTon
      mySingleTon.printName()// Kimchohee 출력됨
  }
  
  VS
  
  // 자바에서 MySingleTon 클래스의 객체 사용하기
  public static void main(String[] args){
      MySingleTon mySingleTon = MySingleTon.getInstance();
      mySingleTon.printName(); // Kimchohee 출력됨
  }
  ~~~
  - 코틀린에서 싱글톤 패턴이 적용된 클래스의 객체를 사용하기 위해서는 해당 클래스 이름 자체를 사용하면 됨

<br>

* `object` 키워드를 사용해 싱글톤 패턴이 적용된 클래스를 만들 때는 부모 클래스 상속도 가능
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

## ✅ companion object가 뭐예요?<a id="6"></a>

- companion object = object declaration을 일반 클래스 내부에서 바로 작성하는 것
- `companion` 라는 키워드를 object declaration 앞에 붙여줌
- companion 한글 번역 = "동반자"
- companion object = "동반자 객체"

<br>

- 바로 아래 코드 봐보자!
- ~~~kotlin
  class MyClass {
      // MySingleTon 이라는 이름 안 써도 됨
      companion object MySingleTon {
          fun printName() {
              println("kimchohee")
          }
      }
      
      ...
  }
  ~~~
  - MyClass라는 일반 클래스 내부에서 바로 싱글톤 패턴이 적용된 MySingleTon 클래스를 작성하는 모습
  - companion object의 이름은 생략 가능(이름 없어도 됨)
  - MyClass 안에 작성된 MySingleTon 객체는 작성 시점에서 바로 객체로 생성됨
  - 또 딱 한 번만 생성되어 메모리에 한 번만 할당됨

<br>

- companion object 안에 선언된 멤버 변수/메소드 호출법
- ~~~kotlin
  fun main() {
      // companion object 클래스의 이름이 있는 경우
      val myName = MyClass.MySingleTon.printName()
      
      // companion object 클래스의 이름이 없는 경우
      val myName = MyClass.printName()
  }
  ~~~
  - companion object 안에 선언된 변수/메소드 호출할 때는 위 코드와 같이 호출
  - 클래스 이름 자체가 companion object로 작성한 클래스의 객체 참조자 이름으로 사용됨 

## ✅ companion object 더 알아보기<a id="7"></a>

- companion object 사용시 주의점
  - companion object가 자바의 static처럼 보이지만 진정한 static은 아님
  - 자바의 static 변수/메소드
    - 싱글톤 패턴과 비슷한 개념
    - 어떤 클래스의 객체 생성 시 객체가 생성되는 횟수에 상관 없이 static으로 선언된 변수/메소드는 딱 한 번만 생성되어 메모리에 한 번만 할당됨
    - static 변수는 컴파일 시(런타임 때가 아님) 메모리에 할당됨
  - companion object로 선언한 클래스는 왜 자바의 static처럼 보일까?
    - `companion object` 로 선언한 클래스는 작성 시점에서 바로 객체화되고, 단 한 번만 메모리에 할당되기 때문
    - static 변수와 동일하다고 생각할 수 있음
    - 심지어 자바로 작성된 static 변수를 코틀린 언어으로 변환시키면 companion object 안에 static으로 선언했던 변수들이 선언되어 있는 모습으로 바뀜
  - 하지만 companion object와 자바의 static 변수/메소드는 다름

<br>

- 자바의 static 변수/메소드 예시
- ~~~java
  public class Example {
      // static 변수
      static int number = 0;
      
      // static 메소드
      public static Int getNumber() {
          return number;
      }
  }
  ~~~
  
<br>

- companion object의 실체
- ~~~kotlin
  class MyClass {
      companion object Counter {
          // 멤버 변수
          private var count: Int = 0
          
          // 멤버 메소드
          fun count(): Int {
              return count++
          }
      }
  }
  ~~~
  - MyClass라는 일반 클래스 내부에 Counter라는 이름의 companion object가 선언되어 있다고 가정

<br>

- 위 companion object를 자바로 변환하면 아래와 같음
- ~~~java
  public final class MyClass {
      private static int count;
      public static final MyClass.Counter Counter = new MyClass.Counter(); // Counter 클래스 생성

      // inner class로 작성되는 Counter 클래스
      public static final class Counter {
          // 첫 번째, 생성자 함수 = private
          private Counter() {}
          
          public final void count() {
              MyClass.count = MyClass.count + 1;
              return count;
          }
          
          // 두 번째 생성자 함수 = public
          public Counter(DefaultConstructorMarker d) {
              this(); // = Counter();
          }
      }
  }
  ~~~
    - companion object로 선언된 Counter 클래스는 자바에서는 사실 MyClass의 inner class로 작성됨
    - inner class = 클래스 내에 또 클래스가 작성되는 것
    - Counter 클래스의 생성자 함수가 private과 public 두 가지로 생성되어 있음
    - MyClass에서 Counter 클래스의 public 생성자 함수를 호출해 Counter 클래스의 객체를 생성하는 코드가 있음
    - Counter 클래스의 객체는 static final 변수에 할당됨(final 변수 = 값을 바꿀 수 없음)
    - 즉, Counter 클래스의 객체는 MyClass 클래스의 객체가 생성될 때 자동으로 생성되는 "객체"임

<br>

- companion object가 자바의 static 변수/메소드와 같지 않은 이유
- ~~~java
  public final class MyClass {
      private static int count;
      
      public static Int count() {
          count++;
          return count;
      }
  }
  ~~~
    - 위와 같이 변환되어야 자바의 static 변수/메소드랑 같다고 할 수 있음
    - static 메소드일 것 같았던 count() 메소드는 Counter 클래스의 멤버 메소드이기 때문에 static과 다름
    
<br>

- 그러나 companion object 작성 시 멤버 메소드에는 `@JvmStatic` 어노테이션을 붙이고, 멤버 변수에는 `@JvmField` 어노테이션을 붙이면 JVM이 companion object로 선언한 클래스의 멤버 변수/메소드를 진짜 자바의 static 변수/메소드처럼 변환해줌
- ~~~kotlin
  class A {
      companion object {
          @JvmStatic
          fun count() {
              ...
          }
          
          @JvmField
          val number = 0
      }
  }
  ~~~
  - 즉, companion object로 선언한 클래스의 멤버 변수/메소드를 inner class로 만들지 않고 static 변수/메소드와 같은 모습으로 변환함
  - 어노테이션 관련해서는 [여기](https://kotlinlang.org/docs/java-to-kotlin-interop.html#static-fields) 보기

# 끝!

마지막 수정일 : 2021/08/22
