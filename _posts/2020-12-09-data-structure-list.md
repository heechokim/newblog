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

    > 바킹독님 물어보고 답 오면 적기..

* __리스트__

    * 선형 자료구조이다.

    * 단, 데이터가 메모리 상에 비연속적으로 저장되어 있다.

## 3️⃣ List 심화 - Iterator란?

위에서 보았던 [Java 도큐먼트 - List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) 를 보면

<img width="978" alt="03" src="https://user-images.githubusercontent.com/31889335/101656422-05119880-3a86-11eb-93c5-dbaaf0cdc2aa.png">

위 그림과 같이 List 인터페이스가 Super Interface(부모 인터페이스)를 상속받고 있음을 알 수 있다.

List 인터페이스가 상속받고 있는 부모 인터페이스에는 __Collection 인터페이스__ 와 __Iterable 인터페이스__ 가 있다.

<img width="222" alt="04" src="https://user-images.githubusercontent.com/31889335/101656736-646fa880-3a86-11eb-9e3e-95a57c99f3b1.png">

사실은 위 그림에서 알 수 있듯이 List 인터페이스는 Collection 인터페이스 하나 만 상속받고, 

<img width="271" alt="05" src="https://user-images.githubusercontent.com/31889335/101656934-a8fb4400-3a86-11eb-8b3b-fe72e448b83d.png">

Collection 인터페이스가 Iterable 인터페이스를 상속받고 있는 모습이다.

이 두 인터페이스 중 __Iterable 인터페이스__ 의 역할에 대해 알아보자.

[Java 도큐먼트 - Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html) 을 보면 

<img width="839" alt="06" src="https://user-images.githubusercontent.com/31889335/101657165-ed86df80-3a86-11eb-9e47-40b84dbe63dc.png">

For-each라는 Loop문의 타겟이 되는 객체가 되는 것을 허용할 수 있게 하기 위해 만들어진 인터페이스라고 적혀 있다.

여기서 객체는 배열이나 리스트처럼 여러 원소들을 가지고 있는 대상을 말한다. 즉, 여기서 객체는 배열, 리스트 등을 말한다.

> For-each Loop문은 어떤 배열이나 리스트 안에 저장되어 있는 원소를 맨 첫 원소부터 가장 마지막 원소까지 순서대로 순회하는 반복문이다.

<img width="647" alt="07" src="https://user-images.githubusercontent.com/31889335/101657513-540bfd80-3a87-11eb-9cb4-8c903c675731.png">

iterable이라는 영어 단어의 뜻이 __"반복 가능"__ 인 것처럼 어떠한 객체가 For-each 반복문에 적용되도록 하는 인터페이스라고 생각할 수 있다.

객체가 For-each 반복문에 적용된다는 것은 해당 객체가 가지고 있는 모든 원소들이 For-each 반복문에 의해 순회되는 대상이 된다는 의미이다.

Iterable 인터페이스가 정의하고 있는 메소드는 아래와 같이 3개의 메소드이다.

<img width="1006" alt="08" src="https://user-images.githubusercontent.com/31889335/101657772-9e8d7a00-3a87-11eb-9710-425c960f1572.png">

언어마다 정의하고 있는 함수의 종류나 개수가 다를 수 있지만 여기서 중요한 것은 Iterator() 라는 함수이다.

__Iterator() 함수__ 는 __어떤 객체가 가지고 있는 원소(element)에 대한 반복자(iterator) 를 반환__ 하는 함수이다.

여기서 iterator가 한글로 '반복자'로 변역되어 이해가 잘 안될 수 있는데 그냥 __"어떤 객체가 가지고 있는 원소 접근을 일반화하는 수단"__ 이라고 부르는 것이 가장 정확하다.

