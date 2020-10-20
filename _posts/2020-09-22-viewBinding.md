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
        }

        // activity lifecycle 에 따라 onStart() 콜백 메소드에 다음과 같이 작성함!
        override fun onStart() {
            super.onStart()
            binding.ivPicture.setOnClickListener {
                Toast.makeText(this, "토스트!", Toast.LENGTH_SHORT).show()
            }
        }
    }
    ~~~

이렇게 작성한 앱을 실행시키면 다음과 같다!

![ViewBinding1](https://user-images.githubusercontent.com/31889335/95436854-4ce54800-098f-11eb-80cd-8c1be26a52de.gif)

위 코드는 [android github - viewBinding](https://github.com/android/architecture-components-samples/tree/master/ViewBindingSample) 과 문서를 참고하여 작성하였다!

## 5️⃣ Fragment에서 view binding 사용하기

1. __프래그먼트를 생성한다.__

    위에서 만든 프로젝트에 새로운 Fragment를 생성하고 이 Fragment의 xml 파일을 fragment_main.xml 이라고 이름을 지어보자.

    ~~~xml
    // fragment_main.xml 파일
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainFragment">

        <TextView
            android:id="@+id/tv_fragment_title"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:textSize="20sp"
            android:background="#ABFFE66D"
            android:textColor="#FF1919"
            android:gravity="center"/>

    </FrameLayout>
    ~~~

    <img width="122" alt="02" src="https://user-images.githubusercontent.com/31889335/96567404-afc8ce80-1301-11eb-8370-dda242300563.png">

    위 fragment_main.xml 파일은 위와 같은 모습을 하게 된다.

2. __액티비티 위에 프래그먼트를 붙인다.__

    위에서 만든 프래그먼트를 MainActivity 위에 붙여보자!

    MainActivity 위에 붙이기 위해 먼저 activity_main.xml 에 \<fragment> 태그를 사용하여 다음과 같이 fragment를 위치시킨다.

    ~~~xml
    // activity_main.xml 파일
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

        <!-- 추가한 코드 -->
        <fragment
            android:id="@+id/fragment_main"
            android:name="com.example.viewbinding.MainFragment"
            android:layout_width="300dp"
            android:layout_height="0dp"
            app:layout_constraintTop_toBottomOf="@+id/iv_picture"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintBottom_toBottomOf="parent" />

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

    <img width="240" alt="03" src="https://user-images.githubusercontent.com/31889335/96568625-1dc1c580-1303-11eb-9ebf-e9efff9db75d.png">

    위 xml 파일은 위와 같은 모습을 하고 있다.

3. __프래그먼트에서 ViewBinding 사용하기__

    프래그먼트의 lifeCycle에 따라 프래그먼트가 자신의 UI를 그리기 시작하는 곳인 onCreateView() 콜백 메소드에서 뷰 바인딩을 시켜주면 된다.

    ~~~kotlin
    // 1. 먼저 fragment binding 클래스를 null로 초기화해준다. (onDestroyView() 메소드에서 사용하기 위한 변수이다.)
    private var fragmentMainBinding: FragmentMainBinding? = null

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // 2. view binding 시켜준다. (이때, 프래그먼트에서는 inflate() 인자가 layoutInflater이 아님!)
        val binding = FragmentMainBinding.inflate(inflater, container, false)
        binding.tvFragmentTitle.text = "안녕 나는 프래그먼트야!"
        return binding.root
    }

    // 3. 프래그먼트 lifeCycle에 따르면 프래그먼트와 연결된 뷰 계층이 떨어질때 onDestroyView() 콜백 메소드가 실행된다. 이 때 binding 클래스를 null 처리 해준다.
    override fun onDestroyView() {
        fragmentMainBinding = null
        super.onDestroyView()
    }
    ~~~

    이 때 주의할 점은 프래그먼트는 뷰 보다 오래 지속되기 때문에 프래그먼트의 onDestroyView에서 binding class 인스턴스 참조를 정리해주어야 한다.

    다음과 같은 결과 화면이 나오면 프래그먼트에서 뷰 바인딩을 잘 사용한 것이다.

    <img width="320" alt="04" src="https://user-images.githubusercontent.com/31889335/96569791-8493ae80-1304-11eb-9d78-77969e7f7164.png">

    위 코드는 [android github - viewBinding](https://github.com/android/architecture-components-samples/tree/master/ViewBindingSample) 과 문서를 참고하여 작성하였다!

    > --> [위 코드에 대한 github](https://github.com/choheeis/Android_YoungChaYoungCha/tree/master/ViewBinding)

## 6️⃣ findViewById보다 ViewBinding을 사용해야 좋은 이유

1. null 안전성 보장 : findViewById() 는 인자 안에 개발자가 직접 작성하는 R.id 값을 통해 뷰를 참조한다. 따라서 개발자의 실수로 존재하지 않는 id를 적어주는 경우 null 에러가 발생할 가능성이 있다.

    하지만 ViewBinding은 binding class에 이미 id 값이 있는 뷰들의 참조가 자동으로 되어 있고, 그것을 가져다 사용하기만 하면 되므로 존재하지 않는 id를 참조할 가능성이 없다!

2. type 안전성 보장 : findViewById() 는 id 뿐 아니라 해당 뷰의 type도 개발자가 직접 작성해주어야 했다.

    ~~~kotlin
    // 예시 - \<> 안에 TextView를 써줘야 했음
    val text = view.findViewById<TextView>(R.id.text)
    ~~~

    하지만 binding class가 자동으로 생성되면서 이러한 type에 대한 것도 자동으로 참조되므로 개발자가 직접 작성할 일이 없게 된다.

따라서! 이전에는 개발자가 직접 뷰 바인딩 코드를 작성해야 했고, 발생하는 작은 실수들때문에 레이아웃과 코드 상의 불일치 문제가 종종 발생했었다. 

이러한 에러 및 수정을 위한 시간 낭비를 조금이라도 줄이기 위해 ViewBinding이 등장한 것이고, 사용해야 하는 이유이다!

> --> [android - ViewBinding에 대한 유튜브 소개 영상](https://www.youtube.com/watch?v=W7uujFrljW0)

## 7️⃣ ViewBinding vs DataBinding

후에 알아볼 내용이지만 ViewBinding 말고 DataBinding 이라는 것도 안드로이드는 지원하고 있다.

이 둘의 차이점은 뭘까?

> 이 부분은 data binding 공부 후 작성하자!