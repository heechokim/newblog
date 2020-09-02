---
layout: post
title:  "[Kotlin] 1️⃣ 인텔리제이를 사용하여 코틀린 시작하기"
date:   2020-09-01 18:34:10 +0700
categories: [kotlin]
---

# [1️⃣ 인텔리제이를 사용하여 코틀린 시작하기]

전에는 c++로 알고리즘 문제를 풀었지만 코틀린이라는 언어에 더 익숙해지기 위해 코틀린으로 알고리즘 문제를 풀기로 결심하였다.

코틀린으로 알고리즘 문제를 풀기 위해 사용할 IDE는 IntelliJ로 결정하였고, IntelliJ에서 뭘 어떻게 시작해야 하는지 알아보기 위해 [Kotlin 공식 사이트 - Getting Started with IntelliJ](https://kotlinlang.org/docs/tutorials/jvm-get-started.html) 문서를 읽어보았다!

<br>

## 1️⃣ 새 프로젝트 생성하기

[인텔리제이 설치](https://www.jetbrains.com/idea/download/) 를 한 후, 맨 처음으로 새 프로젝트를 생성해야 한다.

<img width="400" alt="01" src="https://user-images.githubusercontent.com/31889335/91739374-c64d8600-ebec-11ea-828d-1a353564173f.png">

그 다음, 

<img width="604" alt="02" src="https://user-images.githubusercontent.com/31889335/91742803-cef48b00-ebf1-11ea-8253-9489b1252041.png">

위 그림과 같이 왼쪽 항목에서 Kotlin을 선택한 후, 오른쪽 항목에서 JVM|IDEA 를 선택한다.

<img width="480" alt="03" src="https://user-images.githubusercontent.com/31889335/91742808-d2881200-ebf1-11ea-9c5e-6dae0318d6cb.png">

그 다음으로 위 그림과 같이 프로젝트를 설정할 원하는 경로를 설정하고 프로젝트 이름을 입력한 후 finish 버튼을 누른다.

<br>

## 2️⃣ 새 코틀린 파일 생성하기

새 프로젝트를 생성했으면 새 코틀린 파일을 만들어야 한다.

<img width="611" alt="04" src="https://user-images.githubusercontent.com/31889335/91743150-5641fe80-ebf2-11ea-9995-cca435438574.png">

위 그림과 같이 src 폴더에서 오른쪽 클릭한 후, New --> Kotlin File/Class 를 선택한다.

<img width="362" alt="05" src="https://user-images.githubusercontent.com/31889335/91743432-bf297680-ebf2-11ea-961c-0d66c745f544.png">

그 다음, File 항목을 선택하고 코틀린 파일의 이름을 입력한다.

<br>

## 3️⃣ 코틀린 코드 작성하기

새 코틀린 파일을 생성한 후에는 HelloWorld! 라는 문자열을 출력함으로써 코틀린 코드가 잘 컴파일 되는지 확인해보자!

<img width="287" alt="06" src="https://user-images.githubusercontent.com/31889335/91743624-0e6fa700-ebf3-11ea-8892-95640cb13bed.png">

만든 새로운 파일에 위와 같은 main 함수를 작성하고, 출력 함수인 println() 함수를 이용하여 Hello World!를 출력해보자!

인텔리제이의 한 가지 팁은 main 만 입력한 후, 탭 키를 누르면 자동으로 main 함수가 완성된다.

<img width="319" alt="07" src="https://user-images.githubusercontent.com/31889335/91743789-542c6f80-ebf3-11ea-93ae-19bf4af3beb0.png">

코드를 작성한 후, main 함수 앞에 보이는 초록색 화살표 버튼을 클릭하면 Run HelloWorldKt 이라는 항목이 나온다. 이것을 클릭해보자.

<img width="363" alt="08" src="https://user-images.githubusercontent.com/31889335/91743921-96ee4780-ebf3-11ea-8a31-554185d8943a.png">

이렇게 Hello World! 라는 문자열이 잘 출력되는 것을 확인할 수 있다!

<br>

# 끝!