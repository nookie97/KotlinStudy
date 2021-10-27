# DataBinding
 Android 생태계에서 이미 많이 사용되고 있는 DataBinding(데이터바인딩)은 간단하게 xml파일에 data를 연결(binding)해서 사용하는 것인데 Activity에서 따로 View들을 정의해서 사용하지 않아도 되고 Data를 view에 연결시켜 두면 data가 변할 때 따로 세팅해주지 않아도 변경되게 할 수 있음

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


