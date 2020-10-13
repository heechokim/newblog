---
layout: post
title:  "[Kotlin] 8️⃣ 코틀린에서 Stack 사용하기"
date:   2020-10-12 18:34:10 +0700
categories: [kotlin]
---

## 0️⃣ Stack 이란?

자료구조 Stack에 관련한 개념은 [자료구조에 관련된 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 에서 알아볼 수 있기 때문에 이 포스팅에서 따로 설명하지 않을 것이다.

또한 Stack을 활용하는데 사용되는 기본 함수들인 pop, push, top 등도 [c++ stl을 사용하여 stack 만지기 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-02/C++Stack) 에서 확인할 수 있으니 따로 설명하지 않을 것이다.

## 1️⃣ 코틀린에는 Stack을 구현해놓은 라이브러리가 없다?

이전에 c++로 알고리즘 문제를 풀 때는 stl이라는 c++ 표준 라이브러리에 stack 자료구조와 stack에 사용되는 함수들(push, pop 등)이 이미 구현되어 있었다. 

따라서 이 라이브러리를 include 하여 stack을 직접 구현하지 않고도 사용할 수 있었지만 코틀린 표준 라이브러리인 kotlin stdlib에는 Stack이 구현되어 있는 라이브러리가 존재하지 않는다..

그럼 Koltin에서는 어떻게 Stack을 사용할 수 있을까?

Kotlin에 존재하는 여러 자료구조들 중 Stack처럼 사용할 수 있을 법한 자료구조를 Stack 처럼 사용하면 된다.

이 때, 중요한 것은 Stack은 pop(), push() 함수의 시간복잡도가 O(1) 로 엄청 빠르다. 그렇기 때문에 Stack과 비슷한 성능을 내는 자료구조를 Stack처럼 사용해야 한다.

## 2️⃣ 방법 1 ) MutableList Stack 처럼 사용하기

__[첫 번째 방법](https://play.kotlinlang.org/byExample/01_introduction/06_Generics)은 MutableList Stack처럼 사용하는 것__ 이다.

MutableList에 대해서는 [Kotlin - 배열에 관련된 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-10/kotlinArray) 에서 MutableList 부분을 보면 된다!

~~~kotlin
fun main() {
    // stack
    var mutableList = mutableListOf<Int>()

    // push = add()
    mutableList.add(1)
    mutableList.add(2)
    mutableList.add(3)

    // pop = removeAt(가장 마지막 인덱스)
    mutableList.removeAt(mutableList.size-1)

    // top = 배열의 가장 마지막 원소
    print(mutableList[mutableList.size-1])

    // isEmpty or isNotEmpty
    println(mutableList.isEmpty())
    println(mutableList.isNotEmpty())

    // stack 크기
    println(mutableList.size)
}
~~~

## 3️⃣ 방법 2 ) ArrayList를 Stack 처럼 사용하기

MutableList와 거의 비슷한 자료구조인 ArrayList로도 위와 똑같은 방법으로 stack처럼 사용할 수 있다.

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

    // top = 배열의 가장 마지막 원소
    print(arrayList[arrayList.size-1])

    // isEmpty or isNotEmpty
    println(arrayList.isEmpty())
    println(arrayList.isNotEmpty())

    // stack 크기
    println(arrayList.size)
}
~~~

## 4️⃣ 방법 3 ) Java의 Stack 라이브러리 사용하기

코틀린은 자바에서 사용하는 라이브러리를 가져다 사용할 수 있다. 

자바에는 이미 Stack 자료구조가 구현되어 있고, push() / pop() 등의 함수도 구현되어 있기 때문에 가져다 사용하면 된다.

~~~kotlin
import java.util.*

fun main() {
    val javaStack = Stack<Int>()

    // push
    javaStack.push(1)
    javaStack.push(2)
    javaStack.push(3)

    // pop
    javaStack.pop()
    javaStack.pop()

    // top = peek() 이라는 함수로 스택의 가장 윗 원소를 pop 없이 구할 수 있음
    println(javaStack.peek())

    // size
    println(javaStack.size)
}
~~~

## 5️⃣ __Stack(java)__ vs __MutableList Stack__ vs __ArrayList Stack__ 걸리는 시간 비교

~~~kotlin
fun main() {
    // 1. 자바 Stack 사용시 push, pop 시간 측정
    var javaStack = Stack<Int>()
    println("Java Stack Push Time : " + measureNanoTime {
        for(i in 0..100000){
            javaStack.push(i)
        }
    })
    println("Java Stack Pop Time : " + measureNanoTime {
        for(i in 0..100000){
            javaStack.pop()
        }
    })

    // 2. 코틀린 MutableList 사용시 push, pop 시간 측정
    var mutableListStack = mutableListOf<Int>()
    println("Kotlin MutableList Push Time : " + measureNanoTime {
        for(i in 0..100000){
            mutableListStack.add(i)
        }
    })
    println("Kotlin MutableList Pop Time : " + measureNanoTime {
        for(i in 0..100000){
            mutableListStack.removeAt(mutableListStack.size-1)
        }
    })

    // 3. 코틀린 ArrayList 사용시 push, pop 시간 측정
    var arrayListStack = arrayListOf<Int>()
    println("Kotlin ArrayList Push Time : " + measureNanoTime {
        for(i in 0..100000){
            arrayListStack.add(i)
        }
    })
    println("Kotlin ArrayList Pop Time : " + measureNanoTime {
        for(i in 0..100000){
            arrayListStack.removeAt(arrayListStack.size-1)
        }
    })
}
~~~

위와 같은 코드로 총 3개의 자료구조를 사용해 stack을 만들고 10만개의 원소를 push, pop 할 때의 시간을 측정해보았다!

<img width="318" alt="01" src="https://user-images.githubusercontent.com/31889335/95884410-080a4880-0db7-11eb-9453-1a54cfb5e43d.png">

__놀랍게도 자바의 Stack이 가장 느렸고, ArrayList를 사용할 때가 가장 빨랐다!__

for문으로 돌리기도 했고, 10만개까지만 테스트 해봤기에 빠르기 순서가 정확하지 않을 수 있지만 여기서 중요한 것은 ArrayList와 MutableList를 Stack 처럼 사용할 수 있을 것 같다는 것이다!