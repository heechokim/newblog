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

## 2️⃣ __객체 초기화__ 할 때 사용되는 코틀린 언어 패턴들

1. __lateinit 키워드를 통해 초기화를 지연시킬 수 있다!__

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

    ✍🏻 [lateinit 키워드에 대한 kotlin 도큐먼트](https://kotlinlang.org/docs/reference/properties.html#late-initialized-properties-and-variables) 를 참고하여 작성합니다.

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

✍🏻 [SAM Conversions 에 대한 kotlin 도큐먼트](https://kotlinlang.org/docs/reference/java-interop.html#sam-conversions)를 참고하여 작성합니다.

코틀린은 SAM 변환(conversion) 기능을 제공한다.

__SAM Conversion__ 이란 __Single Abstract Method Conversion__ 의 약자로, __단일 추상 메소드 변환__ 이라고 정의할 수 있다.

그렇다면 __단일 추상 메소드 변환__ 이라는 것은 무엇일까?

단일 추상 메소드 변환에 대해 알아보기 전에! 먼저 __kotlin 언어로 java 코드를 호출할 수 있음__ 에 대해 알고 있어야 한다. 코틀린은 자바와 상호작용을 할 수 있도록 디자인된 언어이기 때문에 개발 언어가 kotlin 인 환경이라도 자바 코드를 호출할 수 있다.

예를 들어, java에 구현되어 있는 Queue 자료구조를 코틀린에서 다음과 같이 사용할 수 있다.

~~~kotlin
// 개발 언어는 kotlin 인데 java의 Queue 자료구조를 가져다 쓰는 모습
import java.util.*

fun main() {
    var queue: Queue<Int> = LinkedList<Int>()
}
~~~

이렇게 kotlin 언어를 사용하는 개발 환경에서 java 코드를 호출하여야 할 경우 작동하는 기능이 있는데 그게 바로 SAM conversion 기능이다.

코틀린 언어를 사용하는 환경에서 자바의 메소드를 호출하면 호출된 자바 메소드가 자동으로 __코틀린의 인터페이스 메소드 구현체__ 로 변환되는 현상을 SAM conversion 기능 이라고 한다.

조금 더 자세히, 이해하기 쉽게 알아보자.

예를 들어, kotlin 으로 안드로이드 앱 개발을 할 때 자주 작성하는 아래의 코드를 봐보자.

~~~kotlin
val loginButton: Button = findViewById(R.id.btn_login)

...

loginButton.setOnClickListener {
    // 로그인 버튼을 클릭했을 때 실행되어야 하는 코드를 작성한다.
}
~~~

위 코드를 하나 하나 뜯어보자.

먼저, loginButton은 Button 객체이다.  

코틀린의 인터페이스 구현체로 변환될 때 자바 메소드의 매개 변수 타입이 코틀린 인터페이스 메소드 구현체의 매개 변수 타입과 일치하도록 변환된다.













## 3️⃣ __정적 변수__ 를 선언할 때 사용되는 코틀린 패턴

__정적 변수__ 란 

1. __companion object를 사용하여 정적 변수를 선언하는 패턴__

    자바에서는 __static__ 이라는 키워드로 절대 저장된 값이 변할 수 없는 변수를 선언할 수 있다. 이러한 변수를 __정적변수__ 라고 한다.

    코틀린에서도 정적 변수를 선언할 수 있는데 정적 변수를 선언하기 위해 __companion object(동반자 객체)__ 라는 블록을 제공하고 있다.

    ~~~kotlin
    class LoginFragment : Fragment() {

        ...

        companion object {
            // const라는 키워드도 붙인다
            private const val TAG = "LoginFragment"
        }
    }
    ~~~

    위 코드는 정적 변수인 TAG 를 companion object 블록 안에서 선언한 코드이다!

    정적 변수를 companion object 키워드를 사용하지 않고 LoginFragment 파일의 가장 최상위에 작성할 수도 있지만 파일 최상위에는 정적 변수 이외에도 여러 객체 변수들이 선언되어 있을 수 있기 때문에 companion object 블럭으로 정적 변수들만 분리시켜놓는 것이 좋다.

<br>

## 4️⃣ Null 허용 여부에 대해 명시할 때 사용되는 코틀린 패턴

1. __? 기호를 사용하여 Null 허용 여부를 명시하는 패턴__

    코틀린은 앱 전체에서 타입 안정성을 유지할 수 있도록 null 허용 여부 규칙을 엄격하게 지키도록 한다.

    원칙적으로 코틀린에서 객체 참조에 null 값이 포함될 수 없지만 null 값을 포함시켜야 하는 경우가 발생한다면 __? 기호__ 를 변수의 type 명 뒤에 추가하여 __해당 변수에는 null 값이 저장될 수 있다__ 는 것을 명시해야 한다.

    ~~~kotlin
    var name: String = null // 컴파일 에러
    
    var name: String? = null // 컴파일 성공
    ~~~

    이렇게 null 허용 여부를 명시해줌으로써 앱을 비정상 종료시킬 수 있는 __NullPointerException__ 에러가 발생할 가능성이 적어진다.

    이렇게 null 허용을 하게 해준다는 점이 자바와 코틀린의 가장 큰 차이점이라고 할 수 있다!

    <br>

2. __? 와 !!연산자 기호의 차이점__

    예를 들어, name이라는 String? 타입의 변수에 저장된 문자열 값의 앞과 뒤에 공백이 포함되어 있다고 가정해보자.

    따라서 trim() 이라는 함수를 사용하여 앞과 뒤에 존재하는 공백을 제거한 문자열을 얻고자 한다.

    String? 타입의 문자열을 trim()으로 자르는 방법은 여러 가지가 있다.

    첫 번째는 !! 연산자를 사용하는 방법이다.

    ~~~kotlin
    val realName = name!!.trim()
    ~~~

    !! 연산자는 name을 항상 null 이 아닌 값으로 간주하는 연산자이다. 만약 name에 null이 들어오게 되면 NullPointerException 에러를 발생시킨다.

    따라서 name!!.trim() 이라고 작성할 경우 name은 원래 String? 타입으로 null이 들어올 수 있지만 항상 null이 들어오지 않는다고 간주하고 코드를 실행시키는 것이다.

    !! 연산자는 실행 속도가 빠르지만 NullPointerException 에러를 발생시켜 앱의 비정상 종료 가능성이 높아지므로 최소한으로 사용하는 것이 좋다.

    두 번째 방법은 ? 기호를 사용하는 방법이다.

    ~~~kotlin
    val realName = name?.trim()
    ~~~

    이 경우에 만약 name이 null이라면 name?.trim() 값도 null이 되지만 NullPointerException은 발생하지 않는다.

    만약 name이 null일 경우 name?.trim() 값이 null이 되는 것이 아니라 다른 default 값이 되기를 바란다면 아래와 같은 코드를 작성하면 된다.

    ~~~kotlin
    val realName = name?.trim() ?: "Default Name"
    ~~~

    <br>

## 5️⃣ 속성 초기화할 때 사용되는 코틀린 패턴

코틀린에서 속성을 초기화하는 방법은 여러 가지가 있다.

1. 첫 번째 방법

    ~~~kotlin
    class LoginFragment : Fragment() {
        val index: Int = 12
    }
    ~~~

2. 두 번째 방법 --> __init 블럭__ 사용

    ~~~kotlin
    class LoginFragment : Fragment() {
        val index: Int

        init {
            index = 12
        }
    }
    ~~~

# 끝!