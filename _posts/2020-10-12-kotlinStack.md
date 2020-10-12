---
layout: post
title:  "[Kotlin] 6️⃣ 코틀린에서 Stack 사용하기"
date:   2020-10-12 18:34:10 +0700
categories: [kotlin]
---

## 0️⃣ Stack 이란?

자료구조 Stack에 관련한 개념은 [자료구조에 관련된 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 에서 알아볼 수 있기 때문에 이 포스티에서 따로 설명하지 않는다.

또한 Stack을 활용하는데 사용되는 기본 함수들도 [c++ stl을 사용하여 stack 만지기 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-02/C++Stack) 에서 확인할 수 있다.

## 1️⃣ ArrayList를 Stack처럼 사용하기

이전에 c++로 알고리즘 문제를 풀 때는 stl이라는 c++ 표준 라이브러리에 stack 자료구조와 stack에 사용되는 함수들(push, pop 등)이 이미 구현되어 있었다. 

그래서 구현되어 있는 것을 가져다 사용했었는데 Koltin에서는 어떻게 Stack 문제를 풀어야할까?

__첫 번째 방법은 ArrayList를 Stack처럼 사용하는 것__ 이다.

ArrayList에 대해서는 [Kotlin - Array에 관련된 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-10/kotlinArray) 에서 ArrayList 부분을 보면 된다!

~~~kotlin
fun main() {
    // stack
    var arrayList = arrayListOf<Int>()

    // push = add()
    arrayList.add(1)
    arrayList.add(2)
    arrayList.add(3)

    // pop = removeAt(가장 마지막 인덱스)
    arrayList.removeAt(arrayList.size-1)

    // top
    print(arrayList[arrayList.size-1])
    
    // isEmpty or isNotEmpty
    println(arrayList.isEmpty())
    println(arrayList.isNotEmpty())
}
~~~

> 두 번째 방법은 자바의 Stack Collection 사용하기



