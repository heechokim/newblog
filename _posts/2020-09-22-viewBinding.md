---
layout: post
title:  "[안드로이드] 📲 View Binding을 사용하여 더 쉽게 뷰 객체를 inflate하자!"
date:   2020-09-22 18:34:10 +0700
categories: [안드로이드]
---

## 1️⃣ View Binding이 무엇이고 왜 등장했을까?

View Binding은 안드로이드 스튜디오가 3.6 Canary 11로 버전 업그레이드 하면서 추가된 기능이다. JetPack에 포함된 라이브러리는 아니다.

결론부터 말하자면 View Binding 기능은 뷰와 상호작용하는 코드를 이전보다 더욱 쉽고 간결하게 작성할 수 있도록 해주는 녀석이다.

그렇다면 이전의 뷰와 상호작용하는 코드는 어떠했길래 더 쉬운 방법이 등장한걸까?

> 여기부터는 [참고 블로그 포스팅](https://velog.io/@dev_2dong/Android-%EA%B3%B5%EC%8B%9D%EB%AC%B8%EC%84%9C-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-Architecture-Component2-View-Binding#view-bindng---%EB%8F%84%EC%9E%85) 을 참고하여 작성하였다.

지금까지는 View 컴포넌트들을 코드에서 직접 사용할 수 있는 객체로 만들기 위해 __findViewById()__ 라는 메소드를 사용하였다.

~~~kotlin
// 예시
val textView : TextView = findViewById(R.id.tv_title)
~~~

하지만 이 방법으로 View 컴포넌트들을 객체화 시킬 경우 몇 가지 문제점이 발생하였다.

1. 코드로 제어해야 하는 View 컴포넌트들이 많으면 많을수록 findViewById() 를 호출하는 코드가 View 컴포넌트 개수만큼 늘어났다. (코드가 길어지고 양이 많아지는 문제 발생)

2. findViewById() 함수는 속도가 느리다.

3. 개발자의 실수로 존재하지 않는 컴포넌트의 id를 불러왔다면 null이 발생하게 된다. 즉, null 관련 에러를 발생시켜 null 값에 안전하지 못하다.

따라서 findViewById() 함수로 View 컴포넌트를 객체화시키는 방법이 개선될 필요가 있었던 것이다!

> 하지만 코틀린에서는 findViewById() 함수를 호출하지 않고도 View를 객체화 할 수 있는 기능이 존재한다. [코틀린 확장 플러그인에 대한 이전 포스팅](https://choheeis.github.io/newblog//articles/2019-08/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-overview) 을 읽어보면 코틀린에서 findViewById()를 사용하지 않고 View를 객체화하는 방법을 알 수 있을 것이다.
>
> 코틀린 확장 플러그인에서 View를 객체화하는 방법과 View Binding 기능을 사용하여 View를 객체화 하는 방법에 대한 차이도 공부할 필요가 있겠다!

그럼 지금부터 View Binding 을 사용해보자.

## 2️⃣ View Binding을 사용하기 위한 세팅

> 다음 내용들은 [Android Developer - View Binding](https://developer.android.com/topic/libraries/view-binding) 문서를 참고하여 작성하였다.

View Binding을 사용하기 위해 app 수준의 build.gradle 파일에 다음과 같은 코드를 추가한다.

~~~kotlin
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
~~~

## 3️⃣ binding class가 생성되다!

위와 같이 View Binding 사용함을 설정하면 각 XML 파일 마다 해당 XML 파일 이름을 카멜표시법으로 변환하고 끝에 Binding을 붙인 이름의  __binding class__ 가 생성되게 된다.

예를 들어, 이름이 result_profile.xml 인 XML 파일의 binding class는 ResultProfileBinding 이라는 이름으로 생성되는 것이다.

또한 각 XML에 대응되는 binding class에는 각 XML의 root view 및 ID가 있는 모든 뷰의 직접적인 참조가 포함되어 있다.

따라서 만약 아래와 같은 레이아웃을 나타내는 xml 파일의 이름이 activity_main.xml 이라고 가정하면

<img width="193" alt="01" src="https://user-images.githubusercontent.com/31889335/95431191-c4af7480-0987-11eb-8971-88486ddd921e.png">

~~~xml
<!-- 위 그림에 해당하는 activity_main.xml 파일 -->
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tv_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <ImageView
        android:id="@+id/iv_picture"
        android:layout_width="300dp"
        android:layout_height="200dp"
        app:layout_constraintTop_toBottomOf="@+id/tv_title"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="50dp"
        android:background="#FFB2B2"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="@+id/iv_picture"
        app:layout_constraintStart_toStartOf="@+id/iv_picture"
        app:layout_constraintBottom_toBottomOf="@+id/iv_picture"
        app:layout_constraintEnd_toEndOf="@+id/iv_picture"
        android:text="안녕 난 id가 없단다"/>
</androidx.constraintlayout.widget.ConstraintLayout>
~~~

이 xml 파일에 대응되는 binding class의 이름은 ActivityMainBinding 이고 이 클래스에는 tv_title이라는 textView와 iv_picture 이라는 ImageView에 해당하는 두 개의 필드가 존재할 것이다.

이 때, "안녕 난 id가 없단다" 를 보여주는 TextView는 id 값이 설정되어 있지 않기 때문에 ActivityMainBinding 클래스에 해당 필드가 존재하지 않는다.

즉, 이러한 사실로 보았을 때 view binding 기능을 사용하지 않았을 때에는 findViewById() 메소드를 사용하여 tv_title에 해당하는 textView와 iv_picture에 해당하는 ImageView를 일일이 코드로 직접 참조하여 객체화시켜야 했지만 view binding을 사용하면 자동으로 binding class에 id를 갖는 모든 뷰가 이미 참조되어 생성된다는 것을 알 수 있다.

따라서 레이아웃에 배치된 뷰 개수 만큼의 findViewBindId() 메소드를 호출하는 코드를 작성해야 했던 불편함이 해결되게 된다!

또한 모든 binding class는 __getRoot()__ 라는 이름의 메소드를 가지고 있다. 이 메소드는 binding class에 대응되는 레이아웃 파일의 root view의 직접 참조를 제공하는 메소드이다.

따라서 위 xml 코드에서 root view는 ConstraintLayout이므로 getRoot() 메소드 호출에 의해 ConstraintLayout이 반환된다.

## 4️⃣ Activity에서 View Binding 사용하기

view binding을 사용하여 생성된 binding class의 인스턴스를 세팅하는 작업은 액티비티의 __onCreate()__ 콜백 메소드 안에서 해야한다.

액티비티에서 view binding을 사용하는 일반적인 순서는 다음과 같다. 다음 순서 설명에서 예시로 할 xml 파일은 위에서 사용한 activity_main.xml 파일이다.

1. __view binding 기능에 의해 생성된 ActivityMainBinding 클래스의 인스턴스를 생성한다.__

    이 때, lateinit 예약어를 사용하여 onCreate() 콜백 메소드가 호출된 후에 생성되도록 해주었다.

    ~~~kotlin
    class MainActivity : AppCompatActivity() {

        private lateinit var binding: ActivityMainBinding
        
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
        }
    }
    ~~~

2. __각 binding class에는 inflate() 메소드가 포함되어 있다. 이 메소드를 호출시킴으로써 binding class의 인스턴스가 생성되게 된다.__

    ~~~kotlin
    class MainActivity : AppCompatActivity() {

        private lateinit var binding: ActivityMainBinding

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)

            binding = ActivityMainBinding.inflate(layoutInflater)
        }
    }
    ~~~

