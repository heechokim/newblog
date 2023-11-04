---
layout: post
title:  "[안드로이드] px/dp/sp에 대해서 + 관리하는 법"
date:   2020-12-15 18:34:10 +0700
categories: [안드로이드]
---

## 1️⃣ 안드로이드 뷰 크기 설정할 때 사용하는 단위들

✍🏻 [Android Developer 도큐먼트 - 리소스 유형, Dimenstion](https://developer.android.com/guide/topics/resources/more-resources#Dimension)을 참고하여 작성합니다.

안드로이드 개발을 할 때 뷰(View)의 크기를 xml 파일에서 설정해주어야 한다.

이 때 안드로이드에서 지원하는 __크기 단위(측정 단위)__ 는 여러 가지가 있는데 각 단위에 대해 알아보자.

* __px(Pixel)__

    안드로이드의 px는 화면의 실제 픽셀에 대응되는 측정 단위이고, 화면에서 픽셀은 화면을 구성하는 최소 단위이다.

    화면을 구성하는 픽셀 수는 디바이스의 해상도마다 다를 수 있다.

    만약 px로 어떠한 이미지 사이즈를 측정한다면 디바이스별로 해당 이미지의 크기가 달라질 수 있다.

    밑에 나오는 dp에 대해서 읽어보면 px에 대해 자연스럽게 이해할 수 있을 것이다.

* __dp(Density Independence Pixel)__

    dp의 의미는 __밀도에 독립적인 픽셀__ 이라는 의미이다. 밀도에 독립적인 픽셀이라는 말에 대해서 이해하기 위해 조금 더 살펴보자.

    휴대폰 디바이스는 공장에서 제조될 때, 같은 화면 사이즈(크기)를 가진 두 대의 디바이스라도 __화면의 밀도는 다를 수 있다.__

    밀도가 다르다는 말은 인치당 도트(화소, 픽셀) 수가 디바이스마다 다르다는 것이다.

    <img width="467" alt="04" src="https://user-images.githubusercontent.com/31889335/102985321-5758bc00-4552-11eb-82ef-455b335162aa.png">

    <img width="625" alt="01" src="https://user-images.githubusercontent.com/31889335/102197992-31556b00-3f05-11eb-99bd-5eb31f58d7c2.png">

    위 그림은 같은 인치당 도트 수가 다를 때 보이는 화면 차이를 나타낸다. 도트 수가 많을수록 화질이 좋아짐을 알 수 있다.

    __해상도__ 라는 용어도 디바이스 화면의 가로, 세로에 몇 개의 도트가 존재하는가를 나타내는 용어이다.(예 - 1,920 x 1,080 해상도 = 가로 1920개의 px(도트, 화소)와 세로 1080개의 px로 이루어진 디스플레이)

    즉, 휴대폰 디바이스는 화면의 가로 세로 길이가 같아도 휴대폰 스펙에 따라 도트 수가 다를 수 있다!( = 밀도가 다르다)

    안드로이드는 이렇게 화면 밀도가 다른 다양한 휴대폰 디바이스가 존재하는 세계에서, 사용자들에게 시각적인 좋은 경험을 제공하기 위해 __dp__ 라는 측정 단위를 제공한다.

    __dp__ 란? __화면의 물리적인 밀도에 기반한 상대적인 픽셀__ 이다.

    예를 들어, 밀도가 낮은 화면에서 2dp x 2dp 이미지와 밀도가 높은 화면에서 2dp x 2dp 이미지는 가로 세로 길이가 같으므로 눈으로 보았을 때 크기가 같아보인다.

    하지만 실제로 2dp x 2dp 이미지에 사용된 픽셀 수는 다르다.

    밀도가 높은 화면의 2dp x 2dp 이미지에 사용된 픽셀 수가 더 많을 것이다.

    * 밀도가 높은 화면에서 2dp x 2dp 이미지
    
        <img width="332" alt="03" src="https://user-images.githubusercontent.com/31889335/102984676-43f92100-4551-11eb-922e-38c6c0453f0e.png">

    * 밀도가 낮은 화면에서 2dp x 2dp 이미지

        <img width="319" alt="02" src="https://user-images.githubusercontent.com/31889335/102984680-46f41180-4551-11eb-8c4d-ae058fd688e7.png">


    위와 같이, 안드로이드 앱 개발 시 화면에 보여질 뷰의 width, height 측정 단위를 dp로 사용하면 밀도가 다른 두 개의 휴대폰 디바이스라도 사용자 눈에는 같은 사이즈로 보이도록 해준다.

    그렇기 때문에 dp를 통해 다양한 기기에서 UI적 요소의 크기에 일관성을 부여할 수 있게 된다!

* __sp(scale-independent pixel)__

    sp의 의미는 __배율 독립형 픽셀__ 이다.

    sp는 dp와 같은 측정단위이지만 사용자가 지정한 배율에 따라 변할 수 있다.

    sp는 글자 크기를 측정하는 단위로 사용되는데 만약 사용자가 글자 크기 설정(작게, 보통, 크게)을 바꾼다면 __화면 밀도와 사용자의 환경 설정 모두에 따라 조정__ 된다.

## 2️⃣ Android 프로젝트에서 dimension 관리하기

Android 앱 개발을 하다보면 xml 상에 작성되는 여러 뷰들에 저마다의 크기를 지정해줘야 한다.

주로 dp나 sp 단위를 사용하여 크기를 지정할 것이다.

이러한 크기 단위(dimension)에 사용되는 수치를 하나의 파일에서 관리할 수 있다.

1. __res/values/filename.xml 리소스 파일 생성하기__

    res 폴더의 values 폴더 안에 xml 파일을 생성한다.

2. __리소스 파일 작성하기__

    이 파일의 root 요소는 \<resource> 이고, 그 안에는 \<dimen>라는 요소를 작성해야 한다.

    \<dimen> 요소의 속성 중 name 이라는 속성은 해당 리소스를 참조할 때 ID로 사용된다.

    \<dimen> 요소로 관리할 수 있는 측정 단위들은 dp, sp, px, in, mm, pt 이다.

    ~~~xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <dimen name="textview_height">25dp</dimen>
        <dimen name="textview_width">150dp</dimen>
        <dimen name="ball_radius">30dp</dimen>
        <dimen name="font_size">16sp</dimen>
    </resources>
    ~~~

3. __리소스 참조하기__

    이 파일에 작성한 리소스들을 kotlin 파일이나 xml 파일에서 참조하려면 아래와 같이 참조할 수 있다.

    * kotlin 파일에서 참조

        R.dimen.ID

        ~~~kotlin
        val fontSize: Float = resources.getDimension(R.dimen.font_size)
        ~~~

    * xml 파일에서 참조

        @dimen/ID

        ~~~xml
        <TextView
            android:layout_height="@dimen/textview_height"
            android:layout_width="@dimen/textview_width"
            android:textSize="@dimen/font_size"/>
        ~~~

# 끝!
