# 프로젝트 파일 
 Android 스튜디오는 기본적으로 Android 뷰에 프로젝트 파일을 표시합니다. 이 뷰는 디스크에 있는 실제 파일 계층 구조를 반영하지는 않지만, 모듈 및 파일 형식별로 구성되어 있으며 자주 사용되지 않는 파일이나 디렉터리는 숨기므로 프로젝트의 주요 소스 파일을 간단하게 탐색할 수 있습니다. 디스크 상의 구조와 비교하여 몇 가지 구조적으로 변경된 사항은 다음과 같습니다.
 * 프로젝트의 모든 빌드 관련 구성파일을 최상위 Gradle Script 그룹에 표시함
 * 각 모듈의 모든 매니페스트 파일을 모듈 수준의 그룹에 표시함(제품 버전 및 빌드 유형별로 매니페스트 파일이 다른 경우)
 * 모든 대체 리소스 파일을 리소스 한정자별 개별 폴더 대신 단일 그룹에 표시합니다. 예를 들어, 런처 아이콘의 모든 밀도 버전이 나란히 표시됩니다
 
![스크린샷 2021-09-28 오전 11 32 48](https://user-images.githubusercontent.com/66652964/135012978-03b31b67-bb18-462a-982e-a133d9a56881.png)


### manifest
    AndroidManifest.xml 파일을 포함
### java
    JUnit 테스트 코드를 비롯한 자바 소스 코드 파일을 포함합니다. 이 파일은 패키지 이름별로 구분됩니다.
### res
    코드가 아닌 모든 리소스(예: XML 레이아웃, UI 문자열, 비트맵 이미지 등)를 포함합니다. 이 리소스는 리소스에 상응하는 
    하위 디렉터리로 나뉩니다. 가능한 모든 리소스 유형에 관한 자세한 내용은 리소스 제공을 참고하세요.
    
## 프로젝트 구조 설정 
Android 스튜디오 프로젝트에 관한 여러 설정을 변경하려면 File > Project Structure를 클릭하여 Project Structure 대화상자를 엽니다. 여기에는 다음 섹션이 포함되어 있습니다.
 * SDK Location: 프로젝트에서 사용하는 JDK, Android SDK 및 Android NDK의 위치를 설정합니다.
 * Project: Gradle 및 Gradle용 Android 플러그인의 버전과 저장소 위치 이름을 설정합니다.
 * Modules: 타겟 SDK 및 최소 SDK, 앱 서명, 라이브러리 종속 항목을 비롯한 모듈별 빌드 구성을 수정할 수 있습니다.
 
 ### Preferences  
 * SDK Location
 * File Color, VCS(버전컨트롤), plugin
 
## 어플리케이션 기본 항목
 ### Manifest
 Android 시스템이 앱 구성 요소를 시작하려면 시스템은 우선 앱의 매니페스트 파일, AndroidManifest.xml을 읽어서 해당 구성 요소가 존재하는지 확인함
 앱은 이 파일 안에 모든 구성 요소를 선언해야 하며, 이 파일은 앱 프로젝트 디렉토리의 루트에 있어야 함
 * 앱이 요구하는 모든 사용자 권한(예: 인터넷 액세스, 사용자의 연락처에 대한 읽기 액세스)
 * 앱에서 사용하거나 요구하는 하드웨어 및 소프트웨어 기능(예: 카메라, 블루투스 서비스, 멀티터치 화면)
 * 앱이 링크되어야 하는 API 라이브러리(Android 프레임워크 API 제외)
 * 패키지명, 앱이름, 앱 구성요소(activity, service, receiver, provider)
 * exported : 다른 애플리케이션의 구성요소로 액티비티를 시작할 수 있는지 설정
 * https://developer.android.com/guide/topics/manifest/manifest-intro?hl=ko
 
 ### 앱 구성요소
 * 액티비티 <activity>
 * 서비스  <service>
 * 브로드캐스트 리시버 <receiver> 
 * 콘텐츠 제공자 <provider> 
 
 ### 앱 리소스
 소스 코드와 별도로 이미지, 오디오 파일, 그리고 앱의 시각적 표현과 관련된 것 등의 여러 리소스가 필요
 * 액티비티 사용자 인터페이스의 애니메이션, 메뉴, 스타일, 색상, 레이아웃을 XML 파일로 정의
 * anim/
 * drawable/
 * mipmap/
 * layout/
 * menu/
 * values/color/
 * values/string/
 * values/styles/
 * values/dimens/
 
 ## Gradle Script
 고급 빌드 툴킷인 Gradle을 사용하여 빌드 프로세스를 자동화하고 관리하는 한편, 개발자가 유연한 맞춤 빌드 구성을 정의하도록 허용
 참고 : Gradle과 Android 플러그인은 Android 스튜디오와 독립적으로 실행되므로, 빌드 도구를 별도로 업데이트해야 함
 ### 실행/디버그 구성 변경
 앱을 처음으로 실행하는 경우 Android 스튜디오에서 기본 실행 구성이 사용되며 실행 구성은 APK 또는 Android App Bundle에서 앱을 배포할지 여부, 실행할 모듈, 배포할 패키지, 시작할 활동, 대상 기기, 에뮬레이  터 설정, logcat 옵션 등을 지정함. 
 기본 실행/디버그 구성은 APK를 빌드하고, 기본 프로젝트 활동을 시작하며, 대상 기기 선택에 Select Deployment Target 대화상자를 사용함
 ### 프로젝트 빌드
 * Clean Project : 중간/캐싱된 파일을 모두 삭제합니다.
 * Rebuild Project :	선택된 빌드 변형에 관해 Clean Project를 실행하고 APK를 생성합니다.
 * Build Bundle(s) / APK(s) > Build APK(s) : 선택된 변형에 관해 현재 프로젝트 내 모든 모듈의 APK를 빌드함 빌드가 완료되면 확인 알림이 표시되어 APK 파일의 링크 및 APK Analyzer에서 APK 파일을 분석하는 링크가 제공됩니다.
 * Generate Signed Bundle / APK : 새로운 서명 구성을 설정하고 서명된 App Bundle 또는 APK를 빌드하여 출시 키로 앱에 서명해야만 Play Console에 앱을 업로드할 수 있음
 
 ### Gradle 설명
 
