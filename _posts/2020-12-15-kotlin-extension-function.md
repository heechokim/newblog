---
layout: post
title:  "[Kotlin] 코틀린의 확장 함수(Extension Function)"
date:   2020-12-15 18:34:10 +0700
categories: [kotlin]
---

> __💁🏼‍♀️ 프롤로그__
>
> 2019/07 ~ 2020/01 SOPT라는 IT 창업 대외 동아리 활동을 했었다.
>
> 세션에서 코틀린 확장함수에 대해 배우게 되었고, [인턴즈](https://github.com/INTENRZ/Android_INTERNZ) 라는 App을 개발할 때 사용했었다.
>
> 프로젝트들을 정리하면서 그 때 사용했던 '확장함수'에 대해 다시 직접 공부해봐야겠다는 생각이 들어 공부해보려고 한다!
>
> ✍🏻 [kotlin 도큐먼트 - Extenstions](https://kotlinlang.org/docs/reference/extensions.html) 를 참고하여 작성합니다.

## 0️⃣ 코틀린은 클래스를 확장할 수 있다?

코틀린은 클래스를 확장해서 새로운 기능을 개발할 수 있도록 지원해준다.

물론 코틀린이나 자바에서 '상속'을 통해 Super Class(부모 클래스)를 확장할 수도 있지만 코틀린에서 지원하는 클래스 확장 개념은 상속을 통한 확장과는 다른 방법으로 클래스를 확장한다.

코틀린에서 제공하는 클래스 확장은 다른 클래스로부터 상속을 받지 않아도 되고, 어떠한 디자인 패턴을 이용해서 확장하는 것도 아니다.

코틀린만의 특별한 방법이 있다.

## 1️⃣ 그래서, 클래스를 확장한다는게 뭔데! 왜 확장하는 거야?

'클래스를 확장한다'라는 개념을 쉽게 이해하기 위해 한 가지 예시를 살펴보자.

예를 들어서 앱 개발 시 __외부 라이브러리(third-party library)를 사용하는데 이 라이브러리는 어떠한 변경도 안되는 라이브러리__ 라고 가정해보자.

즉, 얼굴도 모르는 다른 개발자가 개발한 라이브러리를 사용하는 경우이다.

이럴 경우에, 코틀린에서 제공하는 클래스 확장 개념을 사용하면 __외부 라이브러리가 제공하는 자체 클래스는 변경할 수 없지만 이를 확장하여 개발자가 원하는 새로운 함수들을 만들 수 있게 된다.__

또 클래스를 확장하면서 만든 __새로운 함수들을 마치 외부 라이브러리의 클래스가 제공하는 원래 함수인 것 마냥__ 사용할 수도 있다.

이렇게 클래스를 확장하면서 만든 새로운 함수를 __확장 함수(extension function)__ 라고 부른다.

코틀린이 제공하는 클래스 확장 개념을 사용하면 확장 함수 뿐 아니라 __확장 프로퍼티(extension property)__ 라는 것도 만들 수 있다.

확장 프로퍼티는 어떤 것도 변경할 수 없는 클래스에 새로운 프로퍼티를 추가할 수 있게 해준다.

말 그대로, __코틀린에서 클래스를 확장한다__ 라는 개념은 어떠한 클래스가 존재하는데 이 클래스를 직접 수정할 수 없을 때 __기존 클래스는 그대로 두고 클래스 주변에 새로운 함수나 프로퍼티를 추가하여 클래스의 크기를 늘린다(확장한다)__ 라고 생각하면 된다.

<img width="566" alt="01" src="https://user-images.githubusercontent.com/31889335/102179222-20e3c700-3eea-11eb-9939-89c47ab447ec.png">

## 2️⃣ 확장 함수 만드는 방법

코틀린의 클래스 확장 개념을 통해 확장 함수를 선언하고 만들 수 있다는 것을 알게 되었다.

이제 확장 함수를 어떻게 만드는지 알아보자.

먼저 확장 함수를 만들기 위해 알아야 할 몇 가지 용어가 있다.

* __receiver type(수취인 타입)__ : 확장 함수를 추가할 클래스를 말한다. 즉, 확장 대상이 될 클래스이다.

* __receiver object(수취인 객체)__ : 확장 함수 내부를 구현할 때 __this__ 키워드를 사용하여 receiver type이 가지고 있는 public 인스턴스에 접근할 수 있다. 이렇게 접근한 객체를 receiver object 라고 부른다.

이제 확장 함수를 어떻게 정의하고 구현하는지 아래 코드를 통해 알아보자.

아래 코드는 MutableList\<Int> 라는 클래스를 확장하여 swap()이라는 확장 함수를 추가한 모습이다.

~~~kotlin
/* MutableList 안의 두 원소 위치를 바꿔주는 함수 */
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    // this는 MutableList를 가리킨다.
    val tmp = this[index1]
    this[index1] = this[index2]
    this[index2] = tmp
}
~~~

먼저 확장 함수를 정의할 때는 receiver type에 __.(점 연산자)__ 를 붙여서 새로운 함수를 만들어주면 된다.

정의한 함수를 구현할 때는 __this__ 키워드를 사용해서 receiver type에 속한 인스턴스에 접근할 수 있다.

위 코드에서 this는 MutableList\<Int> 클래스에 존재하는 리스트를 의미하게 된다.

확장 함수로 만든 swap() 함수를 호출하려먼 아래와 같이 하면 된다.

~~~kotlin
fun main() {
    val testList = mutableListOf<Int>(1, 2, 3)

    // swap() 함수 호출
    testList.swap(0, 2)
    print(testList)
}

/* MutableList 안의 두 원소 위치를 바꿔주는 함수 */
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1]
    this[index1] = this[index2]
    this[index2] = tmp
}
~~~

