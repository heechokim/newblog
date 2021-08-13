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
  var textView: TextView = findViewById(R.id.text1)
  textView.text = "안녕"
  // 또는
  var textView = findViewById<TextView>(R.id.text1)
  textView.text = "안녕"
  // 또는
  var textView = findViewById(R.id.text1) as TextView
  textView.text = "안녕"
  ~~~

* <위 코드 참고> 안드로이드 개발자 뿐 아니라, 모든 개발자들은 반복적인 코드 작성을 피하려는 경향이 있고 피해야만 한다. 보통 __boilerplate code(보일러 플레이트 코드)__ 라고 불리는, 최소한의 변경으로 여러곳에서 재사용되며 반복적으로 비슷한 형태를 띄는 코드를 작성하는 것은 참 귀찮고 하기 싫은 작업이다. 안드로이드 개발에도 boilerplate code 급으로 반복적으로 사용되는 코드가 있는데 그게 바로 findViewById() 메소드를 호출하는 코드이다. 뷰 계층(hierarchy)안에 존재하는 특정 뷰의 참조를 사용해야 할 때마다 안드로이드 개발자는 해당 뷰의 정확한 ID를 가지고 위와 같은 코드를 작성해야했다. 만약 참조해야 하는 서로 다른 뷰의 개수가 100개라면 findViewById() 메소드를 호출하는 코드를 100번(100줄을 차지할 것...) 작성해야 한다.
    
* 이러한 불편함을 해결하기 위해 Google I/O 2019 에서는 View Binding이라는 새로운 메커니즘을 소개했다! View Binding이라는 메커니즘은 __모든 XML 레이아웃에 정의된 뷰들의 참조를 처음부터 어딘가에 만들어놓고, 코드 상에서는 바로 원하는 뷰를 가져다 쓸 수 있게 하자!__ 이다. 따라서 View Binding을 사용하면 findViewById() 메소드를 호출하여 사용하려는 뷰의 참조를 가져오는 코드 자체를 작성할 필요가 없게 된다.
    
* ~~~build.gradle
  // build.gradle(App 모듈 수준)
  android {
      ...
      buildFeatures {
          viewBinding = true
      }
  }
  ~~~
    
* <위 코드 참고> 이렇게나 좋아보이는 View Binding을 사용하려면 App 모듈 레벨의 build.gradle 파일에 위와 같이 "View Binding을 사용하겠어여~ true~~" 라고 작성해주면 된다. 그런데 보통 새로운 기능은 라이브러리 형태로 제공되는 경우가 많아서, 그것을 사용하기 위해 App 모듈 레벨의 build.gradle 파일의 dependencies{ } 블럭 안에 종속성을 추가해줘야 하는 경우가 많지 않나? 왜 View Binding은 라이브러리 종속성 추가가 아닌 위와 같은 정의를 통해 사용할 수 있는 것일까? View Binding은 Gradle(=안드로이드 앱 빌드 툴)이 제공하는 Android Gradle Plugin에 포함되어 등장했기 때문이다! 따라서 앱 빌드 시 Android Gradle Plugin에 포함된 View Binding 이라는 것을 사용하겠다는 의미로 위 코드처럼 빌드 옵션으로써 작성해주면 되는 것이다. (정확히는 Android Gradle Plugin 3.6 버전부터 View Binding이 포함되었다..!)
  
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
  
* <위 코드 참고> View Binding을 사용하겠다는 정의를 해주면 앱 빌드 시 컴파일러가 자동으로 프로젝트 내의 모든 XML 레이아웃에 대해, 뷰들의 참조를 어딘가에 생성해놓는다. 여기서 '어딘가에' 라는 곳이 그래서 어딜까? 예를 들어, 만약 activity_main.xml 라는 레이아웃 파일이 프로젝트 내에 존재한다면 컴파일러는 ActivityMainBinding.java이라는 이름의 binding class를 자동으로 생성하고 이 클래스 안에 activity_main.xml 에 존재하는 모든 뷰(단, ID가 정의되어 있지 않은 뷰는 제외)의 참조를 생성하여 가지고 있는다. activity_main.xml 파일 뿐만 아니라 모든 XML 파일에 하나씩 대응되는 binding class가 생성되는 것이다. (binding class의 이름은 자동으로 해당 xml 파일의 이름을 Pascal 형태로 바꾼 다음 가장 끝에 "Binding"을 붙여서 지어진다.) 
  
