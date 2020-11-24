---
layout: post
title:  "[Kotlin] 6️⃣ 코틀린의 Collection 패키지"
date:   2020-10-13 18:34:10 +0700
categories: [kotlin]
---

## 0️⃣ 코틀린의 Collection 패키지가 뭔가!

사실 이 포스팅은 코틀린의 배열인 List, ArrayList, MutableList 등을 먼저 공부한 후 작성하는 것이다.

배열들을 공부하다 보니 Collection 패키지에 대해 궁금하게 되었고, 한 번 공부를 해야겠다고 생각했다.

코틀린은 자체적으로 내장하고 있는 __표준 라이브러리(kotlin standard library = kotlin stdlib)__ 를 가지고 있다.

이 표준 라이브러리 안에는 코틀린 언어가 가지고 있는 다양한 함수나 기능들이 구현되어 있는데 이것들이 기능에 따라 분류되어 __폴더로 묶여 관리__ 되고 있다.

코틀린 표준 라이브러리를 구성하는 여러가지 폴더들을 __패키지(package)__ 라고 한다.

코틀린의 Collection도 표준 라이브러리 안에 존재하는 패키지들 중 하나의 패키지이다.

[kotlin standard library 공식 문서](https://kotlinlang.org/api/latest/jvm/stdlib/) 에 보면 

<img width="762" alt="01" src="https://user-images.githubusercontent.com/31889335/95838116-5b12da00-0d7c-11eb-836d-6e86e71df2b8.png">

위와 같이 kotlin stdlib 라이브러리 안에 정말 많은 패키지들이 있고, 그 중 __kotlin.collections__ 라는 이름의 패키지가 있는 것을 볼 수 있다.

그럼 이 패키지는 어떤것들을 묶어놓은 폴더인지 알아보자!

## 1️⃣ kotlin.collections 패키지 안에 있는 것들

kotlin.collections 패키지 안에는 다양한 자료구조들과 각 자료구조를 조작할 수 있는 함수들이 구현되어 있고, 이 자료구조들을 더욱 쉽게 사용할 수 있도록 해주는 확장 함수들도 구현되어 있다.

__그럼 kotlin.collections 패키지 안에 구현되어 있는 자료구조들은 어떤 것들일까?__

일단 collections이라는 용어는 __다양한 종류의 그룹들__ 로 이해하면 쉽다. 즉, kotlin.collections 패키지 안에는 다양한 종류의 그룹들이 구현되어 있다.

> kotlin 외의 다양한 다른 언어들에서도 비슷한 의미의 collection이라는 개념을 많이 사용한다.

여기서 __그룹__ 이라는 것의 예로는 일반적인 __1차원 배열__, __2차원 배열__, __Map__, __Set__ 등의 자료구조들이 존재한다.

코틀린에서는 __다양한 종류의 그룹__ 들을 크게 __List__, __Map__, __Set__ 이라는 3가지 큰 분류로 나누어 collections 라는 패키지 안에 넣어놓았다.

3가지 분류인 List, Map, Set은 각각 더 다양한 자료구조로 나뉘어 구현되어 있다.

<img width="737" alt="02" src="https://user-images.githubusercontent.com/31889335/95866868-67f6f400-0da3-11eb-8098-59727c382199.png">

위 다이어그램은 kotlin.collections 패키지 안에 구현되어 있는 자료구조들이 구현될 때 사용하는 인터페이스들이다.

예를 들어, kotlin.cellections 패키지안에 구현되어 있는 자료구조 중 List 분류에 속하는 [ArrayList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-array-list/) 라는 자료구조를 보자.

<img width="728" alt="03" src="https://user-images.githubusercontent.com/31889335/95868009-beb0fd80-0da4-11eb-8327-99e5cf0aa7f9.png">

ArrayList 클래스 정의를 보면 위 다이어그램에서 볼 수 있는 [MutableList](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/) 라는 인터페이스를 구현하여 만들어져 있음을 알 수 있다.

또 MutableList 라는 인터페이스는

<img width="732" alt="04" src="https://user-images.githubusercontent.com/31889335/95868333-10598800-0da5-11eb-9236-367ff84dc0d0.png">

위 다이어그램에서 볼 수 있는 [List](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/) 와 [MutableCollection](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-collection/) 이라는 인터페이스를 구현하여 만들어져 있음을 알 수 있다.

즉, 위 다이어그램에 있는 인터페이스들을 서로 구현하여 만들어진 다양한 그룹형 자료구조들이 kotlin.collecions 라는 패키지를 구성하고 있다는 것이다.

## 2️⃣ 이제 kotlin.collections 패키지 안에 있는 자료구조들을 공부할 시간!

kotlin.collections 패키지 안에 있는 자료구조들은 각 자료구조들을 공부한 내용의 포스팅에서 알아보자!

* [List 분류의 자료구조들을 공부한 포스팅](https://choheeis.github.io/newblog//articles/2020-10/kotlinArray)

* Map 분류의 자료구조들을 공부한 포스팅 --> 공부 전

* Set 분류의 자료구조들을 공부한 포스팅 --> 공부 전

# 끝!