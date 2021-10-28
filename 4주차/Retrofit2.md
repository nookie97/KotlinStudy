# Retrofit

Retrofit2는 Square사에서 만든 네트워킹 라이브러리로, 오래 전에 Retrofit이라는 라이브러리를 출시한 뒤 기존 라이브러리의 문제점을 개선해 현재의 Retrofit2을 만듦

Retrofit2는 Retrofit의 업그레이드 된 버전이기 때문에 일반적으로 1과 2를 구분짓지 않고 Retrofit이라고 통용 그렇기 때문에 포스팅 중에 Retrofit이라고 하는 것은 모두 Retrofit2를 의미하는 것

Retrofit2는 네트워크 요청을 처리하기 위해서 OkHttp를 사용하고 enqueue와 excute를 사용하여 동기, 비동기 처리를 지원함. 또한 JSON에서 Java 객체로 변환 해 줄 JSON 변환기가 내장되어있지 않기 때문에 JSON 변환기 라이브러리에 대한 지원을 제공

## 장점 - 속도, 편의성, 가독성
 * OkHttp에서는 사용 시 대개 Asynctask를 통해 비동기로 실행하는데, Asynctask가 성능상 느리다는 이슈가 있었음. Retrofit에서는 Asynctask를 사용하지 않고 자체적인 비동기 실행과 스레드 관리를 통해 속도를 훨씬 빠름
 * OkHttp에서도 쿼리스트링, request, response 설정 등 반복적인 작업이 필요한데, Retrofit에서는 이런 과정을 모두 라이브러리에 넘겨서 처리하도록 하였고, 따라서 사용자는 함수 호출시에 파라미터만 넘기면 되기 때문에 작업량이 훨씬 줄어들고 사용하기 편리
 * 인터페이스 내에 annotation을 사용하여 호출할 함수를 파라미터와 함께 정의해놓고, 네트워크 통신이 필요한 순간에 구현없이 해당 함수를 호출하기만 하면 통신이 이루어지기에 코드를 읽기가 매우 편함 Asynctask를 쓰지 않기에 불필요하게 코드가 길어질 필요도 없으며, 콜백 함수를 통해 결과가 넘어오도록 되어있어 매우 직관적인 설계가 가능

## 사용법
> 1. 라이브러리 추가 & INTERNET 권한 추가
 
build.gradle (:app) 파일에 라이브러리를 추가한다.
```
dependencies {
    ...
    
    // Retrofit, Gson, RxAdapter
    implementation "com.squareup.retrofit2:retrofit:$retrofit_version"
    implementation "com.squareup.retrofit2:converter-gson:$retrofit_version"
    implementation "com.squareup.retrofit2:adapter-rxjava2:$retrofit_version"
    
    // okhttp
    implementation "com.squareup.okhttp3:okhttp:$okhttp_version"
    implementation "com.squareup.okhttp3:logging-interceptor:$okhttp_version"
}
```
 * retrofit: Retrofit 라이브러리

 * converter-gson: Json데이터를 사용자가 정의한 Java 객채로 변환해주는 라이브러리

 * adapter-rxjava2: Retrofit을 Rx형태로 사용하도록 설정해주는 어댑터 라이브러리

 * okhttp, logging-intercepter: Retrofit을 사용해 받는 HTTP데이터들을 로그상으로 확인하기 위해 사용하는 라이브러리

AndroidManifest.xml 파일에 INTERNET 권한을 추가한다.
```
<uses-permission android:name="android.permission.INTERNET" />
```
> 2. 응답 데이터 정의
```Json
{
  "code": "0200",
  "msg": "LOGIN SUCCESS",
  "info": {
    "COMMON": {
      "MEM_ID": "nookie97",
      "MEM_CD": "8678",
      "MU_ID": "nookie9999",
      "MU_CD": "200362",
      "MU_NAME": "홍석진",
      "PRIVACY_CHECK": "1",
      "PRIVACY_URL": "",
      "U_IMG_PATH": "https://content.chungchy.com/data/ELSD/data/user_img/nookie97/....",
      "MEM_POINT_EUM": "1",
      "USER_POINT": 2226
    },
    "CLASS": [
      {
        "CLASS_CD": "74199",
        "CLASS_NAME": "androidtest"
      },
      {
        "CLASS_CD": "74200",
        "CLASS_NAME": "iostest"
      }
    ]
  }
}
```

```Kotlin
data class UserListContents(
    @SerializedName("code")
    val code: String,
    @SerializedName("msg")
    val msg: String,
    @SerializedName("info")
    val info: MainInfo
)
data class MainInfo(
    @SerializedName("COMMON")
    val COMMON: CommonInfo,
    @SerializedName("CLASS")
    val CLASS: List<ClassInfo>,
    @SerializedName("msg")
    val msg: String,
    @SerializedName("url")
    val url: String
)

data class CommonInfo(
    @SerializedName("MEM_ID")
    val MEM_ID: String,
    @SerializedName("MEM_CD")
    val MEM_CD: String,
    @SerializedName("MU_ID")
    val MU_ID: String,
    @SerializedName("MU_CD")
    val MU_CD: String,
    @SerializedName("MU_NAME")
    val MU_NAME: String,
    @SerializedName("PRIVACY_CHECK")
    val PRIVACY_CHECK: String,
    @SerializedName("PRIVACY_URL")
    val PRIVACY_URL: String,
    @SerializedName("U_IMG_PATH")
    var U_IMG_PATH: String,
    @SerializedName("MEM_POINT_EUM")
    val MEM_POINT_EUM: String
)

data class ClassInfo(
    @SerializedName("CLASS_CD")
    val CLASS_CD: String,
    @SerializedName("CLASS_NAME")
    val CLASS_NAME: String
)
```
Gson을 사용할 때 클래스 필드에 @SerializedName 어노테이션을 붙여야 한다.

 