testList.swap(0, 2) 라는 코드를 보면 마치 MutableList\<Int> 에 swap() 함수가 원래부터 내장되어 있는 것 마냥 사용하고 있다.

위 코드의 출력 결과는

<img width="89" alt="02" src="https://user-images.githubusercontent.com/31889335/102185083-d8c9a200-3ef3-11eb-9600-dcd9f0041259.png">

위와 같다. Index 0 원소와 Index 2 원소가 잘 바뀌어 있다.

위 코드에서 확장의 대상이 된 클래스(receiver type)는 MutableList\<Int> 였지만 당연히 Generic 타입인 MutableList\<T>에 관해서도 확장할 수 있다.

~~~kotlin
// Generic 타입에 관해서도 확장이 가능하다.
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1]
    this[index1] = this[index2]
    this[index2] = tmp
}
~~~

위 코드처럼 확장하면 Int형 외의 다른 타입의 데이터를 저장하는 MutableList에도 swap() 확장 함수를 사용할 수 있다.

## 3️⃣ 확장 함수가 갖는 특징

코틀린의 클래스 확장 개념을 통해 만든 확장 함수는 몇 가지 특징을 갖는다.

1. __확장 함수는 정적 바인딩된다.__

    확장 함수는 __정적 바인딩__ 처리된다. 

    __정적 바인딩이란?__

    함수를 만들어 컴파일하면 함수의 코드가 메모리 어딘가에 저장되고, 함수를 호출하는 부분에는 해당 함수의 코드가 저장된 메모리 주소 값이 저장된다. (이 작업을 함수 바인딩이라고 한다.)

    따라서 프로세서는 함수 호출 부분에 저장된 메모리 주소를 보고 해당 메모리 주소에 있는 코드를 실행하게 된다.

    함수 바인딩에는 정적 바인딩과 동적 바인딩이 있다.
    
    * __정적 바인딩__ : 함수 호출 부분에 메모리 주소 값을 저장하는 작업이 __컴파일 시간__ 에 행해지기 때문에 컴파일 이후로 이 값이 변경되지 않는다.

    * __동적 바인딩__ : 함수 호출 부분에 메모리 주소 값을 저장하는 작업이 컴파일 시간에는 보류되고, __런타임__ 에 결정된다.

    이해를 돕기 위해 아래와 같은 코드를 봐보자.

    ~~~kotlin
    open class Shape

    // Rectangle = Shape 클래스를 상속하는 SubClass
    class Rectangle: Shape()

    // Shape 클래스의 확장 함수
    fun Shape.getName() = "Shape"

    // Rectangle 클래스의 확장 함수
    fun Rectangle.getName() = "Rectangle"

    fun printClassName(s: Shape) {
        println(s.getName())
    }    

    // 매개 변수의 타입이 Shape인 함수에 Rectangle 타입의 인스턴스를 전달함
    printClassName(Rectangle())
    ~~~

    위 코드에서 주의 깊게 보아야 할 것은 가장 마지막 줄의 코드이다.

    printClassName() 이라는 함수의 매개변수 타입은 Shape 인데, printClassName()을 호출할 때는 printClassName(Rectangle()) 이라고 호출하였다.

    즉, Rectangle 타입의 인스턴스를 전달한 것이다.

    하지만 확장 함수의 특징에 의해 확장 함수의 호출 부분에 저장되는 메모리 주소가 이미 컴파일 시간에 결정되어 버렸다.

    즉,

    ~~~kotlin
    // 확장 함수를 호출하는 부분
    fun printClassName(s: Shape) {
        println(s.getName())
    }
    ~~~

    위 코드가 컴파일될 때, 확장 함수의 호출 부분인 s.getName() 부분에는 이미 Shape 클래스의 확장 함수인 getName() 함수가 저장된 메모리 주소가 저장되어 있는 것이다.

    따라서 printClassName(Rectangle()) 이라고 호출해도 프로세서는 s.getName() 호출 부분에 저장되어 있는 메모리 주소(Shape 클래스의 확장 함수 위치)만을 알고 있기 때문에 그 위치에 있는 코드를 실행시키게 된다.

    따라서 위 코드의 실행 결과는 "Rectangle"이 아니라 "Shape" 이 출력되게 된다.

