---
layout: post
title:  "[안드로이드] RecyclerView 만들기"
date:   2020-12-29 18:34:10 +0700
categories: [안드로이드]
---

> 💁🏼‍♀️ __프롤로그__
>
> RecyclerView에 대해서는 [이미 작성해놓은 포스팅](https://choheeis.github.io/newblog//articles/2019-12/%EB%A6%AC%EC%82%AC%EC%9D%B4%ED%81%B4%EB%9F%AC%EB%B7%B0)이 있지만..
>
> 다시 봐보니 미흡한 부분이 많은 것 같아서 다시 공부 후, 포스팅해보려 한다!

## 1️⃣ What is RecyclerView?

* [Android Developer 도큐먼트 - RecyclerView로 목록 만들기](https://developer.android.com/guide/topics/ui/layout/recyclerview?hl=ko)를 참고하여 작성합니다.
* RecyclerView는 앱에서 __대량의 데이터 세트__ 또는 __자주 변경되는 데이터에 기반한 요소__ 의 __스크롤 목록__ 을 표시해야 할 때 사용하는 뷰이다.
* RecyclerView는 아래와 같은 모습이고, CardView와 구분할 수 있으면 좋다.
    <img width="514" alt="01" src="https://user-images.githubusercontent.com/31889335/103273889-b2cbf380-4a03-11eb-959f-7f4ec5ef9ebb.png">
* RecyclerView는 기존의 [ListView](https://developer.android.com/reference/android/widget/ListView) 라는 뷰보다 더 유연해지고 진보된 버전의 뷰이다.
* RecyclerView를 동작시키는 작동 모델에는 여러 가지 구성 요소가 함께 작동하는 구조이다.
    * __[RecyclerView 객체](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView)__ : 레이아웃 안에서 스크롤 목록의 전체 UI를 제공하는 뷰 객체이다.
        * RecyclerView 객체는 개발자가 [LayoutManager](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.LayoutManager) 를 통해 제공한 뷰로 채워진다.
        * 표준 레이아웃 매니저인 LinearLayoutManager 나 GridLayoutManager를 사용해도 되고, 직접 구현해도 된다.
    * __[ViewHolder 객체](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.ViewHolder)__ : 목록의 뷰(주로 Item View라고 부름)를 표현하는 객체이다.
        * <img width="241" alt="02" src="https://user-images.githubusercontent.com/31889335/103288281-de60d500-4a27-11eb-886d-449f9e5a7e6a.png">
        * 위 그림의 빨간 박스 부분이 목록의 뷰(Item View)이다.
        * ViewHolder 객체는 [RecyclerView.ViewHolder](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.ViewHolder) 라는 클래스를 확장해서 정의한 클래스의 인스턴스이다.
        * 각 뷰 홀더는 목록의 단일 항목(Item View)을 표현하는 역할을 한다.
        * 만약 음악 앨범 목록을 나타내는 RecyclerView가 존재한다면 각 뷰 홀더는 목록의 각 앨범을 표현한다.
    * __Adapter__ : 뷰 홀더 객체를 관리하는 곳이다. [RecyclerView.Adapter](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter) 라는 클래스를 확장한 클래스를 정의하여 Adapter를 만든다.
        * 어댑터에 의해 뷰 홀더가 관리된다. 어댑터는 필요에 따라 뷰 홀더를 만든다.
        * 어댑터는 뷰 홀더에 데이터를 바인딩한다.
        * 뷰 홀더를 특정 위치에 할당하고 어댑터의 __onBindViewHolder()__ 메서드를 호출하여 데이터를 바인딩한다.
* RecyclerView의 장점
    * RecyclerView의 작동 모델은 성능 최적화 작업이 많이 포함되어 있다.
    * 목록이 처음 게재되면 어댑터가 뷰 홀더를 만들고 데이터를 바인딩한다.
    * 사용자가 목록을 스크롤하면 어댑터가 필요에 따라 새로운 뷰 홀더를 만든다.
    * 만약 사용자가 같은 방향으로 계속 스크롤하면 가장 오래전에 화면 밖으로 나간 뷰 홀더를 재사용하고, 데이터만 새로운 데이터로 바인딩하여 새 목록의 뷰로 추가된다.
    * 사용자가 스크롤 방향을 바꾸면 화면 밖으로 스크롤됐던 뷰 홀더는 곧바로 되돌아온다.
    * 표시된 항목이 변경되면 적절한 __[RecyclerView.Adapter.notify~()](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemChanged(int))__ 메소드를 호출하여 어댑터에 알려주면 된다.
    * 즉, RecyclerView는 뷰 홀더 객체를 목록의 Item 개수만큼 생성하지 않고, 적절히 재사용할 수 있기 때문에 성능이 좋다.
    * <img width="664" alt="03" src="https://user-images.githubusercontent.com/31889335/103289958-ca1ed700-4a2b-11eb-96b3-dcca74b0b21d.png">
    * 위 그림처럼 뷰 홀더를 화면에 보여질 (Item 개수 + 여분 개수) 만큼만 만들고, 나머지는 재사용하게 된다.
    * 따라서 메모리 사용량을 줄이면서 많은 양의 데이터를 목록화해서 보여줄 수 있다.

## 2️⃣ RecyclerView 사용법

1. __라이브러리 추가하기__
    * RecyclerView를 사용하려면 __앱 모듈의 build.gradle 파일__ 에 다음과 같이 recyclerview 라이브러리를 추가해야한다.
        ~~~kotlin
        dependencies {
            implementation 'androidx.recyclerview:recyclerview:(version)'
        }
        ~~~
    * [recyclerView 최신 버전 확인](https://developer.android.com/jetpack/androidx/releases/recyclerview?hl=ko) 링크!
2. __레이아웃의 적합한 위치에 RecyclerView 객체 추가하기__
    * 레이아웃 xml 파일 안에서 목록 스크롤이 들어가야할 위치에 RecyclerView 객체를 추가한다.
    
        <img width="476" alt="04" src="https://user-images.githubusercontent.com/31889335/103291804-e6bd0e00-4a2f-11eb-92d6-adb89dcdab16.png">
3. __Item View 레이아웃 만들기__
    * 목록에 존재할 아이템 뷰 레이아웃을 만든다.
    * <img width="241" alt="02" src="https://user-images.githubusercontent.com/31889335/103288281-de60d500-4a27-11eb-886d-449f9e5a7e6a.png">
    * Item View 레이아웃은 위 그림의 빨간 박스로 표시된 부분의 레이아웃을 말한다.
    * <img width="549" alt="06" src="https://user-images.githubusercontent.com/31889335/103296867-5f28cc80-4a3a-11eb-9fb7-add513836114.png">
    * 이 포스팅에서는 item_test.xml 파일을 생성하고 간단하게 텍스트 뷰 두 개로 이루어진 Item View 레이아웃을 작성하였다.
4. __Item View에서 사용되는 데이터를 data class로 정의하기__
    * 리사이클러뷰는 뷰 홀더에 데이터 바인딩을 한다.
    * 어댑터에 바인딩할 데이터를 제공하기 위해 사용할 때 data class를 사용한다.
    * <img width="441" alt="10" src="https://user-images.githubusercontent.com/31889335/103296794-31dc1e80-4a3a-11eb-8083-fd5f9b591686.png">
5. __Adapter 클래스 만들기__
    * RecyclerView.Adapter 클래스를 상속한 클래스를 정의한다.
    * 클래스를 초기화 할 때, 뷰 홀더에 바인딩할 데이터 set을 전달하도록 한다.
    * RecyclerView.Adpater 클래스를 상속하면 ViewHolder 클래스를 \<> 안에 넣어줘야 한다.
        <img width="673" alt="07" src="https://user-images.githubusercontent.com/31889335/103297158-13c2ee00-4a3b-11eb-8b25-56ee6d9d0595.png">
    * 따라서 inner class로 ViewHolder 클래스를 만든다.
        <img width="926" alt="08" src="https://user-images.githubusercontent.com/31889335/103297207-30f7bc80-4a3b-11eb-8b3b-84ba7acbaee4.png">
    * ViewHolder 는 각 ItemView를 표현하는 클래스라고 위에서 알아봤다!
    * 따라서 먼저 만들어놓은 Item View 레이아웃에서 데이터 바인딩이 필요한 부분의 뷰만 객체화 시키면 된다.
    * 만든 ViewHolder 클래스를 \<> 안에 넣고, 이번에는 빨간 줄을 처리해보자.
    * 리사이클러뷰 어댑터 클래스에서는 RecyclerView.Adapter 클래스에 존재하는 함수들을 override 하여 확장한 클래스로 만들어야 해서 빨간 줄이 뜨는 것이다!
    * <img width="344" alt="09" src="https://user-images.githubusercontent.com/31889335/103296209-f725b680-4a38-11eb-958a-a562d118a6a3.png">
    * 빨간 줄 부분에 있는 전구 모양 아이콘을 클릭하면 Implement members 라는 메뉴가 나온다. 이 메뉴를 클릭해서 RecyclerView.Adapter로부터 override 해야하는 메소드들을 만들어보자.
    * <img width="920" alt="11" src="https://user-images.githubusercontent.com/31889335/103297289-584e8980-4a3b-11eb-81b8-e3e5cda7d770.png">
    * __onCreateViewHolder()__ 는 필요할 시 어댑터에서 새로운 뷰 홀더를 생성하기 위한 메소드이다.
        * 따라서 LayoutInflater를 통해 Item View 레이아웃을 객체화하여 생성해주는 코드를 작성해야 한다.
        * <img width="770" alt="12" src="https://user-images.githubusercontent.com/31889335/103297796-618c2600-4a3c-11eb-8b99-07f2b4d03077.png">
    * __onBindViewHolder()__ 는 뷰 홀더에 데이터가 바인딩될 때 호출되는 메소드이다.
        * 이 메소드에서 데이터 set의 index를 통해 바인딩할 데이터를 얻고, 적합한 뷰에 바인딩시켜주면 된다.
        * <img width="491" alt="13" src="https://user-images.githubusercontent.com/31889335/103298324-7321fd80-4a3d-11eb-899d-709f6518b394.png">
    * __getItemCount()__ 는 데이터 set의 개수를 반환하는 메소드이다.
        * <img width="266" alt="14" src="https://user-images.githubusercontent.com/31889335/103298423-a6fd2300-4a3d-11eb-85a5-4afb1df0051d.png">


> 도큐먼트 '레이아웃 관리자가 어댑터의 onCreateViewHolder() 메서드를 호출합니다.~' 부터 이어서하고 new -> file -> fragment(list) 로 만든거 비교하고, apply 나 recyclerview 속성들 추가 공부하기

<!-- 
6. __RecyclerView에 LayoutManager와 어댑터를 연결시키기__
    <img width="710" alt="05" src="https://user-images.githubusercontent.com/31889335/103293799-e292ef80-4a33-11eb-9e00-4723e9600ae6.png"> -->



   
        
