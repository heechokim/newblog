---
layout: post
title:  "[안드로이드] RecyclerView가 뭐예요?"
date:   2021-03-10 18:34:10 +0700
categories: [안드로이드]
---

> __참고 자료__
>
> [Android Developer 도큐먼트 - RecyclerView로 목록 뷰 구현하기](https://developer.android.com/guide/topics/ui/layout/recyclerview)
>
> [Stack Overflow - RecyclerView와 ListView의 차이점](https://stackoverflow.com/questions/26728651/recyclerview-vs-listview)
>
> [Android Developer 유튜브 채널 - RecyclerView ins and outs Google I/O 2016](https://www.youtube.com/watch?v=LqBlYJTfLP4)
>
> [Android Developer 유튜브 채널 - ListView Animations](https://www.youtube.com/watch?v=8MIfSxgsHIs)

<br>

## 0️⃣ 프롤로그

안드로이드 앱 개발을 하다보면 정말 많이 사용되는 뷰 모양 몇 가지를 자연스럽게 알 수 있게 됩니다. 많이 사용되는 뷰 모양 중 '목록 뷰', '리스트 뷰' 라고 불리는 뷰 모양이 있습니다.

<img width="400" alt="01" src="https://user-images.githubusercontent.com/31889335/110605656-ad8de080-81cc-11eb-99e9-f98f941f9afc.png">

위와 같은 모양의 뷰를 구현하기 위해 사용하는 레이아웃은 'RecyclerView'입니다. 저는 그 동안 여러 번 RecyclerView와 관련된 클래스들을 구현해왔지만 누군가 'RecyclerView의 내부 동작을 설명해봐!' 라고 했을 때 정확히 잘 설명할 자신이 없다는 것을 깨닫게 되었습니다ㅠㅠ😭

이 포스팅은 RecyclerView의 구현 방법부터 내부 동작 등을 깊게 알아본 후 작성한 포스팅입니다.

## 1️⃣ What is RecyclerView?

* __RecyclerView는 무엇일까?__

    * <img width="220" alt="02" src="https://user-images.githubusercontent.com/31889335/110606298-66ecb600-81cd-11eb-95e9-30a872fc3e92.png">

    * (위 그림 참고) RecyclerView는 안드로이드 프레임워크에서 제공하는 뷰 레이아웃 중 하나입니다. RecyclerView의 모습은 위 그림(=이메일 앱에서 받은 편지 목록을 나타내는 화면)과 같습니다. '목록을 나타내는 뷰 레이아웃' 라고 생각하면 쉽습니다. 특히, 위 그림에서 빨간 박스로 표시한 부분이 목록을 나타내주는 뷰 안에서 '동일한 형태로 계속 반복'되고 있다는 것을 알 수 있습니다. '동일한 형태(=반복되는 Structure)'라는 말의 의미는 빨간 박스의 형태를 분석해보면 이해할 수 있습니다. 빨간 박스 안, 왼쪽 상단에 이메일을 보낸 사람의 이름을 보여주는 텍스트 뷰가 배치되어 있고, 그 아래 이메일의 제목을 보여주는 텍스트 뷰가 배치되어 있으며 그 아래에는 이메일의 내용을 미리 보여주는 텍스트 뷰가 배치되어 있습니다. 또 오른쪽 상단에 이메일을 받은 후 지난 시간을 보여주는 텍스트 뷰가 배치되어 있고, 그 아래 별표 모양의 뷰가 배치되어 있습니다. 이러한 모습인 빨간 박스 부분이 형태는 동일하게, 다만 데이터만 계속 바뀌면서! '목록을 나타내주는 뷰'를 이루고 있습니다. 즉, 목록을 나타내주는 뷰는 동일한 형태의 빨간 박스가 계속 반복되면서 '보낸 사람 이름', '이메일 제목', '이메일 내용' 등 빨간 박스 안 데이터만 바뀌는 뷰라고 이해할 수 있습니다.

    * RecyclerView에서는 위에서 쉽게 표현하기 위해 '빨간 박스 부분'라고 언급한 부분을 'ItemView(아이템 뷰)'라고 부릅니다. 아이템 뷰들이 일렬로 나열되어 목록처럼 보이는 뷰 레이아웃을 RecyclerView라고 합니다. 

    * <img width="220" alt="02" src="https://user-images.githubusercontent.com/31889335/110943146-36984980-837e-11eb-9c35-e79f2202b2c3.png">

    * (위 그림 참고 - [출처](http://www.digitstory.com/recyclerview-multiple-viewholders/)) 더 나아가서 RecyclerView를 구성하는 ItemView들이 모두 동일한 형태가 아닌 경우도 있을 수 있습니다. 위 그림을 보면 파란색 박스 부분과 핑크색 박스 부분은 둘 다 하나의 RecyclerView를 구성하는 ItemView지만 두 ItemView의 형태가 동일하지 않습니다. 파란색 박스 부분과 달리 핑크색 박스 부분에는 메시지를 표시하는 텍스트 뷰가 포함되어 있습니다. RecyclerView는 이렇게 목록을 이루는 ItemView의 형태가 동일하지 않은 경우에도 하나의 목록으로 구현할 수 있도록 도와줍니다.

* __안드로이드 프레임워크가 제공하는 RecyclerView는 어디에 구현되어 있을까?__

    * RecyclerView는 Android Jetpack의 구성 요소 중 하나입니다. 즉, 안드로이드 프레임워크에서 라이브러리 형식으로 제공하고 있어 개발자는 이 라이브러리를 가져다 사용하면 되고 원하는 모양으로 만들기 위해 적절히 응용하기만 하면 됩니다.

    * <img width="886" alt="03" src="https://user-images.githubusercontent.com/31889335/110795213-f1aadf00-82b9-11eb-8af6-43a6d030c511.png">

    * (위 그림 참고) [Jetpack RecyclerView에 관한 라이브러리 문서](https://developer.android.com/jetpack/androidx/releases/recyclerview?hl=ko)를 보면 RecyclerView는 __androidx.recyclerview__ 라는 이름의 패키지로 제공되는 Jetpack 라이브러리임을 알 수 있습니다. 2018년 12월에 1.1.0-alpha01 버전이 처음 배포되었고 최근 2021년 2월까지 지속적으로 업데이트되고 있는 라이브러리임을 알 수 있습니다. androidx.recyclerview 패키지 안에는 2개의 API가 포함되어 있는데 이 2개의 API는 __androidx.recyclerview.widget__ 과 __androidx.recyclerview.selection__ 이라는 패키지로 묶여 제공됩니다. 

        * 패키지(Package) : 자바에서 클래스를 체계적으로 관리하기 위해 사용하는 것입니다. 패키지의 형태는 파일 시스템의 '폴더'와 비슷합니다. 하지만 '폴더'와 자바의 '패키지'의 차이점은 패키지는 자바 클래스를 유일하게 만들어주는 식별자 역할도 한다는 점입니다. 예를 들어, Test라는 이름의 자바 클래스가 존재하고 이 클래스가 com.android라는 이름의 패키지 안에 포함된다고 가정해봅시다. 이 때 Test클래스의 전체 이름을 com.android.Test.class 라고 부를 수 있습니다. 따라서 클래스의 이름이 같아도 다른 패키지에 포함되어 있으면 서로 다른 클래스로 인식됩니다. (참고 - https://yolojeb.tistory.com/8)
        
        * androidx.recyclerview 라는 이름의 패키지는 하위 폴더로 widget과 selection 라는 폴더를 가지고 있는 것입니다. 또한 각 폴더 안에 RecyclerView를 동작하게 하는 내부 클래스들이 실제로 구현되어 있습니다. 내부 클래스 파일들이 폴더(패키지)로 묶여 제공되는 것입니다.

* __RecyclerView가 왜 등장했을까?(ListView(리스트 뷰) 라는 것이 있었다!)__

    * RecyclerView가 등장하기 전, 안드로이드 프레임워크은 '목록 뷰'를 만들 때 사용하도록 [ListView](https://developer.android.com/reference/android/widget/ListView) 라는 것을 제공했습니다. 이 ListView를 사용해도 RecyclerView와 모양이 비슷한 '목록 뷰'를 만들 수 있습니다. 또한 ListView는 API level 1(Android 1.0) 때 등장한 것으로 안드로이드 프레임워크이 꽤 오랫동안 제공하던 API였습니다. 그럼 이미 ListView라는 것이 존재함에도 불구하고 2018년에 왜 RecyclerView가 새롭게 등장했을까요?

    * ListView에는 고질적인 문제가 있었기 때문입니다. ListView에 들어가는 ItemView의 갯수가 수백 개, 수천 개 이상으로 많아질 경우 ListView가 ItemView를 생성하는 __속도가 매우 느려졌기__ 때문입니다. 특히, 안드로이드의 초창기 디바이스는 메모리 용량이 요즘의 디바이스에 비해 매우 작았고 그로 인해 메모리가 제한적이어서 더욱 느렸습니다. 따라서 ListView를 사용해 한 번에 많은 수의 ItemView를 생성하는 것이 쉽지 않았습니다. 즉, ListView에 많은 수의 ItemView를 넣으면 앱 성능(App Performance)적인 측면에서 좋지 않았습니다.

    * 그렇다면 어떻게 해야 많은 수의 ItemView를 한 번에 빠르게 만들 수 있을까요? 이에 대한 해결책으로 많은 개발자들은 '속임수(smoke and mirrors = 진실 은폐)'를 사용하는 방법을 선택했습니다. 속임수는 바로 __유저의 눈에 당장 보이는 ItemView만 생성하자!__ 였습니다. ListView가 처음 뜨는 화면에는 유저의 눈에 당장 보여야 하는 ItemView만 한 번에 생성하여 보여주고, 그 후에 유저가 스크롤을 하게 되어 새로운 ItemView들이 새로 보이기 시작하면 그 때 새롭게 보이는 ItemView만 계속적으로 생성하는 방법입니다. 마치 [이 영상](https://youtu.be/t8T8bStabq0?t=100)에서 강아지가 기차를 타고 가는데 기찻길을 직접 놓으면서 가는 것과 비슷한 방법입니다.(~~링크로 걸어 놓은 애니메이션 어렸을 때 많이 봤었는데 .. RecyclerView 공부하다가 추억에 빠진다 ㅎㅎ~~)

    * 기존 ListView의 기본 내부 동작 원리는 앞서 설명한 것처럼 한 순간에 모든 ItemView를 생성하지 않고 유저의 눈에 보이는 ItemView만 생성함으로써 마치 ItemView를 생성하는 속도가 빠르게 보이는 것처럼 작동됐습니다.

    * 이러한 내부 동작을 가능하게 하기 위해 ListView는 __Adapter__ 라는 것을 사용했습니다. Adapter는 ListView를 동작하게 하는 구성 요소(=Component, 컴포넌트) 중 하나입니다. Adapter는 ListView에 포함될 ItemView를 생성하고 ItemView에 데이터를 채워주는 역할을 했습니다. 개발자는 Adapter 클래스가 제공하는 getView()라는 메소드를 호출함으로써 Adapter가 생성하고 데이터까지 채워놓은 ItemView를 얻을 수 있습니다.(ListView 안에서 어느 위치(=position)에 어떤 ItemVIew가 들어가야 하는지도 Adapter가 알고 있었습니다.)  
 
    * ListView는 앞서 설명했던 속임수 외에 또 다른 속임수가 하나 더 있었습니다. 위에서 ListView에 포함될 ItemView의 갯수가 100개지만 당장 유저의 눈에는 5개의 ItemView만 보이는 ListView가 존재한다면 이 ListView는 100개의 ItemView를 미리 모두 생성하지 않고 당장 보이는 5개의 ItemView만 생성하여 보여준다고 설명했습니다. 만약 유저가 스크롤을 내려서 6번째 ItemView가 새로 보여져야 한다면 어떤 일이 벌어질까요? ListView는 이 때 또 다른 속임수를 사용합니다. 이 때 Adapter는 새로운 ItemView를 생성하지 않고 __1번째 위치에 보여주기 위해 생성했던 ItemView를 그대로 재사용하여 6번째 ItemView로 제공__ 했습니다.(ItemView는 재사용하여 그대로 사용하고 6번째 ItemView에 들어갈 데이터만 변경해주는 방식입니다.) 단, 1번째 ItemView의 형태와 6번째 ItemView의 형태가 동일한 경우에만 재사용됩니다. 이러한 두 번째 속임수를 사용함으로써 6번째 ItemView를 생성하기 위해 사용되는 비용을 줄일 수 있었습니다.

    * 점점 시간이 지나고.. ListView에는 이 두 개의 속임수를 사용한 기능 외에도 점점 더 많은 기능이 추가되어야 했습니다. 왜냐하면 ListView가 앱에서 너무 많이, 자주 사용되는 레이아웃이기 때문에 개발자들이 ListView API의 업데이트마다 요구하는 사항이 많아졌기 때문입니다. 따라서 Android API level이 새로 출시될 때마다 ListView에는 새로운 기능이 계속 추가되었습니다. ([여기](https://www.youtube.com/watch?v=wDBM6wVEO70)를 보면 ListView에 추가된 기능들을 확인할 수 있습니다.) Google은 점점 더 많아지는 요구 사항을 ListView API에 추가 구현하여 제공하려다 보니 너무 복잡해짐을 느꼈고 부가 기능들이 많아져 오히려 오작동을 하게 되는 경우도 있었습니다.

    * 결국 많은 요구 사항들을 ListView에 추가하다 보니 안드로이드 프레임워크가 제공하는 __다른 뷰 레이아웃에서 이미 제공하고 있는 기능과 비슷한 기능들이 ListVie에 추가__ 되게 되었습니다. 예를 들어, ListView에 추가된 Selection이라는 기능은 View 클래스가 제공하는 Focus라는 기능과 역할이 중복되었습니다. View 클래스는 현재 포커스 된 뷰를 다루는 기능을 제공하고 있었습니다. 그러나 ListView의 Selection도 이와 비슷하게 현재 포커스 된 ItemView를 다루는 기능이였습니다. 또 추가된 ItemView 클릭 리스너 기능과 View 클릭 리스너 기능도 비슷한 기능이였습니다. ListView가 제공하는 ItemView 클릭 리스너를 구현하면 유저가 ItemView를 클릭했을 때 실행할 로직을 작성할 수 있었습니다. 그러나 View 클래스에서 제공하는 클릭 리스너를 사용해서도 ItemView를 클릭했을 때 실행할 로직을 작성할 수 있었습니다. (ItemView 클래스가 View 클래스를 상속했기 때문) 따라서 개발자들은 대체 어느 것을 사용해야 하는가에 관한 질문을 지속적으로 문의했습니다.

    * ![05](https://user-images.githubusercontent.com/31889335/110972166-09a85e80-839f-11eb-90e1-0ce68ce5f4e3.gif)

    * (위 영상 참고 - [출처](https://www.youtube.com/watch?v=8MIfSxgsHIs)) 그러나 ListView가 가지고 있는 이슈들 중 최고봉은 __ListView의 ItemView 애니메이션을 구현하는 것이 정말 정말 어렵다__ 라는 문제였습니다. [이 영상](https://www.youtube.com/watch?v=8MIfSxgsHIs)을 보면 ListView 애니메이션을 구현하는 것이 왜 어려운지 알 수 있습니다. 이 영상에서 구현하고자 하는 애니메이션은 ItemView를 클릭하면 해당 ItemView가 fade out(서서히 사라지기) 애니메이션 처리되면서 결과적으로는 완전히 사라지는 것입니다.

    * ![06](https://user-images.githubusercontent.com/31889335/110972820-c4d0f780-839f-11eb-8013-cccadcfd79be.gif)

    * (위 영상 참고) ItemView를 하나씩 클릭했을 때는 원하는 대로 잘 동작하는 것 같지만 위 영상처럼 여러 개의 ItemView를 클릭한 후 바로 ListView를 빠르게 스크롤하면 몇 개의 ItemView가 아무 데이터도 가지고 있지 않은 빈 모습으로 중간 중간 보이는 현상이 발생합니다. 
    
    * 이러한 현상은 ListView의 동작 원리가 ItemView를 재사용하도록 동작하기 때문에 발생합니다. 먼저, 유저가 ItemView를 클릭하면 해당 ItemView에 fade out 효과가 적용되기 시작합니다. 하지만 유저가 클릭과 동시에 ListView를 아래로 빠르게 스크롤 한다면 클릭한 ItemView는 fade out 효과가 시작되자마자 화면 밖으로 나가게 되어 유저의 눈에 보이지 않는 범위에 포함되게 됩니다. 
    
    * ListView 내부 동작에 의하면 유저의 눈에 보이지 않는 ItemView는 새롭게 보일 ItemView로 재사용됩니다. 따라서 이 ItemView는 fade out이 진행되고 있음과 동시에 재사용될 ItemView로 선택되게 됩니다.(fade out 효과가 완전히 완료될 때까지 시간이 걸리기 때문에 fade out 효과가 진행되고 있는 도중에 재사용 대상으로 선택되는 것입니다.) 
    
    * 하지만 해당 ItemView는 fade out 효과가 완전히 끝난 후에야 메모리 상에서 삭제됩니다. 즉, fade out 효과가 진행되고 있는 한 내부적으로는 메모리 상에서 이 ItemView에 해당하는 객체(object)가 존재합니다. 따라서 fade out 효과가 진행 중인 객체와 재사용될 ItemView로 선택된 객체는 내부적으로 동일한 객체가 됩니다. 
    
    * fade out 효과가 완전히 끝나면 이 ItemView에 해당하는 객체는 메모리 상에서 삭제됩니다. 하지만 이 객체는 재사용되기로 선택된 객체이기도 하기 때문에 ListView의 끝에 다시 붙어야 합니다. 그러나 fade out 효과 완료로 인해 메모리 상에서 사라져 버렸으니 ListView 입장에서는 새로운 데이터를 세팅할 재사용 ItemView가 존재하지 않게 됩니다. 따라서 사용자의 눈에도 중간 중간 데이터가 세팅되지 않은 빈 공간이 보이게 됩니다. 이것을 ListView의 '붕괴(disruption) 현상'이라고 부릅니다.

    * ![07](https://user-images.githubusercontent.com/31889335/111035271-f1971480-845c-11eb-8b41-1b3ea3898bd6.gif)

    * (위 영상 참고) ListView의 붕괴 현상을 해결하기 위해 안드로이드 프레임워크가 제공하는 [ViewPropertyAnimator](https://developer.android.com/reference/android/view/ViewPropertyAnimator) API를 사용하면 해결할 수 있습니다.(ViewPropertyAnimator은 Android level 12 때 추가된 API입니다.) ViewPropertyAnimator를 사용하면 fade out 효과가 진행 중인 아이템 뷰는 아직 작업이 진행 중인 객체로 인식되어 재사용될 ItemView로 선택되지 않습니다. 따라서 위 영상처럼 붕괴 현상이 해결됩니다.

    * <img width="360" alt="02" src="https://user-images.githubusercontent.com/31889335/111036153-1c836780-8461-11eb-8a59-4e5bbc76ef4f.png">

    * (위 그림 참고) 사람들은 ListView를 더욱 복잡한 레이아웃으로 변형하여 개발하기 시작했습니다. 심지어 위 그림의 오른쪽에서 볼 수 있는 것 같은 엇갈린 그리드 레이아웃(staggered grid)도 등장했습니다.(심지어 엇갈린 모양이 화면 크기에 반응형으로 달라지기까지 해야했습니다.) 이러한 것들을 기존의 ListView API를 사용하여 구현하려면 개발자는 굉장히 많은 코드 수정을 해야했습니다. 특히, ItemView의 재사용을 담당하는 Adpater 클래스의 수정이 많았습니다. 

    * 개발자들은 ListView를 더 잘 응용하기 위해 __ViewHolder 패턴__ 이라는 것을 등장시켜 개발하곤 했습니다.(ViewHolder 패턴에 대해서는 조금 아래에서 설명할 예정입니다.) Google Android API 팀은 ViewHolder 패턴을 사용한 가장 좋은 코드 예제를 ListView API에 추가하기로 결정했습니다.

    * 더불어 기존 ListView 구현 시 구글은 개발자들에게 ItemView를 생성하는 로직(onCreate)과 생성된 ItemView에 데이터를 연결(binding)시키는 로직(onBind)을 분리하길 권장했습니다. 따라서 개발자들은 if 조건문을 통해 재사용될 ItemView가 존재하면 바로 데이터를 재사용될 ItemView에 연결하고, 재사용될 ItemView가 존재하지 않으면(null) 새로운 ItemView를 생성하는 코드를 추가해야 했습니다. 하지만 이러한 로직 분리 작업을 많은 개발자들이 쉽게 까먹는 경우가 많았고, 그로 인해 ListView의 ItemView 재사용 기능이 동작하지 않게 되어 앱 성능을 악화시키는 원인이 되었습니다.
 
    * 또한 ListView에 보여질 데이터가 바뀐 경우, __기존의 ListView의 Adapter는 데이터가 변경되었다는 사실만 알 수 있을 뿐 구체적으로 목록에서 몇 번째 데이터가 변경되었는지 인식할 수 없었습니다.__ 예를 들어, 사용자의 눈에 보이고 있는 ItemView 중 세 번째 ItemView에 연결된 데이터가 어떠한 처리에 의해 바뀌게 되면 Adapter는 데이터가 바뀌었다는 사실만 알 뿐 몇 번째 ItemView에 연결된 데이터를 바꿔야 하는지는 알 수 없었습니다. 따라서 세 번째 ItemView의 데이터를 바꾸기 위해 모든 ItemView에 데이터를 다시 연결해야 했습니다. 이러한 문제는 애니메이션 처리를 더욱 어렵게 했습니다.

    * 이렇게 다양한 곳에서 문제점이 등장하기 시작하자 Google Android API 팀은 기존 ListView가 가지고 있던 문제점들을 해결한 새로운 API인 RecyclerView를 개발해 내놓게 되었습니다. RecyclerView는 ListView가 가지고 있던 실수와 문제들이 또 다시 반복되지 않도록 철저히 구현되었습니다. Google I/O 발표에 따르면 'Google Android API 개발 팀은 ListView를 통해 기존 ListView 설계에 존재했던 실수들을 깨닫게 되었다' 라고 말하면서 이 실수들을 다시는 반복하지 않고자 RecyclerView를 개발하게 되었다고 말하기도 합니다.

* __RecyclerView의 내부 동작__

    * RecyclerView의 내부 아키텍처는 __컴포넌트(Componenet) 기반 아키텍처__ 입니다.

    * ![09](https://user-images.githubusercontent.com/31889335/111988900-73212d80-8b54-11eb-97ca-621a242dc35b.png)

    * (위 그림 참고) RecyclerView 내부 아키텍처를 구성하는 컴포넌트는 RecyclerView, LayoutManager, Item Animator, Adapter 등 입니다. 그 중 __Layout Manager__, __Item Animator__, __Adapter__ 라는 3가지 컴포넌트가 가장 중요합니다. Layout Manager는 ItemView를 올바른 위치에 배치해주는 컴포넌트이고, Item Animator는 ItemView의 애니메이션을 담당하는 컴포넌트 입니다. Adapter는 RecyclerView에 ItemView를 제공해주는 컴포넌트 입니다. 이 3가지 컴포넌트가 적절하게 상호작용한 결과로 ItemView를 RecyclerView(목록형 레이아웃) 컴포넌트 안에 배치해줍니다.

    * 이렇게 RecyclerView, Layout Manager, Item Animator, Adapter 등의 컴포넌트가 서로 상호작용하여 하나의 목록형 레이아웃을 동작하게끔 만들기 때문에 RecyclerView 내부 아키텍처를 '컴포넌트 기반 아키텍처'라고 부릅니다.

## 2️⃣ RecyclerView의 내부 동작을 자세히 알아보자~

* __Layout Manager가 뭘까?__

    * ![10](https://user-images.githubusercontent.com/31889335/111990090-f4c58b00-8b55-11eb-9461-f8f582a7320a.png)

    * (위 그림 참고) Layout Manager는 목록형 레이아웃(=RecyclerView)의 모습을 __선형(Linear)__ 으로 구성할 수 있도록 또는 __격자형(Grid)__ 으로 구성할 수 있도록 또는 __엇갈린 격자형(Staggerd Grid)__ 으로 구성할 수 있게 해주는 작업을 담당하는 컴포넌트입니다.

    * 즉, Layout Manger는 RecyclerView의 모양을 만드는 작업을 책임지고 RecyclerView 모양에 따라 ItemView를 적절한 위치에 배치하는 작업을 담당합니다. Layout Manager에게 이러한 책임이 부여되었기 때문에 RecyclerView 본인은 자기 자신이 선형 모양이 될지, 격자형 모양이 될지, 엇갈린 격자형 모양이 될지에 관해 알고 있지 않고, 각 ItemView가 어느 위치에 놓여야 하는지에 관해서도 관여하지 않습니다.

    * ![11](https://user-images.githubusercontent.com/31889335/111997193-22aecd80-8b5e-11eb-9e06-5355fa2ac121.png)

    * (위 그림 참고) 만약 유저가 RecyclerView를 스크롤하고 있는 상황이라면 어떻게 동작할까요? RecyclerView는 유저의 손가락(스크롤 인식)과만 상호작용 합니다. 만약 Linear 모양의 RecyclerView가 존재하고 유저가 목록을 더 보기 위해 위로 스크롤했다고 가정해봅시다. 이러한 경우, RecyclerView는 유저 손가락과의 상호작용 결과로 새로운 ItemView를 보여줘야 한다는 것을 인식합니다. 하지만 새로운 ItemView를 그래서 어디에 배치해야 할 지에 관해서는 RecyclerView는 모르고 Layout Manager가 알고 있습니다. 따라서 RecyclerView는 Layout Manager에게 스크롤되어 새로운 ItemView를 보여줘야 한다고 알립니다. 스크롤 알림을 받은 Layout Manager는 적절한 위치(=이 상황에서는 목록 가장 아래에 새로운 ItemView 추가)에 데이터가 연결된 ItemView를 배치합니다. 즉, Layout Manager가 ItemView를 적절한 위치에 배치하는 순간에는 데이터가 ItemView에 연결되어 있는 상태입니다.

* __Adapter가 뭘까?__

    * RecyclerView도 ListView와 마찬가지로 Adapter에 의존합니다. RecyclerView에서의 Adapter도 ItemView를 생성(create)하는 작업을 담당하는 컴포넌트 입니다. 하지만 ListView의 Adapter와 다른 점은 ItemView 생성 외에도 __ViewHolder라는 것을 생성__ 하는 작업도 담당하고 있다는 점입니다. __ViewHolder란__ ItemView 재사용을 위해 요소들을 추적 관측하는 용도로 사용하는 컴포넌트 입니다.(ViewHolder에 관해서는 조금 더 아래에서 설명할 예정입니다.)

    * 또한 Adapter는 Data Set이 변경되었을 때 RecyclerView에게 알리는(=notify) 작업도 담당하며 유저가 ItemView를 클릭할 때 발생하는 상호작용(=클릭 리스너) 처리 작업도 담당합니다. 또 RecyclerView를 구성하는 ItemView의 형태가 동일하지 않고 다른 경우를 처리하는 작업도 Adapter가 담당합니다.

* __ViewHolder가 뭘까?__

    * 앞에서 Adapter가 생성한다고 언급한 ViewHolder는 무엇일까요? ViewHolder의 life cycle(=생명 주기, 탄생에서부터 소멸까지 과정)을 아는 것이 굉장히 중요합니다.

    * RecyclerView의 내부 동작을 구성하는 컴포넌트는 앞에서 언급한 Layout Manager, Adapter 등 외에도 Cache, Recycled Pool 등이 있습니다.

    * ![12](https://user-images.githubusercontent.com/31889335/112832993-0f0ce500-90d1-11eb-9dd9-3df62ccc6b6e.png)

    * (위 그림 참고) 위에서 RecyclerView가 유저와의 상호작용의 결과, 스크롤 알림을 Layout Manager에게 알린다고 설명했습니다. 알림을 받은 Layout Manager는 몇 번째 위치(=position)에 새로운 ItemView가 배치되어야 하는지를 계산하고 이 위치를 다시 RecyclerView에게 알립니다. RecyclerView에게 위치를 알리는 이유는 RecyclerView에게 해당 위치의 뷰를 달라고 요청하는 것입니다. 그럼 RecyclerView는 Cache(=캐시)에서 해당 위치에 배치하도록 지정된 ItemView가 있는지 확인합니다.(RecyclerView 내부 동작 원리 상 일정 양의 ItemView를 캐시에 저장해놓기 때문) 만약 해당 위치에 배치되어야 하는 ItemView가 Cache에 저장되어 있다면 RecyclerView는 해당 ItemView를 다시 Layout Manager에게 전달합니다.
    
        * __Cache(캐시)__ : 임시 저장 공간, 저장해 놓은 것을 빠르게 찾을 수 있으나 저장 공간의 용량이 작아 많은 양을 저장할 순 없다.

    * ![13](https://user-images.githubusercontent.com/31889335/112835413-331df580-90d4-11eb-8a6b-954550227d40.png)

    * (위 그림 참고) 하지만 만약 해당 위치에 배치될 ItemView가 캐시에 저장되어 있지 않다면 RecyclerView는 Adapter에게 해당 위치에 배치될 ItemView의 모양(ViewType)을 물어봅니다. (위에서 RecyclerView를 구성하는 ItemView가 모두 동일한 모양일 수도 있지만 다른 모양의 ItemView들이 섞여서 하나의 RecyclerView를 구성할 수도 있다고 설명했습니다.) 그럼 Adapter는 해당 위치에 배치될 ItemView의 모양을 RecyclerView에게 알려줍니다. 이번에는 RecyclerView가 Recycled Pool에 이 모양을 위한 ViewHolder가 있는지 체크합니다.

    * ![14](https://user-images.githubusercontent.com/31889335/112836310-60b76e80-90d5-11eb-9353-de00083e4af9.png)

    * (위 그림 참고) 만약 Recycled Pool에 해당 모양을 위한 ViewHolder가 존재하지 않는다면 RecyclerView는 Adapter에게 해당 모양을 위한 새로운 ViewHolder 생성을 요청합니다.

    * ![15](https://user-images.githubusercontent.com/31889335/112836849-1387cc80-90d6-11eb-825e-71f380552191.png)

    * (위 그림 참고) 그러나 만약 Pool에 해당 모양을 위한 ViewHolder가 존재한다면 RecyclerView는 Adapter에게 이 ViewHolder를 ItemView가 배치될 위치와 연결(bind)해달라고 요청합니다. 위치와 연결된 ViewHolder는 Layout Manager에게 최종적으로 전달됩니다.

    > 조금만 기다려주세요 열심히 공부 중입니다 :)
