---
layout: post
title:  "[Kotlin] 3️⃣ 코틀린의 입출력 함수들"
date:   2020-09-02 18:34:10 +0700
categories: [kotlin]
---

# [3️⃣ 코틀린의 입출력 함수들]

<br>

## 1️⃣ 출력 함수들

* __[println()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/println.html)__ 함수

    코틀린의 가장 대표적인 출력 함수는 println() 이다.

    <img width="233" alt="01" src="https://user-images.githubusercontent.com/31889335/91863600-41c13d00-ecaa-11ea-96c5-89c897a9220a.png">

    println() 함수는 코틀린의 표준 라이브러리 중 kotlin.io 라는 패키지에 속해있는 함수이다. 

    println() 함수는 기본적으로 standard output stream에 저장된 출력 정보 + 줄바꿈 기호를 포함한 상태로 출력한다.

    따라서 println() 함수로 출력한 결과에는 항상 개행이 따라오게 된다!

<br>

## 2️⃣ 입력 함수들

* __[readLine()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/read-line.html)__ 함수

    <img width="257" alt="02" src="https://user-images.githubusercontent.com/31889335/91864605-5baf4f80-ecab-11ea-8757-158ee1df4515.png">

    readLine() 함수는 코틀린의 표준 라이브러리 중 kotlin.io 라는 패키지에 속해있는 함수이다.

    readLine() 함수는 standard input stream 으로부터 한 줄을 읽는다.

    여기서 '한 줄'이라는 말은 콘솔창에서 줄 개행을 기준으로 한 줄씩 읽는다는 의미이다.

    이 함수는 반환값이 null 또는 String이다. 즉, null이 아닌 경우에는 모든 입력을 문자열로 반환한다는 것이다.

    null이 반환되는 경우는 입력의 대상이 파일일 때 파일의 맨 끝에 도달해 더 이상 읽을 데이터가 없을 경우이다.

<br>