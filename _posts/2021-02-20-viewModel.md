---
layout: post
title:  "[안드로이드] ViewModel이 뭐예요?(ViewModel, ViewModelProvider)"
date:   2021-02-20 18:34:10 +0700
categories: [안드로이드]
---

> __참고 자료__
>
> [Android Developer 도큐먼트 - ViewModel 개요](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=ko)
>
> [Youtube 영상 - Android Jetpack : ViewModel](https://www.youtube.com/watch?v=5qlIPTDE274)
>
> [](https://blog.mindorks.com/android-viewmodels-under-the-hood)

<br>

## 0️⃣ 프롤로그

[Android 권장 아키텍처](https://developer.android.com/jetpack/guide)에 관해 공부하고 프로젝트에 적용하려고 보니 생각보다 ViewModel의 이해도가 깊어야 함을 알게 되었습니다. 이 포스팅을 작성하면서 ViewModel을 좀 더 깊이 이해해보고자 합니다😁

또한 이 포스팅에서 알아볼 ViewModel은 Android Architecture Components(AAC)에 속하는 ViewModel입니다.

ViewModel의 빠른 이해를 위해 [이 블로그의 다른 포스팅 - Android Clean Architecture](https://choheeis.github.io/newblog//articles/2020-05/android-clean-architecture)를 읽고 오는 것을 추천드립니다~

## 1️⃣ ViewModel이 등장하기 전 존재했던 문제점

* __AAC의 ViewModel 기원__

    * 사실 ViewModel은 [MVVM](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EB%B7%B0%EB%AA%A8%EB%8D%B8)이라는 소프트웨어 개발 디자인 패턴의 구성요소로 존재했습니다. 즉, ViewModel은 Android 앱 개발에서만 사용되는 개념이 아닙니다.

    * Google은 원래 MVVM 디자인 패턴의 구성 요소로 사용되던 ViewModel을 Android 앱 개발에 적용하기 쉽고 더욱 편리하게 MVVM 패턴을 설계하도록 라이브러리화하여 제공합니다. 이 라이브러리가 바로 [AAC에 속해 있는 ViewModel](https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=ko#declaring_dependencies) 입니다.

    * Google에서 제공하는 ViewModel 라이브러리 종속성을 프로젝트에 추가하면 이 라이브러리가 제공하는 [ViewModel](https://developer.android.com/reference/androidx/lifecycle/ViewModel?hl=ko) 클래스를 사용하여 Android MVVM 디자인 패턴을 쉽게 구현할 수 있습니다.

* __ViewModel을 사용하지 않으면 생길 수 있는 문제점 1__

    * Activity로 구현한 하나의 화면이 있다고 가정해봅시다. 이 화면에서 버튼을 클릭할 때마다 카운트 변수가 1씩 증가하면서 화면에 보여지게 됩니다.

    *   ~~~kotlin
        // MainActivity.kt 파일
        class MainActivity : AppCompatActivity() {
            lateinit var count: TextView
            lateinit var button: TextView
            private var num = 0

            override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                setContentView(R.layout.activity_main)

                count = findViewById(R.id.count)
                button = findViewById(R.id.button)

                button.setOnClickListener {
                    num++
                    count.text = num.toString()
                }
            }
        }
        ~~~

    * (위 코드 참고) 위 코드는 문제점이 생기는 상황을 설명하기 위해 간단히 작성한 테스트 앱(이하 카운트 앱이라고 언급함)의 MainActivity 코드 입니다.

    * 이 화면을 가로로 돌린다면 어떻게 될까요???

    * ![01](https://user-images.githubusercontent.com/31889335/108592861-64a7f080-73b3-11eb-9ad9-0904311fec16.gif)

    * (위 영상 참고) 위 영상에서 볼 수 있듯이 4까지 증가한 카운트 변수는 화면이 가로로 전화되자마자 다시 0으로 초기화됩니다.(~~아닛!!! 이럴 수가!!~~) 화면이 가로로 전환되어도 4로 유지되어야 하는데 유지되지 않으니 이것이 문제점입니다.

    * <img width="942" alt="02" src="https://user-images.githubusercontent.com/31889335/108593090-054ae000-73b5-11eb-8bb6-d974c1332353.png">

    * (위 그림 참고) Activity는 가로 모드로 변경될 경우 Activity의 수명 주기 콜백 메소드 호출이 위 그림과 같은 순서로 호출됩니다. 단지 같은 Activity 화면을 돌렸을 뿐이지만 onPause -> ~ -> onDestroy 콜백 메소드가 호출되고 다시 onCreate -> ~ -> onResume 콜백 메소드가 호출됩니다.

    * 더 자세히는, 세로로 있던 Activity의 인스턴스가 onPause -> ~ -> onDestroy 콜백 메소드를 거쳐 소멸되고 새로운 가로 Activity의 인스턴스가 onCreate -> ~ -> onResume 콜백 메소드를 거쳐 생성되는 것입니다.

    * [onDestroy()](https://developer.android.com/guide/components/activities/activity-lifecycle?hl=ko#ondestroy) 콜백 메소드는 Activity 인스턴스가 소멸되기 직전에 호출되는 메소드이므로 onDestroy() 콜백 메소드가 호출된 후에는 Activity의 인스턴스가 메모리에서 소멸됩니다.

    * 따라서 MainActivity 클래스의 멤버 변수로 존재하며 버튼을 클릭할 때마다 1씩 증가하는 수를 저장하는 num 변수는 MainActivity 클래스의 인스턴스가 메모리에서 사라지면 같이 사라지게 됩니다. 가로 MainActivity 클래스의 인스턴스가 새롭게 생성되면서 이 멤버 변수 num도 새로 생성되게 되어 초기값인 0이 되는 것입니다.

    * 정리하자면 같은 Activity에 대해서 새로운 인스턴스가 생성될 경우 멤버 변수에 저장된 데이터가 유지되지 않는점이 문제였습니다.

    * 이렇게 사용자의 눈에 보이는 즉, 화면에 보이는 데이터를 __UI 관련 데이터(UI 데이터)__ 라고 부릅니다.

* __ViewModel을 이해하기 전, 알아두어야 할 Android 프레임워크의 특징__

    * Android 프레임워크는 Activity와 Fragment 같은 __UI 컨트롤러__ 의 __수명 주기__ 를 관리합니다.

    * 조금 더 자세히는, Android 프레임워크는 사용자의 작업에 의한 응답이나 기기 이벤트에 의한 응답으로 UI 컨트롤러를 제거하기도 하고 다시 만들기도 합니다. 이렇게 Android 프레임워크 시스템에서 자체적으로 UI 컨트롤러를 제거하거나 다시 만드는 경우에는 UI 컨트롤러에 저장된 모든 UI 데이터가 삭제됩니다.

    * UI 데이터가 단순한 데이터일 경우 Activity 클래스에서 제공하는 [onSaveInstanceState()](https://developer.android.com/reference/android/app/Activity?hl=ko#onSaveInstanceState(android.os.Bundle)) 라는 콜백 메소드를 사용하면 UI 컨트롤러가 다시 생성될 때 호출되는 [onCreate()](https://developer.android.com/reference/android/app/Activity?hl=ko#onCreate(android.os.Bundle)) 콜백 메소드에서 인자인 bundle로부터 데이터를 복원할 수 있습니다.(~~onSaveInstanceState() 콜백 메소드와 onCreate() 콜백 메소드에 대한 설명은 이 포스팅에서는 생략할 예정입니다ㅠ~~)

    * 하지만 onSaveInstanceState() 콜백 메소드를 사용하는 방법은 목록(list) 데이터 같은 대용량 데이터에 적합하지 않고 직렬화했다가 다시 역직렬화할 수 있는 소량의 데이터를 대상으로만 적합합니다.

* __ViewModel을 사용하지 않으면 생길 수 있는 문제점 2__

    * UI 데이터를 외부(서버 or 데이터베이스)로부터 가져와서 보여줘야 하는 경우, Activity/Fragment와 같은 UI 컨트롤러(=UI 기반 클래스)에 외부와 통신하는 코드를 작성해야 합니다. 이렇게 UI 컨트롤러에 데이터베이스나 네트워크에서 데이터를 로드하는 코드를 넣는다면 UI 컨트롤러 클래스가 커지게 됩니다.

    * UI 컨트롤러의 기본적인 역할은 UI 데이터를 표시하거나 사용자의 작업에 반응하고(이벤트에 반응) 운영체제와 앱의 커뮤니케이션 (권한 요청 등)을 담당하는 것입니다. 따라서 UI 컨트롤러 클래스는 이러한 기본적인 역할에 해당하는 코드만 포함하도록 하는 것이 좋습니다.

    * 만약 UI 컨트롤러 클래스에 데이터를 외부에서 로드하는 로직을 포함한 다양한 로직이 작성되어 UI 컨트롤러에만 과도한 책임이 몰려있다면 다른 클래스로 작업이 위임되지 않고 UI 컨트롤러 혼자서 앱의 모든 작업을 처리하는 구조가 될 것입니다. 이러한 구조는 테스트와 유지 보수를 어렵게 합니다.

    * 따라서 UI 컨트롤러가 담당했던 책임 중 UI 데이터를 소유하고 관리하는 로직을 ViewModel 역할을 하는 클래스에 부여하여 UI 컨트롤러의 책임을 줄여주는 것이 좋습니다.

## 2️⃣ What is ViewModel?

* __AAC ViewModel__

    * 이 포스팅에서 알아볼 ViewModel은 Android Architecture Components(AAC)에 속해 있는 ViewModel입니다.

    * 더 정확히는 Google이 제공하는 AAC ViewModel 라이브러리에 속한 [ViewModel 클래스](https://developer.android.com/reference/androidx/lifecycle/ViewModel?hl=ko)의 역할을 알아볼 예정입니다.

* __문제점을 해결한 ViewModel__

    * 앞서 알아본 문제점들을 해결하고자 등장한 것이 ViewModel 입니다. 기존에는 Activity 클래스 안에서 관리하던 UI 데이터를 앞으로는 ViewModel 역할을 하는 클래스 안에서 저장하고 관리하게 하는 방법입니다.(ViewModel에서 UI 데이터를 관리하면 문제가 왜 해결되는지는 조금 아래에서 살펴볼 예정입니다)

    * 또한 기존에는 Activity/Fragment 같은 UI 기반 클래스 안에 UI 컴포넌트를 그리는 로직, UI 컴포넌트를 핸들링하는 로직, 외부(서버)로부터 데이터를 가져오는 로직, 데이터를 가공하는 로직 등 다양한 기능을 구현하기 위한 로직이 모두 포함되었습니다. 따라서 Activity/Fragment 클래스 안의 코드가 많아지고 유지 보수가 어려웠다는 문제도 존재했습니다.

    * ViewModel이 등장하면서 UI 데이터 관리 로직을 Activity/Fragment 클래스 안이 아닌 ViewModel 역할을 하는 클래스 안에서 구현하게 되면서 UI 기반 클래스 안에 코드가 많아지고 유지 보수가 어려워지는 문제를 해결할 수 있고, Android 앱 개발의 원칙 중 하나인 [관심사 분리](https://developer.android.com/jetpack/guide?hl=ko#separation-of-concerns) 원칙도 훨씬 명확하게 따를 수 있습니다.

    * UI 기반 클래스에 모든 책임을 부여하는 것이 아니라 ViewModel에게 UI 데이터를 가지고 관리하라는 책임을 나눠줌으로써 관심사를 분리하게 된 것입니다. (Android 앱 개발 원칙에 의하면 Activity/Fragment는 UI를 그리는 책임과 사용자와 상호작용(사용자가 발생시키는 이벤트에 반응하는 것)하는 책임만 부여해야 합니다.)

* __ViewModel 클래스__

    * Google이 제공하는 AAC ViewModel 라이브러리는 [ViewModel](https://developer.android.com/reference/androidx/lifecycle/ViewModel?hl=ko) 이라는 클래스를 제공합니다. 이 클래스를 사용하여 Android MVVM 패턴을 쉽게 구현할 수 있습니다.

    * ViewModel 클래스는 Activity나 Fragment의 수명 주기를 고려하여 UI 관련 데이터를 저장하고 관리할 수 있도록 설계된 클래스입니다. 따라서 ViewModel 클래스를 사용하면 화면 회전과 같은 UI 데이터가 소실될 가능성이 있는 상황에서도 UI 데이터를 유지할 수 있습니다.

    * <img width="365" alt="03" src="https://user-images.githubusercontent.com/31889335/108620333-f0328780-746e-11eb-8daa-6d508c7c5cbf.png">

    * (위 그림 참고) AAC ViewModel 라이브러리에서 제공하는 ViewModel 클래스는 Object 클래스를 상속하는 추상 클래스입니다.

    * <img width="616" alt="04" src="https://user-images.githubusercontent.com/31889335/108623977-427ea300-7485-11eb-9275-cfc2da047f3f.png">

    * (위 그림 참고) ViewModel 클래스의 생성자는 ViewModel() 이고 이 클래스에 구현되어 있는 메소드는 onCleared() 콜백 메소드 하나 뿐 입니다.

    * [onCleared()](https://developer.android.com/reference/androidx/lifecycle/ViewModel?hl=ko#onCleared()) 콜백 메소드는 해당 ViewModel 인스턴스가 더 이상 사용되지 않고 메모리에서 소멸되는 순간 호출됩니다. 따라서 ViewModel 인스턴스가 메모리에서 소멸되면 ViewModel 안에서 실행 중이던 작업들도 실행될 필요가 없으니 onCleared() 콜백 메소드 안에서 실행되던 작업들을 끝내주는 작업을 구현해주면 메모리 누수(Memory Leak)을 예방할 수 있습니다.(이거 뭔 말인지 모르겠음..)

## 3️⃣ ViewModel 구현하기

* __프로젝트에 AAC ViewModel 라이브러리 종속성 추가하기__

    * Google이 제공하는 AAC ViewModel 라이브러리를 프로젝트에 추가해야 ViewModel 클래스를 사용해 안드로이드 MVVM 패턴을 구현할 수 있습니다.

    * [여기](https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=ko#declaring_dependencies)를 참고하여 ViewModel 라이브러리를 프로젝트에 추가하면 됩니다. 그 전에 먼저 [Google Maven 저장소](https://developer.android.com/studio/build/dependencies?hl=ko#google-maven)를 프로젝트에 추가해야 Google Maven 저장소에 저장되어 있는 ViewModel 라이브러리를 가져올 수 있습니다.

    *   ~~~gradle
        // build.gradle 파일(프로젝트 수준)
        allprojects {
            repositories {
                google()
            }
        }

        // build.gradle 파일(앱 모듈 수준)
        dependencies {
            def lifecycle_version = "2.2.0" // 2021년 2월 기준 최신 안정화 버전으로 세팅

            implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
        }
        ~~~

    * (위 코드 참고) AAC ViewModel 라이브러리 종속성을 프로젝트에 추가했다면 이제 ViewModel 라이브러리가 제공하는 ViewModel 클래스를 사용하여 MVVM 패턴에서 ViewModel 역할을 하는 클래스를 구현할 수 있습니다.

* __ViewModel 클래스를 상속받아 ViewModel 역할을 하는 클래스 만들기__

    * AAC ViewModel 라이브러리가 제공하는 ViewModel 클래스를 상속하는 클래스를 만들면 이 클래스가 Activity/Fragment의 수명 주기를 고려하여 UI 데이터를 관리하는 ViewModel 역할을 하는 클래스가 됩니다.

    * <img width="281" alt="05" src="https://user-images.githubusercontent.com/31889335/108624849-c38c6900-748a-11eb-839c-961bfa83d710.png">

    * (위 코드 참고) 위와 같이 ViewModel 클래스를 상속하는 CountViewModel 이라는 클래스를 만들어봤습니다. 이제부터 이 CountViewModel 클래스는 Activity나 Fragment의 수명 주기를 고려하여 UI 데이터를 관리하는 클래스가 됩니다.

    * 앞으로 이어질 내용에서 ViewModel 클래스를 상속한 클래스를 'ViewModel 역할을 하는 클래스' 라고 언급할 것입니다. 'ViewModel 클래스'라고 언급하는 것은 AAC ViewModel 라이브러리에서 제공되는 ViewModel 클래스를 의미하는 것입니다.

* __ViewModel 역할을 하는 클래스가 관리할 UI 데이터 만들어주기__

    * ViewModel 역할을 하는 클래스를 생성했다면 이제 이 클래스에서 저장하고 관리할 UI 데이터 변수를 생성해줘야 합니다.

    * UI 데이터 변수는 보통 AAC에 속한 또 다른 라이브러리인 [LiveData](https://developer.android.com/topic/libraries/architecture/livedata)를 사용하여 ViewModel 역할을 하는 클래스 내부에 생성합니다.(~~이 포스팅에서는 LiveData에 관한 자세한 내용은 설명하지 않을 예정입니다~~)

    * <img width="454" alt="06" src="https://user-images.githubusercontent.com/31889335/108625362-e2402f00-748d-11eb-8f52-46303ff1e683.png">

    * (위 코드 참고) 카운트 앱의 MainActivity에 선언했던 num 변수(UI 데이터 변수)를 이제는 ViewModel 역할을 하는 클래스 내부에 선언하고 이 변수를 LiveData로 감싸면 됩니다.

* __ViewModel 역할을 하는 클래스에서 관리하는 UI 데이터를 UI 컨트롤러로 가져와 사용자에게 보여주기__

    * ViewModel 역할을 하는 클래스에서 관리하는 UI 데이터를 사용자에게 보여주기 위해서는  Activity/Fragment 같은 UI 컨트롤러 클래스로 ViewModel 역할을 하는 클래스에 저장된 UI 데이터를 가져와 적절한 뷰에 보여주는 코드를 작성해야 합니다.

    *   ~~~kotlin
        // 카운터 앱의 MainActivity.kt 파일
        class MainActivity : AppCompatActivity() {
            lateinit var count: TextView
            lateinit var button: TextView
            private var num = 0

            override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                setContentView(R.layout.activity_main)

                count = findViewById(R.id.count)
                button = findViewById(R.id.button)

                // ViewModel 역할을 하는 클래스와 UI 컨트롤러 클래스 연결하기
                val countViewModel: CountViewModel = ViewModelProvider(this).get(CountViewModel::class.java)
            }
        }
        ~~~

    * (위 코드 참고) AAC ViewModel 라이브러리가 제공하는 또 다른 클래스 중 [ViewModelProvider라는 클래스](https://developer.android.com/reference/androidx/lifecycle/ViewModelProvider?hl=ko)가 존재합니다. 이 클래스는 UI 컨트롤러에게 ViewModel 역할을 하는 클래스를 제공하여 UI 컨트롤러와 ViewModel 역할을 하는 클래스를 연결해주는 기능이 구현되어 있는 클래스입니다.

    * 따라서 ViewModelProvider 클래스에서 제공하는 기능을 사용하여 MainActivity와 CountViewModel을 연결해주고 CountViewModel 클래스에 저장된 UI 데이터를 MainActivity 클래스로 가져와야 합니다.

    * ViewModelProvider 클래스가 제공하는 기능을 사용하기 위해서는 먼저 MainActivity 클래스 내에서 ViewModelProvider 클래스의 인스턴스(=객체)를 생성해야 합니다. ViewModelProvider 클래스의 생성자 함수를 호출하여 인스턴스를 생성할 수 있습니다.

    * <img width="833" alt="07" src="https://user-images.githubusercontent.com/31889335/108625874-8fb44200-7490-11eb-9dbc-ab28f12041df.png">

    * (위 그림 참고) ViewModelProvider 클래스의 생성자 함수는 위와 같이 3가지 모습으로 존재합니다. default 생성자 함수인 첫 번째 함수를 호출하여 ViewModelProvider 클래스의 인스턴스를 생성해보겠습니다.(두 번째, 세 번째 생성자 함수를 사용해야 하는 경우도 있으니 상황에 맞는 생성자 함수를 선택해 호출하면 됩니다.) [default 생성자 함수](https://developer.android.com/reference/androidx/lifecycle/ViewModelProvider#ViewModelProvider(androidx.lifecycle.ViewModelStoreOwner))는 인자로 ViewModelStoreOwner라는 인터페이스의 구현체(인스턴스)를 전달해야 합니다. 

    * <img width="822" alt="08" src="https://user-images.githubusercontent.com/31889335/108627129-5a5f2280-7497-11eb-8172-49f7e93f86b6.png">

    * (위 그림 참고) [ViewModelStoreOwner](https://developer.android.com/reference/androidx/lifecycle/ViewModelStoreOwner)은 AAC ViewModel 라이브러리가 제공하는 인터페이스 입니다. 이 인터페이스를 실제로 구현하고 있는 클래스는 많지만 눈여겨볼 클래스는 AppCompatActivity 클래스와 Fragment 클래스입니다.(ComponentActivity까지 추가로 눈여겨봅시다!)

    * ViewModelStoreOwner 인터페이스의 존재 목적은 ViewModelStore를 소유하고 있는 대상을 명시하기 위함입니다.(ViewModelStore에 대해서는 아래 내용에서 이어서 언급할 예정입니다.) 
    
    * <img width="452" alt="11" src="https://user-images.githubusercontent.com/31889335/108634578-8392a980-74bd-11eb-8298-b53c43df7276.png">

    * (위 그림 참고) ViewModelStoreOwner 인터페이스 내부에는 getViewModelStore() 라는 추상 메소드 하나가 정의되어 있습니다. 따라서 ViewModelStoreOwner 인터페이스를 구현하는 클래스들은 클래스 내부에 getViewModelStore() 함수를 오버라이딩하여 구현하고 있을 것입니다. 

    *   ~~~java
        // ComponentActivity 클래스 내부에 구현되어 있는 getViewModelStore() 함수
        // Java 코드

        /**
        * Returns the {@link ViewModelStore} associated with this activity
        *
        * @return a {@code ViewModelStore}
        * @throws IllegalStateException if called before the Activity is attached to the Application
        * instance i.e., before onCreate()
        */
        @NonNull
        @Override
        public ViewModelStore getViewModelStore() {
            if (getApplication() == null) {
                throw new IllegalStateException("Your activity is not yet attached to the "
                        + "Application instance. You can't request ViewModel before onCreate call.");
            }
            if (mViewModelStore == null) {
                NonConfigurationInstances nc =
                        (NonConfigurationInstances) getLastNonConfigurationInstance();
                if (nc != null) {
                    // Restore the ViewModelStore from NonConfigurationInstances
                    mViewModelStore = nc.viewModelStore;
                }
                if (mViewModelStore == null) {
                    mViewModelStore = new ViewModelStore();
                }
            }
            return mViewModelStore;
        }
        ~~~
    
    * (위 코드 참고) MainActivity 클래스는 보통 AppCompatActivity 클래스를 상속하는데 AppCompatActivity 클래스가 상속하는 부모 클래스를 타고 타고 들어가면  [ComponentActivity 라는 클래스](https://android.googlesource.com/platform/frameworks/support/+/8514bc0f4d930b5470435aa365719b2a6a3ad2f3/activity/src/main/java/androidx/activity/ComponentActivity.java)가 존재합니다. 이 클래스 내부에 ViewModelStoreOwner 인터페이스에 정의되어 있는 getViewModelStore() 메소드가 실제로 구현되어 있습니다. 따라서 ComponentActivity 클래스를 상속하는 서브 클래스인 MainActivity 클래스에서도 getViewModelStore() 메소드를 바로 호출할 수 있습니다.

    * (위 코드 참고) getViewModelStore() 메소드의 return 값을 보니 해당 Activity와 연관된 ViewModelStore 클래스의 인스턴스를 반환하고 있습니다.(예시로 ComponentActivity 클래스 내에 구현된 getViewModelStore() 함수를 보고 있기 때문에 Activity와 연관된 ViewModelStore 클래스의 인스턴스를 반환하고 있지만 Fragment 클래스 내에 구현된 getViewModelStore() 함수는 해당 프래그먼트와 관련된 ViewModelStore 클래스의 인스턴스를 반환할 것입니다.) 여기서 [ViewModelStore](https://developer.android.com/reference/androidx/lifecycle/ViewModelStore) 는 AAC ViewModel 라이브러리에서 제공하는 또 다른 클래스입니다. 이 클래스는 ViewModel 역할을 하는 클래스의 인스턴스들이 UI 컨트롤러의 수명 주기에 관계 없이 어딘가에 지속적으로 저장되도록 설계된 클래스입니다. 

    *   ~~~java
        // ViewModelStore 클래스
        // Java 코드
        public class ViewModelStore {
            private final HashMap<String, ViewModel> mMap = new HashMap<>();

            final void put(String key, ViewModel viewModel) {
                ViewModel oldViewModel = mMap.put(key, viewModel);
                if (oldViewModel != null) {
                    oldViewModel.onCleared();
                }
            }
            final ViewModel get(String key) {
                return mMap.get(key);
            }
            /**
            *  Clears internal storage and notifies ViewModels that they are no longer used.
            */
            public final void clear() {
                for (ViewModel vm : mMap.values()) {
                    vm.onCleared();
                }
                mMap.clear();
            }
        }
        ~~~

    * (위 코드 참고) [ViewModelStore.java](https://android.googlesource.com/platform/frameworks/support/+/a9ac247af2afd4115c3eb6d16c05bc92737d6305/lifecycle/viewmodel/src/main/java/androidx/lifecycle/ViewModelStore.java) 클래스 코드를 직접 봐보면 HashMap 자료구조를 사용하여 HashMap 안에 key-value 쌍으로 ViewModel들의 인스턴스를 저장하고 있음을 확인할 수 있습니다. ViewModel 클래스를 상속하여 만든 ViewModel 역할을 하는 클래스들의 인스턴스가 ViewModelStore 클래스의 HashMap에 저장되는 것입니다. 따라서 이 ViewModelStore 클래스의 인스턴스가 메모리에 남아있는 한 UI 컨트롤러 인스턴스가 메모리에서 소멸되고 다시 생성되어도 해당 UI 컨트롤러와 연결된 ViewModel 인스턴스를 HashMap에서 다시 찾을 수 있는 것입니다.(즉, ViewModel 역할을 하는 클래스에 UI 데이터를 저장할 경우 UI 데이터가 소실되지 않습니다.)

    *   ~~~kotlin
        // 카운터 앱의 MainActivity.kt 파일
        class MainActivity : AppCompatActivity() {
            lateinit var count: TextView
            lateinit var button: TextView
            private var num = 0

            override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                setContentView(R.layout.activity_main)

                count = findViewById(R.id.count)
                button = findViewById(R.id.button)

                // ViewModel 역할을 하는 클래스와 UI 컨트롤러 클래스 연결하기
                val countViewModel: CountViewModel = ViewModelProvider(this).get(CountViewModel::class.java)
            }
        }
        ~~~

    * (위 코드 참고) 이제 다시 ViewModelProvider 생성자 함수를 호출하는 내용으로 돌아가 봅시다. ViewModelProvider 생성자 함수의 인자로 전달해야 하는 ViewModelStoreOwner 인터페이스의 구현체는 MainActivity 자신입니다.(MainActivity의 부모 클래스인 ComponentActivity 클래스에서 이 인터페이스를 구현하고 있기 때문입니다.) 따라서 MainActivity 자기 자신을 가리키는 __this__ 키워드를 ViewModelProvider 생성자 함수의 인자로 전달하면 됩니다. 
    
    *   ~~~kotlin
        // 카운터 앱의 MainActivity.kt 파일 onCreate() 메소드 내부 발췌
        val countViewModel: CountViewModel = ViewModelProvider(this).~~
        ~~~

    * (위 코드 참고) 이러한 방법으로 ViewModelProvider 클래스의 생성자 함수를 호출함으로써 ViewModelProvider 클래스의 인스턴스를 생성하게 되고 이 클래스에서 제공하는 기능을 사용하여 UI 컨트롤러와 ViewModel 역할을 하는 클래스를 연결해줄 수 있습니다.(일반적으로 Activity에서는 onCreate() 콜백 메소드 내에서 ViewModel 인스턴스를 요청합니다.)

    * <img width="873" alt="10" src="https://user-images.githubusercontent.com/31889335/108633309-f3516600-74b6-11eb-8d4f-7f7f6d026e91.png">

    * (위 그림 참고) ViewModelProvider 클래스에서 제공하는 get() 메소드를 호출하면 ViewModelProvider 인스턴스 생성시 생성자 함수 인자로 전달했던 ViewModelStoreOwner(Activity 또는 Fragment)과 연관된 ViewModel 인스턴스를 반환합니다. ViewModelStoreOwner와 연관된 ViewModel 인스턴스가 여러 개 존재할 수도 있으니 get() 함수 인자로 원하는 ViewModel 클래스 명을 전달해야 해당 ViewModel 클래스의 인스턴스를 반환하여 UI 컨트롤러에 전달할 수 있습니다.

    * 만약 get() 함수 인자로 전달한 ViewModel 클래스 명과 일치하는 ViewModel 인스턴스가 ViewModelStore에 존재하지 않을 경우 ViewModel 인스턴스를 새로 생성한 후 UI 컨트롤러에게 반환하고 이미 존재하는 ViewModel 클래스일 경우 이미 존재하는 ViewModel 인스턴스를 UI 컨트롤러에게 반환합니다.
    
* __ViewModel 역할을 하는 클래스로부터 UI 데이터 가져와 뷰에 세팅해주기__

    * 위에서 알아본 방법으로 MainActivity의 onCreate() 콜백 메소드 내부에서 CountViewModel 클래스의 인스턴스를 가져왔습니다.

    * 그렇다면 이제 UI 컨트롤러(=MainActivity)는 CountViewModel 클래스에 저장되어 있는 UI 데이터인 num 변수를 사용하여 적합한 뷰에 데이터를 세팅해주면 됩니다.

    * 이를 위해 CountViewModel 클래스 외부에서도 CountViewModel 클래스 내부의 private 변수인 num 변수에 접근할 수 있게 새로운 함수 하나를 구현해줍시다.(private 변수에 접근할 수 있도록 getter/setter 형식의 함수를 구현하기)

    *   ~~~kotlin
        // CountViewModel.kt 파일
        class CountViewModel : ViewModel() {
            private val num: MutableLiveData<Int> = MutableLiveData()
            
            // CountViewModel 클래스 외부에서 num 변수에 접근할 수 있도록 num 변수를 반환하는 public 함수 구현해주기
            fun getNum(): LiveData<Int> {
                return num
            }
        }
        ~~~

    * (위 코드 참고) LiveData\<Int> 타입의 인스턴스를 반환하는 getNum() 함수를 생성하였고, 외부에서도 이 함수에 접근할 수 있도록 public 함수로 구현했습니다.(~~이 포스팅에서는 LiveData 관련된 부분은 설명하지 않습니다ㅠㅠ ViewModel에 관한 이해에 더 집중할 것이니 LiveData 부분은 대충 이런게 있구나.. 하고 넘어가도 됩니다~~)

    *   ~~~kotlin
        class MainActivity : AppCompatActivity() {
            lateinit var count: TextView
            lateinit var button: TextView
            private var num = 0

            override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                setContentView(R.layout.activity_main)

                count = findViewById(R.id.count)
                button = findViewById(R.id.button)

                val countViewModel: CountViewModel = ViewModelProvider(this).get(CountViewModel::class.java)

                // CountViewModel 클래스가 관리하는 UI 데이터를 MainActivity로 가져오기
                countViewModel.getNum().~~
            }
        }
        ~~~

    * 이제 다시 UI 컨트롤러(MainActivity)로 돌아와 CountViewModel 클래스가 제공하는 getNum() 함수를 호출함으로써 UI 데이터인 num 변수를 가져올 수 있습니다.

    *   ~~~kotlin
        class MainActivity : AppCompatActivity() {
            lateinit var count: TextView
            lateinit var button: TextView
            private var num = 0

            override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                setContentView(R.layout.activity_main)

                count = findViewById(R.id.count)
                button = findViewById(R.id.button)

                val countViewModel: CountViewModel = ViewModelProvider(this).get(CountViewModel::class.java)

                // MainActivity가 CountViewModel 클래스가 관리하는 UI 데이터를 관찰하고 있다가 값에 변화가 있으면 변한 값을 TextView에 세팅해주기
                countViewModel.getNum().observe(this, Observer { // it: Int
                    count.text = it.toString()
                })
            }
        }
        ~~~

    * (위 코드 참고) 이전에 CountViewModel 클래스가 저장하고 관리하는 UI 데이터를 LiveData라는 것으로 감싸 선언했었습니다. LiveData가 제공하는 observe() 라는 함수를 사용하여 CountViewModel 클래스에 저장되어 있는 UI 데이터의 값이 바뀌는지 바뀌지 않는지를 UI 컨트롤러에서 관찰할 수 있습니다. 즉, UI 컨트롤러에서 UI 데이터를 계속 관찰하고 있는 것입니다. 관찰하다가 UI 데이터의 값이 바뀌면 observe() 함수의 인자로 전달한 observer의 onChanged() 함수가 실행됩니다. 따라서 onChanged() 함수 내에 MainActivity의 TextView에 UI 데이터를 세팅하는 코드를 작성해주면 됩니다.(~~이 내용은 LiveData와 밀접한 내용이므로 이 포스팅에서는 그냥 그렇구나.. 하고 넘어갑시다,,,ㅋㅋ큐ㅠ~~)

## 4️⃣ ViewModel의 수명 주기

* __ViewModel도 수명 주기를 가지고 있다!__

    * AAC ViewModel 라이브러리가 제공하는 ViewModel 클래스를 상속하여 MVVM 패턴의 ViewModel을 구현하면 UI 컨트롤러의 수명 주기에 관계 없이 UI 데이터를 유지할 수 있다고 했습니다.

    * UI 데이터를 유지할 수 있는 이유는 ViewModel 인스턴스가 살아있는 수명 주기가 UI 컨트롤러보다 길기 때문입니다. 위에서 ViewModel 인스턴스는 ViewModelStore 클래스의 HashMap 자료 구조 안에 저장됨을 알아보았습니다. 또한 ViewModelStore 인스턴스는 UI 컨트롤러 인스턴스가 메모리에서 소멸되어도 같이 사라지지 않기 때문에 UI 컨트롤러 인스턴스가 재생성될 때 메모리에 아직 살아있는 ViewModelStore 에서 해당 UI 컨트롤러와 연결된 ViewModel을 다시 가져옵니다.

    * 그럼 ViewModel 인스턴스는 메모리에 언제까지 남아있게 되는 걸까요??? ViewModel 인스턴스가 메모리에 남아 있는 기간을 ViewModel의 수명 주기라고 부릅니다.

    * ViewModel 인스턴스의 수명 주기는 ViewModelProvider를 통해 ViewModel 인스턴스를 가져올 때 ViewModelProvider 생성자 함수의 인자로 전달하는 ViewModelStoreOwner의 수명 주기를 따르게 됩니다. 

    * <img width="379" alt="12" src="https://user-images.githubusercontent.com/31889335/108635864-1b47c600-74c5-11eb-8768-6f532c51f3cd.png">

    * (위 그림 참고) 만약 ViewModelProvider 생성자 함수로 전달된 ViewModelStoreOwner가 Activity였다면 해당 ViewModel 인스턴스 수명 주기는 Activity의 수명 주기가 완전히 끝날 때까지 입니다. Activity의 수명 주기가 완전히 끝나는 시점은 위 그림에서 볼 수 있듯이 Finished 상태가 된 시점입니다. 따라서 이 때의 ViewModel 인스턴스는 Activity가 완전히 끝나지 않으면 계속 메모리에 남아있게 됩니다.

    * 따라서 Activity의 수명 주기를 따르는 ViewModel 인스턴스는 Activity 화면이 가로로 회전하여 onPause() -> ~ -> onDestroy() -> onCreate() -> ~ -> onResume() 콜백 메소드가 호출되는 동안에도 메모리에 남아있기 때문에 ViewModel 클래스에 저장된 UI 데이터는 소실되지 않는 것입니다. onDestroy()에 의해 Acitivity의 인스턴스가 메모리에서 사라지고 다시 새로운 인스턴스로 생성되어 onCreate()가 호출되더라도 이 새로운 Activity의 인스턴스는 아직 남아있는 ViewModel 인스턴스에 재연결됩니다.

    * Activity에서 ViewModel 인스턴스를 가져오는 코드가 onCreate() 콜백 메소드 내에 구현되는 이유도 화면이 회전되어 새로운 인스턴스가 생성되면서 onCreate() 콜백 메소드가 다시 호출될 때 ViewModel을 재연결시켜주기 위함입니다.

    * Fragment의 수명 주기를 따르는 ViewModel 인스턴스는 Fragment가 Activity에서 분리될 때까지 메모리에 살아있습니다.

    * 만약 ViewModelStoreOwner이 완전히 소멸될 경우(Activity의 경우에는 Finished 상태가 될 경우) Android 프레임워크는 ViewModel 인스턴스를 메모리에서 제거합니다. 이 때 ViewModelStore 클래스에서 제공하는 [clear()](https://developer.android.com/reference/androidx/lifecycle/ViewModelStore#clear()) 메소드가 자동으로 호출되어 ViewModel들을 저장하고 있는 HashMap을 비우고 ViewModel들에게 더 이상 자신들이 사용되지 않음을 알립니다.

* __ViewModel 역할을 하는 클래스는 Activity/Fragment의 뷰를 직접 참조해서는 안됩니다!__

    * ViewModel 역할을 하는 클래스의 존재 목적은 UI 데이터를 저장하고 관리(UI 데이터를 외부로 넘길 수 있게 하거나 UI 데이터 값을 변경함)하는 것 뿐입니다.

    * 이 때 주의해야 할 점은 ViewModel 역할을 하는 클래스 내부에서 Activity/Fragment의 뷰 인스턴스를 직접 참조해서는 안 된다는 것입니다. 다시 말하면 ViewModel 역할을 하는 클래스는 자신이 어떤 Activity/Fragment와 연결되고 자신이 가지고 있는 UI 데이터가 어떤 뷰에 세팅되는지에 관한 코드가 절대 작성되면 안 된다는 것입니다.

    * 만약 ViewModel 역할을 하는 클래스 내부에서 Activity의 뷰 인스턴스를 직접 참조한다고 가정해봅시다. 이 때 해당 Activity의 화면을 가로로 회전하여 세로 Activity 인스턴스가 메모리에서 제거되었다면 어떻게 될까요???

    * 세로 Activity 인스턴스가 메모리에서 사라지더라도 ViewModel 인스턴스는 메모리에 그대로 남아있게 됩니다. 이렇게 아직 살아있는 ViewModel 역할을 하는 클래스는 이제 이미 사라지고 없는 Activity의 뷰 인스턴스를 계속 참조하게 됩니다. 이것이 바로 메모리 누수(Memory Leak) 현상 입니다.

> https://blog.mindorks.com/android-viewmodels-under-the-hood
https://blog.mindorks.com/shared-viewmodel-in-android-shared-between-fragments

