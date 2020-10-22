---
layout: post
title:  "[Kotlin] 8️⃣ 코틀린에서 Queue 사용하기"
date:   2020-10-22 18:34:10 +0700
categories: [kotlin]
---

## 0️⃣ Queue 란?

자료구조 Queue에 관련한 개념은 [자료구조에 관련된 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 의 큐 부분에서 알아볼 수 있기 때문에 이 포스팅에서 따로 설명하지 않을 것이다.

또한 Queue을 활용하는데 사용되는 기본 함수들인 pop, push, front 등도 [c++ stl을 사용하여 queue 만지기 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-03/C++Queue) 에서 확인할 수 있으니 따로 설명하지 않을 것이다. 

다만 언어마다 Queue를 활용하는데 사용되는 기본 함수들의 이름은 조금 다를 수 있다!

## 1️⃣ 코틀린에는 자료구조 Queue를 구현해놓은 라이브러리가 없다?

이전에 c++로 알고리즘 문제를 풀 때는 stl이라는 c++ 표준 라이브러리에 자료구조 queue와 queue에 사용되는 함수들(push, pop 등)이 이미 구현되어 있었기 때문에 

이 라이브러리를 include 하여 queue를 직접 구현하지 않고도 사용할 수 있었다.

__하지만__ 코틀린 표준 라이브러리인 kotlin std-lib에는 Queue가 구현되어 있는 라이브러리가 존재하지 않는다..ㅠ

그럼 Koltin에서는 어떻게 Queue를 사용할 수 있을까? 코틀린에서 큐를 사용할 수 있는 방법은 두 가지가 있다.

먼저 Kotlin에 존재하는 여러 자료구조들 중 Queue를 대신하여 큐처럼 사용할 수 있을 법한 자료구조를 사용하면 된다.

이 때, 중요한 것은 Queue의 pop(), push() 함수의 시간복잡도가 O(1) 로 엄청 빠르다. 그렇기 때문에 어떤 자료구조의 맨 앞 데이터를 삭제하거나 맨 뒤 데이터를 삽입할 때 Queue와 비슷한 시간 복잡도를 갖는 자료구조를 Queue처럼 사용해야 한다.

## 2️⃣ 방법 1 ) ArrayList를 큐처럼 사용하기

__첫번째 방법은 ArrayList를 큐처럼 사용하는 것__ 이다.

ArrayList에 대한 설명은 [이전 포스팅 - 코틀린의 List들](https://choheeis.github.io/newblog//articles/2020-10/kotlinList) 에서 확인하자.

ArrayList의 index 0 번째 데이터를 큐의 맨 앞 원소라고 생각하고, ArrayList의 맨 뒤에 추가되는 데이터를 큐의 맨 뒤에 추가되는 원소라고 생각하면 된다.

~~~kotlin
fun main() {
    // 1. ArrayList를 큐처럼 사용하기
    var queue = arrayListOf<Int>()

    // 2. add() : 큐의 맨 뒤에 데이터 삽입하기
    queue.add(1)
    queue.add(2)
    queue.add(3)

    // 3. removeAt(0) : 큐의 맨 앞 데이터 삭제하기
    queue.removeAt(0)

    // 4. 큐의 맨 앞 원소 확인하기
    print(queue[0])
}
~~~

## 3️⃣ 방법 2 ) Java에 구현되어 있는 Queue 라이브러리 사용하기

코틀린은 JVM 위에서 돌아가는 언어이기 때문에 자바에 존재하는 라이브러리를 가져다 사용할 수 있는 언어이다.

자바에는 이미 Queue라는 자료구조가 구현되어 라이브러리화 되어 있고, 큐에서 사용되는 함수들도 이미 구현되어 있다.

다만 [여기](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/Queue.html) 를 읽어보면 Queue라는 것이 클래스 형태가 아니라 인터페이스 형태로 구현되어 있고, 이 인터페이스를 구현하고 있는 여러 클래스가 존재한다는 것을 알 수 있다.

<img width="952" alt="01" src="https://user-images.githubusercontent.com/31889335/96832453-bed28c80-1479-11eb-8f4a-2f645ece52be.png">

따라서 자바의 큐를 사용하려면 큐 인터페이스를 구현하고 있는 클래스들의 인스턴스를 선언함으로써 사용할 수 있다!

주로 LinkedList 라는 클래스를 선언하여 큐를 사용한다.

<img width="381" alt="02" src="https://user-images.githubusercontent.com/31889335/96834002-3dc8c480-147c-11eb-940c-bbce6ee16f9f.png">

자바의 큐 인터페이스에 구현되어 있는 기본 함수들은 위와 같다.

위 표를 보면 큐에 사용되는 메소드들이 삽입, 삭제, 확인하는 세 가지 기능으로 분류되고 각 기능에는 2개의 메소드들이 구현되어 있음을 알 수 있다.

각 메소드들의 차이점은 [여기](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/Queue.html) 에서 읽어볼 수 있다.

~~~kotlin
import java.util.*

fun main() {
    // 큐 선언하기
    var queue: Queue<Int> = LinkedList<Int>()

    // 맨 뒤에 데이터 삽입하기
    queue.add(1)
    queue.add(2)
    queue.add(3)

    // 맨 앞 데이터 삭제하기
    queue.poll()

    // 맨 앞 데이터 확인하기
    print(queue.peek())
}
~~~