산봇 개발 가이드
====
산봇 개발 가이드
----
1. 개발 환경 기본정보
 * 개발환경 정보
    - Android 6.0
    - API Level 14이상 설치
 * 개발시 고려사항
    - Google이 제공하는 서비스를 지원하지 않습니다. Google이 제공하는 관련 APIs를 사용하는 개발을 피해주시기 바랍니다.
    - Boot Broadcast를 사용하기 위해 최소한 앱을 1회 실행하여야 합니다. 산봇과 관련된 기능을 사용하기 위해 아래의 연동가이드를 참고해 주세요.
2. 가이드 개발환경
 * Android Studio 3.0
 * Androidd 6.0 (SDK API Level 14) 이상 설치
3. 프로젝트 구조 설정
 * 3.1 산봇 제어 라이브러리 등록 방법
   - aar 파일을 libs폴더에 넣도록 한다.
   - 아래의 코드를 App Module Level의 build.gradle에 추가한다.
 <pre><code>
 repositories {
         flatDir {
             dirs 'libs'
         }
     }
 dependencies {
     compile(name:'SanbotOpenSdk_XXX', ext:'aar')
 }
 </code></pre>
