# 생명주기

![스크린샷 2021-10-25 오전 11 13 14](https://user-images.githubusercontent.com/66652964/138625307-1a587b59-6862-4775-8efc-3f7792654ced.png)


1. onCreate()

Activity가 처음 만들어질 때 호출되는 함수이면서, 어플리케이션이 처음 시작할 때 최초로 한번 실행되는 함수이다. 주로 view를 만들거나 view resource bind , data to list 등을 onCreate()에서 담당하며, 이전 상태의 정보를 담고있는 Bundle을 제공한다.


2. onStart()
Activity가 다시 시작되기 전에 호출, Actvitiy가 멈춘 후 호출되는 함수, Activity가 사용자에게 보여지기 직전에 호출되는 함수


3. onResume()
Activity가 비로소 화면에 보여지는 단계, 사용자에게 Focus를 잡은 상태


3-1 onRestart()

Activity가 멈춰있다가 다시 호출될 때 불리는 함수, 즉 Stopped상태였을 때 다시 호출되어 시작될 때 불린다.



---------- 다른 Activity가 호출 되는 경우 ---------- 

4. onPause()

 Activity위에 다른 Activity가 올라와서 focus를 잃었을 때 호출되는 함수. 
 완전 Activity가 가려지지 않은 상태에서 호출되는 함수.
 즉 일부분이 보이거나 투명상태일 경우 호출된다.
 다른 Activity가 호출되기 전에 실행되기 때문에 onPause()함수에서 시간이 많이 소요되는 작업이나, 
 많은 일을 처리하면, 다른 Activity가 호출되는 시간이 지연되기 때문에 많은 일을 처리하지 않도록 주의하자.
 영구적인 data는 여기서 저장한다.


5. onStop()

Activity위에 다른 Activity가 완전히 올라와 화면에서 100% 가려질 때 호출되는 함수. 홈키를 누르는 경우. 
또는 다른 액티비티페이지 이동이 있는 경우. 만약 이상태에서 Activity가 다시 불려지면, onRestart()함수가 호출된다.


6. onDestroy()

Activity가 완전히 스택에서 없어질 때 호출되는 함수, 즉 제거되는 경우. 
finish() 메소드가 호출되거나, 시스템 메모리 확보를 위해 호출된다.

</br>

# Fragment 생명주기

![fragmen](https://user-images.githubusercontent.com/66652964/138625404-bdd95d0d-6935-4901-b4e8-cc296feb884f.png)


# 인텐트
Intent는 메시징 객체로, 다른 앱 구성 요소(activity, service, receiver)로부터 작업을 요청하는 데 사용

## 명시적 인텐트
  앱 내의 특정 액티비티나 서비스 등 특정한 앱 구성 요소를 시작하는 데 사용하는 인텐트 구성 요소의 이름이 명시 되어 있음
 ```kotlin
  val intent = Intent(this, TestActivity::class.java)
  startActivity(intent)
 ```
  
## 암시적 인텐트
 특정 구성 요소의 이름을 대지 않지만, 그 대신 수행할 일반적인 작업을 선언하여 다른 앱의 구성 요소가 이를 처리
 
 문자메세지, 전화연결, 지도 앱 연결, 브라우저 연결 등이 있음
 ```kotlin
 val sendIntent = Intent()
 sendIntent.action = Intent.ACTION_SEND
 sendIntent.putExtra(Intent.EXTRA_TEXT, "test")
 sendIntent.type = "text/plain"
 startActivity(sendIntent)
 ```
 
 
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
모든 바인딩 클래스에는 해당 레이아웃 파일의 루트뷰에 대한 직접 참조를 제공하는 getRoot() 메소드도 포함됨 
이 예제에서 ActivityMainBinding 클래스의 getRoot() 메소드는 LinearLayout 루트보기를 반환합니다.
생성된 바인딩 클래스의 인스턴스를 얻으려면 정적 inflate() 메소드를 호출하십시오. 일반적으로 setContentView()를 호출하여 클래스의 루트뷰를 매개 변수로 전달하여 화면에서 활성화된 뷰로 만듭니다. 
아래의 예제에서는 액티비티에서 ActivityMainBinding.inflate()를 호출하고, setContentView에 루트뷰를 적용하는 예제를 보여줍니다.

 
 
 
  ``` Kotlin 
  private lateinit var binding: ActivityMainBinding
@Override
fun onCreate(savedInstanceState: Bundle) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
  ```
  
  findViewById와의 차이점
뷰 바인딩은 findViewById를 사용하는 것보다 중요한 장점을 가지고 있습니다.
Null safety: 뷰 바인딩은 뷰에 대한 직접 참조를 생성하므로 잘못된 뷰 ID로 인해 NPE(Null Pointer Exception)발생할 위험이 없습니다. 또한 뷰가 레이아웃의 일부 구성에만있는 경우 바인딩 클래스에 해당 참조가 포함 된 필드는 @Nullable로 표시됩니다.
Type safety: 각 바인딩 클래스의 필드에는 XML 파일에서 참조하는 뷰와 일치하는 타입을 갖고 있습니다. 이것은 클래스를 캐스팅할 때 발생할수 있는 오류가 없음을 의미합니다.
레이아웃 파일과 실제 자바/코틀린 코드간에 일치하지 않아 발생하는 문제가 런타임이 아닌 컴파일타임에 발생하기 때문에 더 빠르게 에러를 잡고, 개발자는생산성을 늘릴 수 있습니다.
Data binding과 View binding의 차이
뷰 바인딩과 데이터 바인딩 라이브러리 둘다 뷰를 직접 참조하는 바인딩 클래스를 생성합니다. 그러나 주목할만한 차이점이 있습니다.
데이터 바인딩 라이브러리는 <layout> 태그를 사용하여 만든 레이아웃만 처리합니다.
뷰 바인딩은 레이아웃 변수 또는 레이아웃 표현식을 지원하지 않으므로 XML의 데이터와 레이아웃을 바인딩하는 데 사용할 수 없습니다.
내부적으로 데이터 바인딩 클래스를 생성할때 루트뷰에 tag를 삽입하는데 뷰바인딩은 그런 작업이 없다.
뷰바인딩은 데이터바인딩보다 어노테이션 프로세싱의 일부를 사용하기 때문에 더 빠르게 바인딩 클래스를 생성한다.

  
