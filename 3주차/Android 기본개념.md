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
 
