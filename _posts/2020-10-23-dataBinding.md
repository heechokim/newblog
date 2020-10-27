---
layout: post
title:  "[안드로이드] 🙇🏻‍♀️ Data Binding"
date:   2020-10-23 18:34:10 +0700
categories: [안드로이드]
---

## 1️⃣ Data Binding 이란?

Data binding에 대해 알아보기 전에, [이전 포스팅 - View Binding](https://choheeis.github.io/newblog//articles/2020-09/viewBinding) 를 먼저 읽어보는 것이 앞으로 알아볼 Data Binding 을 더 쉽게 이해할 수 있을 것이다.

> 아래에서 설명할 내용들은 모두 [Android Developer - Data binding 개요](https://developer.android.com/topic/libraries/data-binding) 문서를 바탕으로 작성될 것이다.

이제 Data Binding에 대해 설명하기 위해 View Binding을 예로 들어보자.

xml 파일에 정의해놓은 뷰들을 __실제 메모리에 올리고 코드로 조작할 수 있게 하기 위해 해야하는 몇 가지 작업__ 을 이전보다 쉽게 할 수 있도록 해주는 기능이 __View Binding__ 이였다.

이러한 View Binding 과 비슷하게 __Data Binding 은 어떠한 뷰에 보여질 데이터들을 연결하는 작업을 이전보다 쉽게 할 수 있도록 지원하는 기능__ 이다!

예를 들어, 지금까지의 안드로이드 개발 과정에서 인사말을 보여주는 textView 에 viewModel에서 받아온 "안녕하세요" 라는 String 데이터를 보여주기 위해 작성한 코드가 아래와 같다고 가정해보자.

~~~kotlin
tv_hello.text = viewModel.hello
~~~

그런데 Data Binding 기능을 사용하는 프로젝트에서는 

~~~xml
<TextView
    android:text="@{viewmodel.hello}" />
~~~

위와 같이 코틀린(or 자바) 코드 대신 xml 파일 안에서 바로 데이터를 연결시켜줄 수 있게 된다!

즉, __Data Binding__ 은 이전에는 코드로 UI 뷰 객체를 직접 정의하고 직접 데이터를 연결시켜줘야 했던 불편함을 해결해주는 기능이라고 할 수 있다.

바로 layout 파일인 xml 파일 내에서 데이터를 연결할 수 있기 때문에 UI 뷰 객체를 선언하거나 연결하는 코드 자체가 필요 없게 된다😎

> 와우.. 진짜 대박이다

## 2️⃣ Data Binding 을 사용하기 위한 세팅

Data Binding이 무엇인지 대략적으로 알게 되었으면 실제로 Data Binding을 사용하는 프로젝트를 만들어보자.

> 아래에서 설명할 모든 내용은 [Android Developer - Data Binding 시작하기](https://developer.android.com/topic/libraries/data-binding/start) 문서를 참고하여 작성할 것이다.

가장 먼저 Data Binding 기능을 사용하겠다는 세팅을 해주기 위해 

app 모듈의 build.gradle 파일에 아래와 같은 코드를 추가해야한다.

~~~
android {
        ...
        buildFeatures {
            dataBinding true
        }
}
~~~

## 3️⃣ data binding 사용하기

1. __xml 파일 구성하기__

    data binding을 사용하여 xml 파일 내에서 바로 데이터를 연결시키기 위해서는 기존의 xml 파일을 작성하던 방식과 조금 다르게 작성해야 한다.

    먼저 xml 파일을 구성하는 루트 태그는 __\<layout>__ 이어야 하고, 이 \<layout> 태그 안에 xml 파일 내에서 연결한 데이터를 표시하는 __\<data>__ 태그와 다른 뷰 태그들이 위치하게 된다.

    다음은 data binding을 사용하여 activity_main.xml 파일에 작성한 코드이다.

    <img width="514" alt="01" src="https://user-images.githubusercontent.com/31889335/97318460-3e63bf80-18af-11eb-8d7f-f2750f97a36c.png">

    위 코드를 보면 루트 태그가 \<layout> 태그로 되어 있고, 그 안에 \<data> 태그 부분과 \<ConstraintLayout> 태그 부분이 들어가 있는 것을 확인할 수 있다.

    기존 xml 파일을 작성할 때는 루트 태그에 바로 \<ConstraintLayout> 같은 태그를 사용했었는데 data binding 을 사용할 때는 루트 태그를 \<layout> 태그로 먼저 크게 감싼 후, 그 안에 데이터와 뷰들을 작성하는 방식이라는 것을 이해할 수 있을 것이다.

    빨간 표시는 일단 무시하자! (먼저 처리해주어야 하는 과정을 설명을 위해 나중으로 미뤄서 그렇다!)

    \<data> 태그는 해당 xml 파일에서 데이터 연결을 위해 변수처럼 사용할 것들을 정의하는 태그이다.

    \<data> 태그 안에 \<variable> 이라는 "변수" 태그가 존재하고, 이 태그의 속성으로 name과 type을 지정해주면 된다.

    이 부분은 아래 내용을 통해 더 자세히 알아보자!

    또한 xml 안에서 사용되는 여러 속성들의 속성값으로 \<data> 태그 안에서 정의해준 변수들을 사용하려면 __@{}__ 구문을 사용하면 된다.

2. __데이터 클래스 작성하기__

    위에서 살펴본 activity_main.xml 파일에서 빨간 표시(오류 표시)가 되어 있던

    <img width="384" alt="02" src="https://user-images.githubusercontent.com/31889335/97320031-c5656780-18b0-11eb-8151-a5274298b6e7.png">

    이 부분에 대해 알아보자.

    사실은 data class 타입의 User 라는 이름의 데이터 객체가 존재하여야 위 user 데이터 변수를 xml 파일에 정의할 수 있다.

    따라서 다음과 같은 firstName과 lastName이라는 필드를 가지고 있는 User라는 data class를 생성해보자.

    ~~~kotlin
    data class User (val firstName: String, val lastName: String)
    ~~~

    이제 다음 코드가 이해될 것이다!

    <img width="384" alt="02" src="https://user-images.githubusercontent.com/31889335/97320031-c5656780-18b0-11eb-8151-a5274298b6e7.png">

    이 코드는 해당 xml 파일 안에서 user 라는 이름의 데이터를 변수처럼 사용할 것이라는 뜻이다.

    이 때, user 라는 이 변수의 타입은 위에서 생성한 User 라는 데이터 객체 타입임을 type 속성에 작성해준 것이다.

    그럼 아래 코드에서의 빨간 표시도 이해가 될 것이다.

    <img width="534" alt="03" src="https://user-images.githubusercontent.com/31889335/97321448-38231280-18b2-11eb-9e14-e56a65b868c2.png">

    위 코드에서 text 속성의 속성값이였던 user.firstName과 user.lastName은 User 데이터 클래스의 firstName과 lastName 이라는 필드를 가리킨다는 것을 알 수 있다.

    즉, User 데이터 클래스를 생성하고 나면 activity_main.xml 파일 안의 빨간 오류 표시들이 모두 사라지게 된다!

3. __xml 파일 inflate 시키기__

    위 두 단계를 거쳐 activity_main.xml 파일을 완성했으면 MainActivity.kt 파일에 이 xml 을 연결해야 정상적으로 앱을 실행했을 때 원하는 화면이 출력될 것이다.

    data binding도 view binding과 마찬가지로 각 xml 파일마다 binding class가 자동으로 생성되게 된다.

    다만, view binding에서의 binding class와 다른 점은 data binding에서의 binding class에는 뷰에 대한 binding 정보 뿐만 아니라 __\<data> 태그에서 정의된 데이터 변수들에 대한 binding 정보도 포함__ 되어 있다는 것이다!

    이제 MainActivity.kt 파일의 onCreate 콜백 메소드에 activity_main.xml 파일을 inflate 시켜보자!

    <img width="808" alt="04" src="https://user-images.githubusercontent.com/31889335/97323526-3bb79900-18b4-11eb-9fca-df6794b75fa2.png">

    위 코드와 같이 binding class를 인스턴스화 하는 과정에서 DataBindingUtil 이라는 클래스 안의 setContentView() 메소드를 통해 활성 뷰를 activity_main.xml 로 설정하면 된다.

    그 다음, activity_main.xml 파일에서 데이터 변수로 사용되었던 user이 실제로 어떤 데이터인지 명시해주면 된다!

    <img width="336" alt="05" src="https://user-images.githubusercontent.com/31889335/97323883-9650f500-18b4-11eb-84dc-8347529bc11a.png">

    DataBindingUtil 클래스를 사용하는 방법 외에도 inflate 하는 방법은 두 가지 정도가 더 있다!

    ~~~kotlin
    val binding: ActivityMainBinding = ActivityMainBinding.inflate(getLayoutInflater())
    ~~~

    만약 Fragment나 RecyclerView Adapter에서 데이터 바인딩을 사용한다면

    ~~~kotlin
    val listItemBinding = ListItemBinding.inflate(layoutInflater, viewGroup, false)
    // or
    val listItemBinding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false)
    ~~~

    이와 같이 inflate 해줄 수도 있다.

## 4️⃣ data binding에 사용되는 표현식 언어

표현식 언어(expression language)란 위 예시에서도 @{user.firstName}으로 사용되었던 __@{}__ 구문 안을 이루고 있던 식에 관한 언어이다.

표현식 언어에 대한 것은 [여기](https://developer.android.com/topic/libraries/data-binding/expressions?hl=ko) 문서의 표현식 언어 부분을 참고하자!

정말 많은 표현들이 있다..!

# 끝!

이 포스팅을 보고 data binding에 대해 전부 알 수는 없지만(알아야 할 것들이 조금 더 많이 있음ㅎㅎ) 대략적으로 data binding이 이런것이다~ 정도는 알 수 있을 것이다! 😃