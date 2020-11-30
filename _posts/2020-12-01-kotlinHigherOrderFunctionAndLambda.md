---
layout: post
title:  "[Kotlin] 코틀린의 고차 함수와 람다, 익명 함수에 대해서"
date:   2020-12-01 18:34:10 +0700
categories: [kotlin]
---

코틀린의 고차 함수(Higher-Order Function)와 람다(Lambda)와 익명 함수(Anonymous Function)에 대해 알아보기 전에 __코틀린 함수가 일급 함수임에 대해 이해해야 한다.__

## 0️⃣ 코틀린의 함수는 일급 함수다!

✍🏻 [kotlin 도큐먼트 - Higher-Order Functions and Lambdas](https://en.wikipedia.org/wiki/First-class_function) 를 참고하여 작성합니다.

코틀린 함수는 __일급 함수(first-class function)__ 이다.

일급 함수라는 것에 대해 알아보기 위해 [위키피디아 - First Class Function](https://en.wikipedia.org/wiki/First-class_function) 을 읽어보자.

Computer Science 분야에서는 어떠한 프로그래밍 언어의 함수가 __first-class citizen(일급 시민)__ 으로 다뤄진다면 해당 프로그래밍 언어가 __first-class function(일급 함수)__ 를 가지고 있는 언어라고 부른다.

정리해보면 __어떤 프로그래밍 언어의 함수가 일급 시민(first-class citizen) 이라면 그 언어의 함수는 일급 함수(first-class function)라고 부른다__ 는 것이다!

그럼 이번에는 일급 함수의 정의에서 사용된 __first-class citizen(일급 시민)__ 이라는 것이 무엇인지 [위키피디아 - First Class Citizen](https://en.wikipedia.org/wiki/First-class_citizen) 을 읽어보고 알아보자.

일급 시민은 __일급 객체(first-class Object)__ 라고도 통용된다. 이 포스팅에서는 앞으로 일급 시민을 일급 객체라고 표현할 것이다.

컴퓨터 프로그래밍 언어 디자인 분야에서 일급 객체란 말만 들으면 퀄리티가 높아 다른 것들과 차별성을 띄는 객체일 것 같지만 그게 아니다! __일급 객체란 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체__ 를 말한다.

'다른 객체들에 적용 가능한 연산을 모두 지원한다' 라는 말이 잘 이해되지 않는다면 먼저 아래에서 볼 수 있는 __일급 객체가 갖는 특징__ 에 대해 알아보면 이해될 것이다.

* __일급 객체는 함수의 매개변수가 될 수 있다.__

* __일급 객체는 함수의 return 값이 될 수 있다.__

* __일급 객체는 할당 명령문(=, 대입)의 대상이 될 수 있다.__

* __일급 객체는 동일 비교(==, equal)의 대상이 될 수 있다.__

즉, 어떤 객체가 함수의 매개변수에 사용될 수 있고, 함수의 return 값이 될 수 있고, 할당 명령문의 대상이 될 수 있고, 동일 비교의 대상이 될 수 있다면 해당 객체를 일급 객체라고 명칭하는 것이다.

이 특징들을 살펴보고 나니 그동안 자연스럽게 써오던 변수들이 대부분 일급 객체의 특징을 가지고 있다는 것을 깨닫게 될 것이다.

하지만 C언어의 배열과 문자열은 매개변수로 넘길 때 포인터 형태로 넘어가야 하기 때문에 일급 객체로 볼 수 없긴 하다.

이렇게 일급 객체가 갖는 특징을 알아보니 위에서 잘 이해되지 않았던 '다른 객체들에 적용 가능한 연산을 모두 지원하는 객체' 라는 말이 이해될 것이다.

지금까지는 __"일급 객체"__ 에 대해 알아본 것이고 다시 돌아가서 __"일급 함수"__ 의 정의를 이해해보자.

위에서 __어떤 프로그래밍 언어의 함수가 일급 객체(first-class object) 이라면 그 언어의 함수는 일급 함수(first-class function)라고 부른다__ 고 했다.

코틀린의 함수는 일급 함수라고 했으니 코틀린의 함수가 일급 객체라는 것임을 알 수 있다.

그렇다면 위에서 알아보았던 일급 객체의 4가지 특징의 주어를 "코틀린의 함수"로 바꿔보자!

* __코틀린의 함수는 함수의 매개변수가 될 수 있다.__

* __코틀린의 함수는 함수의 return 값이 될 수 있다.__

* __코틀린의 함수는 할당 명령문(=, 대입)의 대상이 될 수 있다.__

* __코틀린의 함수는 동일 비교(==, equal)의 대상이 될 수 있다.__

이 4가지 특징이 바로 코틀린의 함수가 가지고 있는 특징이며 코틀린 함수가 왜 일급 함수라고 불리는지 이해하기 위한 가장 좋은 사실들이다! 

또 이러한 특징을 갖는 일급 함수는 __함수형 프로그래밍(Functional Programming)__ 스타일에 필수적으로 존재해야 하는 요소이다.

## 1️⃣ 고차 함수(Higher-Order Function)란?

위에서 알아본 일급 함수의 특징을 잘 이해하고 있다면 고차 함수에 대해 이해하는 건 식은 죽 먹기다!

__고차 함수__ 란 Computer Science에서 통용되는 용어 중 하나로, __어떤 함수가 다른 함수의 매개변수가 되거나(OR) 다른 함수의 return 값이 된다면 고차 함수__ 라고 부른다.

즉, 어떤 함수가 일급 함수의 아래 두 가지 특징 중

* __어떤 함수는 다른 함수의 매개변수가 될 수 있다.__

* __어떤 함수는 다른 함수의 return 값이 될 수 있다.__

하나를 만족시킨다면 해당 함수를 고차 함수라고 부른다는 것이다!

아래 코드는 고차 함수의 예시를 코틀린으로 작성해본 코드이다.

✍🏻 [Android Developer 도큐먼트 - 고차 함수](https://developer.android.com/kotlin/learn?hl=ko#higher-order) 를 참고하여 가져온 예시입니다.

~~~kotlin
fun stringMapper(str: String, mapper: (String) -> Int): Int {
    // Invoke function
    return mapper(str)
}
~~~

위 stringMapper() 함수는 총 두 개의 매개변수를 넘기는데 첫 번째 매개변수는 str 이라는 이름의 String 형 변수이고, 두 번째 매개변수는 mapper 라는 이름의 함수이다.

stringMapper() 함수는 고차 함수가 될 수 있는 조건 중 __어떤 함수는 다른 함수의 매개변수가 될 수 있다.__ 라는 조건을 만족하기 때문에 고차 함수의 예시로 적합하다.

그럼 다른 곳에서 이 stringMapper() 를 호출하려면 어떻게 호출해야 할까?

~~~kotlin
stringMapper("Android", { input ->
    input.length
})
~~~

위와 같이 첫 번째 매개 변수에는 "Android" 라는 String 형 데이터를 넘기고, 두 번째 매개 변수에는 함수를 구현하여 넘기면 된다.

## 2️⃣ 람다(Lambda) 표현식이란?

✍🏻 [kotlin 도큐먼트 - 람다 표현식](https://kotlinlang.org/docs/reference/lambdas.html#lambda-expressions-and-anonymous-functions) 을 참고하여 작성합니다.

고차 함수에 대해서 알아보았다면 이제 람다표현식(Lambda Expression) 이 무엇인지 알아보자.

__람다 표현식이란 이름 없는 함수를 정의할 때 사용되는 표현식이다.__

밑에서 추가로 배울 익명 함수도 람다 표현식과 함께 이름 없는 함수를 정의할 때 사용되는 표현식임을 알아두자.

~~~kotlin
max(strings, { a, b -> a.length < b.length })
~~~

위 코드에서 볼 수 있는 map() 이라는 함수는 고차 함수이다. (위에서 고차 함수에 대해 이해했다면 바로 인정할 것이다!)

map() 함수의 두 번째 매개변수를 통해 함수가 넘겨지는데 이 함수의 모양새가 조금 익숙하지 않을 수 있다.

두 번째 매개변수로 넘겨지는 함수의 모양새만 따로 자세히 봐보자!

~~~kotlin
{ a, b -> a.length < b.length }
~~~

사실 위와 같은 모양새의 함수는 아래의 compare 이라는 함수와 동일한 함수이다.

~~~kotlin
fun compare(a: String, b: String): Boolean {
    return a.length < b.length
}
~~~

즉 여기서 하나 깨달을 수 있는 것이 있다.

위에서 람다 표현식과 익명 함수는 이름 없는 함수를 정의하기 위해 사용되는 표현식이라고 했는데 compare 이라는 이름이 있는 함수를 이름이 없는 

~~~kotlin
{ a, b -> a.length < b.length }
~~~

로 표현했다는 것이 된다. 즉, 이 함수 표현식은 람다 표현식이거나 익명 함수이거나 둘 중 하나일 것이다!

결론부터 말하자면 위 표현식은 __람다 표현식__ 이다.

람다 표현식을 사용해서 이름이 있는 함수를 굳이 만들지 않고도 간편하고 빠르게 이름 없는 함수를 구현할 수 있다.

위에서 봤었던

~~~kotlin
max(strings, { a, b -> a.length < b.length })
~~~

이 max() 함수도 두 번째 인자로 이름 없는 함수를 람다 표현식을 사용해 간편하게 구현하여 전달한 것이다.

람다 표현식은 항상 __{ }__ 중괄호로 둘러싸져 있으며 __->__ 기호 뒤에서부터가 함수의 body 부분이다.

__->__ 기호 앞에는 함수에 전달될 매개 변수를 표시해야 하고, 두 개 이상의 매개 변수를 전달할 때는 __,(콤마)__ 를 통해 구분해야 한다.

매개 변수를 표시할 때 매개 변수의 데이터 타입은 명시해도 되고 명시하지 않아도 된다.

만약 위 예시에서 매개 변수의 데이터 타입을 명시한다면

~~~kotlin
{ a: String, b: String -> a.length < b.length }
~~~

으로 표현된다.

코틀린에서 지원하는 람다 표현식 Convention이 하나 있는데 [kotlin 도큐먼트 - Passing trailing lambdas(후행 람다 전달)](https://kotlinlang.org/docs/reference/lambdas.html#passing-a-lambda-to-the-last-parameter) 문서를 보면 알 수 있을 것이다.

만약 

~~~kotlin
max(strings, { a, b -> a.length < b.length })
~~~

이 max() 함수처럼 어떤 함수가 매개 변수 중 가장 마지막 매개 변수에 함수가 전달되는 고차함수라면 위 함수 코드를 아래와 같이 바꿀 수 있다는 Convention이 존재한다.

~~~kotlin
max(strings) { a, b -> a.length < b.length }
~~~

또는 

~~~kotlin
max(strings) { a, b -> 
    a.length < b.length
}
~~~

위와 같이 개행을 통해 조금 더 보기 편하게 바꾸기도 한다.

또한 어떤 함수가 단 하나의 매개 변수만 전달하는데 그 매개 변수로 함수가 전달된다면 다음과 같이 함수의 __()__ 괄호 표시를 아예 생략할 수도 있다.

~~~kotlin
// () 표시가 생략 안 된 경우
run ({ println("...") })

// () 표시가 생략된 경우
run { println("...") }
~~~

## 3️⃣ 익명 함수(Anonymous Function) 이란?

✍🏻 [kotlin 도큐먼트 - 익명 함수](https://kotlinlang.org/docs/reference/lambdas.html#anonymous-functions) 를 참고하여 작성합니다.

익명 함수는 위에서 배운 람다 표현식과 아예 다른 개념이 전혀 아니다!

람다 표현식에 몇 가지 코드가 추가된 표현식이 익명 함수이다.

위에서 람다 표현식은

~~~kotlin
{ a, b -> a.length < b.length }
~~~

위와 같은 모습으로 표현되는 것이라고 알아보았다.

이 람다 표현식에 대해서 조금 더 깊게 생각해보면 __람다 표현식은 함수의 return 타입을 명시하지 않고 있다__ 는 것을 깨닫게 될 것이다.

하지만 어떤 경우는 굳이 굳이 람다 표현식을 사용하면서 동시에 함수의 return 타입을 명시해줘야 하는 경우가 있을 수도 있다.

이런 경우에는 

~~~kotlin
fun(a: String, b: String): Boolean {
    return a.length < b.length
}
~~~

위와 같이 함수 이름은 없지만 fun 키워드와 return 타입까지 사용해서 명시해줄 수 있다.

이와 같은 표현식을 __익명 함수__ 라고 한다.

## 4️⃣ 추가적으로 알아두면 좋은 것들

일급 함수인 코틀린의 함수가 가진 4가지 특징에 대해 알아보았을 때

__코틀린의 함수는 할당 명령문(=, 대입)의 대상이 될 수 있다.__ 라는 특징이 존재했었다.

따라서 아래와 같은 코드가 성립한다는 것인데

~~~kotlin
// getLength()라는 함수가 존재한다고 가정
val testFun = getLength("Kimchohee")
~~~

만약 위의 getLength() 함수가 이름이 없는 람다 표현식이라면 어떤 코드가 작성될까?

~~~kotlin
val testFun = { name -> name.length }
~~~

라는 모습의 코드로 작성될 것이다!

이 때, testFun 이라는 이름의 변수의 데이터 타입까지 명시하고 싶다면 어떻게 명시해야 할까?

~~~kotlin
val testFun: (String) -> Int = { name -> name.length }
~~~

라고 명시하면 된다.

데이터 타입을 __(String) -> Int__ 라고 명시한 것인데 람다 표현식의 매개 변수에 해당하는 데이터 타입은 괄호()를 사용하여 표현하고, return 타입에 해당하는 데이터 타입은 괄호()를 사용하지 않는다.

# 끝!