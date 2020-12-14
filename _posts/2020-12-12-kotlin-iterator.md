---
layout: post
title:  "[Kotlin] 코틀린의 Iterator"
date:   2020-12-12 18:34:10 +0700
categories: [kotlin]
---

> __🙋🏻‍♀️ 프롤로그__
> 
> 자료구조 Linked List에 대해 공부한 후, [백준 - 1406 에디터](https://www.acmicpc.net/problem/1406) 문제를 풀어봤다.
>
> 그런데 계속 시간초과가 나서 다른 사람들 코드를 보았더니 Stack으로 풀거나 Iterator를 사용해야 시간 안에 풀린다는 것을 알게 되었다!
>
> 나도 한 번 Iterator를 사용해서 풀어보기 위해 코틀린의 Iterator에 대해 공부하게 되었다.

## 0️⃣ Collection에 대해서 먼저 알아오세요~!

Iterator 라는 것을 알아보기 위해서는 [코틀린의 Collection](https://choheeis.github.io/newblog//articles/2020-10/kotlinCollection) 에 대한 이해가 필요하다!

## 1️⃣ Iterator이 무엇인가?!

✍🏻 [kotlin 도큐먼트 - Iterators](https://choheeis.github.io/newblog//articles/2020-10/kotlinCollection) 를 참고하여 작성합니다.

Iterator은 Collection 개념에 속하는 자료구조들의 원소들을 순회할 때 공통적으로 사용할 수 있는 녀석이다.

다시 말해 Collection 개념에 속하는 자료구조들은 ArrayList, Map, Set 등 다양한데 이 자료구조들의 원소를 순회할 때 공통적으로 사용할 수 있는게 Iterator 라는 것이다!

따라서 Collection 개념에 속하는 자료구조들의 원소를 각각 하나 하나 탐색하거나 출력해야 할 때, 원소들의 값을 업데이트 해야 할 때 사용하면 유용하다.

Iterator는 [코틀린 Standard Library](https://choheeis.github.io/newblog//articles/2020-12/kotlin-stdlib) 에서 제공하고 있어서 필요할 때 가져다 사용하면 된다.

Iterator의 개념적인 부분을 알아보았으니 좀 더 코드적인(?) 부분을 알아보자!

## 2️⃣ Iterator 사용법

Collection 자료구조의 원소 순회에 사용할 수 있는 Iterator를 사용하려면 어떻게 해야할까?

먼저 Iterator가 어디에 구현되어 있는지 알아야 한다.

Iterator는 [Iterable 인터페이스](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-iterable/) 를 구현하고 있는 자료구조 클래스에 구현되어 있다.

[코틀린의 Collection](https://choheeis.github.io/newblog//articles/2020-10/kotlinCollection) 을 읽어보고 왔다면 위 말이 이해될 것이다.

<img width="652" alt="06" src="https://user-images.githubusercontent.com/31889335/102019594-c9ddd500-3db7-11eb-8c78-6da60c20b0b9.png">

위 다이얼로그를 보면 Iterable 인터페이스를 상속하는 인터페이스는 List 인터페이스와 Set 인터페이스이다.

따라서 Collection 개념에 속하는 자료구조 중 List 인터페이스와 Set 인터페이스를 구현하고 있는 자료구조는 대부분 Iterator를 구현하고 있을 것이다.

구현되어 있는 Iterator는 __[iterator()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-iterable/iterator.html)__ 라는 함수를 호출함으로써 사용할 수 있다.

iterator()를 호출하면 Iterator가 생성되고 해당 자료구조의 첫 번째 원소를 가리키게 된다.

<img width="651" alt="07" src="https://user-images.githubusercontent.com/31889335/102019845-05c56a00-3db9-11eb-94cd-18cac4f3be07.png">

이 상태에서 __[next()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-iterator/next.html)__ 라는 함수를 호출하면 Iterator가 가리키고 있던 원소의 값을 return 하고, Iterator는 그 다음 원소를 가리키게 된다.

<img width="614" alt="08" src="https://user-images.githubusercontent.com/31889335/102019878-3ad1bc80-3db9-11eb-9e80-149ea14eaa39.png">

next() 함수를 호출하기 전에 Iterator가 가리킬 다음 원소가 존재하는지 여부를 체크해야 한다면 __hasNext()__ 라는 함수를 사용하면 된다.

~~~kotlin
fun main() {
    // 1. 리스트 자료구조 생성
    val list = listOf<Int>(1, 2, 3)
    // 2. Iterator 생성
    val iterator = list.iterator()
    // 3. 각 원소 출력
    while(iterator.hasNext()) {
        println(iterator.next())
    }
}
~~~

<img width="28" alt="09" src="https://user-images.githubusercontent.com/31889335/102020446-a6695900-3dbc-11eb-969e-26165c502f3d.png">

위 코드의 출력 결과는 위와 같다.

이렇게 Iterator를 사용해서 Collection 개념에 속하는 자료구조들의 원소를 순회할수도 있지만 코틀린에서는 이와 똑같은 다른 방법이 존재한다.

바로 __for문__ 을 이용하는 것이다.

Collection 개념에 속하는 자료구조들에 for문을 사용하면 __암묵적으로 Iterator__ 를 사용하는 것과 같다.

따라서 아래 코드는 위의 iterator() 를 사용한 코드와 동일하게 동작한다.

~~~kotlin
fun main() {
    val list = listOf<Int>(1, 2, 3)
    for(i in list) {
        println(i)
    }
}
~~~

<img width="28" alt="09" src="https://user-images.githubusercontent.com/31889335/102020446-a6695900-3dbc-11eb-969e-26165c502f3d.png">

출력 결과는 위와 같다.

마지막으로 __forEach()__  함수를 사용하는 것도 Iterator를 사용하는 것과 동일하다.

forEach() 함수는 해당 자료구조의 원소를 처음부터 마지막 원소까지 자동으로 순회하고, forEach문에 작성된 코드를 각 원소에 적용시킨다.

~~~kotlin
fun main() {
    val list = listOf<Int>(1, 2, 3)
    list.forEach { 
        println(it)
    }
}
~~~

<img width="28" alt="09" src="https://user-images.githubusercontent.com/31889335/102020446-a6695900-3dbc-11eb-969e-26165c502f3d.png">

출력 결과는 위와 같다.

## 3️⃣ 심화된 Iterator인 ListIterator!

코틀린에는 Collection 개념에 속하는 자료구조 중 특별히 __List형 자료구조__ 에 사용할 수 있는 Iterator인 __[ListIterator](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list-iterator/)__ 이 존재한다.

> List형 자료구조에 대해서는 [이 블로그의 다른 포스팅 - 코틀린의 List형 자료구조](https://choheeis.github.io/newblog//articles/2020-10/kotlinList) 를 읽어보면 알 수 있을 것이다.

ListIterator는 List형 자료구조들이 구현하고 있는 인터페이스이기 때문에 List형 자료구조에서만 사용할 수 있다. 

ListIterator는 __listIterator()__ 함수 호출을 통해 생성할 수 있다.

ListIterator가 기존 Iterator와 다른 점에 대해 알아보자.

기존 Iterator는 next() 함수를 호출하면 첫 번째 원소부터 차례대로 그 다음 원소를 가리켰다. 즉, 한쪽으로만 순회하는 단방향이였지만, __ListIterator__ 는 __양쪽으로 순회가 가능한 양방향 Iterator__ 이다!

따라서 __hasPrevious()__ 함수로 이전 원소가 존재하는지 확인할 수 있고, next() 함수와 같은 기능을 하지만 Iterator가 가리키는 방향이 반대인 __previous()__ 함수가 존재한다.

또한 ListIterator는 __nextIndex()__ 와 __previousIndex()__ 라는 함수가 추가로 존재하여 index까지 확인할 수 있는 기능이 추가되었다.

기존의 Iterator는 인덱스를 확인할 수 없었다.

더불어 원칙은 listIterator() 함수를 호출해 ListIterator를 생성하면 가장 첫 번째 원소를 가리키게 되지만 __[listIterator(index)](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/list-iterator.html)__ 를 호출하여 ListIterator를 생성하면 원하는 index부터 가리키도록 생성할 수 있다.

~~~kotlin
fun main() {
    val list = listOf<Int>(1, 2, 3)
    val listIterator = list.listIterator()

    println("Iterator이 증가하는 방향으로 출력하기!")
    while(listIterator.hasNext()) {
        println(listIterator.next())
    }

    println("Iterator이 감소하는 방향으로 출력하기!")
    while (listIterator.hasPrevious()) {
        println(listIterator.previous())
    }
}
~~~

위 코드의 출력 결과는

<img width="263" alt="10" src="https://user-images.githubusercontent.com/31889335/102021008-3066f100-3dc0-11eb-9900-b4c3259d98b2.png">

이와 같다.

## 4️⃣ 심화된 Iterator인 MutableIterator!

[이 블로그의 다른 포스팅 - 코틀린의 Collection](https://choheeis.github.io/newblog//articles/2020-10/kotlinCollection) 을 읽어봤다면 mutable 특징을 가진 인터페이스에 대해서도 알게 되었을 것이다.

코틀린의 Iterator에는 Collection 개념에 속하는 자료구조 중 mutable 특징을 가진 인터페이스를 구현하는 자료구조에 특화된 __[MutableIterator](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-iterator/)__ 라는 것도 존재한다!

MutableIterator은 위에서 알아본 다른 Iterator들과 다른 특징이 하나 있다.

바로 __remove()__ 함수를 통해 해당 자료구조의 원소를 제거할 수도 있다는 점이다. (mutable 특징을 가진 인터페이스를 구현한 자료구조는 원소를 제거할 수 있기 때문!)

이 때, 알아두어야 할 것은 MutableIterator은 따로 생성하는 함수가 존재하는 것이 아니라 mutable한 자료구조의 iterator() 함수를 호출하면 자동으로 MutableIterator 이 호출되어 remove() 함수를 사용할 수 있게 된다는 것이다.

__remove() 함수__ 는 __next() 함수에 의해 가장 마지막으로 반환된 원소를 삭제__ 한다.

이를 아래 코드를 통해 알아보자.

~~~kotlin
fun main() {
    // 1. mutable한 리스트 생성
    val mutableList = mutableListOf<Int>()
    mutableList.add(1)
    mutableList.add(2)
    mutableList.add(3)

    // 2. Iterator 생성(첫 번째 원소 가리킴)
    val mutableIterator = mutableList.iterator()
    mutableIterator.next()
    mutableIterator.next()

    // 3. 가장 마지막으로 반환된 두 번째 원소를 삭제
    mutableIterator.remove()

    // 4. 새로운 Iterator를 생성하여 처음부터 순회하며 출력
    val newIterator = mutableList.iterator()
    while(newIterator.hasNext()) {
        println(newIterator.next())
    }
}
~~~

위 코드의 출력 결과는

<img width="26" alt="11" src="https://user-images.githubusercontent.com/31889335/102022897-24cdf700-3dcd-11eb-836e-423b44e0ae24.png">

위와 같다.

추가적으로 mutable한 자료 구조의 Iterator를 생성할 때 listIterator() 함수를 통해 ListIterator를 생성하면 remove() 함수 외에도 __add()__ 함수를 통해 새로운 원소를 추가할 수 있고, __set()__ 함수를 통해 기존의 원소 값을 변경할 수도 있게 된다.

__add()__ 함수는 Iterator가 add() 함수 호출 시 가리키고 있는 원소의 __이전 위치에 새로운 원소를 추가 하고 Iterator가 추가한 원소를 가리키게__ 한다.

__set()__ 함수는 Iterator가 set() 함수 호출 시 가리키고 있는 원소의 __데이터를 변경__ 한다.

~~~kotlin
fun main() {
    // 1. mutable한 리스트 생성 후 원소 추가
    val mutableList = mutableListOf<Int>()
    mutableList.add(1)
    mutableList.add(2)
    mutableList.add(3)

    // 2. listIterator 생성
    val mutableIterator = mutableList.listIterator()

    // 3. Iterator가 두 번째 원소를 가리키도록 하기
    mutableIterator.next()

    // 4. 두 번째 원소 앞에 새로운 원소 추가하고 Iterator가 추가한 원소 가리키게 하기
    mutableIterator.add(4)
    mutableIterator.next()
    mutableIterator.set(10)

    val newIterator = mutableList.iterator()
    while(newIterator.hasNext()) {
        println(newIterator.next())
    }
}
~~~

위 코드의 출력 결과는

<img width="32" alt="12" src="https://user-images.githubusercontent.com/31889335/102023201-1f71ac00-3dcf-11eb-9d32-a01bd1362710.png">

위와 같다.

## 5️⃣ Iterator가 존재하는 이유?

[이 블로그의 다른 포스팅 - 코틀린의 List형 자료구조](https://choheeis.github.io/newblog//articles/2020-10/kotlinList)를 읽어보고 오자.

읽어보고 왔다면 코틀린의 List 인터페이스를 구현하는 자료구조는 index를 통한 접근이 가능하다는 것을 알게 되었을 것이다.

따라서 사실 위에서 본 코드에 사용된 list, mutableList 자료구조들은 모두 List 인터페이스를 구현하고 있기 때문에 index를 통해 접근이 가능하다.

다음 코드가 가능하다는 것이다.

~~~kotlin
fun main() {
    val mutableList = mutableListOf<Int>()
    mutableList.add(1)
    mutableList.add(2)
    mutableList.add(3)

    // 인덱스를 통한 접근 및 원소 변경 가능
    println(mutableList[2])
    mutableList[2] = 10
    println(mutableList[2])
}
~~~

<img width="32" alt="13" src="https://user-images.githubusercontent.com/31889335/102023599-16cda580-3dd0-11eb-853a-04e97c4d35f4.png">

위 코드의 출력 결과는 위와 같다.

하지만 [이 블로그의 다른 포스팅 - 2. LinkedList](https://choheeis.github.io/newblog//articles/2020-12/data-structure-linked-list) 에서 알 수 있듯이, List형 자료구조의 원소에 인덱스로 접근하게 되면 무조건 맨 첫 번째 원소부터 차례대로 방문해서 찾기 때문에 __O(N)__ 의 시간복잡도가 걸리게 된다.

그러나 인덱스를 통한 접근 대신 __Iterator__ 를 통한 접근을 하면 next(), previous() 함수를 통해 이전 원소 및 다음 원소에 바로 접근할 수 있게 된다.

즉, next(), previous() 함수는 맨 처음 원소부터 방문하지 않는다는 의미이다.

따라서 인덱스를 통한 접근보다 빠른 시간 내에 원소를 순회할 수 있게 된다!

# 끝!