> 어떤 사람은 [자신의 블로그 글](http://occamsrazr.net/tt/entry/iterator-%EB%B0%98%EB%B3%B5%EC%9E%90#recentTrackback) 에서 Iterator 에 대한 한글 번역을 반복자 대신 '훑개' 로 하는 것이 좋을 뻔 했다는 글을 보았다.

이 Iterator() 함수가 실제로 어떻게 구현되어 있고, 어떻게 원소 접근을 하고 있는지 궁금했다.

<img width="1407" alt="09" src="https://user-images.githubusercontent.com/31889335/101660443-879c5700-3a8a-11eb-97ee-4cb1d9a64da0.png">

Iterator()는 Iterable 인터페이스에 존재하므로 Iterable 인터페이스를 직접 구현하고 있는 자료구조(Class)의 코드를 뜯어보면 알 수 있을 것이라고 생각했다.

따라서 위 그림에서 알 수 있는 Iterable 인터페이스를 구현한 클래스들 중 Linked List 클래스를 뜯어보았다. (IntelliJ의 기능 중 command키 누르기 + 마우스 클릭 하면 해당 클래스의 본 구현 코드를 볼 수 있는데 이것을 사용해서 Linked List 클래스를 뜯어봤다.)

Linked List 안에는 또 수 많은 class들이 존재했는데 그 중 __ListItr라는 클래스__ 를 찾았고, 이 클래스는 ListIterator라는 인터페이스를 구현하고 있었다.

<img width="612" alt="11" src="https://user-images.githubusercontent.com/31889335/101668462-ed411100-3a93-11eb-9587-20235d375cf3.png">

ListIterator 인터페이스도 뜯어보니 Iterable() 인터페이스를 상속받는 인터페이스였다. 따라서 이 ListItr 클래스가 구현하고 있는 함수들을 보면 Iterator()가 무엇인지 대충 알 수 있을 것이라고 생각했다.

ListItr 클래스 코드를 보기 전에 먼저 알아두어야 할 것이 있다.

Node라는 클래스인데 아래와 같은 코드로 구현되어 있다.

~~~java
private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
~~~

위 코드를 보면 

<img width="114" alt="10" src="https://user-images.githubusercontent.com/31889335/101668658-25e0ea80-3a94-11eb-9679-3c2dc2c75d5e.png">

이와 같은 모습을 Node 클래스로 구현하고 있음을 알 수 있을 것이다.

이제 본격적으로 ListItr 클래스를 구현하고 있는 코드를 봐보자. 엄청 길다,, ㅎㅎ

~~~java
private class ListItr implements ListIterator<E> {
        private Node<E> lastReturned;
        private Node<E> next;
        private int nextIndex;
        private int expectedModCount = modCount;

        // 생성자 함수
        ListItr(int index) {
            // assert isPositionIndex(index);
            next = (index == size) ? null : node(index);
            nextIndex = index;
        }

        // 다음 노드가 존재하는지 true or false로 반환하는 함수
        public boolean hasNext() {
            return nextIndex < size;
        }

        // 다음 노드를 반환하는 함수
        public E next() {
            checkForComodification();
            if (!hasNext())
                throw new NoSuchElementException();

            lastReturned = next;
            next = next.next;
            nextIndex++;
            return lastReturned.item;
        }

        // 이전 노드가 존재하는지 true of false로 반환하는 함수
        public boolean hasPrevious() {
            return nextIndex > 0;
        }

        // 이전 노드를 반환하는 함수
        public E previous() {
            checkForComodification();
            if (!hasPrevious())
                throw new NoSuchElementException();

            lastReturned = next = (next == null) ? last : next.prev;
            nextIndex--;
            return lastReturned.item;
        }

        public int nextIndex() {
            return nextIndex;
        }

        public int previousIndex() {
            return nextIndex - 1;
        }

        // 해당 노드를 제거하는 함수
        public void remove() {
            checkForComodification();
            if (lastReturned == null)
                throw new IllegalStateException();

            Node<E> lastNext = lastReturned.next;
            unlink(lastReturned);
            if (next == lastReturned)
                next = lastNext;
            else
                nextIndex--;
            lastReturned = null;
            expectedModCount++;
        }

        // 해당 노드의 data를 set하는 함수
        public void set(E e) {
            if (lastReturned == null)
                throw new IllegalStateException();
            checkForComodification();
            lastReturned.item = e;
        }

        // 해당 노드의 이전에 새로운 노드를 추가하는 함수
        public void add(E e) {
            checkForComodification();
            lastReturned = null;
            if (next == null)
                linkLast(e);
            else
                linkBefore(e, next);
            nextIndex++;
            expectedModCount++;
        }
~~~

먼저 ListItr 클래스의 생성자 함수를 보자.

~~~java
 // 생성자 함수
ListItr(int index) {
    // assert isPositionIndex(index);
    next = (index == size) ? null : node(index);
    nextIndex = index;
}
~~~

예를 들어, 

~~~java
// java
ListItr listItr = ListItr(3)
~~~

이렇게 클래스를 객체로 선언하면 생성자 함수에 의해 next라는 Node가 하나 생성되는데 index=3이 List의 size와 같으면(즉, 리스트의 가장 마지막 index이면) next Node는 null이고 같지 않으면 next Node는 생성된다.

정리해보면 ListItr(3)을 선언하면 3이 리스트의 가장 마지막 원소가 아닌 이상 next라는 이름의 Node가 생기고 이 next Node의 index는 3이된다.

ListItr 클래스에서 중요하게 봐야할 함수는 __next()__, __previous()__, __remove()__, __add()__ 이다.

* __next()__ 함수는 ListItr(3)의 다음 노드가 존재할 경우 다음 노드를 반환한다.

* __previous()__ 함수는 ListItr(3)의 이전 노드가 존재할 경우 이전 노드를 반환한다.

* __remove()__ 함수는 ListItr(3)이 생성한 index 3에 해당하는 노드를 삭제한다.

* __add()__ 함수는 ListItr(3)이 생성한 index 3에 해당하는 노드 앞에 새로운 노드를 삽입한다.

즉, 위와 같이 ListItr 클래스는 리스트의 특정 index 에 해당하는 노드에 접근하여 해당 노드에 대한 삭제 및 추가 작업, 해당 노드의 이전 및 다음 노드 확인 등의 작업을 할 수 있게 해준다.

따라서 Iterator의 정의였던 __"어떤 객체가 가지고 있는 원소 접근을 일반화하는 수단"__ 라는 의미가 어느 정도 이해될 것이다.

# 끝!