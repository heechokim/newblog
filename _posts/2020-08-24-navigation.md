---
layout: post
title:  "[안드로이드] 🗺 navigation이라는 것이 있다!"
date:   2020-08-24 18:34:10 +0700
categories: [안드로이드]
---

# [🗺 navigation이라는 것이 있다!]

sunFlower 앱을 클론 코딩하면서 jetpack에 속한 정말 다양한 라이브러리들을 사용해보기 시작했다.

그 중 가장 내용도 많고, 기본이 될 것만 같은 라이브러리인 navigation에 대해서 공부해보자.

이번에도 어김없이 [Android developer - navigation](https://developer.android.com/guide/navigation) 문서를 읽어볼 것이고,

<img width="283" alt="01" src="https://user-images.githubusercontent.com/31889335/91011749-d62efe00-e61f-11ea-8f40-e98dbbebea75.png">

위와 같이 navigation 라는 컴포넌트의 구성 하위에는 정말 많은 문서들이 있다.

가장 먼저 개요(Overview)부터 읽어볼 것이다.

<br>

## 1️⃣ 2018년 5월 경 Android Platform에 Navigation 이라는 것이 등장하다!

[Navigation의 릴리즈 문서](https://developer.android.com/jetpack/androidx/releases/navigation?hl=ko#1.0.0-alpha01) 를 보면 최초 등장이 2018년 5월 8일이라고 되어있다.

약 2년 전에 등장한 Navigation은 이제 Android app 개발에 있어 중요한 존재가 되었다.

Navigation이 대체 뭘까?

## 2️⃣ Navigation이 뭐 하는 애인가?

Navigation 에 대해서 간단하게 알아보기 위해 [Navigation 유튜브 공식 영상](https://youtu.be/Y0Cs2MQxyIs) 를 먼저 봐보자!

Android Platform에서 사용하는 'Navigation' 이라는 단어는 __사용자가 앱 내의 여러 콘텐츠를 여기 저기 왔다 갔다하며 탐색하는 동안 일어나는 상호작용__ 이라는 뜻으로 사용된다.

Android Jetpack의 Navigation은 이러한 Navigation의 의미에 맞게 사용자가 앱 내를 자유롭게 탐색할 수 있는 앱을 구현할 때 도움이 되는 라이브러리이다.

## 3️⃣ 사용자가 앱 내의 여러 콘텐츠를 탐색하는게 그렇게 중요해?

[안드로이드 아키텍처에 관한 이전 포스팅](https://choheeis.github.io/newblog//articles/2020-05/AndroidArchitecture) 을 보면 __모바일 앱 사용자 환경__ 이라는 개념에 대해 공부한 내용이 있다. 이 내용을 다시 한 번 보고 오자!

보고 왔다면 모바일 앱 사용자 환경이 끊어지지 않도록 앱을 개발하는 것이 중요하다는 것을 알게 되었을 것이다.

이러한 모바일 앱 사용자 환경은 대부분 사용자가 앱 내 다양한 화면 및 콘텐츠를 탐색하는 흐름에 의해 만들어지기 때문에 앱 개발에 있어서 __사용자의 앱 내 탐색__ 을 고려하는 것은 매우 중요하다!

그렇기 때문에 Navigation 이라는 탐색을 위한 라이브러리도 출시된 것이 아닐까?

## 4️⃣ Navigation의 구성 요소

Android Jetpack의 Navigation을 사용하여 탐색이 용이한 앱을 구현하려면 Navigation에 속하는 3가지 구성 요소를 알고 있어야 한다.

아래에서 설명할 3가지 개념들을 보면 이해가 될 듯 말 듯 할 것이다... 그래도 일단 읽어보자!

1. __NavGraph(탐색 그래프)__

    Android Jetpack의 Navigation이 가지고 있는 첫 번째 구성 요소는 Navigation Graph 이다.

    Navigation Graph는 탐색과 관련된 모든 정보를 모아 놓은 것이다. Navigation 이 등장하면서 새롭게 생긴 resource이고, xml 파일로 표현된다.
    
    조금 더 자세히 설명하면 탐색과 관련된 모든 정보를 하나의 Navigation Graph라는 xml 파일에 모아 놓는다. 심지어 시각적으로 탐색의 모든 정보를 볼 수도 있다.
    
    이 파일 안에서 destination 이라고 부르는 사용자가 탐색 가능한 앱 내의 모든 개별 컨텐츠를 표시하고, 사용자가 앱 내에서 탐색할 수 있는 모든 경로를 나타낸다.

    <img width="686" alt="04" src="https://user-images.githubusercontent.com/31889335/92093220-0c356480-ee0e-11ea-989b-111a7e2211e4.png">

    탐색 그래프는 위 그림과 같이 생겼다. 위 그림에서 볼 수 있는 각 화면들은 사용자가 탐색하며 볼 수 있는 화면이다. 즉, Fragment들 이라고 생각하면 된다. 이 화면들은 미리보기 이미지로 보인다.

    이 각 화면을 Navigation에서는 __destination(목적지)__ 라고 부른다.

    또 위 그림에서 볼 수 있는 화살표들은 각 destination들을 연결하고 탐색 순서를 나타낸다. 이 화살표들을 __action__ 이라고 부른다. 이러한 action을 통해 사용자가 앱을 탐색할 때 취할 수 있는 경로를 시각적/논리적으로 표현할 수 있다.

    따라서 destination들과 action들로 이루어진 xml 파일인 NavGraph 파일은 앱의 모든 탐색 경로를 시각적으로 한 눈에 볼 수 있도록 도와준다.

    특히 화살표인 action으로 각 화면을 연결함으로써 사용자가 한 destination에서 다른 destination으로 이동할 수 있는 방법을 쉽게 파악할 수 있다.

2. __NavHost__

    Android Jetpack의 Navigation이 가지고 있는 두 번째 구성 요소는 NavHost 이다.

    NavHost는 Navigation Graph에서 destination이라고 불리는 개별 컨텐츠가 표시되는 __빈 컨테이너__ 이다.

    빈 컨테이너라는 것의 의미를 더 자세히 알아보자면 다음 그림과 같이 HavHost라는 빈 컨테이너에 탐색 대상이 되는 destination 들이 넣어졌다 빠졌다 하기 때문에 NavHost를 빈 컨테이너라고 하는 것이다.

    <img width="898" alt="07" src="https://user-images.githubusercontent.com/31889335/92237332-247eaf80-eef2-11ea-8b4d-31f837d6e9e3.png">

    NavHost라고 불리는 것은 NavHostFragment로 구현된다.

3. __NavController__

    Android Jetpack의 Navigation이 가지고 있는 세 번째 구성 요소는 NavController 이다.

    NavController는 앱 탐색을 직접 관리하는 객체이다.

    즉, 앱 내에서 사용자가 개별 컨텐츠(=destination)들을 탐색하며 이동할 때 NavHost에서 destination(=개별 컨텐츠)의 전환을 NavController를 통해 조율한다.

    NavController에 대해서는 이 글을 더 읽어보면 자연스럽게 더 정확히 알 수 있을 것이다.

이 세 가지 구성 요소의 관계는 다음과 같다.

__사용자가 앱을 탐색하는 동안 NavGraph에서 특정 경로를 따라 이동할지, 특정 destination으로 직접 이동할지 NavController에게 알려준다.__

__그러면 NavController가 NavHost에 적절한 대상을 표시하게 된다.__

## 5️⃣ 사용자가 앱 내부를 탐색할 때 어떤 순서로 탐색할까?

> 여기부터는 [Android developer - navigation principles](https://developer.android.com/guide/navigation/navigation-principles) 문서를 읽고 작성한 내용이다.

로그인 화면 같은 조건부 화면을 제외하고 사용자가 앱 내부를 탐색할 때 어떤 순서로 탐색하는지 생각해보자.

<img width="850" alt="02" src="https://user-images.githubusercontent.com/31889335/91959795-168b2c00-ed44-11ea-9de8-f59b3d50e651.png">

위 그림처럼 가장 먼저 사용자는 런처(launcher)에서 앱 아이콘을 터치하여 앱을 실행시킬 것이다.

그 다음, 로그인 화면 같은 조건부 화면이 바로 나올 수도 있지만 사용자 탐색 흐름을 고려할 때는 조건부 화면은 고려하지 않을 것이다.

따라서 앱을 실행시킨 후에는 첫 번째 파란색 화살표가 가리키는 화면 같은 메인 화면을 탐색하게 될 것이다. (위 그림은 SunFlower라는 앱이고, 해당 앱에서 메인 화면은 My garden 이라는 화면에서 보이는 List Screen 이다.)

그 다음으로 사용자는 메인 화면에서 세부 화면으로 들어가며 탐색할 것이다.

사용자는 세부 화면에서 다시 메인 화면으로 돌아오기 위해 위 그림의 빨간색 화살표가 가리키는 것 처럼 back 버튼이나 back 화살표 버튼을 클릭하여 탐색한 경로 그대로 뒤돌아 나오게 된다.

## 6️⃣ stack 으로 표현되는 탐색 상태

위와 같은 흐름으로 사용자가 탐색할 때 Android는 사용자의 화면 탐색 히스토리를 __Stack__ 이라는 자료구조 형태로 저장한다.

간단히 살펴보자면 다음 그림과 같다.

<img width="769" alt="03" src="https://user-images.githubusercontent.com/31889335/91965755-e778b880-ed4b-11ea-825f-36a1962b85b6.png">

스택의 가장 상단에 있는 화면이 현재 보여지고 있는 화면이다.

따라서 사용자가 back 버튼을 누르면 스택 상단에 있던 화면이 pop 되어 사라지고, 한 단계 바로 아래에 있던 화면이 스택의 최상단이 되면서 사용자에게 이전 화면이 보이게 되는 것이다.

이에 대한 더 자세한 내용은 [Android developer - activity stack](https://developer.android.com/guide/components/activities/tasks-and-back-stack) 부분에서 볼 수 있다!

## 7️⃣ Navigation을 본격적으로 사용해보자!

앞에서 언급했던 [Navigation 유튜브 공식 영상](https://youtu.be/Y0Cs2MQxyIs) 를 이 쯤에서 다시 한 번 봐보자! 훨씬 더 이해가 잘 될 것이고, 앞으로 할 것들에 대한 예습이 될 수 있을 것이다.

이제부터 [Android developer - Navigation Getting start](https://developer.android.com/guide/navigation/) 문서를 읽으면서 Navigation을 사용법을 알아가보자!

> Navigation을 사용하려면 안드로이드 스튜디오 3.3 버전 이상이 준비되어 있어야 한다. 이 버전부터 NavGraph 같은 새로운 resource들을 지원하기 때문이다.

1. __Navigation 라이브러리 종속성 추가하기__

    Navigation은 Jetpack에 속해있는 라이브러리이기 때문에 프로젝트에 아래 코드와 같이 라이브러리 종속성 추가를 해줘야 한다.

    ~~~kotlin
    dependencies {
    def nav_version = "2.3.0"

    // Java language implementation
    implementation "androidx.navigation:navigation-fragment:$nav_version"
    implementation "androidx.navigation:navigation-ui:$nav_version"

    // Kotlin
    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

    // Feature module Support
    implementation "androidx.navigation:navigation-dynamic-features-fragment:$nav_version"

    // Testing Navigation
    androidTestImplementation "androidx.navigation:navigation-testing:$nav_version"
    }
    ~~~

    <br>

2. __NavGraph(탐색 그래프) 만들기__

    Navigation의 가장 첫 시작은 탐색 그래프를 만드는 것부터 시작한다.

    프로젝트에 NavGraph를 추가하려면 res 폴더에서 마우스 오른쪽 버튼을 클릭하여 New -> Android Resource File을 선택한 후, 파일 이름을 입력(파일 이름 예 - nav_graph.xml) 한다.

    그 다음, Resource Type을 Navigation으로 선택한 후 파일 생성을 완료하면 된다.

    이렇게 navigation 파일을 생성하면 res 폴더 안에 자동으로 navigation 이라는 폴더가 생성되고 그 안에 새로 만든 NavGraph 파일이 위치하게 된다.

    <img width="202" alt="05" src="https://user-images.githubusercontent.com/31889335/92095852-2c1a5780-ee11-11ea-8f7d-a077283553c9.png">

    <br>
    <br>

    위에서 만든 NavGraph 파일을 열면 아래와 같은 탐색 그래프 편집기가 열린다.

    NavGraph xml 파일을 코드로 직접 작성할 수도 있고, 아래와 같은 시각적인 편집기를 사용하여 작성할 수도 있는 것이다.

    <img width="854" alt="06" src="https://user-images.githubusercontent.com/31889335/92097981-a946cc00-ee13-11ea-8938-e7905526ca7e.png">

    * Destination 패널 : 현재 그래프 편집기에 있는 탐색 호스트와 모든 action들을 나열한다.

    * 그래프 편집기 : 탐색 그래프가 시각적으로 표시되는 곳이다. 

    * Attributes 패널 : 탐색 그래프에서 현재 선택된 항목의 속성들을 보여준다.

    이렇게 시각적인 편집기로도 탐색 그래프를 작성할 수 있지만 __Code__ 버튼을 눌러 코드로 직접 작성할 수도 있다.

    ~~~kotlin
    <?xml version="1.0" encoding="utf-8"?>
    <navigation xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:app="http://schemas.android.com/apk/res-auto"
                android:id="@+id/nav_graph">

    </navigation>
    ~~~

    Code 버튼을 눌러보면 다음과 같이 \<navigation> 이라는 루트 요소가 생겨 있는 것을 볼 수 있을 것이다.

    <br>

4. __NavHost 추가하기__

    NavGraph를 생성했으면 다음으로는 NavHost를 적합한 곳에 추가해야 한다.

    이 포스팅에서는 activity_main.xml 에 NavHost를 위치시킬 것이다. NavHost는 NavHostFragment라는 뷰로 구현해야 하는데 구현 방법은 아래와 같다.

    <img width="526" alt="08" src="https://user-images.githubusercontent.com/31889335/92240395-8988d400-eef7-11ea-9b7a-02d293959ae7.png">

    위 코드처럼 FragmentContainerView 라는 뷰를 작성하고 name을 위 코드와 같이 androidx.navigation.fragment.NavHostFragment 라고 작성하면 NavHostFragment가 생성된다.

    또 defaultNavHost 속성을 사용하면 안드로이드 시스템 뒤로 가기 버튼이 차단된다.

    navGraph 속성에는 이 NavHostFragment가 연결되어야 할 NavGraph를 작성해줘야 한다.

    이렇게 하면 NavHost 생성이 완료된다!

    <br>

5. __NavGraph에 Destination 추가하기__

    이제 NavGraph에 탐색 대상이 되는 Destination 들을 추가해보자.

    <img width="814" alt="09" src="https://user-images.githubusercontent.com/31889335/92241766-be962600-eef9-11ea-9ec8-95809d0de709.png">

    위에서 생성한 nav_graph.xml 을 열면 나오는 편집기에서 왼쪽 상단에 위치한 New Destination 버튼을 클릭한 후, Create New Destination을 클릭한다.

    <img width="898" alt="10" src="https://user-images.githubusercontent.com/31889335/92241955-074ddf00-eefa-11ea-82fb-a01ae3cfb503.png">

    그 다음 빈 프레그먼트를 클릭한 후, 프레그먼트 파일의 이름을 설정하고 완료한다.

    그러면 Fragment 하나가 생성될 것이고 NavGraph에도 시각적으로 방금 만든 Fragment가 나타나게 될 것이다!

    <img width="1181" alt="11" src="https://user-images.githubusercontent.com/31889335/92242162-58f66980-eefa-11ea-8b06-f39c9eb0e86d.png">

    또 nav_graph.xml 파일을 Code로 보면 다음과 같이 방금 만든 Fragment가 추가되어 있을 것이다.

    <img width="533" alt="12" src="https://user-images.githubusercontent.com/31889335/92242315-8fcc7f80-eefa-11ea-9707-1b2c5b731e5e.png">

    이 때, 알 수 있는 것은 코드상에서 보이는 id, name, label, layout과 같은 속성들이 편집기의 attribute 패널에도 있음을 알 수 있다.

    <img width="911" alt="13" src="https://user-images.githubusercontent.com/31889335/92242641-0ec1b800-eefb-11ea-94f6-dad5a8d3897a.png">

    즉, destination 의 구성 속성들을 편집기에서도 작성할 수 있는 것이다.

    <br>

6. __Destination들 연결하기 = action 만들기__

    위와 같이 탐색의 대상이 되는 destination 을 만들었다면 탐색 흐름과 순서를 나타내는 화살표인 action을 만들어 각 destination 들을 연결시켜 줘야 한다.

    하지만 action을 통한 연결은 사용자가 앱에서 탐색할 수 있는 경로에 대한 __논리적 연결__ 일 뿐이기 때문에 이 다음 단계에서 실제로 하나의 destination에서 다른 destination 으로 이동시키는 코드를 작성해줌으로써 __물리적 연결__ 까지 해줘야 한다.

    action을 사용해 하나의 destination에서 다른 destination 으로 연결된 탐색 경로를 나타내기 위해 5번에서 한 방법으로 FragmentSecond 라는 destination 을 아래 그림과 같이 하나 더 생성하자.

    <img width="454" alt="14" src="https://user-images.githubusercontent.com/31889335/92243267-2f3e4200-eefc-11ea-93c9-a590f51ff0ad.png">

    이제 NavGraph 편집기에서 fragmentFirst 라는 destination 를 클릭해보면 오른쪽 측면에 동그라미 버튼이 하나 생길 것이다.

    이 동그라미 버튼을 클릭한 후, fragmentSecond 라는 destination 쪽으로 드래그 한 후 마우스 클릭을 떼면 다음과 같이 action이 생성될 것이다.

    <img width="454" alt="15" src="https://user-images.githubusercontent.com/31889335/92243497-89d79e00-eefc-11ea-84e4-2d8b2e1b4779.png">

    이렇게 한 후, 편집기 모드에서 Code 모드로 바꿔서 봐보면 

    <img width="547" alt="16" src="https://user-images.githubusercontent.com/31889335/92243621-b986a600-eefc-11ea-9492-52c166009ed2.png">

    이렇게 fragmentFirst 뷰 속성으로 action 태그가 자동으로 추가되어 있는 것을 확인할 수 있다.

    action 태그의 속성에서는 해당 action의 id를 설정할 수 있고, 이 action이 가리키는 목적지가 fragmentSecond임을 명시해줘야 한다.

    <br>

7. __실제 탐색을 위해 물리적인 코드 작성하기__

    6번에서 한 것은 탐색 그래프를 작성함으로써 논리적인 탐색 경로를 생성한 것이기 때문에 실제 코틀린/자바 코드로 물리적인 이동을 작성해줘야 한다.

    이 때 사용되는 것이 __NavController__ 객체이다!

    이 NavController는 NavHost 내에서 탑색을 직접 관리하는 객체이다.

    따라서 각 NavHost에는 그에 대응하는 NavController 들이 하나씩 존재한다.

    코틀린에서는 다음과 같은 3개의 메소드 중 하나를 사용하여 NavController 객체를 가져올 수 있다.

    * [Fragment.findNavController()](https://developer.android.com/reference/kotlin/androidx/navigation/fragment/package-summary#findnavcontroller)

    * [View.findNavController()](https://developer.android.com/reference/kotlin/androidx/navigation/package-summary#%28android.view.View%29.findNavController%28%29)

    * [Activity.findNavController(viewId: Int)](https://developer.android.com/reference/kotlin/androidx/navigation/package-summary#findnavcontroller)

    이렇게 물리적인 destination 간의 이동을 할 때 꼭 사용하는 것을 권장하는 것이 하나있다!

    __Safe Args Gradle 플러그인__ 을 사용하면 안전 탐색을 할 수 있고 destination 간에 탐색하며 전달하는 인자를 사용 가능하게 하는 간단한 객체를 사용할 수 있다.

    Safe Args Gradle 플러그인을 사용하기 위해서는 최상위 buile.gradle 파일(프로젝트 수준)에 다음과 같은 classpath를 작성해주어야 한다.

    ~~~kotlin
    buildscript {
        repositories {
            google()
        }
        dependencies {
            def nav_version = "2.3.0"
            classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
        }
    }
    ~~~

    또, 앱 모듈 build.gradle 파일(앱 수준)에 다음과 같은 플러그인도 추가해줘야 한다.

    ~~~kotlin
    apply plugin: 'androidx.navigation.safeargs.kotlin'
    ~~~

    > 여기부터는 [Android developer - 대상간 데이터 전달](https://developer.android.com/guide/navigation/navigation-pass-data#Safe-args) 문서를 읽고 더 자세히 작성했다!

    













    


    