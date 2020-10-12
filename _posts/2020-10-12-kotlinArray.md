---
layout: post
title:  "[Kotlin] 7️⃣ 코틀린의 배열들"
date:   2020-10-12 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ 코틀린에서 배열을 사용하려면?

코틀린에서 배열은 kotlin 표준 라이브러리에 구현되어 있다. 그 중 __kotlin.collections__ 라는 패키지로 묶어서 이 안에 배열과 관련된 여러 자료 구조들이 구현되어 있다.

따라서 코틀린에서 배열을 사용하려면 kotlin 표준 라이브러리에 있는 배열을 사용하면 된다!

## 2️⃣ kotlin.collections 패키지 둘러보기!

[kotlin.collections 문서](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/) 를 보면 kotlin 표준 라이브러리의 kotlin.collections 패키지에 어떤 것들이 구현되어 있는지 알 수 있다.

<img width="728" alt="01" src="https://user-images.githubusercontent.com/31889335/95757699-6fa29400-0ce2-11eb-81e9-ea7c7d7589f2.png">

위 그림의 설명을 읽어보면 이 패키지에는 List, Set, Map 등의 Collection type들과 확장 함수들이 구현되어 있다다.

즉, 이 패키지는 일반적인 배열 뿐만 아니라 Set, Map, HashMap, List, ArrayList, LinkedHashMap 등 Collection 형의 다양한 자료구조들이 구현되어 있는 곳이고 이 자료구조들을 조금 더 쉽게 사용할 수 있도록 도와주는 확장 함수들이 구현되어 있는 곳이다.

그렇기 때문에 패키지 이름이 __kotlin.collections__ 인 것이다.

## 3️⃣ List 라는 Collection type의 자료구조

[List](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/) 는 kotlin.collections 패키지에 구현되어 있는 자료구조 중 하나이다.

List는 일반적으로 순서가 있는 자료구조이다. List에 대한 자세한 설명은 [자료구조에 대한 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 에서 __리스트 부분__ 을 읽어보면 된다.

또 [List](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/) 문서에 List 자료구조에서 사용할 수 있는 함수들도 볼 수 있다.

다만 이 List 라는 자료구조는 선언시 초기화한 값들을 읽기만(read-only) 할 수 있게 되어 있다. 즉, List에 저장된 데이터를 추가, 삭제, 변경할 수 없는 것이다.

만약 저장된 데이터를 추가, 삭제, 변경하려면 List가 아니라 __MutableList__ 라는 자료구조를 사용해야 한다!

## 4️⃣ MutableList 라는 Collection type의 자료구조

[MutableList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/) 은 kotlin.collections 패키지에 구현되어 있는 자료구조 중 하나이다.

위에서 설명한 List 자료구조에서 데이터를 추가하고 삭제할 수 있도록 구현된 자료구조이다. (mutable = 변하기 쉬운)

[MutableList 문서](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/) 를 보면 MutableList 자료구조에 사용할 수 있는 여러가지 함수들을 알 수 있다.

## 5️⃣ ArrayList 라는 Collection type의 자료구조

[ArrayList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/) 는 kotlin.collections 패키지에 구현되어 있는 자료구조 중 하나이다.

<img width="446" alt="02" src="https://user-images.githubusercontent.com/31889335/95757709-74674800-0ce2-11eb-90c4-51f755e7c372.png">

ArrayList는 위에서 설명한 MutableList라는 자료구조를 상속한 형태이기 때문에 ArrayList에 데이터를 추가하거나 삭제할 수 있다.

여기에 더불어 __[RandomAccess](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-random-access.html)__ 라는 인터페이스를 구현하고 있는 것을 볼 수 있다.

RandomAccess에 대해 찾아보니 List 자료구조에서 인덱스를 사용하여 데이터에 접근할 수 있는 기능을 지원하는 인터페이스라고 한다.

즉, ArrayList는 __List 형태가 아닌 Array 형태__ 라는 의미이다. 

[자료구조에 대한 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 을 보면 배열(Array)과 리스트(List)의 차이를 알 수 있고, 어느 경우에 배열을 사용하는 것이 좋은지, 리스트를 사용하는 것이 좋은지를 알 수 있을 것이다.

배열과 리스트의 차이를 알고 나면 ArrayList가 List 형태가 아닌 Array 형태라는 것이 이해될 것이고 왜 RandomAccess 라는 인터페이스를 구현하고 있는지도 이해될 것이다!

ArrayList의 또 다른 특징이 있다.

__ArrayList는 자체적으로 메모리 효율성을 높이는 기능이 없다.__

이 말을 이해하기 위해 [c++의 vector 포스팅](https://choheeis.github.io/newblog//articles/2020-01/C++Vector)에서 vector가 동적으로 크기를 변경하는 과정 부분을 읽어보면 좋다.

vector는 초반에 임의의 크기의 배열을 만들어 놓고 데이터를 추가하다가 배열이 꽉 차게 되면 기존 배열의 복사본에 임의의 크기를 더 늘린 새로운 배열을 만드는 형식으로 크기가 변경된다.

이러한 방법으로 크기를 늘렸을 때 새로 늘린 크기 만큼의 메모리 공간에 데이터를 꽉 채워 추가하지 않는한 메모리 낭비가 생길 수 있다. 

ArrayList도 vector와 같은 방법으로 데이터를 추가하기 때문에 메모리 낭비가 발생할 수 있다. 하지만 ArrayList에는 데이터가 저장되지 않아 낭비되는 메모리를 자체적으로 관리하는 기능이 없다.

[ArrayList 를 조작할 수 있는 함수들](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/#functions) 이나 [ArrayList 의 확장 함수들](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/#extension-functions) 을 보고 입맛대로 사용하면 된다.

* __ArrayList 를 사용할 때 자주 사용되는 함수들__

    * __1. ArrayList 선언 및 초기화하기__

        [ArrayList 문서](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/#constructors) 를 보면 초기화 하는 방법이 나와있지만

        [arrayListOf() 문서](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/array-list-of.html) 에서 볼 수 있듯이 kotlin.collections 패키지 내에 ArrayList를 선언하고 초기화할 때 더욱 쉽게 할 수 있도록 구현해놓은 확장함수를 사용하면 된다.

        ~~~kotlin
        fun main() {
            // 빈 배열(ArrayList) 선언
            val arrayList = arrayListOf<Int>()

            // 1, 2, 3으로 초기화한 배열(ArrayList)
            val arrayList2 = arrayListOf(1, 2, 3)
        }
        ~~~

    * __2. ArrayList의 크기 알기__

        ~~~kotlin
        val arrayList2 = arrayListOf(1, 2, 3)
        
        // size 라는 property 사용
        println(arrayList2.size)
        ~~~
        
    * __3. ArrayList의 끝에 데이터 추가하기__

        [add()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/add.html) 함수 사용하기

    * __4. ArrayList에 저장된 데이터를 인덱스 사용해 제거하기__

        [removeAt()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/remove-at.html) 함수 사용하기










