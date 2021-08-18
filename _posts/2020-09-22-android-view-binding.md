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
> [투덜이의 리얼 블로그 - view binding](https://tourspace.tistory.com/314)  
> [Github - ViewBindingSample](https://github.com/android/architecture-components-samples/tree/master/ViewBindingSample)
>
> __목차__  
> [0. 프롤로그](#0)  
> [1. What is View Binding?](#1)  
> [2. view binding 의 장점](#2)  
> [3. 자동으로 생성되는 binding class에 대해 집중적으로 알아보자!](#3)  
> [4. <include> 태그로 정의되어 있는 뷰에도 view binding이 적용될까?](#4)  
> [5. Google에서 제공해주는 샘플 코드로 배우는 View Binding](#5)

<br>

## 0️⃣ 프롤로그<a id="0"></a>

나도 이제.. findViewById() 그만 사용하자!!!!

## 1️⃣ What is View Binding?<a id="1"></a>

* View Binding은 뷰와 상호작용 하는 코드를 이전보다 더 쉽게 사용할 수 있도록 해주는 녀석이다. View Binding에 대해 알아보기 전에, 아주 간단하게 View Binding의 장점을 말해보자면 "View Binding을 사용함으로써 __findViewById() 메소드를 호출하지 않아도 된다__" 라고 말할 수 있다.

* ~~~kotlin
  var textView: TextView = findViewById(R.id.text1) // 해당 뷰 객체의 참조값을 가져옴
  textView.text = "안녕"
  // 또는
  var textView = findViewById<TextView>(R.id.text1)
  textView.text = "안녕"
  // 또는
  var textView = findViewById(R.id.text1) as TextView
  textView.text = "안녕"
  
  
  // 100번 비슷한 코드를 작성해야 하는 대참사..
  var button1: Button = findViewById(R.id.button1)
  var button2: Button = findViewById(R.id.button2)
  var button3: Button = findViewById(R.id.button3)
  var button4: Button = findViewById(R.id.button4)
  ...
  ~~~

* (위 코드 참고) 안드로이드 개발자 뿐 아니라, 모든 개발자들은 반복적인 코드 작성을 피하려는 경향이 있고 피해야만 한다. 보통 __boilerplate code(보일러 플레이트 코드)__ 라고 불리는, 최소한의 변경으로 여러곳에서 재사용되며 반복적으로 비슷한 형태를 띄는 코드를 작성하는 것은 참 귀찮고 하기 싫은 작업이다. 안드로이드 개발에도 boilerplate code 급으로 반복적으로 사용되는 코드가 있는데 그게 바로 findViewById() 메소드를 호출하는 코드이다. 특정 뷰 객체의 참조값을 사용해야 할 때마다 안드로이드 개발자는 해당 뷰의 정확한 ID를 가지고 위와 같은 코드를 작성해야했다. 만약 하나의 클래스 내에서 참조값을 가져와야 하는 서로 다른 뷰 객체의 개수가 100개라면, findViewById() 메소드를 호출하는 코드를 100번(100줄을 차지할 것...) 작성해야 할 것이다.
    
* 이러한 반복적인 코드 작성을 해결하기 위해 Google I/O 2019 에서는 View Binding이라는 새로운 메커니즘을 소개했다! View Binding이라는 메커니즘은 __모든 xml 파일에 정의된 뷰들의 객체 참조를 처음부터 어딘가에 미리 가져와놓고, Activity나 Fragment 같은 클래스 내 코드 상에서는 원하는 뷰 객체의 참조를 간편하게 바로 가져다 쓸 수 있게 하자!__ 이다.
    
* ~~~build.gradle
  // build.gradle(App 모듈 수준)
  android {
      ...
      buildFeatures {
          viewBinding = true
      }
  }
  ~~~
    
* (위 코드 참고) View Binding을 사용하려면 app 모듈 수준의 build.gradle 파일에 위와 같이 "View Binding을 사용하겠어여~ true~~" 라고 작성해주면 된다. 그런데 보통 새로운 기능은 라이브러리 형태로 제공되는 경우가 많아서, 그것을 사용하기 위해 app 모듈 레벨의 build.gradle 파일의 dependencies{ } 블럭 안에 종속성을 추가해줘야 하는 경우가 많지 않나? 왜 View Binding은 라이브러리 종속성 추가가 아닌 위와 같은 정의를 통해 사용할 수 있는 것일까? View Binding은 Gradle(=안드로이드 앱 빌드 툴)이 제공하는 Android Gradle Plugin에 포함되어 등장했기 때문이다! 따라서 앱 빌드 시 Android Gradle Plugin에 포함된 View Binding을 사용하겠다는 의미로 위 코드처럼 빌드 옵션으로써 작성해주면 되는 것이다. (정확히는 Android Gradle Plugin 3.6 버전부터 View Binding이 포함되었다..!)
  
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
  
* (위 코드 참고) build.gradle 파일에 View Binding을 사용하겠다는 정의를 해두면 앱 빌드 시 컴파일러가 자동으로 해당 모듈 내의 모든 xml 파일에 대해, 뷰 객체들의 참조를 어딘가에 미리 가져와 놓는다. 여기서 '어딘가에' 라는 곳이 그래서 어딜까? 예를 들어, 만약 activity_main.xml 라는 파일이 App 모듈 내에 존재한다면 컴파일러는 ActivityMainBinding.java이라는 이름의 binding class를 자동으로 생성한다. 이 클래스의 멤버 변수(=properties)들은 activity_main.xml에 정의된 모든 뷰(단, ID가 정의되어 있지 않은 뷰는 제외) 객체들로 선언되고, 각 뷰 객체의 참조값이 멤버 변수에 저장된다. activity_main.xml 파일 뿐만 아니라 모든 xml 파일에 대응되는 binding class가 하나씩 생성된다. (binding class의 이름은 자동으로 해당 xml 파일의 이름을 Pascal 형태로 바꾼 다음 가장 끝에 "Binding"을 붙여서 지어진다.) 
  
* (위 코드 참고) binding class는 자바 클래스로 생성되지만 멤버 변수들에 자동으로 @Nullable이나 @NonNull 어노테이션들이 붙여지면서 생성된다. 이는 코틀린으로 작성 중인 코드에서 자바 코드로 작성되어 있는 binding class를 호출해야 할 때, 각 멤버 변수들에 Null이 들어갈 수 없음을 코틀린은 판단할 수 있다.(자바는 변수 값에 null이 들어갈 수 있고, 코틀린은 null이 들어갈 수 없기 때문에 코틀린에서 인식할 수 있는 어노테이션들을 자바 코드에 붙여줌으로써 null 허용 여부의 차이를 맞춰줘야 코틀린에서 자바 코드를 호출할 수 있다.) 즉, binding class는 자바나 코틀린 모두에서 사용 가능한 형태로 생성된다.
  
## 2️⃣ view binding 의 장점<a id="2"></a>
  
* ~~~kotlin
  // binding class 예시(원래는 자바 코드여야 하는데 보기 쉽게 코틀린으로 작성)
  class ActivityMainBinding {
      val hello: Button // 1. TextView인데 Button으로 타입이 정의되는 경우 없을 것! = type-safe
      // 또는
      val hello: TextView = findViewById(R.id.존재하지 않는 ID) // 2. 존재하지 않는 ID로 참조하여 null을 가지게 되는 경우 없을 것 = null-safe
  }
  ~~~
                            
* (위 코드 참고) View Binding의 장점은 크게 "type-safe"와 "null-safe" 이다. View Binding은 binding class 생성 시 개발자가 아닌 컴파일러가 뷰 객체의 참조(reference)값를 가져와 멤버 변수에 저장한다. 따라서 binding class의 멤버 변수의 타입이 뷰와 다른 타입으로 선언된다거나 뷰의 ID 값이 틀리거나 존재하지 않아서 null값을 저장하게 된다거나 하는 문제가 발생하지 않는다. (위 코드의 주석으로 작성해놓은 내용들은 view binding 등장 전, 기존의 findViewById() 메소드를 사용할 시 개발자들이 가장 많이 하는 실수였다..) 
  
* 또한 view binding을 사용 중인 경우, Android Studio에서 xml 파일의 내용을 변경할 즉시, 해당 xml 파일에 대한 binding object만을 메모리에서 바로 갱신하도록 최적화되어 있다. 따라서 xml 파일 내용 변경 시 Android Studio에서 이 변경 사항을 바로 적용하기 위해 프로젝트 전체를 rebuild를 할 필요가 없다. (Android Studio 3.6버전부터 view binding을 사용하기에 좋도록 최적화되어 있음!)
  
## 3️⃣ 자동으로 생성되는 binding class에 대해 집중적으로 알아보자!<a id="3"></a>
  
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
  
* (위 코드 참고) binding class에는 xml 파일에 정의되어 있고 ID가 존재하는 모든 뷰들의 참조값을 저장할 멤버 변수들이 선언된다. 따라서 개발자는 Activity나 Fragment 같은 클래스에서 binding class의 인스턴스를 생성한 후, 해당 binding class 내의 멤버 변수를 그냥 가져다 사용하기만 하면 된다. 또한, rootView라는 이름의 멤버 변수는 자동으로 항상 선언되는데 이 변수에는 해당 xml 파일에 존재하는 모든 뷰들 중 가장 top에 존재하는 뷰 객체의 참조값이 저장된다. 위에서 예시로 든 activity_main.xml을 예로 들면 ConstraintLayout이 root view이다.
  
* binding class 안에는 __inflate()__ 라는 public static 멤버 메소드도 자동으로 정의된다. 이 메소드는 해당 레이아웃의 inflate 작업을 한 후 뷰 객체의 참조값들을 멤버 변수들에 저장한다. 그 후, binding class의 인스턴스를 생성하여 반환해주는 메소드이다.(안드로이드에서 inflate의 정의는 xml에 정의된 뷰들을 메모리에 객체화시키는 행위이다. 예를 들어 xml 파일에 <TextView> 태그로 정의되어 있는 어떤 textView는 [TextView](https://developer.android.com/reference/android/widget/TextView) 클래스의 객체로 생성되어야만 실제로 사용할 수 있게 된다. 즉, xml에 정의된 뷰들을 객체화해서 코드에서 사용할 수 있게 하는 행위가 inflate이다.) inflate() 메소드는 2가지 형태가 있는데 각각의 형태에 대해서와 언제 어떤 형태의 inflate() 메소드를 사용해야 하는지는 [여기](https://medium.com/androiddevelopers/use-view-binding-to-replace-findviewbyid-c83942471fc)를 보면 알 수 있다.(링크 걸어놓은 문서에서 조금 아랫 부분에서 볼 수 있다ㅎㅎ)
  
* binding class 안에는 __getRoot()__ 라는 public 멤버 메소드도 자동으로 정의되는데, 이 메소드는 해당 xml 파일의 root view 객체의 참조값을 반환한다. (root view는 xml 파일에 존재하는 모든 뷰들 중 가장 top에 존재하는 뷰이다.)
  
* ~~~kotlin
  @NonNull
  public static ActivityAwesomeBinding bind(@NonNull View rootView) {
    /* Edit: Simplified code – the real generated code is an optimized version */
    Button button = rootView.findViewById(R.id.button);
    TextView subtext = rootView.findViewById(R.id.subtext);
    TextView title = rootView.findViewById(R.id.title);
    if (button != null && subtext != null && title != null) {
      return new ActivityAwesomeBinding((ConstraintLayout) rootView, button, subtext, title);
    }
    throw new NullPointerException("Missing required view […]");
  }
  ~~~
  
* (위 코드 참고) binding class 안에는 __bind()__ 라는 public static 멤버 메소드도 자동으로 정의된다. 이 메소드는 이미 xml 파일의 뷰들이 inflate된 경우, inflate 작업 없이 뷰 객체의 참조값만을 저장하고 있는 binding class를 반환해주는 메소드이다. bind() 메소드의 실제 구현 코드를 보면(위 코드보다 실제는 더 길다) 이미 뷰들이 객체화되어 있다는 것을 전제로, findViewById() 메소드를 사용해 해당 뷰 객체의 참조값들을 가져오는 것을 볼 수 있다. 다만 모든 뷰 객체 값들에 대해 null 체크를 수행하는 코드가 내제되어 있다. bind() 메소드는 내부에서 결국 findViewById()를 사용하긴 하지만, 안전하게 뷰 객체의 참조값을 저장하고 있는 binding class를 반환해준다.
  
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
  
* (위 코드 참고) view binding을 사용하기 위한 시작은 binding class의 멤버 메소드인 inflate()를 호출하여 뷰들을 객체화시키고 binding class의 인스턴스를 가져오는 것이 시작이다. 그 다음, binding class의 멤버 메소드인 getRoot()를 호출하여 해당 xml 파일의 root view 객체의 참조값을 가져온 후, onCreate() 메소드 내의 setContentView() 메소드의 인자로 이 값을 넘겨준다. 이는 인자로 넘겨준 참조값에 해당하는 레이아웃을 MainActivity라는 액티비티의 레이아웃으로 사용하겠다고 알리는 것이다. (view binding 사용하지 않을 때에는 onCreate() 메소드 내에 setContentView(R.layout.activity_main) 라는 코드가 기본적으로 생성된다. 기존의 이 작업을 view binding에서는 root view 객체 참조값을 setContentView() 인자로 넘겨주는 것!)
  
* (위 코드 참고) 코틀린 언어로 작성중인 코드에서 자바 코드로 작성된 ActivityMainBinding을 호출할 수 있는 이유는 위~에서 언급했듯이 binding class의 멤버 변수에 @NonNull 어노테이션들이 붙여져있기 때문이다!
  
* Activity에서 view binding을 사용하는 방법과 Fragment나 RecyclerView에서 view binding을 사용하는 방법은 조금 다르다. 이 부분은 [Android Developers - View Binding](https://developer.android.com/topic/libraries/view-binding)를 보면 알 수 있으므로 설명은 생략..ㅎ.ㅎ
  
## 4️⃣ \<include\> 태그로 정의되어 있는 뷰에도 view binding이 적용될까?<a id="4"></a>
  
* ~~~xml
  <!-- included_buttons.xml => included 될 뷰 xml -->
  <androidx.constraintlayout.widget.ConstraintLayout>
      <Button android:id="@+id/include_me" />
  </androidx.constraintlayout.widget.ConstraintLayout>
  
  <!-- activity_awesome.xml -->
  <androidx.constraintlayout.widget.ConstraintLayout>
      <include android:id="@+id/includes" layout="@layout/included_buttons"
  </androidx.constraintlayout.widget.ConstraintLayout>
  ~~~
    
* (위 코드 참고) 위 코드에서 included_buttons.xml처럼 어떤 xml 파일에 \<include\> 태그로 끼어 들어가 있는 뷰도 존재할 수 있다. view binding은 모듈 내의 모든 xml 파일에 대해 binding class가 생성된다고 했으니, included_buttons.xml 파일에 대해서도 binding class가 생성될 것이다. (binding class의 이름은 IncludeButtonsBinding 이겠지)
    
* ~~kotlin
  // ActivityAwesomeBinding 클래스 예시
  public final class ActivityAwesomeBinding implements ViewBinding {
  ...

  @NonNull
  public final IncludedButtonsBinding includes; // <include> 태그의 id 값과 동일한 이름을 갖는 멤버 변수가 선언되고, 이 변수의 타입은 IncludeButtonsBinding 이다.
  ~~
    
* (위 코드 참고) 이 때 ActivityAwesomeBinding 클래스 내부에는 \<include\> 태그의 id 값과 동일한 이름을 갖는 멤버 변수가 선언되고, 이 변수의 타입은 IncludeButtonsBinding으로 선언된다. 따라서 \<include\> 태그를 사용하여 어떤 xml 파일을 끼어넣고 싶은 경우 반드시 \<include\> 태그에 id 값을 할당해줘야 한다. 그래야 \<include\> 태그에 해당하는 뷰에 대해서도 view binding이 정상적으로 적용된다.
  
## 5️⃣ Google에서 제공해주는 샘플 코드로 배우는 View Binding<a id="5"></a>
  
* 깃헙 architecture-components-samples 레포지토리의 [ViewBindingSample](https://github.com/android/architecture-components-samples/tree/main/ViewBindingSample) 샘플 앱을 보면서 이제까지 알아본 view binding에 대해 확인해보자.
  
* 샘플 코드를 보면 공식 문서들에서 미쳐 확인하지 못했던 것들도 더 나와있다. 예를 들면, onStart() 메소드 내에 작성된 코드나, Fragment에서 onDestroyView()에서 null 을 넣어주는 코드나..! 꼭 확인하자.

# 끝!
  
마지막 수정일 : 2021년 8월 18일
