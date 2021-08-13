---
layout: post
title:  "[안드로이드] View Binding이 뭐예요?"
date:   2020-09-22 18:34:10 +0700
categories: [안드로이드]
---

> __참고 자료__  
> [Android Developers - View Binding](https://developer.android.com/topic/libraries/view-binding)  
> [Youtube Android Developers - Replace findViewById with view binding](https://www.youtube.com/watch?v=W7uujFrljW0)  
> [Medium Android Developers - Now in Android #4, view binding](https://medium.com/androiddevelopers/now-in-android-4-23b0d6cdea40)  
> [Medium Android Developers - Use view binding to replace findViewById](https://medium.com/androiddevelopers/use-view-binding-to-replace-findviewbyid-c83942471fc)  
> [Github - ViewBindingSample](https://github.com/android/architecture-components-samples/tree/master/ViewBindingSample)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. What is View Binding?](#1)  
> [2. 자동으로 생성되는 binding class에 대해 집중적으로 알아보자!](#2)  

<br>

## 0️⃣ 프롤로그<a id="0"></a>

나도 이제.. findViewById() 그만 사용하자!!!!

## 1️⃣ What is View Binding?<a id="1"></a>

* View Binding은 뷰와 상호작용 하는 코드를 이전보다 더 쉽게 사용할 수 있도록 해주는 녀석이다. 아주 간단하게 View Binding의 장점을 말해보자면, "View Binding을 사용함으로써 __findViewById() 메소드를 호출하지 않아도 된다__" 라고 말할 수 있다.

* ~~~kotlin
  var textView: TextView = findViewById(R.id.text1) // 해당 뷰 객체의 참조값을 가져옴
  textView.text = "안녕"
  // 또는
  var textView = findViewById<TextView>(R.id.text1)
  textView.text = "안녕"
  // 또는
  var textView = findViewById(R.id.text1) as TextView
  textView.text = "안녕"
  ~~~

* <위 코드 참고> 안드로이드 개발자 뿐 아니라, 모든 개발자들은 반복적인 코드 작성을 피하려는 경향이 있고 피해야만 한다. 보통 __boilerplate code(보일러 플레이트 코드)__ 라고 불리는, 최소한의 변경으로 여러곳에서 재사용되며 반복적으로 비슷한 형태를 띄는 코드를 작성하는 것은 참 귀찮고 하기 싫은 작업이다. 안드로이드 개발에도 boilerplate code 급으로 반복적으로 사용되는 코드가 있는데 그게 바로 findViewById() 메소드를 호출하는 코드이다. 특정 뷰 객체의 참조를 사용해야 할 때마다 안드로이드 개발자는 해당 뷰의 정확한 ID를 가지고 위와 같은 코드를 작성해야했다. 만약 하나의 클래스 내에서 참조값을 가져와야 하는 서로 다른 뷰 객체의 개수가 100개라면, findViewById() 메소드를 호출하는 코드를 100번(100줄을 차지할 것...) 작성해야 할 것이다.
    
* 이러한 반복적인 코드 작성의 불편함을 해결하기 위해 Google I/O 2019 에서는 View Binding이라는 새로운 메커니즘을 소개했다! View Binding이라는 메커니즘은 __모든 xml 파일에 정의된 뷰들의 객체 참조를 처음부터 어딘가에 미리 가져와놓고, Activity나 Fragment 같은 클래스 내 코드 상에서는 원하는 뷰 객체의 참조를 간편하게 바로 가져다 쓸 수 있게 하자!__ 이다.
    
* ~~~build.gradle
  // build.gradle(App 모듈 수준)
  android {
      ...
      buildFeatures {
          viewBinding = true
      }
  }
  ~~~
    
* <위 코드 참고> View Binding을 사용하려면 app 모듈 수준의 build.gradle 파일에 위와 같이 "View Binding을 사용하겠어여~ true~~" 라고 작성해주면 된다. 그런데 보통 새로운 기능은 라이브러리 형태로 제공되는 경우가 많아서, 그것을 사용하기 위해 app 모듈 레벨의 build.gradle 파일의 dependencies{ } 블럭 안에 종속성을 추가해줘야 하는 경우가 많지 않나? 왜 View Binding은 라이브러리 종속성 추가가 아닌 위와 같은 정의를 통해 사용할 수 있는 것일까? View Binding은 Gradle(=안드로이드 앱 빌드 툴)이 제공하는 Android Gradle Plugin에 포함되어 등장했기 때문이다! 따라서 앱 빌드 시 Android Gradle Plugin에 포함된 View Binding을 사용하겠다는 의미로 위 코드처럼 빌드 옵션으로써 작성해주면 되는 것이다. (정확히는 Android Gradle Plugin 3.6 버전부터 View Binding이 포함되었다..!)
  
* ~~~kotlin
  // activity_main.xml 예시
  <ConstraintLayout>
      <TextView android:id="@+id/hello" />
      <Button android:id="@+id/button1" />
  </ConstraintLayout>
                            
  ⬇️
                            
  // ActivityMainBinding.java 클래스 예시
  public final class ActivityMainBinding implements ViewBinding  {
      @NonNull 
      private final ConstraintLayout rootView; // rootView 라는 변수는 항상 자동으로 선언됨
      @NonNull 
      private final TextView hello;
      @NonNull 
      private final Button button1;
  }
  ~~~
  
* <위 코드 참고> build.gradle 파일에 View Binding을 사용하겠다는 정의를 해두면 앱 빌드 시 컴파일러가 자동으로 해당 모듈 내의 모든 xml 파일에 대해, 뷰 객체들의 참조를 어딘가에 미리 가져와 놓는다. 여기서 '어딘가에' 라는 곳이 그래서 어딜까? 예를 들어, 만약 activity_main.xml 라는 파일이 App 모듈 내에 존재한다면 컴파일러는 ActivityMainBinding.java이라는 이름의 binding class를 자동으로 생성한다. 이 클래스의 멤버 변수에 activity_main.xml에 정의된 모든 뷰(단, ID가 정의되어 있지 않은 뷰는 제외) 객체의 참조값이 저장된다. activity_main.xml 파일 뿐만 아니라 모든 xml 파일에 대응되는 binding class가 하나씩 생성되는 것이다. (binding class의 이름은 자동으로 해당 xml 파일의 이름을 Pascal 형태로 바꾼 다음 가장 끝에 "Binding"을 붙여서 지어진다.) 
  
* <위 코드 참고> binding class는 기본적으로 자바 클래스로 생성되지만 이 때 자동으로 @Nullable이나 @NonNull 같은 어노테이션들이 변수에 붙여지도록 생성되기 때문에 코틀린에서 자바 코드인 binding class를 호출할 때, 각 멤버 변수들에 Null이 들어갈 수 없음을 판단할 수 있다.(자바는 변수 값에 null이 들어갈 수 있고, 코틀린은 null이 들어갈 수 없기 때문에 코틀린에서 인식할 수 있는 어노테이션들을 자바 코드에 붙여줌으로써 null 허용 여부의 차이를 맞춰줘야 코틀린에서 자바 코드를 호출할 수 있다.)
  
* ~~~kotlin
  // binding class 예시(원래는 자바 코드여야 하는데 보기 쉽게 코틀린으로 작성)
  class ActivityMainBinding {
      val hello: Button // 1. TextView인데 Button으로 타입이 정의되는 경우 없을 것! = type-safe
      // 또는
      val hello: TextView = findViewById(R.id.존재하지 않는 ID) // 2. 존재하지 않는 ID로 참조하여 null을 가지게 되는 경우 없을 것 = null-safe
  }
  ~~~
                            
* <위 코드 참고> View Binding의 장점은 크게 "type-safe"와 "null-safe" 이다. View Binding은 개발자가 아닌 컴파일러가 뷰 객체의 참조(reference)값를 가져오기 때문에 뷰 객체의 타입이 다르게 정의된다거나 ID 값이 틀리거나 존재하지 않아서 null값을 가져오게 된다거나 하는 문제가 발생하지 않는다. (위 코드의 주석으로 작성해놓은 내용들은 findViewById() 메소드를 호출하는 코드를 작성할 때 개발자들이 가장 많이 하는 실수이다.)
  
## 2️⃣ 자동으로 생성되는 binding class에 대해 집중적으로 알아보자!<a id="2"></a>
  
* binding class에는 xml 파일에 정의되어 있고 ID가 존재하는 모든 뷰들의 참조값이 변수에 저장된다. 따라서 개발자는 Activity나 Fragment 같은 클래스에서 binding class의 인스턴스를 생성한 후, 해당 binding class 내의 멤버 변수를 그냥 가져다 사용하기만 하면 된다.
  
* binding class 안에는 __inflate()__ 라는 public static 멤버 메소드도 자동으로 정의된다. 이 메소드는 해당 레이아웃의 inflate 작업을 해주고 동시에 binding class의 인스턴스를 생성하여 반환해주는 메소드이다.(안드로이드에서 inflate의 정의는 xml에 정의된 뷰들을 메모리에 객체화시키는 행위이다. 예를 들어 xml 파일에 정의되어 있는 어떤 textView는 [TextView](https://developer.android.com/reference/android/widget/TextView) 클래스의 객체로 생성되어야만 실제로 사용할 수 있게 된다. 즉, xml에 정의된 뷰들을 객체화해서 코드에서 사용할 수 있게 하는 행위가 inflate이다.)
  
* binding class 안에는 __getRoot()__ 라는 public 멤버 메소드도 자동으로 정의되는데, 이 메소드는 해당 xml 파일의 root view 객체의 참조값을 반환한다. (root view는 xml 파일에 존재하는 모든 뷰들 중 가장 top에 존재하는 뷰이다.)
  
* binding class 안에는 __bind()__ 라는 public 멤버 메소드도 자동으로 정의된다. 이 메소드는 이미 Activity나 Fragment에 레이아웃이 inflate된 경우, inflate 작업 없이 뷰 객체 참조만을 가지고 있는 메소드이다.
  
* ~~~kotlin
  // MainActivity.kt 에서 view binding 사용 예시
  override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
  
      val binding = ActivityMainBinding.inflate(layoutInflater) // 1. inflate() 메소드를 호출하여 activity_main.xml 파일의 뷰들을 객체화하고 binding class의 인스턴스를 반환한다.
      binding.hello.text = "안녕" // 2. findViewById() 호출 없이, binding class 내 멤버 변수인 hello를 바로 사용함으로써 뷰 객체의 참조값을 가져온다.
      ... 
  
      val view = binding.root // 3. getRoot() 메소드를 호출하여 root view 객체의 참조값을 가져온다.
      setContentView(view) // 4. MainActivity라는 액티비티의 레이아웃이 activity_main.xml 임을 알린다.
  }
  ~~~
  
* <위 코드 참고> view binding을 사용하기 위한 시작은 binding class의 멤버 메소드인 inflate()를 호출하여 뷰들을 객체화시키고 binding class의 인스턴스를 가져오는 것이 시작이다. 그 다음, binding class의 멤버 메소드인 getRoot()를 호출하여 해당 xml 파일의 root view 객체의 참조값을 가져온 후, onCreate() 메소드 내의 setContentView() 메소드의 인자로 참조값을 넘겨준다. 인자로 넘겨준 참조값에 해당하는 레이아웃을 MainActivity라는 액티비티의 레이아웃으로 사용하겠다고 알리는 것이다. (view binding 사용하지 않을 때에는 onCreate() 메소드 내에 setContentView(R.layout.activity_main) 라는 코드가 기본적으로 생성된다. 기존의 이 작업을 view binding에서는 root view 객체 참조값을 setContentView() 인자로 넘겨주는 것!)
  
  
* Activity에서 view binding을 사용하는 방법과 Fragment에서 view binding을 사용하는 방법은 조금 다르다. 이 부분은 [Android Developers - View Binding](https://developer.android.com/topic/libraries/view-binding)를 보면 알 수 있으므로 설명은 생략..ㅎ.ㅎ

# 끝!
  
이 포스팅에서 생략했던 view binding에 대한 더 깊은 내용들은 [Medium Android Developer - Use view binding to replace findViewById](https://medium.com/androiddevelopers/use-view-binding-to-replace-findviewbyid-c83942471fc)에 자세히 나와있다! 참고한 문서들 중 이 문서가 가장 깊고 자세한 듯!
  
마지막 수정일 : 2021년 8월 14일
