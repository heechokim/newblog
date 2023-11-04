---
layout: post
title:  "[안드로이드] 안드로이드 버전이 뭐예요?(targetSdkVersion, compileSdkVersion, API 레벨)"
date:   2021-03-18 18:34:10 +0700
categories: [안드로이드]
---

> __참고 자료__
>
> [책 - 안드로이드 프로그래밍 Next Step](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=110015332)
>
> [Android Developers 사이트 - API 레벨이란?](https://developer.android.com/guide/topics/manifest/uses-sdk-element)
>
> [Android Developers 사이트 - \<uses-sdk>](https://developer.android.com/guide/practices/compatibility?hl=ko#Versions)
>
> [Android Developers 사이트 - 다양한 플랫폼 버전 지원](https://developer.android.com/training/basics/supporting-devices/platforms?hl=ko)
>
> [Android Developers 사이트 - 최신 SDK를 타겟팅해야 하는 이유](https://developer.android.com/distribute/best-practices/develop/target-sdk?hl=ko#why-target)

<br>

## 0️⃣ 프롤로그

기억 상 중학교 1학년 시절부터 스마트폰을 썼던 것 같습니다.. 갤럭시 노트 1이 처음 나왔을 때 큰 화면과 노트 펜을 보고 너무 가지고 싶었죠^^.. ㅋㅋㅋ 부모님을 졸라 손에 넣은 갤럭시 노트 1!! 1년 정도 쓰다가 변기통에 빠뜨려 사망하고 말았습니다.. 

그 때 갤럭시 노트 1에 탑재된 안드로이드 운영체제 버전이 아마 진저브레드였던 것 같습니다. 

안드로이드 운영체제가 탑재된 모바일 디바이스를 사용하다보면 몇 년에 한 번씩 안드로이드 운영체제 버전이 업그레이드 되어 업데이트하면 더 세련된 화면들로 바뀌는 현상을 경험했을 것입니다.

이번 포스팅에서는 '안드로이드 버전'과 관련된 내용을 공부해보겠습니다! 포스팅 내용에 잘못된 개념이나 잘못된 사용법이 작성되어 있다면 댓글로 알려주세요🧚🏻‍♀️

## 1️⃣ What is 안드로이드 버전?

* __먼저 읽고 오면 좋은 포스팅__

    * [이 블로그의 다른 포스팅 - 안드로이드가 뭐예요?](https://choheeis.github.io/newblog//articles/2021-03/android)를 먼저 읽고 오면 이 포스팅에서 자주 사용할 '안드로이드', '안드로이드 운영체제' 라는 용어에 대해 혼동하지 않고 잘 이해할 수 있을 것입니다 :)

* __Android에도 Version이 있다?__

    * <img width="826" alt="01" src="https://user-images.githubusercontent.com/31889335/111580326-ac773780-87fa-11eb-8367-27e5890b38fa.png">

    * (위 그림 참고) [Android Developers 사이트의 플랫폼 개요](https://developer.android.com/about?hl=ko)만 들어가도 최신 Android Version에 대해 소개하고 홍보하는 글을 볼 수 있습니다. 2021년 3월 기준 최신 버전은 Android 11 이네요!

    * <img width="200" alt="02" src="https://user-images.githubusercontent.com/31889335/111580561-12fc5580-87fb-11eb-987e-b31e21092e0c.png">

    * (위 그림 참고) [Android Developers 사이트의 플랫폼 출시](https://developer.android.com/about/versions/11?hl=ko)에서 볼 수 있는 버전 리스트를 보면 '롤리팝', '마시멜로우', '누가', '오레오', '파이' 등 예전에 출시되었던 안드로이드 버전들에 대한 설명도 볼 수 있습니다.

    * 앱 개발자는 앱을 처음 출시한 후 지속적으로 새로운 기능을 추가하거나 기존 기능을 개선하여 새로운 버전으로 업그레이드하는 경우가 많습니다. 따라서 해당 앱을 설치하여 사용하고 있던 사용자들은 앱이 제공하는 새로운 기능을 사용하기 위해 새로운 버전으로 업데이트하는 경우가 대부분 입니다. 안드로이드 앱만 봐도 지속적으로 새로운 버전이 등장하는 것처럼 '안드로이드 자체'도 새로운 기능과 개선된 기능을 포함한 NEW Version이 지속적으로 출시되고 있습니다.

    * 즉, 안드로이드 운영체제는 계속해서 발전해가고 있다는 것입니다.

* __Android Version을 한 눈에 볼 수 있는 표__

    * <img width="946" alt="03" src="https://user-images.githubusercontent.com/31889335/111586467-37a8fb00-8804-11eb-8c7f-f9273767561e.png">

    * (위 그림 참고 - [출처 : Android Devlopers 사이트](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)) 위 표는 안드로이드의 버전을 최신순부터 쭉~ 한 눈에 정리해놓은 표입니다. 앞으로 새로운 안드로이드 버전이 계속 출시된다면 이 표도 계속 업데이트 될 것입니다.

    * (위 그림 참고) 위 표를 보면 일반 사람들에게 익숙한 '롤리팝(LOLLIPOP)', '젤리빈(JELLY_BEAN)', '아이스크림 샌드위치(ICE_CREAEM_SANDWICH)' 등의 이름들이 __버전 코드(Version Code)__ 라는 항목에 작성되어 있습니다. __버전 코드라는 것은 숫자로 표시된 버전명을 문자열로 바꾸어 나타낸 것__ 입니다. 예를 들어, 안드로이드 버전 '5.0'를 'LOLLIPOP'이라는 문자열로 바꾸어 부르는 것입니다.

    * (위 그림 참고) 위 표를 보면 버전 코드 외에도 __플랫폼 버전__ 과 __API 레벨__ 이라는 항목이 있습니다. 심지어 하나의 API 레벨에 여러 개의 플랫폼 버전이 존재하기도 합니다. 예를 들어, 위 표를 보면 API 레벨 14에 해당하는 플랫폼 버전은 android 4.0, android 4.0.1, android 4.0.2 3개가 존재합니다. '플랫폼' 이라는 것에 대해서는 [이 블로그의 다른 포스팅 - 안드로이드가 뭐예요?](https://choheeis.github.io/newblog//articles/2021-03/android)에서 설명하고 있습니다. 이 포스팅을 보고 온다면 '플랫폼'이 '안드로이드'를 말하는 것이며 동시에 '안드로이드 플랫폼'을 말하고 있음을 알 수 있을 것입니다. 따라서 '플랫폼 버전'은 '안드로이드 버전'을 나타냅니다. 플랫폼 버전 중 android 4.0 버전을 보면 처음엔 android 4.0이 출시되었다가 조금 더 개선하여 android 4.0.1 으로 버전 업그레이드를 했고, 마지막으로 android 4.0.2로 추가 업그레이드된 것입니다.

    * (위 그림 참고) 또 안드로이드는 각 플랫폼 버전에 대응되는 __플랫폼 명(안드로이드 버전 명)__ 을 만들어 일반 사람들에게 쉽게 안드로이드 버전을 소개합니다. 그러나 이러한 플랫폼 명과 API 레벨, 플랫폼 버전 등이 일대일 매칭이 되지 않아 혼동되는 경우가 많습니다. 혼동될 경우 위 표를 봅시다!

    * 안드로이드 버전 별로 등장한 새로운 기능 및 개선된 기능을 자세히 알아보고 싶다면 [여기](https://developer.android.com/about/versions?hl=ko) 를 참고하면 됩니다.

## 2️⃣ What is API 레벨?

* __API 레벨(API LEVEL)이 무엇인가?__

    * 안드로이드 버전의 역사를 볼 수 있었던 위 표에는 'API 레벨' 이라는 항목이 있었습니다. API 레벨은 가장 초창기 안드로이드 플랫폼 버전인 android 1.0 부터 가장 최신 안드로이드 플랫폼 버전까지 1부터 1씩 증가했음을 알 수 있습니다.

    * __API 레벨__ 은 Android 플랫폼 버전이 업데이트 될 때마다 수정되는 __Android Framework(안드로이드 프레임워크)의 수정 버전을 고유하게 식별해주는 정수 값__ 입니다.

    * [이 블로그의 다른 포스팅 - 안드로이드가 뭐예요?](https://choheeis.github.io/newblog//articles/2021-03/android)를 읽어보면 'Android Framework'가 무엇인지에 대해서 알 수 있기 때문에 이 포스팅에서는 따로 설명하지 않겠습니다.

    * 즉, Android 버전이 업데이트는 Android Framework 업데이트도 포함할 수 있기 때문에 Android Framework의 수정 히스토리를 Android 버전과 별도로 표시하는 것이 API 레벨입니다.

    * <img width="946" alt="03" src="https://user-images.githubusercontent.com/31889335/111586467-37a8fb00-8804-11eb-8c7f-f9273767561e.png">

    * (위 그림 참고) 위 표를 보면 Android 플랫폼 최초 출시 버전인 android 1.0 버전은 API Level 1을 제공했음을 알 수 있습니다. android 1.0 버전부터 계속 후속 릴리즈 버전이 나오면서 API 레벨도 1씩 증가했습니다. 또한 각 Android 플랫폼 버전별로 지원하는 API 레벨이 지정되어 있습니다.

## 3️⃣ Android 앱 개발 시 앱이 어떤 안드로이드 버전에서 동작할 것인지 지정해야 한다!

* __Android 앱 개발 시 앱이 어떤 안드로이드 버전(플랫폼 버전)에서 동작할 것인지 지정해줘야 하는 이유?__

    * 위에서 알아본 것처럼 안드로이드는 시간이 지날수록 새로운 기능과 개선된 기능이 추가되어 업데이트되고 발전해갑니다. 이러한 특징 때문에 개발자가 앱 개발 시 사용한 API 레벨이 모든 앱에서 동일하다는 보장이 없습니다.
    
    * 한 가지 예를 들어봅시다. 예를 들어, 달력 기능과 관련된 API인 [CalendarProvider](https://developer.android.com/guide/topics/providers/calendar-provider?hl=ko) API는 android 4.0(API 레벨 14)버전이 출시될 때 새로 나온 API 입니다. 만약 내가 만든 앱이 달력 관련 기능을 제공하기 위해 CalendarProvider API를 사용했다면 이 앱은 android 4.0 이후 버전이 설치된 디바이스 기기에서만 설치되도록 해야 합니다. 만약 android 4.0 이전 버전이 설치된 디바이스에서 내가 만든 앱을 설치하여 실행한 경우, android 4.0 이전에는 CalendarProvider API가 존재하지 않기 때문에 앱이 정상적으로 실행되지 않을 것입니다.
    
    * 이러한 문제에 대응하기 위해 안드로이드 프로젝트의 build.gradle 파일에 존재하는 minSdkVersion(최소 Sdk 버전)이라는 설정의 값을 14(API 레벨)로 지정해줘야 합니다. 이는 이 앱을 실행하려면 디바이스 기기에 최소한 API 레벨 14는 설치되어 있어야 함을 의미합니다. 다른 말로는, 이 앱이 API 레벨 14가 존재하는 디바이스 기기까지와는 __호환__ 된다고 말할 수 있습니다.

    * Android 플랫폼의 버전이 계속 발전하고 있는 상황에서 __앱 호환성__ 이라는 것은 중요한 요소가 되었습니다.

* __앱 호환성이란?__

    * ![05](https://user-images.githubusercontent.com/31889335/112323966-9d5e2100-8cf5-11eb-8dcc-225f0fd2d43b.png)

    * (위 그림 참고 - [출처](https://developer.android.com/about/versions/11?hl=ko)) Android에서 말하는 __앱 호환성은 앱이 Android 플랫폼 버전의 특정 버전에서 올바르게 실행되는 것__ 을 말합니다. 새로운 Android 플랫폼 버전이 출시되었다는 것은 이전 버전에 비해 개선된 사항들이 새 버전에 추가되었다는 것입니다. 위 그림에서 볼 수 있듯이, 새 버전에는 '개인 정보 보호'와 '보안 향상'에 관련된 변경 사항이 추가될 수도 있고, Android 운영체제 전반에 걸쳐 사용자 환경을 개선하는 변경 사항이 추가될 수도 있습니다. 또 아예 기존에 없었던 새로운 기능이 추가될 수도 있습니다. 이러한 변경 사항은 앱에 영향을 줄 수 있습니다. 따라서 새로운 Android 플랫폼 버전이 출시되면 개발자는 출시 버전에 포함된 변경 사항을 살펴보고 해당 버전에서 앱이 잘 동작하는지 테스트한 다음, 사용자를 위해 호환성 업데이트를 하는 것이 중요합니다.

    * 또한 일반적으로 Android 플랫폼 버전이 새로 출시되면 사용자들은 이에 관심과 흥미를 가지고 자신의 기기에 Android 플랫폼을 업데이트하는 경우가 많습니다. 또 새 변경 사항을 자신이 사용하는 앱에서 경험해 보고 싶어 합니다. 사용자의 이러한 특성을 파악하고 개발자는 Android 플랫폼 버전이 새로 출시되면 앱이 새 버전에서 비정상적으로 동작하는 부분은 없는지 테스트하는 것이 좋습니다.

    * <img width="915" alt="06" src="https://user-images.githubusercontent.com/31889335/112324373-12315b00-8cf6-11eb-8812-5fbdfc4e9e21.png">

    * (위 그림 참고 - [출처](https://developer.android.com/about/versions/12?hl=ko)) 따라서 Google은 Android 플랫폼 최신 버전이 공식적으로 출시되기 전에 개발자 프리뷰(preview) 및 베터 버전을 먼저 출시하여 개발자들에게 테스트 기간을 주고 있습니다.

* __minSdkVersion 지정하기__

    * minSdkVersion은 __앱을 실행하려면 필요한 최소 API 레벨__ 을 의미합니다. 즉, '이 앱을 실행하려는 디바이스 기기에는 최소한 몇 API 레벨을 제공하는 Android 플랫폼 버전이 설치되어 있어야 한다' 라는 의미입니다.

    * 만약 어떤 디바이스 기기에서 A라는 앱을 설치하려고 하는 상황을 가정해봅시다. 만약 이 디바이스 기기의 Android 플랫폼 버전이 A앱에 설정된 minSdkVersion보다 낮은 버전의 API 레벨을 제공한다면 Android 시스템은 자동으로 앱 설치를 방지합니다.

    * minSdkVersion은 항상 지정해줘야 합니다. 만약 이 설정을 지정해주지 않을 경우 기본 값으로 API 레벨 1이 minSdkVersion으로 지정됩니다. API 레벨 1이 설정되었다는 것은 해당 앱이 모든 Android 버전과 호환된다는 의미입니다.

    * 만약 API 레벨 14부터 지원하는 CalendarProvider API를 사용한 앱의 minSdkVersion이 1일 경우(=minSdkVersion을 지정하지 않았을 경우) 해당 앱은 런타임 시 작동이 중단될 수 있습니다. API 레벨 14를 지원하지 않는 디바이스 기기에서도 이 앱을 설치할 수 있지만 정상적인 앱 작동에 필수인 CalendarProvider API를 제공하지 않기 때문입니다. 이렇게 minSdkVersion이 적절하지 않게 설정되면 앱이 중단될 수 있기 때문에 앱에 사용한 API 레벨을 고려하여 적절한 minSdkVersion을 지정해줘야 합니다.

    * 앱의 minSdkVersion을 설정할 수 있게 된 것은 API 레벨 4 이후였습니다.(애초에 minSdkVersion이라는 기능이 API 레벨 4 때 처음 추가되었기 때문)

* __일반적인 Android 앱은 Android 플랫폼의 신규 버전과 호환된다!__

    * 새로운 Android 플랫폼 버전이 출시되면 Android Framework API에 관한 변경도 존재할 수 있습니다. 하지만 신규 Android 플랫폼 버전에 포함된 API 변경 사항은 이전 버전의 API를 Android Framework에서 완전히 삭제한 후 그것을 대체하는 것이 아닙니다. 이전 버전이 제공하는 API는 그대로 두고 거기에 추가로 새로운 API도 제공되는 것입니다.

    * 따라서 이미 이전 API 레벨을 사용하여 개발된 Android 앱이 최신 Android 플랫폼 버전을 설치한 디바이스 기기(최신 API 레벨을 제공)에서 실행된다고 하더라도 앱 실행에 있어서 문제가 되지 않습니다. 왜냐하면 최신 API 레벨은 바로 이전 API 레벨이 제공하는 API도 제공하고, 새롭게 추가된 API도 제공하기 때문입니다.  __즉, 어떠한 이유나 이슈 발생으로  Android Framework에서 완전히 삭제된 API를 사용한 앱이 아니라면, 일반적인 Android 앱은 Android 플랫폼의 이후 버전 및 상위 API 레벨과 호환됩니다.__ 

    * 추가로 예를 들어 보자면, 사용자가 API 레벨 29를 사용하여 개발된 Android 앱을 설치하여 잘 사용하고 있었습니다. 그런데 API 레벨 30을 제공하는 새로운 Android 플랫폼 버전이 등장하여 이 사용자는 디바이스 기기에 설치된 Android 플랫폼 버전을 업데이트 설치했습니다. 이러한 경우는 어떻게 되는 걸까요? Android 플랫폼 업데이트가 정상적으로 설치되면 이 앱은 API 레벨 29와 새 시스템 기능을 가진 환경의 새로운 런타임 버전에서 실행됩니다.

    * ![04](https://user-images.githubusercontent.com/31889335/112164521-4a229a80-8c31-11eb-8264-c90e479d30ef.png)

    * (위 사진 참고) 하지만 위에서 설명한 CalendarProvider API 예시처럼, 경우에 따라서는 어떤 Android 플랫폼 버전에서는 앱이 잘 동작하는데 어떤 Android 플랫폼 버전에서는 앱이 잘 동작하지 않는 경우가 있을 수 있습니다. 이러한 문제에 대응하기 위해 개발자는 다양한 Android 플랫폼에서 앱을 테스트해 볼 필요가 있습니다. 위 그림 처럼 Android SDK는 개발자가 여러 플랫폼 버전을 선택적으로 다운로드할 수 있게 제공되므로 필요한 버전을 다운로드하여 테스트해보면 됩니다.

* __그러나 신규 API 레벨 버전을 사용한 앱을 Android 플랫폼 이전 버전에서 실행하면 100% 호환되지 않는다.__

    * 이전 API 레벨 버전을 사용하여 개발된 앱을 새로운 Android 플랫폼에서 실행시키는 것은 문제가 되지 않는다고 위에서 설명했습니다. 그러나 반대로 신규 API 레벨 버전을 사용한 앱을 이전 Android 플랫폼에서 실행시킬 경우, 이전 플랫폼은 신규 API를 제공하지 않기 때문에 앱이 제대로 동작하지 않을 수 있습니다.

    * 디바이스 기기에 설치된 Android 플랫폼을 최신 버전으로 업데이트한 후에는 이전 플랫폼 버전으로 다운 그레이드될 가능성이 없습니다. 하지만 개발자는 항상 최신 버전으로 업데이트하지 않은 기기가 있을 수 있다는 인식을 해야 합니다.(실제로 제 주변 지인도 귀찮아서 Android 플랫폼 버전을 두 개 버전이 새로 출시될 동안 업데이트 안 한 사람이 있습니다^^..)

    * 즉, 신규 API 레벨을 사용하여 앱을 개발한 후 배포했을 때, 이전 플랫폼 버전이 설치된 기기에서는 제대로 동작하지 않을 수 있습니다.

* __새로운 Android 플랫폼 버전이 출시되었을 때, 앱에 영향을 주는 변경 사항의 두 가지 유형__

    * 새로운 Android 플랫폼 버전이 출시되면 반드시 이전 버전보다 개선된 사항들이 존재합니다.('개선된 사항'이라는 단어를 앞으로 '변경 사항'이라고 언급하겠습니다)

    * 새로운 Android 플랫폼 버전이 제공하는 변경 사항의 유형은 두 가지로 나뉩니다. __모든 앱에 적용되는 변경 사항__ 과 __타겟팅된 앱에 적용되는 변경 사항__ 으로 나뉩니다.

    * ![07](https://user-images.githubusercontent.com/31889335/112330622-8c181300-8cfb-11eb-90cf-f4d9f1d9b5f0.png)

    * (위 그림 참고 - [출처](https://developer.android.com/about/versions/11/behavior-changes-all?hl=ko)) 위 그림은 2021년 3월 기준 가장 최신 Android 플랫폼 버전인 11 에서 제공하는 변경 사항 중 '모든 앱'에 적용되는 변경 사항을 소개하는 글입니다. 즉, Android 11이 설치된 디바이스 기기에서 실행되는 모든 앱에 적용되는 변경 사항인 것입니다. 그림의 출처 사이트에 접속해보면 어떤 변경 사항들이 '모든 앱'에 적용되는 변경 사항인지 알 수 있습니다.

    * 위 그림의 내용에서 빨간색 박스 부분을 보면 'targetSdkVersion에 관계없이'라는 말이 있습니다. 아직 targetSdkVersion이라는 것이 무엇인지 설명하지는 않았지만 분홍색 박스 부분을 추가로 봐봅시다. 'Android 11을 타겟팅하는 앱에만 영향을 주는 변경 사항을 검토해야 한다'는 말이 적혀있습니다. 

    * 파란색 박스 부분과 분홍색 박스 부분을 통해 대충 유추해보자면.. 특정 Android 플랫폼 버전을 타겟팅하는 앱이 존재하는 것 같고, targetSdkVersion이라는 설정으로 타겟팅할 버전을 지정하는 것 같죠? >.< (targetSdkVersion에 대해서는 조금 아래에서 설명할 예정입니다)

    * ![08](https://user-images.githubusercontent.com/31889335/112331866-a43c6200-8cfc-11eb-83bb-3b8808778266.png)

    * (위 그림 참고 - [출처](https://developer.android.com/about/versions/11/behavior-changes-11?hl=ko)) 위 그림의 내용에서 파란색 박스 부분을 보면 Android 11 이상을 타겟팅하는 앱에만 적용되는 변경 사항들이 존재한다는 것을 알 수 있습니다. 또 Android 11을 타겟팅하는 앱은 targetSdkVersion라는 것을 30으로 설정한 앱이라는 것도 알 수 있습니다. 만약 내가 개발한 앱의 targetSdkVersion을 30으로 설정했다면 이 앱이 Android 11에서 변경된 사항들을 반영하도록 수정해야 합니다.(Android 플랫폼 버전 11이 제공하는 API 레벨은 30입니다)

    * 이처럼 새로운 Android 플랫폼 버전이 제공하는 변경 사항의 유형은 '모든 앱에 적용되는 변경 사항' 과 '타겟팅된 앱에 적용되는 변경 사항'으로 두 가지 유형이 존재한다는 것을 알 수 있습니다.

* __targetSdkVersion 지정하기__

    * targetSdkVersion은 __앱의 타겟 API 레벨__ 을 지정하는 정수입니다. '타겟'이라는 말의 의미처럼 해당 앱이 타겟으로 하고 있는 API 레벨이라는 뜻입니다. 만약 targetSdkVersion이 30로 지정되어 있으면 개발자는 API 레벨 30이 제공되는 환경에서 해당 앱 테스트를 완료했고 앱을 실행하는데 문제가 없다는 의미입니다. 즉, [이 블로그](https://mrgamza.tistory.com/615)의 말을 인용하자면 "우리가 최종적으로 호환성을 맞춘 버전이 이 버전이다." 라고 선언하는 것이 targetSdkVersion 입니다.

    * __정리 )__ minSdkVersion은 앱이 호환되는 가장 낮은 API 레벨을 말하고, targetSdkVersion은 앱을 개발하고 테스트한 가장 높은 API 레벨을 말합니다.
    
    * 만약 targetSdkVersion을 설정하지 않을 경우 minSdkVersion과 동일한 값으로 설정됩니다.

    * 신규 Android 플랫폼 버전이 제공하는 변경 사항 중 '타겟팅된 앱에만 적용되는 변경 사항'이 있다는 것을 위에서 언급했습니다. 이러한 변경 사항은 targetSdkVersion에 설정된 값을 보고 변경 사항을 적용할지 말지 결정됩니다.
    > 이 위에서부터 다시 하자. 호환성 프레임워크 도구 부터 읽자.

    > minsdkversion 어디서 작성하는지 , Android 11과 앱의 호환성 테스트(새로운 기능 출시됨)
    > https://developer.android.com/guide/topics/manifest/uses-sdk-element#ApiLevels
    > https://developer.android.com/training/basics/supporting-devices/platforms?hl=ko
    > https://developer.android.com/distribute/best-practices/develop/target-sdk?hl=ko
    > https://developer.android.com/about/versions/11/test-changes?hl=ko#list
    > https://developer.android.com/guide/app-compatibility/test-debug?hl=ko
    > https://developer.android.com/guide/app-compatibility?hl=ko
    > 얘네 참고 문서에 없는 것 추가하기

    * 안드로이드 앱을 조회할 수 있고 설치할 수 있도록 해주는 [Google Play](https://play.google.com/store?utm_source=apac_med&utm_medium=hasem&utm_content=Jan0421&utm_campaign=Evergreen&pcampaignid=MKT-EDR-apac-kr-1003227-med-hasem-py-Evergreen-Jan0421-Text_Search_BKWS-BKWS%7cONSEM_kwid_43700058439438031_creativeid_477136209046_device_c&gclid=CjwKCAjw9MuCBhBUEiwAbDZ-7oqhHVcoSbCI_MqISe9w1YogduL5vR2-m_9-9tKDaEleYvQBQKuBwBoCfx8QAvD_BwE&gclsrc=aw.ds)에는 [Google Play 필터](https://developer.android.com/google/play/filters) 라는 기능이 적용되어 있습니다. Google Play는 Google Play Store에 업로드된 모든 앱의 minSdkVersion과 targetSdkVersion을 확인하여 각 앱이 요구하는 Android 버전을 파악합니다. 앱이 요구하는 버전과 맞지 않는 디바이스 기기를 가진 사용자가 Google Play Store를 실행하여 앱을 조회할 경우, Google Play는 해당 앱을 앱 리스트에 아예 표시하지 않습니다.