이 어노테이션의 역할은 Gson이 JSON 키를 Java 필드에 매핑하는 것을 도와주는 것인데, 이를 사용하지 않으면 클래스 멤버 변수와 JSON 키의 이름이 동일해야 매핑이 된다. 하지만 @SerializedName를 사용하면 JSON 키와 다른 이름의 변수명을 사용할 수 있다. 예를 들어, 위 예제에서처럼 @SerializedName을 사용해 JSON 키 "name"이 Java 필드 "convertName"에 매핑되어야 한다고 Gson에게 알려주어 "name"으로 들어왔지만 "convertName"으로 사용할 수 있는 것이다.

또한 JSON 키는 일반적으로 snake_case 규칙을 따르기 때문에 @SerializedName 어노테이션을 사용해 변수명에 대한 camelCase 규칙을 일관적으로 유지할 수 있다. 

 
하지만 이름의 변경이 없는 경우에도 @SerializedName 어노테이션을 붙이는 것이 좋다. 그 이유는 애플리케이션을 Release 할 때 소스 코드가 난독화 되는 과정에서 Java 필드가 변환되고, 이로 인해 Gson 매핑에 오작동이 일어날 수 있기 때문에 @SerializedName는 필수로 사용하는 것이 좋다고 한다.

> 3. API 인터페이스 정의

필요한 Data Class를 만들었다면 이제 API 통신을 위한 인터페이스를 정의해야 한다.
```Kotlin
interface ServerApi {
    @FormUrlEncoded                      // parameters can only be used with form encoding. - 인코딩 없이는 사용할 수 없음
    @POST("/Api/Newapp/api_login")       // 포스트 형식, 기본 baseurl 다음에 요청 url 작성
    fun getUserList(
        // 제이슨 형태로 묶어서 파라미터로 넘겨줌
        @FieldMap params: HashMap<String, String>
    ) : Call<UserListContents> // 받은 데이터는  UserListContents 담아
}
```

> 4. Retrofit Client 생성
```Kotlin
object RetrofitClass{
    fun getService(): ServerApi = retrofit.create(ServerApi::class.java)
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://www.elsd.co.kr/")
        .client(provideOkHttpClient(AppInterceptor()))
        .addConverterFactory(GsonConverterFactory.create())
        .build()
}
```

> 5. Request 전송

```Kotlin 
RetrofitClass.getService().getUserList( loginParam ).enqueue(object :
  Callback<UserListContents> {
  override fun onFailure(call: Call<UserListContents>, t: Throwable) {
    Log.i("onFailure","연결 실패 : "+t.cause)
  }

  override fun onResponse(call: Call<UserListContents>, response: Response<UserListContents>) {
    if (response.isSuccessful) {
      Log.i("test", response.body().toString())
    }
  }
})
```


Retrofit은 enqueue()를 이용해 비동기로 통신을 할 수 있고, execute()로 동기적으로도 통신을 할 수 있다.

비동기로 통신을 할 경우에는 위와 같이 enqueue()의 인자로 응답이 왔을 때 동작하는 콜백 함수를 지정해 주면 되고, 동기로 통신을 할 경우에는 execute()의 반환 값으로 응답 데이터가 반환된다. 위 예제의 경우는 Response<GeoResponse>가 반환된다.

 이렇게 Retrofit으로 통신하는 기본적인 방법은 끝이 났다. 여기까지만 알아도 Retrofit을 사용하는 데 문제 없지만, 이 포스팅에서는 모든 API 요청에 인증 토큰을 함께 보내야 하는 경우 이용하는 OkHttp Interceptor에 대해 추가로 다룰 것이다.
 
앞의 예제처럼 요청이 적을 때는 @Header 어노테이션을 이용하여 직접 추가할 수 있지만, Http 요청이 많아지고 요청마다 토큰 인증이 필요하다면 요청을 추가 할때마다 지속적으로 매개변수를 추가해주어야 하는 문제가 발생한다. 그렇기 때문에 모든 Http요청에 헤더를 추가하기 위해서는 OkHttp Interceptor를 이용해야 한다.  
  
```Kotlin
// 중간에 데이터 캐치 -> 암호화 되어 있는 데이터를 복호화 하기 위해서 처리 하는 과정
class AppInterceptor : Interceptor {
   @Throws(IOException::class)
      override fun intercept(chain: Interceptor.Chain)
            : Response = with(chain) {
        val request = chain.request()

        val response = chain.proceed(request)
        val rawJson1 = response.body()!!.string()
        //LogLineBreak("Server Data", rawJson1)
        val rawJson = security.decrypt(rawJson1.toString()).toString()
        Log.i("Server Data", rawJson)
        // Re-create the response before returning it because body can be read only once
        return response.newBuilder()
            .body(ResponseBody.create(response.body()!!.contentType(), rawJson)).build()
    }
}
```

  
 





