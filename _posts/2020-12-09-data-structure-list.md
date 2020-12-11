---
layout: post
title:  "[자료구조] 3. List(리스트)"
date:   2020-12-10 18:34:10 +0700
categories: [자료구조]
---

자료구조의 개념과 간략한 설명에 대해서는 [이 블로그의 다른 포스팅 - 꼭 알아두어야 할 자료구조](https://choheeis.github.io/newblog//articles/2019-07/BasicDataStructure) 에서 볼 수 있다.

## 1️⃣ List란 무엇인가?

[이 블로그의 다른 포스팅 - 2.Linked List(연결 리스트)](https://choheeis.github.io/newblog//articles/2020-12/data-structure-linked-list) 를 먼저 읽어보고 오면 Linked List(연결 리스트)에 대해 알게 될 것이다. 그럼 이제 List(리스트)와 Linked List(연결 리스트)는 무엇이 다른지 궁금해질 것이다!

(본인은 이게 궁금해서 공부한 후 포스팅합니다,,)

Linked List(연결 리스트)는 

![단방향리스트](https://user-images.githubusercontent.com/31889335/61359643-b8895400-a8b7-11e9-89bc-44f64bd6213d.PNG)

위와 같은 모습으로 알고 있는데 List(리스트)는 어떤 모습일까?

결과적으로는 List(리스트)는 Linked List(연결 리스트), ArrayList 등 List 관련 자료구조들을 구현할 때 사용되는 __인터페이스__ 이다.

즉, Linked List(연결 리스트)처럼 특정한 모습으로 확정된 자료구조가 아니라 단지 __인터페이스__ 일 뿐이다.

[이 블로그의 다른 포스팅 - 2.Linked List(연결 리스트)](https://choheeis.github.io/newblog//articles/2020-12/data-structure-linked-list) 의 \<4️⃣ Array(배열)과 Linked List(연결 리스트)의 공통점과 차이점> 부분에서 __선형 자료구조__ 가 무엇인지 알 수 있을 것이다.

List는 선형 자료구조에 속하는 여러 자료구조들을 구현하기 위한 가장 기본적인 Base(베이스) 역할을 하는 인터페이스라고 생각하면 된다.

[Java 도큐먼트 - List](https://docs.oracle.com/javase/7/docs/api/java/util/List.html) 를 보면 

<img width="915" alt="01" src="https://user-images.githubusercontent.com/31889335/101651753-c0373300-3a80-11eb-8622-1e02f26b75b3.png">

위와 같이 List 라는 인터페이스를 구현하고 있는 여러 자료구조(Class형태)가 존재함을 알 수 있다.

> __인터페이스__ 란?
>
> 인터페이스 안에 여러 함수들을 정의만 해놓고(구현하지 않음) 다른 클래스에서 이 인터페이스에 속한 함수를 직접 구현하여 사용한다.
>
> 즉, 여러 클래스에서 똑같은 이름의 함수를 조금씩 다른 기능으로 사용해야 하는 경우를 위해 객체 지향 언어에서 등장한 개념이다.

<img width="1026" alt="02" src="https://user-images.githubusercontent.com/31889335/101652003-09878280-3a81-11eb-94ac-4c84870bb80b.png">

List 인터페이스에는 위와 같은 여러 함수들이 정의되어 있는데 모두 선형 자료구조들을 구현할 때 필요할 만한 함수들이다.

따라서 Linked List(연결 리스트)같은 선형 자료구조를 구현해야 할 때 List 인터페이스를 구현하여 만든다.

## 2️⃣ Array(배열)과 List(리스트)의 차이점

위에서 List는 인터페이스라고 알아보았다. 

자바나 코틀린 같은 언어에서는 List라는 인터페이스가 존재하기 때문에 'List는 선형 자료구조를 구현하기 위해 존재하는 인터페이스' 라고 말할 수 있다.

하지만 일반적으로 리스트가 뭐예요? 라는 질문에 '선형 자료구조를 구현하기 위해 존재하는 인터페이스' 입니다. 라고 대답하기 보다는 List를 Linked List(연결 리스트)처럼 모습이 확정된 자료구조라고 생각하고 대답하는 경우도 많다.

특히, Array(배열)과 List(리스트)의 차이를 물어보는 질문에서는 더욱 List를 확정된 모습의 자료구조라고 생각하고 대답하는 것이 질문의 의도에 더 맞을 수도 있다.

* __배열__

    * 선형 자료구조이다.

    * 단, 데이터가 메모리 상에 연속적으로 저장되어 있다.

* __리스트__

    * 선형 자료구조이다.

    * 단, 데이터가 메모리 상에 비연속적으로 저장되어 있다.
    
더 자세한 차이점은 [이 블로그의 다른 포스팅 - 1. Array(배열)](https://choheeis.github.io/newblog//articles/2020-12/data-structure-array) 에서 알 수 있을 것이다!

# 끝!