---
layout: post
title:  "[Kotlin] 💅 안드로이드 개발에서 자주 사용되는 코틀린 패턴"
date:   2020-07-22 18:34:10 +0700
categories: [kotlin]
---

Kotlin으로 안드로이드 개발을 할 때 자주 사용되는 코틀린 언어 패턴이 있다!

패턴이 생기게 된 이유는 코틀린이라는 언어를 통해 안드로이드 개발을 할 때 코틀린의 가장 유용한 점들을 적용하기 위해서이다.

✍🏻 [Android Developer 도규먼트 - kotlin pattern](https://developer.android.com/kotlin/common-patterns?hl=ko)을 참고하여 작성합니다.

## 1️⃣ __상속__ 할 때 사용되는 코틀린 언어 패턴들

✍🏻 [Android Developer 도규먼트 - kotlin pattern 중 상속부분](https://developer.android.com/kotlin/common-patterns#inheritance) 을 참고하여 작성합니다.

1. __상속할 때는 : 기호를 사용한다!__

    코틀린으로 안드로이드 개발을 하다보면 다른 클래스를 부모 클래스로 상속해야 하는 경우가 많다. 
    
    코틀린에서 상속을 할 때는 __:__ 기호를 사용한다.

    ~~~kotlin
    class LoginFragment : Fragment() 
    ~~~

    LoginFragment 클래스는 Fragment 클래스의 __서브클래스(sub class)__ 라고 부른다.

    Fragment 클래스는 LoginFragment 클래스의 __슈퍼클래스(super class)__ 라고 부른다.

    이 두 용어는 계속 사용될 예정이니 잘 알아두도록 하자!

2. __슈퍼 클래스에 존재하는 메소드를 서브 클래스에서 오버라이드하여 재정의할 때는 override 키워드를 사용한다.__

    Fragment 클래스를 상속받은 LoginFragment 클래스는 부모 클래스(super class)인 Fragment 클래스에 이미 정의되어 있는 여러 프래그먼트 수명 주기 함수들을 override 하여 __재정의__ 할 수 있다.

    함수를 재정의할 때는 아래의 코드처럼 __override__ 키워드를 fun 키워드 앞에 붙이면 된다.

    ~~~kotlin
    // Fragment 클래스의 onCreateView() 콜백 메소드를 override 하여 재정의한 모습
    override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.login_fragment, container, false)
    }
    ~~~

3. __super 키워드를 사용하여 슈퍼 클래스에 존재하는 메소드를 호출한다.__

    ✍🏻 [super 키워드에 대한 kotlin 도큐먼트](https://kotlinlang.org/docs/reference/classes.html#calling-the-superclass-implementation) 를 참고하여 작성합니다.

    서브 클래스(sub class) 안에서 자신이 상속받은 슈퍼 클래스(super class)의 public 메소드나 public 속성을 호출할 수 있다.

    이 때 `super` 이라는 키워드를 사용하여 슈퍼 클래스의 메소드 및 속성을 호출한다.

    아래 코드를 예시로 이해해보자.

    ~~~kotlin
    open class Super {
        open fun draw() {
            println("슈퍼 클래스 draw()")
        }
    }

    class Sub : Super() {
        override fun draw(){
            super.draw()
            println("서브 클래스 draw()")
        }
    }

    fun main() {
        val sub = Sub()
        sub.draw()
    }
    ~~~

    위 코드에서 주목해야 하는 부분은 Sub 클래스이다.

    Sub 클래스는 Super 클래스를 상속받은 클래스이다. override 키워드를 사용해서 슈퍼 클래스의 메소드인 draw()를 재정의하고 있다.

    재정의할 때 super.draw() 라고 작성하여 슈퍼 클래스의 draw() 메소드를 한 번 호출하도록 한 모습이다.

    따라서 main() 함수를 호출하여 출력된 결과는

    <img width="131" alt="01" src="https://user-images.githubusercontent.com/31889335/100314228-80d90300-2ff9-11eb-838e-7b8681badf8f.png">

    위와 같다.

    위 내용들을 바탕으로 안드로이드 개발에서 많이 볼 수 있는 코드 중 아래 코드를 이해해보자.

    ~~~kotlin
    class LoginFragment : Fragment() {

    }
    ~~~

    위와 같이 정의된 LoginFragment 클래스에서 super class인 Fragment 클래스에 존재하는 onViewCreated() 라는 메소드를 재정의해보자.

    ~~~kotlin
    class LoginFragment : Fragment() {

        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            println("Kotlin")
        }

    }
    ~~~

    super class인 Fragment 클래스에 존재하는 onViewCreated() 메소드를 LoginFragment 클래스 안에서 재정의하기 위해 `override` 키워드를 사용하였다.

    이 때, `super` 키워드를 사용하여 Fragment 클래스에 존재하는 onViewCreated() 메소드를 한 번 호출 후, 추가로 다른 작업을 하게 하도록 재정의하고 있다.

