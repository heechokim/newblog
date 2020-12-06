---
layout: post
title:  "[안드로이드] 안드로이드 프로세스와 쓰레드"
date:   2020-12-05 18:34:10 +0700
categories: [안드로이드]
---

## 0️⃣ 프로세스와 쓰레드가 무엇?

Computer Science 분야에서는 프로세스와 쓰레드라는 개념이 있다.

간략하게 __프로세스__ 는 메인 메모리에 올라가 CPU의 작업을 받으며 실행되고 있는 프로그램을 말하고, __쓰레드__ 는 프로세스 안에 존재하는 작업의 흐름 단위라고 생각하면 된다.

더 자세한 개념은 이 블로그의 다른 포스팅에서 볼 수 있을 것이다.

> 운영체제 부분에서 쓰레드까지 공부하면 링크걸기!

이 포스팅에서는 Android 앱에서 프로세스와 쓰레드가 어떻게 작동하는지 알아볼 것이다!

## 1️⃣ Android OS에서의 프로세스

✍🏻 [Android Developer 도큐먼트 - 프로세스](https://developer.android.com/guide/components/processes-and-threads#Processes) 를 참고하여 작성합니다.

기본적으로 Android 앱이 처음 실행되면 Android OS는 메인 메모리에 하나의 쓰레드를 가진 하나의 __Linux 프로세스__ 를 올린다. 이 때의 쓰레드를 __Main Thread(메인 쓰레드)__ 라고 부른다.

또한 기본적으로 하나의 Android 앱을 구성하는 [모든 컴포넌트(Android 4대 컴포넌트인 Activity, Service, Broadcast, receiver)](https://developer.android.com/guide/components/fundamentals#Components) 는 동일한 프로세스와 동일한 쓰레드(메인 쓰레드)에서 실행되게 된다.

이처럼 기본적으로 Android 앱이 실행될 때는 1개의 프로세스(1개의 쓰레드를 가진 상태)가 메모리에 올라가 실행되지만 개발자가 프로세스와 쓰레드에 관련한 일련의 조작을 할 수 있다.

어떤 조작들을 할 수 있을까?

Android 앱을 구성하는 __여러 컴포넌트가 각자 별도의 프로세스를 생성해서 실행되게__ 할 수도 있고, 어느 프로세스에서나 __메인 쓰레드 외의 추가 쓰레드를 생성__ 할 수도 있다.

또한, Android 앱이 동일한 Linux 사용자 ID를 공유하고 동일한 인증서로 서명되었을 경우에는 서로 다른 Android 앱의 컴포넌트를 동일한 프로세스에서 실행되게 할 수도 있다.

이 포스팅에서는 실제로 어떤 코드로 어떻게 개발해야 위와 같은 조작을 할 수 있는지까지는 언급하지 않을 것이다. 알고 싶다면 따로 찾아보자.

이 외에도 Android OS 자체에서 스스로 프로세스 및 쓰레드를 조작하는 경우도 있다.

예를 들어 메모리가 부족할 경우 Android OS는 실행되고 있는 어떠한 프로세스를 종료시키기도 한다.

다만, Android OS는 사용자가 현재 눈으로 보고 있는 액티비티와 관련된 프로세스는 종료시키지 않고, 보이지 않는 액티비티와 관련된 프로세스를 종료시킨다.

따라서 프로세스의 종료는 해당 프로세스에서 실행되고 있는 Android 앱 컴포넌트의 상태(눈에 보이냐 보이지 않냐 같은 상태)에 따라 결정된다.


## 2️⃣ Android OS에서의 쓰레드

✍🏻 [Android Developer 도큐먼트 - 쓰레드](https://developer.android.com/guide/components/processes-and-threads#Threads) 를 참고하여 작성합니다.

위에서 알아본 내용을 다시 한번 언급하자면 __기본적으로 Android 앱이 실행되면 하나의 프로세스가 메모리에 올라가고, 이 프로세스는 메인 쓰레드라고 불리는 하나의 쓰레드를 가지고 있다.__ 

이 쓰레드의 이름이 왜 __Main(주요부)__ 쓰레드인지 알아보자.

메인 쓰레드에서는 Android 앱의 UI(android.widget 과 android.view 패키지에 포함되는 구성 요소들)와 사용자가 상호작용할 때 일어나는 이벤트 작업들이 실행되도록 되어있다.

예를 들어, 사용자가 어떠한 버튼(UI 위젯 중 하나)을 클릭했을 때 클릭 이벤트를 발생시키는 작업이 메인 쓰레드에서 실행된다고 볼 수 있다.

그렇기 때문에 Main 쓰레드를 __UI 쓰레드__ 라고도 부른다.

하지만 위에서도 언급했듯이 개발자가 추가 쓰레드를 생성하지 않는 한 Android 앱 내에서 실행되는 모든 작업들은 메인 쓰레드에서만 실행되기 때문에 이벤트 발생 작업들 외의 네트워크 작업이나 DB 쿼리 작업 등도 모두 UI 쓰레드에서 실행된다.

네트워크 작업이나 DB 쿼리 작업은 완료하기까지 긴 시간이 필요한 작업이다.

만약 메인 쓰레드에 네트워크 작업과 DB 쿼리 작업이 존재한다면 이 작업들을 실행하는 동안에는 메인 쓰레드의 UI 작업이 오랜 시간동안 차단되게 된다.

UI 작업이 차단되면 이벤트 발생 작업이 중단되기 때문에 사용자에게는 Android 앱이 중단된 것처럼 보일 것이다.

심지어 Android OS는 UI 쓰레드가 5초 이상 중단되면 사용자에게 __"어플리케이션 응답 없음"__ 이라는 __[ANR](https://developer.android.com/training/articles/perf-anr?hl=ko)__ 다이얼로그 화면을 띄운다.

이 다이얼로그 화면을 보게 된 사용자는 Android 앱에 대한 불만을 가질 수 밖에 없을 것이다.

## 3️⃣ Android OS에서의 Worker 쓰레드

위 내용들을 통해 UI 쓰레드에서 시간이 오래 걸리는 작업을 실행시키면 ANR과 같은 다이얼로그 화면이 뜨게 되어 사용자에게 불만족스러운 사용 경험을 줄 수 있다는 것을 알게되었다.

따라서 __개발자는 UI 작업가 차단되지 않도록 하는데 힘써야한다.__ 그럼 어떻게 UI 작업이 차단되지 않도록 할 수 있을까?

개발자는 하나의 프로세스 안에서 여러 추가 쓰레드를 생성하도록 조작할 수 있다고 위에서 언급했던 것을 기억해보자.

__네트워크 작업이나 DB 쿼리 작업 등 UI 쓰레드에서 작업하면 ANR이 발생할 가능성이 높은 작업들을 따로 추가 쓰레드를 생성하여 작업 쓰레드를 바꿔주는 것이 UI 작업이 차단되지 않도록 하는 방법이다!__

이렇게 메인 쓰레드 외의 추가된 다른 쓰레드를 __Worker 쓰레드(작업자 쓰레드)__ 라고 부른다.

다만, Worker 쓰레드에서는 UI 이벤트 발생 및 UI 업데이트 등의 작업을 할 수 없다!

UI와 관련된 작업들은 모두 UI 쓰레드(메인 쓰레드)에서만 해야하기 때문이다.

따라서 개발자는 UI 관련 작업들은 UI 쓰레드에서 실행시키고, 네트워크 작업이나 DB 쿼리 요청 등의 작업은 Worker 쓰레드에서 작업되도록 조작하면 된다.

Android API에서는 메인 쓰레드 외의 추가 쓰레드(Worker 쓰레드)를 생성시킬 수 있는 __[Thread](https://developer.android.com/reference/java/lang/Thread)__ 클래스를 제공하고 있다.

> 이 포스팅에서 Thread 클래스 사용법에 대해서는 언급하지 않을 것이다.

그런데 거의 대부분의 경우에서는 Worker 쓰레드에서 작업한 결과에 따라 UI가 업데이트 되어야 하는 경우가 존재할 것이다.

즉, Worker 쓰레드에서 UI 쓰레드에 접근(Access)할 수 있어야 한다는 것이다.

Android API 에서는 Worker 쓰레드에서 UI 쓰레드에 접근할 수 있는 여러 방법들을 제공하고 있다.

그 방법들 중 하나는 View 클래스에 속한 메소드인 __[post()](https://developer.android.com/reference/android/view/View#post(java.lang.Runnable))__ 메소드를 이용하는 방법이 있다.

~~~kotlin
fun onClick(v: View) {
    Thread(Runnable {
        val bitmap = processBitMap("image.png")

        // post 메소드를 사용하여 UI 쓰레드에 접근함
        imageView.post {
            imageView.setImageBitmap(bitmap)
        }
    }).start()
}
~~~

위 코드는 Thread 클래스를 사용하여 새로운 Worker 쓰레드를 생성하여 작업하던 중, UI 업데이트가 필요한 곳에서 View.post() 메소드를 사용하여 UI 쓰레드에 접근하고 있는 모습이다.

하지만 복잡한 Android 앱일수록 위와 같은 코드는 양이 많아지고 복잡해질 가능성이 크다. 따라서 [Handler](https://developer.android.com/reference/android/os/Handler) 라는 클래스를 사용해 UI 쓰레드에 접근하는 방안도 등장하였다.

 Android API는 점점 더 발전하면서 __[AsyncTask](https://developer.android.com/reference/android/os/AsyncTask)__ 클래스를 사용하는 방안이 등장하여 한 때 유용하게 사용되었는데 Android API Level 30이 되면서 AsyncTask 가 deprecated 되었다. 그 대신 __[코루틴(Coroutine)](https://developer.android.com/topic/libraries/architecture/coroutines)__ 이라는 것을 사용하여 UI 쓰레드와 Worker 쓰레드의 상호작용을 구현하길 권고하고 있다.

# 끝!