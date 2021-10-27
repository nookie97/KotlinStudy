# DataBinding
 Android 생태계에서 이미 많이 사용되고 있는 DataBinding(데이터바인딩)은 간단하게 xml파일에 data를 연결(binding)해서 사용하는 것인데 Activity에서 따로 View들을 정의해서 사용하지 않아도 되고 Data를 view에 연결시켜 두면 data가 변할 때 따로 세팅해주지 않아도 변경되게 할 수 있음
 
 ## Data binding과 View binding의 차이
 * 뷰 바인딩과 데이터 바인딩 라이브러리 둘다 뷰를 직접 참조하는 바인딩 클래스를 생성합니다. 그러나 주목할만한 차이점이 있습니다.
 * 데이터 바인딩 라이브러리는 <layout> 태그를 사용하여 만든 레이아웃만 처리합니다.
 * 뷰 바인딩은 레이아웃 변수 또는 레이아웃 표현식을 지원하지 않으므로 XML의 데이터와 레이아웃을 바인딩하는 데 사용할 수 없습니다.
 
![다운로드](https://user-images.githubusercontent.com/66652964/139000346-f6528712-d3e4-415a-bc0b-e05ceb8c346c.jpeg)
 
* 데이터 바인딩은 뷰 바인딩의 역할도 할 수 있고 추가로 동적 UI 콘텐츠 선언, 양방향 데이터 결합도 지원
* 뷰 바인딩이 상대적으로 간단하며 퍼포먼스 효율이 좋고 용량이 절약된다는 장점이 있어
구글 공식문서에서는 단순히 findViewById를 대체하기 위한 거라면, 뷰 바인딩을 사용할 것을 권장


```Kotlin
// findViewById 쓸때는 이렇게 했었고
textView.text = "안녕" 
 
// 뷰 바인딩 쓸때는 이렇게 했다.
binding.textView.text = "안녕"
```
이전에는 코드상에 텍스트 뷰에 문장을 넣기 위해서는 직접 코딩이 필요함

```
<TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@{user.name}" />
```

이렇게 하면 자연스레 액티비티에는 로직만을 위한 코드만 남게 되고 뷰와 관련된 작업은 레이아웃 파일에 정의됨

데이터와 뷰를 연결하는 작업을 레이아웃에서 처리하는 기술, 이것을 우리는 데이터 바인딩이라고함

## 방법
```Kotlin
android {
    ...
 
    buildFeatures {
        dataBinding = true
    }
}
```

```Kotlin 
plugins {
    id 'kotlin-kapt'
}
```
build gradle에 추가 

## layout에 추가 
```Kotlin 
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
</layout>
```
data 모델 정의
```Kotlin 
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
            name="activity" // 키 값
            type="com.example.myapplication.MainActivity" /> // 패키지명과 액티비티명 풀 네임으로 쓸것 
    </data>
</layout>
```


