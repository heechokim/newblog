---
layout: post
title:  "[Kotlin] 4️⃣ 코틀린에서 문자열 처리하기"
date:   2020-09-02 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ 공백 포함된 하나의 문자열을 공백을 기준으로 분리하기

공백이 포함된 문자열을 공백을 구분자로 하여 분리해보자!

~~~kotlin
fun main() {
    val input = "kim cho hee".split(" ")

    for(i in (0 until input.size)) {
        println(input[i])
    }
}
~~~

위와 같은 코드를 실행하면 다음과 같이 공백을 기준으로 하여 kim, cho, hee 세 문자열로 분리된 것을 확인할 수 있다.

<img width="121" alt="02" src="https://user-images.githubusercontent.com/31889335/95299576-e7bf2300-08b8-11eb-90f2-29e651cf9d05.png">

* __[split()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/split.html)__ 함수

    <img width="307" alt="01" src="https://user-images.githubusercontent.com/31889335/95299336-81d29b80-08b8-11eb-8202-1090ad465c45.png">

    split() 함수는 코틀린 표준 라이브러리의 kotlin.text 라는 패키지 안에 속해있는 함수이다.

    이 함수는 파라미터 중 하나인 delimiters 를 구분자로 하여 문자열을 순차적으로 분리하고, 나누어진 결과를 String형으로 __배열__ 에 저장한다.

    따라서 위 코드에서는 input[0]에 "kim" 이 저장되고, input[1]에 "cho" 가 저장되며 input[2]에 "hee" 가 저장되는 것이다.

## 2️⃣ 코틀린의 string template 사용하기

> [kotlin 공식 문서 - string templates](https://kotlinlang.org/docs/reference/basic-types.html#string-templates
) 를 기반으로 작성한 내용.

코틀린에서는 literal string 자료에 사용할 수 있는 템플릿이 존재한다!

예를 들어 "Kimchohee JJang" 이라는 문자열의 Kimchohee 부분을 Kimganada로 바꾸고 싶을 때 코틀린 string 템플릿을 사용하면 쉽게 바꿀 수 있다.

~~~kotlin
fun main() {
    val name1 = "Kimchohee"
    val name2 = "Kimganada"
    println("$name1 JJang")
    println("$name2 JJang")
}
~~~

위 코드의 출력 결과는 다음과 같다.

템플릿 사용법은 위 코드에서 볼 수 있는 것처럼 기호 __$__ 표시를 사용하는 것이다.

<img width="133" alt="03" src="https://user-images.githubusercontent.com/31889335/95332802-e0af0980-08e6-11eb-91cd-7bb9a037e104.png">

$ 기호를 사용한 string 템플릿의 또 다른 예시는 다음과 같다.

~~~kotlin
fun main() {
    val tmp = "abcde"
    println("tmp's length is ${tmp.length}")
}
~~~

<img width="149" alt="04" src="https://user-images.githubusercontent.com/31889335/95333861-4e0f6a00-08e8-11eb-9d36-1f15b8accd75.png">

위 코드는 이전 코드와 다르게 코틀린 string 템플릿을 사용할 때 $ 기호 뒤에 {}를 붙여서 사용한다.

$ 기호 뒤에 {} 를 붙이고 붙이지 않고의 차이는 $ 뒤에 오는 변수 이름이 단순한 변수 이름인지 tmp.length 처럼 변수 이름 이상의 것인지에 따라 달라진다.

## 3️⃣ string 을 이루는 char을 순서대로 꺼내기

~~~kotlin
// 방법 1
fun main() {
    val str = "Kimchohee"
    for(char in str) {
        println(char)
    }
}
~~~

위 코드처럼 for문을 작성하면 string을 배열처럼 보고 string을 이루는 각각의 char형 문자 데이터를 하나씩 처음부터 반복한다.

위 코드의 출력 결과는 다음과 같다.

<img width="27" alt="05" src="https://user-images.githubusercontent.com/31889335/95998214-9c37e680-0e6f-11eb-9b57-37a2e8eae6e2.png">

~~~kotlin
// 방법 2
fun main() {
    val str = "Kimchohee"
    str.forEach {
        println(it)
    }
}
~~~

또 다른 방법은 위 코드처럼 forEach 문을 사용하는 것이다. forEach 문은 위에서 본 for(char in str) 과 똑같은 기능을 한다.