3. __binding class에 포함되어 있는 getRoot() 메소드를 사용하여 root view 참조를 가져온 후, root view를 setContentView()에 전달하여 화면상의 active view로 만든다.__

    ~~~kotlin
    class MainActivity : AppCompatActivity() {

        private lateinit var binding: ActivityMainBinding

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)

            binding = ActivityMainBinding.inflate(layoutInflater)
            val view = binding.root
            setContentView(view)
        }
    }
    ~~~

4. __이제 인스턴스를 사용하여 마음껏 뷰에 관한 코드를 작성한다!__

    ~~~kotlin
    class MainActivity : AppCompatActivity() {

        private lateinit var binding: ActivityMainBinding

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)

            binding = ActivityMainBinding.inflate(layoutInflater)
            val view = binding.root
            setContentView(view)

            // 이미지뷰를 클릭하면 토스트가 띄워지도록 함
            binding.ivPicture.setOnClickListener {
                Toast.makeText(this, "토스트!", Toast.LENGTH_SHORT).show()
            }
        }
    }
    ~~~

이렇게 작성한 앱을 실행시키면 다음과 같다!

![ViewBinding1](https://user-images.githubusercontent.com/31889335/95436854-4ce54800-098f-11eb-80cd-8c1be26a52de.gif)

## 5️⃣ Fragment에서 view binding 사용하기

> 여기서부터 똑같은 프로젝트에 다른 액티비티 하나 만들고 그 위에 프래그먼트 올려서 해보기

https://www.youtube.com/watch?v=W7uujFrljW0