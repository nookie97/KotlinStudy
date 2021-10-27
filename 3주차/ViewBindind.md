# 뷰 바인딩
 뷰 바인딩(View Binding) 은 뷰와 상호 작용하는 코드를보다 쉽게 작성할 수있는 기능입니다. 모듈의 build.gradle에서 뷰 바인딩 속성이 활성화되면 해당 모듈에있는 각 XML 레이아웃 파일에 대한 바인딩 클래스가 자동으로 생성됩니다. 바인딩 클래스 인스턴스에는 해당 레이아웃에 ID가 있는 모든 뷰에 대해 직접적으로 참조됩니다.
 ```Kotlin
 android {
     ...
     buildFeatures {
        viewBinding = true
    }
 }
 ```
 
 바인딩 클래스를 생성하는 동안 레이아웃 파일을 무시하려면
 tools : viewBindingIgnore = “true” 속성을 해당 레이아웃 파일의 최상단 루트 뷰에추가하십시오.
  ```
<RelativeLayout
    ...
    tools:viewBindingIgnore="true" >
    ...
</RelativeLayout>
 ```
 
 ## ViewBinding 사용법
 activity_main.xml -> ActivityMainBinding
 </br>
 sub_view.xml -> SubViewBinding
 
 ```
 <LinearLayout ... >
    <TextView android:id="@+id/name" />
    <ImageView android:cropToPadding="true" />
    <Button android:id="@+id/button"
        android:background="@drawable/rounded_button" />
 </LinearLayout>
 ```
 
 생성되는 바인딩 클래스의 이름은 ActivityMainBinding 되고
 이 클래스에는 name이라는 ID를 갖는 TextView와 button이라는 ID를 갖는 Button 필드를 갖고 레이아웃의 ImageView에는 ID가 없으므로 바인딩 클래스에 대한 참조가 없음
 </br>
 * 모든 바인딩 클래스에는 해당 레이아웃 파일의 루트뷰에 대한 직접 참조를 제공하는 getRoot() 메소드도 포함됨 
 * ActivityMainBinding 클래스의 getRoot() 메소드는 LinearLayout 루트를 반환
 * 바인딩 클래스의 인스턴스를 얻으려면 정적 inflate() 메소드를 호출하고 setContentView()를 호출하여 클래스의 루트뷰를 매개 변수로 전달하여 화면에서 활성화된 뷰로 만듦

 
 ``` Kotlin 
 private lateinit var binding: ActivityMainBinding
 @Override
 fun onCreate(savedInstanceState: Bundle) {
     super.onCreate(savedInstanceState)
     binding = ActivityMainBinding.inflate(layoutInflater)
     setContentView(binding.root)
 }
 ```
  
 ## findViewById와의 차이점
   * Null safety: 뷰 바인딩은 뷰에 대한 직접 참조를 생성하므로 잘못된 뷰 ID로 인해 NPE(Null Pointer Exception)발생할 위험이 없고 뷰가 레이아웃의 일부 구성에만있는 경우 바인딩 클래스에 해당 참조가 포함 된 필드는 @Nullable로 표시됩니다.
   * Type safety: 각 바인딩 클래스의 필드에는 XML 파일에서 참조하는 뷰와 일치하는 타입을 갖고 있어 클래스를 캐스팅할 때 발생할수 있는 오류가 없음
   * 레이아웃 파일과 실제 자바/코틀린 코드간에 일치하지 않아 발생하는 문제가 런타임이 아닌 컴파일타임에 발생하기 때문에 더 빠르게 에러를 잡고, 개발자는생산성을 늘릴 수 있습니다.