2. __확장 함수와 이름 및 매개 변수 타입, 매개 변수 개수가 완벽히 같은 함수가 확장 대상 클래스의 멤버 함수로 존재하면 확장 함수는 무시된다.__

    확장 대상 클래스에 이미 존재하는 멤버 함수와 이름도 똑같고, 매개변수도 똑같은 확장 함수를 만들면 확장 함수를 호출해도 멤버 함수가 호출된다.

    즉, 확장 함수는 무시된다.

    단, 이름은 같지만 매개 변수의 type이나 매개 변수 개수가 달라 확장 함수와 멤버 함수를 구분할 수 있다면 원하는 함수를 호출할 수 있다.

    아래 코드를 보고 이해해보자.

    ~~~kotlin
    fun main() {
        Test().solution('c')
        Test().solution(1)
        Test().solution2(3) // 확장 함수가 호출되지 않고, 멤버 함수가 호출된다.
    }

    class Test {
        fun solution(char: Char) {
            println("안녕")
        }

        fun solution2(int: Int) {
            println("반가워")
        }
    }

    // 확장 함수 - Test 클래스 안의 멤버 함수인 solution()과 이름은 같지만 매개 변수 타입이 달라 구분 가능한 함수
    fun Test.solution(int: Int) {
        println("잘가")
    }

    // 확장 함수 - Test 클래스 안의 멤버 함수인 solution2()와 이름도 같고, 매개 변수 타입도 같아서 구분이 불가능한 함수
    fun Test.solution2(int: Int) {
        println("또봐")
    }
    ~~~

    위 코드의 출력 결과는 아래와 같다.

    <img width="54" alt="03" src="https://user-images.githubusercontent.com/31889335/102218181-f19c7c80-3f20-11eb-8020-879714afae03.png">

    확장 함수 중 멤버 함수와 함수 이름과 매개 변수까지 똑같은 함수는 호출되지 않음을 알 수 있을 것이다.

    즉, __확장 함수는 멤버 함수를 오버로딩(overloading)할 수는 있다.__

# 끝!

코틀린이 지원하는 클래스 확장 개념에는 이 외에도 확장 프로퍼티, Nullable 확장 등 조금 더 공부해야할 것들이 존재한다.

이것들은 필요할 때 [kotlin 도큐먼트 - Extenstions](https://kotlinlang.org/docs/reference/extensions.html) 를 보고 추가로 공부한 후, 정리하자!