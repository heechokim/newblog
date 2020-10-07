---
layout: post
title:  "[Kotlin] 4️⃣ 코틀린에서 문자열 처리하기"
date:   2020-09-02 18:34:10 +0700
categories: [kotlin]
---

# [4️⃣ 코틀린에서 문자열 처리하기]

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

<br></br>

* __[split()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/split.html)__ 함수

    <img width="307" alt="01" src="https://user-images.githubusercontent.com/31889335/95299336-81d29b80-08b8-11eb-8202-1090ad465c45.png">

    split() 함수는 코틀린 표준 라이브러리의 kotlin.text 라는 패키지 안에 속해있는 함수이다.

    이 함수는 파라미터 중 하나인 delimiters 를 구분자로 하여 문자열을 순차적으로 분리하고, 나누어진 결과를 String형으로 __배열__ 에 저장한다.

    따라서 위 코드에서는 input[0]에 "kim" 이 저장되고, input[1]에 "cho" 가 저장되며 input[2]에 "hee" 가 저장되는 것이다.
