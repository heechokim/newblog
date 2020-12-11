---
layout: post
title:  "[Kotlin] 코틀린 표준 라이브러리(kotlin stdlib)"
date:   2020-12-12 18:34:10 +0700
categories: [kotlin]
---

## 1️⃣ 코틀린도 표준 라이브러리를 가지고 있다!

✍🏻 [kotlin 도큐먼트 - stdlib](https://kotlinlang.org/api/latest/jvm/stdlib/) 를 참고하여 작성합니다.

코틀린은 코틀린 언어로 개발할 때 필수적으로 사용되는 여러 함수나 자료구조들을 __코틀린 표준 라이브러리(Kotlin Standard Library, kotlin stdlib)__ 로 제공한다.

## 2️⃣ 패키지로 관리되는 코틀린 표준 라이브러리

코틀린 표준 라이브러리 안에 구현되어 있는 다양한 함수나 자료구조들은 비슷한 것끼리 분류되어 __폴더로 묶여 관리__ 되고 있다.

이러한 폴더를 __패키지(package)__ 라고 부른다.

<img width="731" alt="01" src="https://user-images.githubusercontent.com/31889335/101944234-bacc1b00-3c2f-11eb-8c3d-3d350e3c3a27.png">

위 그림에서 볼 수 있듯이, 여러 패키지들이 존재하고 패키지 이름은 'kotlin.~~' 로 지어져있다.(위 그림에서 보여지는 패키지 외에도 다양한 패키지들이 있다.)

__패키지 안에는 어떤 것들이 있을까?__

예시로 [kotlin.collections](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/) 라는 패키지를 살펴보자.

<img width="585" alt="02" src="https://user-images.githubusercontent.com/31889335/101944616-58274f00-3c30-11eb-8d7c-66f1453c4a30.png">

이 패키지에는 Iterable, Collection, List, Set, Map 같은 Collection 타입의 것들과 그와 관련된 확장 함수들이 구현되어 있다.

따라서 코틀린으로 개발하면서 Collection 관련된 것을 사용해야 할 때는 이 패키지에 존재하는 것들을 불러다 사용하면 된다!

이처럼 코틀린 표준 라이브러리에는 정말 무수한 함수와 자료구조들이 구현되어 있기 때문에 어떤 것들이 있는지 아는 것도 중요하고 각 함수나 자료구조의 사용법을 찾아내 적용하는 것도 중요하다!

# 끝!