## 2️⃣ __객체를 초기화__ 할 때 사용되는 코틀린 언어 패턴

✍🏻 [Android Developer 도규먼트 - kotlin pattern 중 초기화 부분](https://developer.android.com/kotlin/common-patterns#fragment-nullability) 을 참고하여 작성합니다.

* __lateinit 키워드를 통해 초기화를 지연시킬 수 있다!__

    kotlin에서는 객체를 선언할 때 __반드시 객체의 속성을 초기화해줘야 한다.__ 그래야 해당 클래스의 인스턴스를 가져올 때 클래스 내부 변수들을 즉시 참고할 수 있기 때문이다.

    > 💁🏼‍♀️ 클래스의 인스턴스를 가져온다?
    >
    > 예를 들어 A라는 클래스가 존재한다고 할 때 다른 곳에서 A라는 클래스를 사용하려면 먼저 A라는 클래스의 인스턴스를 생성해야 한다. 즉, 클래스의 생성자 함수를 호출하여야 한다는 것이다. 이것을 클래스의 인스턴스를 가져온다라고 하는 것!

    그런데 kotlin 언어를 사용하여 안드로이드 앱 개발을 할 때 객체의 속성을 초기화 해주는 부분에서 문제가 발생하는 경우가 존재한다.

    어떤 경우에 어떤 문제가 발생하는지 알아보자.

    만약 아래 코드와 같이 LoginFragment 클래스의 UI View 객체에 대한 멤버 변수(객체)를 선언하고 초기화한다고 가정해보자.

    ~~~kotlin
    class LoginFragment : Fragment() {
        // findViewById() 함수를 사용하여 UI 레이아웃 객체들을 초기화
        private var usernameEditText: EditText = findViewById(R.id.~~)
        private var passwordEditText: EditText = findViewById(R.id.~~)
        private var loginButton: Button = findViewById(R.id.~~)
        private var statusTextView: TextView = findViewById(R.id.~~)

    ...
    }
    ~~~

    하지만 위 코드를 실행시키면 컴파일 에러가 난다.
    
    왜냐하면 안드로이드 앱 개발에는 __생명주기(life cycle)__ 이라는 중요한 개념이 존재하기 때문이다.
    
    kotlin 언어에서 객체의 속성을 초기화해야 한다는 특징이 있으므로 이를 지키기 위해 안드로이드 개발시 LoginFragment 클래스 상단에서 멤버 변수로 View 객체들을 초기화 한다면?
    
    Fragment의 생명 주기에 따르면 UI 레이아웃은 onCreateView() 라는 생명 주기 콜백 메소드가 호출될 때 비로소 inflate 되어 참조할 수 있게 된다.

    하지만 LoginFragment 클래스 상단에서 View 객체들의 속성을 해당 UI 레이아웃에 존재하는 뷰들로 초기화 한다면 아직 onCreateView() 메소드가 실행되기 전이라 UI 레이아웃 뷰들이 참조될 준비가 되어 있지 않은 상태이기 때문에 참조 자체가 안 된다.

    따라서 LoginFragment 클래스 상단에 View 객체들의 속성을 초기화할 계획만 작성해두고 진짜 초기화는 onCreateView() 메소드가 호출될 때까지 __지연시킬 수 있는 방법__ 이 필요하다.

    이를 위해 코틀린에서는 `lateinit` 이라는 키워드를 제공하고 있다.

    ✍🏻 [lateinit 키워드에 대한 kotlin 도큐먼트](https://kotlinlang.org/docs/reference/properties.html#late-initialized-properties-and-variables) 를 추가로 참고하여 작성합니다.

    __lateinit__ 키워드를 붙여주면 해당 멤버 변수(객체)는 해당 클래스 인스턴스를 가져올 때 바로 초기화되는 것이 아니라 클래스의 body 안에서 초기화 될 수 있게 된다.

    다만, lateinit으로 초기화를 지연시킨 경우, 최대한 빨리 초기화를 시켜줘야 한다.
    
    ~~~kotlin
    class LoginFragment : Fragment() {

    // lateinit 키워드를 사용하여 초기화를 미룬 모습
    lateinit var usernameEditText: EditText
    lateinit var passwordEditText: EditText
    lateinit var loginButton: Button
    lateinit var statusTextView: TextView

    // onCreateView 함수가 호출되고 난 바로 다음에 호출되는 onViewCreated 함수 안에서 초기화시켜줌으로써 최대한 빨리 초기화 시켜주는 모습!
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        usernameEditText = view.findViewById(R.id.username_edit_text)
        passwordEditText = view.findViewById(R.id.password_edit_text)
        loginButton = view.findViewById(R.id.login_button)
        statusTextView = view.findViewById(R.id.status_text_view)
    }

    ...
    }
    ~~~

    lateinit 으로 지연시킨 객체 속성을 초기화 전에 엑세스하면 kotlin은 __UninitializedPropertyAccessException__ 을 발생시킨다.

## 3️⃣ __SAM 변환__ 을 적극 사용하는 코틀린 패턴

✍🏻 [Android Developer 도규먼트 - kotlin pattern 중 SAM conversion 부분](https://developer.android.com/kotlin/common-patterns#sam) 과 [SAM Conversions 에 대한 kotlin 도큐먼트](https://kotlinlang.org/docs/reference/java-interop.html#sam-conversions)를 참고하여 작성합니다.

코틀린은 SAM 변환(conversion) 기능을 제공한다.

__SAM Conversion__ 이란 __Single Abstract Method Conversion__ 의 약자로, __단일 추상 메소드 변환__ 이라고 정의할 수 있다.

그렇다면 __단일 추상 메소드 변환__ 이라는 것은 무엇일까?

이해하기 쉽게 알아보자.

예를 들어, kotlin 으로 안드로이드 앱 개발을 할 때 자주 작성하는 코드 중 아래와 같은 코드가 있다.

~~~kotlin
val loginButton: Button = findViewById(R.id.btn_login)

...

// 이 부분 주목!
loginButton.setOnClickListener {
    // 로그인 버튼을 클릭했을 때 실행되어야 하는 코드를 작성한다.
}
~~~

위 코드를 하나 하나 뜯어보자.

먼저, loginButton은 [Button](https://developer.android.com/reference/kotlin/android/widget/Button) 의 객체이다.  

<img width="335" alt="02" src="https://user-images.githubusercontent.com/31889335/100537537-c8b39080-326c-11eb-86c2-9ee2cdc35e88.png">

Button 클래스의 정의를 보면 위 그림에 빨간 줄로 표시해둔 것처럼 super class로 [View 라는 클래스](https://developer.android.com/reference/kotlin/android/view/View) 를 상속받고 있음을 알 수 있다.

View 라는 클래스가 가지고 있는 public 메소드 중 __[setOnClickListener](https://developer.android.com/reference/kotlin/android/view/View#setonclicklistener)__ 라는 메소드가 존재한다. 이 메소드는 view(여기서는 Button이 된다)가 클릭될 때 호출되는 콜백을 등록할 수 있도록 지원해주는 메소드이다.

<img width="790" alt="03" src="https://user-images.githubusercontent.com/31889335/100537698-fc42ea80-326d-11eb-9878-d755fe4cb852.png">

이 메소드의 매개 변수는 [onClickListener](https://developer.android.com/reference/kotlin/android/view/View.OnClickListener) 라는 인터페이스를 가지고 있는 것을 볼 수 있다.

<img width="720" alt="04" src="https://user-images.githubusercontent.com/31889335/100537752-71162480-326e-11eb-8f3e-ebb1fdf62914.png">

따라서 setOnClickListener 이라는 View 클래스의 public 메소드를 sub class에서 호출하려면 매개 변수인 OnClickListener(인터페이스)를 아래와 같이 구현해줘야 한다.

~~~kotlin
// kotlin 에서 OnClickListener를 구현한 모습
loginButton.setOnClickListener(object : View.OnClickListener {
    override fun onClick(p0: View?) {
        TODO("Not yet implemented")
    }
})
~~~

그런데 여기서 주목할 점이 있다. onClickListener 이라는 인터페이스 안에 정의된 메소드는 __onClick__ 이라는 __추상 메소드__ __딱 한 개만 정의되어 있다__ 는 점이다.

<img width="340" alt="05" src="https://user-images.githubusercontent.com/31889335/100537791-d10ccb00-326e-11eb-83f2-c58bb24f4b4e.png">

<img width="812" alt="06" src="https://user-images.githubusercontent.com/31889335/100538038-ab80c100-3270-11eb-8572-c451c2330390.png">

지금까지의 내용을 정리해보면!

버튼에 클릭 이벤트를 주기 위해 setOnClickListener 이라는 메소드를 구현해야 하는데 이 메소드의 매개변수에 OnClickListener 이라는 인터페이스가 존재하므로 이 인터페이스 안에 정의된 메소드를 구현해줘야 한다.

그런데 OnClickListener 라는 인터페이스에 정의되어 있는 메소드는 onClick 이라는 단 한 개의 추상 메소드(단일 추상 메소드)이다.

따라서 setOnClickListener()는 항상 OnClickListener 이라는 인터페이스를 매개 변수로 가져오고 OnClickListener은 항상 onClick 메소드라는 단일 추상 메소드만을 구현하게 된다는 것을 아는 것이 중요하다!

그래서 원래는 아래와 같이 

~~~kotlin
// kotlin 에서 OnClickListener를 구현한 모습
loginButton.setOnClickListener(object : View.OnClickListener {
    override fun onClick(p0: View?) {
        TODO("Not yet implemented")
    }
})
~~~

구현되어야 한다!

그런데 안드로이드 스튜디오를 통해 이것을 구현하려고 하다보면 

<img width="599" alt="07" src="https://user-images.githubusercontent.com/31889335/100544858-fcf27580-329b-11eb-9231-359cf2cff1a6.png">

위 그림과 같이 setOnClickListener를 조금 더 쉽게 작성할 수 있는 메뉴가 나온다.

빨간 박스로 표시를 해둔 메뉴를 선택하면 아래와 같은 코드가 자동으로 작성된다.

~~~kotlin
loginButton.setOnClickListener { 
    TODO("Not yet implemented")
}
~~~

이 코드는 위에서 구현했던 

~~~kotlin
// kotlin 에서 OnClickListener를 구현한 모습
loginButton.setOnClickListener(object : View.OnClickListener {
    override fun onClick(p0: View?) {
        TODO("Not yet implemented")
    }
})
~~~

이 코드와 같은 기능을 하지만 훨~씬 단순하고 간단한 모습이다.

이게 바로 __SAM 변환__ 기능이다!

즉, 매개 변수로 인터페이스가 존재하며 이 인터페이스가 단일 추상 메소드를 가지고 있다면 코틀린에서 위와 같이 간단하게 구현하도록 변환시켜주는 것이다!

## 4️⃣ __companion object__ 를 사용하는 코틀린 패턴

✍🏻 이 부분에 대한 내용은 길이가 길어질 것 같아 따로 공부 후 포스팅할 예정입니다.

> 포스트 링크 연결하기

## 5️⃣ property(속성) delegation(위임) 을 사용하는 코틀린 패턴

✍🏻 이 부분에 대한 내용도 길이가 길어질 것 같아 따로 공부 후 포스팅할 예정입니다.

> 포스트 링크 연결하기

## 6️⃣ Null 허용 여부에 대해 명시할 때 사용되는 코틀린 패턴

✍🏻 [Android Developer 도규먼트 - kotlin pattern 중 Null 허용 부분](https://developer.android.com/kotlin/common-patterns#nullability)

1. __? 기호를 사용하여 Null 허용 여부를 명시하는 코틀린 패턴__

    코틀린은 앱 전체에서 타입 안정성을 유지할 수 있도록 null 허용 여부 규칙을 엄격하게 지키도록 한다.

    원칙적으로 코틀린에서는 객체 참조에 null 값이 포함될 수 없지만 null 값을 포함시켜야 하는 경우가 발생한다면 __? 기호__ 를 변수의 type명 뒤에 추가하여 __해당 변수에는 null 값이 저장될 수도 있다__ 는 것을 명시해야 한다.

    ~~~kotlin
    var name: String = null // 컴파일 에러
    
    var name: String? = null // 컴파일 성공
    ~~~

    이렇게 코틀린은 null 허용 여부를 직접 명시해줘야 한다는 엄격한 규칙을 통해 앱을 비정상 종료시킬 수 있는 __NullPointerException__ 에러가 발생할 가능성을 줄이고자 한다.

2. __자바 코드와 상호적으로 작동하는 경우 달라지는 Null 허용 여부 표시__

    코틀린이라는 언어는 자바와 상호운용(하나의 시스템이 동일 또는 다른 시스템과 아무런 제약 없이 서로 호환되어 사용할 수 있는 성질)을 할 수 있는 언어이다.

    즉, 코틀린 언어를 사용하는 개발 환경에서도 자바 코드를 호출할 수 있다는 것이다.

    다음은 코틀린 언어를 사용하는 개발 환경에서 자바의 Queue를 호출한 코드이다.

    ~~~kotlin
    import java.util.*

    fun main() {
        // 큐 선언하기
        var queue: Queue<Int> = LinkedList<Int>()
    }
    ~~~

    이처럼 코틀린 언어를 사용하는 개발 환경에서도 자바 코드를 자유롭게 호출할 수 있으며 특히 안드로이드 개발에 사용되는 Android API 들이 자바 언어로 작성된 경우가 대부분이라 안드로이드 개발에서는 자주 일어나는 현상이다.

    하지만 자바는 코틀린과 달리 Null 허용 여부에 대해 ? 연산자를 사용하여 명시적으로 나타내라는 엄격한 규칙이 없다!

    자바는 코틀린보다 Null 허용 여부에 대해 덜 엄격한 편이다.

    따라서 자바로 작성된 Android API를 코틀린으로 호출할 경우 Null 허용 여부가 코틀린 규칙에 벗어나는 경우가 발생하게 된다.

    예를 들어 다음과 같은 상황이 발생할 수 있다.

    다음과 같은 자바 언어로 작성된 Account 클래스가 존재한다고 가정해보자.

    ~~~java
    // 자바로 작성된 코드
    public class Account implements Parcelable {
        public final String name;
        public final String type;

        ...
    }
    ~~~

    코틀린 개발 환경에서 위와 같이 자바로 작성된 Account 클래스의 name이라는 멤버를 참조한다면 코틀린 컴파일러는 name 멤버가 String인지 String?(null 허용)인지 알 수 없게 된다.

    코틀린 컴파일러 입장에서 이러한 모호성을 표시하기 위해 __!연산자__ 를 사용하여 __String!__ 이라고 표시된다. 사실 String!는 특별한 의미가 없고, String 이거나 String? 임을 표시하는 것이다.

    코틀린 컴파일러는 String! 표시를 보고 String 또는 String? 둘 중 하나로 값을 할당할 수 있다.

    이러한 모호성을 해결하기 위해서는 코틀린이 모호해할 것을 예상하여 배려를 해줘야 한다^^
    
    자바로 Account 클래스를 작성할 때 아래와 같이 @Nullable 어노테이션을 붙여주면 코틀린의 String?와 같은 역할임을 명시되어 모호성이 해결된다.

    ~~~java
    // 자바로 작성된 코드
    public class Account implements Parcelable {
        // @Nullable 어노테이션을 붙여줌으로써 Null 허용 명시
        public final @Nullable String name;

        // @NonNull 어노테이션을 붙여줌으로써 Null 미허용 명시
        public final @NonNull String type;

        ...
    }
    ~~~

    하지만 코틀린 언어로 안드로이드 개발을 할 때 모호성을 크게 걱정할 필요가 없다! 자바로 작성된 최근의 Android API는 대부분 어노테이션을 붙여주어 코틀린과 상호운용을 원활히 하도록 개선되었기 때문이다.

3. __!! 연산자를 사용하기도 하는 코틀린 패턴__

    코틀린은 앱 전체에서 타입 안정성을 유지할 수 있도록 null 허용 여부 규칙을 엄격하게 지키도록 하여 __? 연산자__ 를 통해 Null 허용 여부를 명시해주어야 함을 배웠다.

    이것은 ? 연산자를 통해 Null이 참조되더라도 NullPointerException 이 발생하지 않고, 앱이 강제 종료되지 않도록 한다는 것이다.

    하지만 어떤 경우에는 Null일 경우 NullPointerException 를 발생시키고 코드 실행을 막아야 하는 경우도 분명 존재할 것이다.

    이럴 때 사용하는 연산자가 __!! 연산자__ 이다. 다음 코드를 봐보자.

    ~~~kotlin
    val name: String? = "Kimchohee"
    val realName = name!!.trim()
    ~~~

    위 코드의 name 변수는 ? 연산자를 통해 Null이 허용된 String? 타입의 변수이다.

    realName 변수는 trim() 이라는 함수를 사용해 name의 맨 앞과 뒤에 공백이 포함되어 있을 경우 공백을 제거한 문자열을 저장하는 변수이다.

    이 경우 만약 name에 "Kimchohee"가 아닌 null이 참조된다면 realName은 null 값의 앞 뒤 공백을 제거하라는 의미가 되므로 이상한 의미가 되버릴 것이다.

    이를 방지하기 위해 

    ~~~kotlin
    val name: String? = "Kimchohee"

    // !! 연산자를 사용해 Null 허용을 막음
    val realName = name!!.trim()
    ~~~

    위와 같이 원래는 Null이 허용된 변수이지만 변수를 사용하는 부분에서는 Null일 경우 NullPointerException 에러를 발생시키라는 !! 연산자를 붙여준 것이다.

    !! 연산자는 이러한 경우에 사용되지만 NullPointerException 에러를 발생시키는 원인이 되므로 자주 사용해서는 안된다.

    따라서 !! 연산자를 사용하지 않고 똑같은 상황을 해결하는 방법도 존재한다!

    바로 __?: 연산자__ 를 사용하는 방법이다.

    이에 대해서는 [이 블로그의 다른 포스팅 - 코틀린에 존재하는 키워드들](https://choheeis.github.io/newblog//articles/2020-11/kotlinKeywords) 에서 알 수 있을 것이다.

    ?: 연산자에 대해 알아보고 나면 !! 연산자를 사용하지 않고 다음과 같이 해결할 수 있음을 알게 될 것이다.

    ~~~kotlin
    val name: String? = "Kimchohee"

    // ?: 연산자를 사용해 Null 허용을 처리함
    val accountName = account.name?.trim() ?: "Default name"
    ~~~

## 7️⃣ 속성을 초기화 할 때 사용되는 코틀린 패턴

✍🏻 [Android Developer 도규먼트 - kotlin pattern 중 속성 초기화 부분](https://developer.android.com/kotlin/common-patterns#init) 을 참고하여 작성합니다.

코틀린은 속성을 초기화하지 않음을 허용하지 않는다. 즉, 클래스가 초기화될 때 클래스 안의 멤버 속성들이 초기화 되도록 코드를 작성해야 한다.

코틀린에서 속성을 초기화하는 방법은 여러 가지가 있다.

1. 첫 번째 방법

    ~~~kotlin
    class LoginFragment : Fragment() {
        val index: Int = 12
    }
    ~~~

    LoginFragment 라는 클래스가 생성될 때 index 라는 멤버 속성도 초기화 된다.

2. 두 번째 방법 --> __init 블럭__ 사용

    ~~~kotlin
    class LoginFragment : Fragment() {
        val index: Int

        init {
            index = 12
        }
    }
    ~~~

    > https://kotlinlang.org/docs/reference/classes.html#constructors 문서 읽고 init 블럭에 대해 조금 더 자세히 알아볼 피요 있음!

3. 세 번째 방법 --> __lateinit__ 사용

이 부분에 대해서는 이 포스팅의 __2️⃣ __객체 초기화__ 할 때 사용되는 코틀린 언어 패턴들__ 부분에서 이미 설명했었다!

# 끝!

대장정의 포스팅이였습니당,,,, 거의 1주일 동안 이어가며 쓴 것 같은데,, 중간 중간 덜 공부한 부분도 있네요ㅠㅠ 휴우! 😂