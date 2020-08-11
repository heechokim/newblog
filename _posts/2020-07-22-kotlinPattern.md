---
layout: post
title:  "[Kotlin] 💅 안드로이드에서 사용되는 코틀린 사용법 패턴"
date:   2020-07-22 18:34:10 +0700
categories: [kotlin]
---

# [💅 안드로이드에서 사용되는 코틀린 사용의 패턴이 있다?]

Kotlin으로 안드로이드 개발을 할 때 자주 사용되는 언어 패턴이 있다!

패턴이 생기게 된 이유는 코틀린이라는 언어를 통해 안드로이드 개발을 할 때 코틀린의 가장 유용한 점들을 적용하기 위해서이다.

<br>

## 1️⃣ __상속__ 할 때 사용되는 코틀린 사용법들

1. __상속할 때는 : 기호를 사용한다!__

    코틀린으로 안드로이드 개발을 하다보면 다른 클래스를 부모 클래스로 상속해야 하는 경우가 많다. 
    
    아래 코드처럼 상속을 할 때는 __:__ 기호를 사용한다.

    ~~~kotlin
    class LoginFragment : Fragment() 
    ~~~

    <br>

2. __함수를 오버라이드하여 재정의할 때는 override 키워드를 사용한다!__

    위에서 본 코드처럼 Fragment 클래스를 상속받은 후에는 Fragment 클래스에 이미 정의되어 있는 여러 프래그먼트 수명 주기 함수들을 LoginFragment 클래스에 override 하여 재정의할 수 있다.

    override 하여 함수를 재정의할 때는 아래의 코드처럼 __override__ 키워드를 fun 키워드 앞에 붙이면 된다.

    ~~~kotlin
    override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.login_fragment, container, false)
    }
    ~~~

    <br>

3. __super 키워드를 사용하여 함수를 참조한다!__

    부모 클래스에 포함되어 있는 멤버 함수를 함수 안의 내용을 바꾸지 않고 그대로 호출하려면 __super__ 키워드를 사용한다.

    ~~~kotlin
    // onViewCreated를 그대로 호출하고 그 후에 다른 작업을 실행시키는 방식으로 재정의하려 super.~~를 사용한 모습
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        println("Kotlin")
    }
    ~~~

<br>

## 2️⃣ __변수 초기화__ 할 때 사용되는 코틀린 사용법들

1. __lateinit 키워드를 통해 초기화를 지연시킬 수 있다!__

    Kotlin에서는 객체를 선언할 때 __반드시__ 객체의 속성을 초기화해줘야 한다. 그래야 해당 클래스의 인스턴스를 가져올 때 클래스 내부 변수들을 즉시 참고할 수 있기 때문이다.

    > --> 클래스의 인스턴스를 가져온다?
    >
    > 예를 들어 A라는 클래스가 존재한다고 할 때 다른 곳에서 A라는 클래스를 사용하려면 가장 먼저 A라는 클래스의 인스턴스를 생성해야 한다. 즉, 클래스의 생성자 함수를 호출하여야 한다는 것이다. 이것을 클래스의 인스턴스를 가져온다라고 하는 것!

    만약 아래 코드와 같이 어떤 Fragment 클래스의 View 객체들의 속성을 초기화 해야하는 경우가 존재한다고 가정해보자.

    ~~~kotlin
    class LoginFragment : Fragment() {
        // 컴파일 에러남
        private var usernameEditText: EditText = findViewById(R.id.~~)
        private var passwordEditText: EditText = findViewById(R.id.~~)
        private var loginButton: Button = findViewById(R.id.~~)
        private var statusTextView: TextView = findViewById(R.id.~~)

    ...
    }
    ~~~

    하지만 위 코드를 실행시키면 컴파일 에러가 날 것이다.
    
    왜냐하면 클래스 상단에서 View 객체들의 속성을 초기화 할 경우 Fragment의 생명주기 때문에 문제가 발생하기 때문이다!
    
    왜냐하면 Fragment의 View객체는 Fragment의 생명 주기 함수인 __onCreateView__ 에서 처음 호출되므로 onCreateView 함수가 호출되기 전까지는 View 객체는 준비 되어있지 않은 상태라고 볼 수 있따.

    ~~~kotlin
    // onCreateView 함수 예시
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_main, container, false);
        return view
    }
    ~~~
    
    위 코드는 onCreateView 함수의 예시이다. 코드를 보면 onCreateView 함수의 반환값이 View객체임을 알 수 있을 것이다.

    즉, onCreatedView 함수가 호출되야만 View 객체가 반환되므로 그 전에는 View 객체가 나타나지 않기 때문에 클래스 상단에서 View 객체들의 속성을 초기화하면 해당 Fragment의 onCreateView 함수가 호출되기도 전에 View 객체를 초기화해달라는 것이 되버린다.
    
    따라서 Fragment의 View 객체들의 속성 초기화는 onCreateView 함수가 호출된 후 초기화해야 한다.
    
    이런 경우, 아래 코드와 같이 __kotlin에서는 클래스 상단에서 View 객체의 속성 초기화를 조금 미룰 수 있다!__

    <br>

    코틀린에서는 __lateinit__ 이라는 키워드를 제공하여 객체의 초기화를 미룰 수 있게 한다.

    하지만 lateinit 으로 객체의 초기화를 미룰 경우에는 최대한 빨리 초기화를 시켜줘야 한다.

    ~~~kotlin
    class LoginFragment : Fragment() {

    // lateinit 키워드를 사용하여 초기화를 미뤘다
    private lateinit var usernameEditText: EditText
    private lateinit var passwordEditText: EditText
    private lateinit var loginButton: Button
    private lateinit var statusTextView: TextView

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

<br>

## 3️⃣ __정적 변수__ 를 선언할 때 사용되는 코틀린 패턴

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