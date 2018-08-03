## 산봇 개발 가이드
===
### 개발 환경 기본정보
 * 개발환경 정보
    - Android 6.0
    - API Level 14이상 설치
 * 개발시 고려사항
    - Google이 제공하는 서비스를 지원하지 않습니다. Google이 제공하는 관련 APIs를 사용하는 개발을 피해주시기 바랍니다.
    - Boot Broadcast를 사용하기 위해 최소한 앱을 1회 실행하여야 합니다. 산봇과 관련된 기능을 사용하기 위해 아래의 연동가이드를 참고해 주세요.
---
### 가이드 개발환경
 * Android Studio 3.0
 * Androidd 6.0 (SDK API Level 14) 이상 설치
---
### 프로젝트 구조 설정
 * 산봇 제어 라이브러리 등록 방법
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
 * 기본 권한 설정
    - 산봇 기능을 사용하기 위해 Manifest.xml 권한을 설정 한다.

 ~~~
  <uses-permission android:name='android.permission.CAMERA' />
  <uses-permission android:name='android.permission.INTERNET'/>
  <uses-permission android:name='android.permission.ACCESS_WIFI_STATE'/>
  <uses-permission android:name='android.permission.ACCESS_NETWORK_STATE'/>
~~~
* Proguard 설정
  - Proguard를 사용한다면 아래와 같이 Proguard 설정을 한다.
~~~
-keep class com.sanbotcloud.opensdk.** {*;}
~~~
---
### 앱 개발 기본 설정
* 컴포넌트 설정
  - Activity : BindBaseActivity를 상속받아 Activity를 구성한다.
  ~~~
  public class SampleActivity extends BindBaseActivity {
  ……
      @Override
      protected void onMainServiceConnected() {
          //control code
      }
  ……
      @Override
      public void onCreate() {
          register(SampleActivity.class);
          super.onCreate();
      }
  ……   
  }

  ~~~
  - Service : BindBaseService를 상속받아 Service를 구성한다.
  ~~~
  public class SampleService extends BindBaseService {
  ……
      @Override
      protected void onMainServiceConnected() {
          //control code
      }
  ……
      @Override
      public void onCreate() {
          register(SampleService.class);
          super.onCreate();
      }
  ……   
  }
  ~~~

  - onMainServiceConnected의 콜백을 받으면 Snabot의 api를 사용할 준비가 되었음을 의미한다.

* Sanbot 컨트롤 매니저 연결
  ~~~
  // Access SpeechManager
  SpeechManager speechManager = (SpeechManager)getUnitManager(FuncConstant. SPEECH_MANAGER);
  
  // Access HardWareManager
  HardWareManager hardWareManager = (HardWareManager)getUnitManager(FuncConstant.HARDWARE_MANAGER);

  // Access HeadMotionManager
  HeadMotionManager headMotionManager= (HeadMotionManager)getUnitManager(FuncConstant.HEADMOTION_MANAGER);

  // Access HandMotionManager
  HandMotionManager handMotionManager= (HandMotionManager)getUnitManager(FuncConstant.HANDMOTION_MANAGER);

  // Access WheelMotionManager
  WheelMotionManager wheelMotionManager= (WheelMotionManager)getUnitManager(FuncConstant.WHEELMOTION_MANAGER);

  // Access SystemManager
  SystemManager systemManager= (SystemManager)getUnitManager(FuncConstant. SYSTEM_MANAGER);

  // Access MediaManager
  MediaManager mediaManager= (MediaManager)getUnitManager(FuncConstant.MEDIA_MANAGER);

  // Access PojectorManager
  PojectorManager projectorManager = (ProjectorManager)getUnitManager(FuncConstant.PROJECTOR_MANAGER);

  // Access ModularMotionManager
  ModularMotionManager modularMotionManager= (ModularMotionManager)getUnitManager(FuncConstant. MODULARMOTION_MANAGER);

  // Access ZigbeeManager
  ZigbeeManager zigbeeManager = (ZigbeeManager)getUnitManager(FuncConstant.ZIGBEE_MANAGER);
  ~~~
- 산봇의 각 Manager에 접근하여 세부 기능을 제어한다.
---
### 기능별 세부 예제
* 음성인식
* 하드웨어 컨트롤
* 얼굴인
---