* binding class는 기본적으로 자바 클래스로 생성되지만 이 때 자동으로 @Nullable이나 @NonNull 같은 어노테이션들이 변수에 붙여지도록 생성되기 때문에 코틀린에서 자바 코드를 호출할 때 해당 변수들에 Null이 들어갈 수 없음을 판단할 수 있다.(자바는 변수 값에 null이 들어갈 수 있고, 코틀린은 null이 들어갈 수 없기 때문에 Kotlin에서 인식할 수 있는 어노테이션들을 자바 코드에 붙여줌으로써 null 허용 여부의 차이를 맞춰줘야 코틀린에서 자바 코드를 호출할 수 있다.)
  
* ~~~kotlin
  // View Binding의 장점
  class ActivityMainBinding {
      val hello: Button // TextView인데 Button으로 타입이 정의되는 경우 없을 것! = type-safe
      // 또는
      val hello: TextView = findViewById(R.id.존재하지 않는 ID) // 존재하지 않는 ID로 참조하여 null을 가지게 되는 경우 없을 것 = null-safe
  }
  ~~~
                            
* <위 코드 참고> View Binding은 뷰들의 참조(reference)를 개발자가 아닌 컴파일러가 자동으로 생성하여 가지고 있는 것이기 때문에 뷰 타입이 다르게 참조된다거나 ID 값이 틀리거나 존재하지 않아서 null을 참조하게 된다거나 하는 문제가 발생하지 않는다는 장점이 있다. 
  
## 2️⃣ 자동으로 생성되는 binding class에 대해 집중적으로 알아보자!<a id="2"></a>
  
* binding class에는 해당 xml 파일에 정의되어 있고 ID가 존재하는 모든 뷰들의 참조가 생성된다. 따라서 개발자는 binding class의 인스턴스를 생성함으로써 해당 binding class 내에 생성된 뷰 참조를 그냥 가져다 사용하기만 하면 된다.
  
* binding class 안에는 __inflate()__ 라는 public static 멤버 메소드도 자동으로 정의된다. 이 메소드는 해당 레이아웃의 inflate 작업을 해주고 동시에 binding class의 인스턴스를 생성하여 반환해주는 메소드이다.(안드로이드에서 inflate의 정의는 xml에 표기된 레이아웃들을 메모리에 객체화시키는 행위이다. 즉, xml에 정의된 뷰들을 객체화해서 코드에서 사용할 수 있게 하는 행위이다.)
  
* binding class 안에는 __getRoot()__ 라는 멤버 메소드도 자동으로 정의되는데, 이 메소드의 반환값은 해당 xml 파일의 root view의 참조이다. (root view는 xml 파일에서 모든 뷰들의 가장 위에 존재하는 뷰 레이아웃임! 즉, 모든 뷰들의 parent layout들 중 가장 Top 인 레이아웃이 root view임)
  
* binding class 안에는 __bind()__ 라는 멤버 메소드도 자동으로 정의된다. 이 메소드는 이미 해당 Activity나 Fragment에 레이아웃이 inflate된 경우, inflate 작업 없이 뷰 참조만을 생성하는 메소드이다.
  
* ~~~kotlin
  // MainActivity.kt 예시
  override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
  
      val binding = ActivityMainBinding.inflate(layoutInflater)
      binding.hello.text = "안녕" // findViewById() 호출 없이, 이미 참조된 뷰를 바로 사용 가능!
      ... 
  
      val view = binding.root // kotlin property syntax 기능을 사용하여 getRoot() -> root 로 사용 가능
      setContentView(view)
  }
  ~~~
  
* <위 코드 참고> view binding을 사용하기 위한 시작은 inflate() 메소드를 호출하여 binding class의 인스턴스를 가져오는 것이 시작이다. 그 다음, 인스턴스화된 binding class의 멤버 메소드인 getRoot()를 호출하여 해당 xml 파일의 root view 참조를 가져온 후, onCreate() 메소드 내의 setContentView() 메소드의 인자로 root view의 참조를 넘겨줌으로써, 인자로 넘겨준 레이아웃을 MainActivity의 레이아웃으로 사용하겠다고 알리면 된다. (view binding 사용하지 않았을 때에는 onCreate() 메소드 오버라이딩하면setContentView(R.layout.activity_main) 라는 코드가 기본적으로 생성되어 있었음)
  
  
* Activity에서 view binding을 사용하는 방법과 Fragment에서 view binding을 사용하는 방법은 조금 다르다. 이 부분은 [Android Developers - View Binding](https://developer.android.com/topic/libraries/view-binding)를 보면 알 수 있으므로 설명은 생략..ㅎ.ㅎ

# 끝!
이 포스팅에서는 생략한 view binding에 대한 조금 더 깊은 내용들은 [Medium Android Developer - Use view binding to replace findViewById](https://medium.com/androiddevelopers/use-view-binding-to-replace-findviewbyid-c83942471fc)에 자세히 나와있다! 
  
마지막 수정일 : 2021년 8월 14일
