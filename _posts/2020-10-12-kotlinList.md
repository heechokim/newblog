---
layout: post
title:  "[Kotlin] 7️⃣ 코틀린의 List들"
date:   2020-10-12 18:34:10 +0700
categories: [kotlin]
---

## 0️⃣ 코틀린에서 List을 사용하려면?

코틀린에서 List은 kotlin 표준 라이브러리(kotlin stdlib) 안의 __kotlin.collections__ 라는 패키지 안에 구현되어 있다.

따라서 코틀린에서 List을 사용하려면 kotlin 표준 라이브러리에 있는 것들을 가져다 사용하면 된다!

[kotlin collection에 관련된 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-10/kotlinCollection) 을 보면 kotlin.collections 패키지 안에는 크게 List, Map, Set 3가지로 분류된 자료구조들이 구현되어 있음을 알 수 있을 것이다.

그 중 List에 관련된 자료구조는 __List__ 분류에 속해있다.

## 1️⃣ List 라는 자료구조

[List](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/) 는 kotlin.collections 패키지에 구현되어 있는 자료구조 중 하나이다.

List는 일반적으로 순서가 있는 __직선형__ 자료구조이다. 

> 주의할 점❗️
>
> [자료구조에 대한 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 에서 __리스트(linked List)__ 을 읽어보면 배열과 리스트의 차이에 대해 알 수 있을 것이다.
>
> 코틀린의 List는 이름만 보면 배열이 아니라 리스트(링크드 리스트)일 것 같지만 직선형 배열이다.
>
> 처음에 List가 linked list를 구현한 자료구조인 줄 알고 원소를 추가, 삭제하는 문제에 List를 가져다 풀었는데 시간초과가 났었다..
> 
> 알고보니 List는 배열이라 중간에 원소를 추가, 삭제하는데에 링크드 리스트 만큼 빠르지 않았던 것..!
>
> <img width="719" alt="03" src="https://user-images.githubusercontent.com/31889335/95871199-56641b00-0da8-11eb-802d-4df6e00ee08e.png">
>
> List를 구현할 때 사용되는 [Collection](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-collection/) 이라는 인터페이스를 보면 
>
> <img width="378" alt="04" src="https://user-images.githubusercontent.com/31889335/95871332-80b5d880-0da8-11eb-97f4-511443ebb0f2.png">
>
> 또 [Iterable](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-iterable/) 이라는 인터페이스로 구현되어 있다.
>
> Iterable 이라는 인터페이스에 대한 설명을 읽어보면 이 인터페이스로 구현된 것들은 sequence of elements 로 나타내질 수 있다고 쓰여있다.
>
> sequence of elements 라는 것은 원소들이 직선형으로 되어 있는 것을 말한다. 따라서 코틀린의 List는 결국 Iterable 이라는 것을 구현하여 만들어진 것이므로 링크드 리스트처럼 원소들의 메모리 공간이 떨어져있는 것이 아니라 연속적으로 이어져있는 배열이라는 뜻이다.

다만 이 List 라는 자료구조는 선언시 초기화한 값들을 읽기만(read-only) 할 수 있게 되어 있다. 즉, List에 저장된 데이터를 추가, 삭제, 변경할 수 없는 것이다.

만약 저장된 데이터를 추가, 삭제, 변경하려면 List가 아니라 __MutableList__ 라는 자료구조를 사용해야 한다!

[List](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/) 자료구조를 사용하여 데이터를 저장하고 관리하고 싶다면 링크로 걸어놓은 문서에 나와있는 여러 함수들을 사용하면 된다!

## 2️⃣ MutableList 라는 자료구조

[MutableList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/) 은 kotlin.collections 패키지에 구현되어 있는 자료구조에서 List 분류에 해당하는 자료구조 중 하나이다.

<img width="505" alt="05" src="https://user-images.githubusercontent.com/31889335/96462472-915ec680-1260-11eb-97c3-666e2110cb29.png">

사실 MutableList는 인터페이스이다.

위 List 자료구조 설명 부분에서 설명한 것처럼 List와 비슷하지만 데이터를 추가하고 삭제할 수 있도록 구현된 자료구조이다. (mutable = 변하기 쉬운)

[MutableList 문서](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/) 를 보면 MutableList 자료구조에 사용할 수 있는 여러가지 함수들을 알 수 있다.

__여기서 한 가지 알 수 있는 것!__

List 분류에 속하는 자료구조들은 상속받는 것들의 트리를 타고 올라가보면 맨 위에는 List 클래스가 있기 때문에 각 자료구조에서 사용되는 여러 함수들이 결국 List에서 상속받은 것들이 많다.

(아래에서 배울 ArrayList도 MutableList를 구현하고 있고, MutableList는 결국 List를 구현하고 있음!)

따라서 List 분류에 속하는 자료구조에 사용할 수 있는 함수는 이름이 같은것도 많고, 사용법이나 작동 원리가 비슷한 것도 많다!

## 3️⃣ ArrayList 라는 자료구조

[ArrayList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/) 는 kotlin.collections 패키지에 구현되어 있는 자료구조에서 List 분류에 해당하는 자료구조 중 하나이다.

<img width="446" alt="02" src="https://user-images.githubusercontent.com/31889335/95757709-74674800-0ce2-11eb-90c4-51f755e7c372.png">

ArrayList는 위에서 설명한 MutableList라는 자료구조를 상속한 형태이기 때문에 ArrayList에도 데이터를 추가하거나 삭제할 수 있다.

ArrayList는 추가, 삭제 외에 또 다른 특징이 있다.

__ArrayList는 자체적으로 메모리 효율성을 높이는 기능이 없다는 것이다.__

이 말을 이해하기 위해 [c++의 vector 포스팅](https://choheeis.github.io/newblog//articles/2020-01/C++Vector)에서 vector가 동적으로 크기를 변경하는 과정 부분을 읽어보면 좋다.

vector는 초반에 임의의 크기의 배열을 만들어 놓고 데이터를 추가하다가 배열이 꽉 차게 되면 기존 배열의 복사본에 임의의 크기(2배 정도)를 더 늘린 새로운 배열을 만드는 형식으로 크기가 변경된다.

이러한 방법으로 크기를 늘렸을 때 새로 늘린 크기 만큼의 메모리 공간에 데이터를 꽉 채워 추가하지 않는한 메모리 낭비가 생길 수 있다. 

ArrayList도 vector와 같은 방법으로 데이터를 추가하기 때문에 메모리 낭비가 발생할 수 있다. 하지만 ArrayList에는 데이터가 저장되지 않아 낭비되는 메모리를 효율적으로 관리하는 기능이 없다.

> --> [MutableList와 ArrayList에 대한 차이를 가장 납득할 만하게 써놓은 블로그](https://zladnrms.tistory.com/140)

[ArrayList 를 조작할 수 있는 함수들](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/#functions) 이나 [ArrayList 의 확장 함수들](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/#extension-functions) 을 보고 입맛대로 사용하면 된다.