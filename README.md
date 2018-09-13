## 산봇 개발 가이드
----
### 개발 환경 기본정보
 * 개발환경 정보
    - Android 6.0
    - API Level 14이상 설치
 * 개발시 고려사항
    - Google이 제공하는 서비스를 지원하지 않습니다. Google이 제공하는 관련 APIs를 사용하는 개발을 피해주시기 바랍니다.
    - Boot Broadcast를 사용하기 위해 최소한 앱을 1회 실행하여야 합니다. 산봇과 관련된 기능을 사용하기 위해 아래의 연동가이드를 참고해 주세요.

### 가이드 개발환경
 * Android Studio 3.0a## 산봇 개발 가이드
----
### 개발 환경 기본정보
 * 개발환경 정보
    - Android 6.0
    - API Level 14이상 설치
 * 개발시 고려사항
    - Google이 제공하는 서비스를 지원하지 않습니다. Google이 제공하는 관련 APIs를 사용하는 개발을 피해주시기 바랍니다.
    - Boot Broadcast를 사용하기 위해 최소한 앱을 1회 실행하여야 합니다. 산봇과 관련된 기능을 사용하기 위해 아래의 연동가이드를 참고해 주세요.

### 가이드 개발환경
 * Android Studio 3.0
 * Androidd 6.0 (SDK API Level 14) 이상 설치

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

### 앱 개발 기본 설정
* 컴포넌트 설정
  - Activity : BindBaseActivity를 상속받아 Activity를 구성한다.
  ~~~
  public class SampleActivity extends BindBaseActivity {
   
      @Override
      protected void onMainServiceConnected() {
          //control code
      }
  ・・・
      @Override
      public void onCreate() {
          register(SampleActivity.class);
          super.onCreate();
      }
  ・・・   
  }

  ~~~
  - Service : BindBaseService를 상속받아 Service를 구성한다.
  ~~~
  public class SampleService extends BindBaseService {
  ・・・
      @Override
      protected void onMainServiceConnected() {
          //control code
      }
 
      @Override
      public void onCreate() {
          register(SampleService.class);
          super.onCreate();
      }
  ・・・   
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
* 음성 출력 [**SpeechManager**]
  ~~~
  public OperationResult startSpeak(String text)
  ~~~
* 음성 입력  [**SpeechManager**]
  - 음성 입력 대기 상태 요청
  ~~~
  speechManager.doWakeUp();
  ~~~
  - 음성 입력 중지 상태 요청
  ~~~
  speechManager.doSleep();
  ~~~
  - 음성 입력
  ~~~
  speechManager.setOnSpeechListener(new RecognizeListener() {
    @Override
    public boolean onRecognizeResult(Grammar grammar) {
      String speechToText = grammer.getText() // 음성입력 처리 콜백
    }
    @Override
    public void onRecognizeVolume(int i) {  
      // 음성입력 볼륨 콜백
    }
  }
  ~~~ 
* 하드웨어 컨트롤 [**HardWareManager**]
   - LED 켜기/끄기
   ~~~
   hardWareManager.switchWhiteLight(true); // turn on
   hardWareManager.switchWhiteLight(false); // turn off
   ~~~
   - 터치 센서 이벤트
   ~~~
   hardWareManager.setOnHareWareListener(new TouchSensorListener() {
    @Override
    public void onTouch(int part) {
        if (part == 11 || part == 12 || part == 13) {
            touchTv.setText(‘Head had been touched’);
        }
      }
    });
   ~~~
   - PIR센서 이벤트
   ~~~
   hardWareManager.setOnHareWareListener(new PIRListener() {
    @Override
    public void onPIRCheckResult(boolean isCheck, int part) {
        System.out.print((part == 1 ? ‘Front of the body’ : ‘Back of the body’) + ‘PIR has been triggered’);
      }
    });
   ~~~
* Movement 컨트롤 [**HeadMotionManager**, **HandMotionManager**, **WheelMotionManager**]
   - 팔 움직임(**HandMotionManager**)
   ~~~
   /**
     *  각도 조작이 없는 팔 움직임
     *  NoAngleHandMotion
     *  Action :
     *  		RelativeAngleHandMotion.ACTION_UP //hands up 
     *  		RelativeAngleHandMotion.ACTION_DOWN //hands down
     *  Speed : 1~8
     *  Part :
     *  		RelativeAngleHandMotion.PART_LEFT  //control left hand
     *  		RelativeAngleHandMotion.PART_RIGHT  //control right hand
     *         RelativeAngleHandMotion.PART_BOTH  //control both
     */ 
     
	   NoAngleHandMotion motion =new NoAngleHandMotion(NoAngleHandMotion._PART_BOTH_, 5 ,NoAngleHandMotion._ACTION_UP);
	   handMotionManager.doNoAngleHandMotion(motion);
   
   /**
     *  각도 설정에 의한 팔 움직임
     *  NoAngleHandMotion
     *  Action :
     *  		RelativeAngleHandMotion.ACTION_UP //hands up 
     *  		RelativeAngleHandMotion.ACTION_DOWN //hands down
     *  Speed : 1 ~ 8
     *  Part :
     *  		RelativeAngleHandMotion.PART_LEFT  //control left hand
     *  		RelativeAngleHandMotion.PART_RIGHT  //control right hand
     *         RelativeAngleHandMotion.PART_BOTH  //control both
     *   Angle : 0 ~ 270
     */ 
     
	   RelativeAngleHandMotion motion =new RelativeAngleHandMotion(NoAngleHandMotion.PART_LEFT, 5 ,NoAngleHandMotion.ACTION_UP, 30);
	   handMotionManager.doRelativeAngleMotion(motion);
   
   ~~~
   - 머리 움직임(**HeadMotionManager**)
   ~~~
   public OperationResult doNoAngleMotion(NoAngleHandMotion noAngleHandMotion)
   ~~~

* 얼굴인식22
* 카메라2
---

 * Androidd 6.0 (SDK API Level 14) 이상 설치

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

### 앱 개발 기본 설정
* 컴포넌트 설정
  - Activity : BindBaseActivity를 상속받아 Activity를 구성한다.
  ~~~
  public class SampleActivity extends BindBaseActivity {
   
      @Override
      protected void onMainServiceConnected() {
          //control code
      }
  ・・・
      @Override
      public void onCreate() {
          register(SampleActivity.class);
          super.onCreate();
      }
  ・・・   
  }

  ~~~
  - Service : BindBaseService를 상속받아 Service를 구성한다.
  ~~~
  public class SampleService extends BindBaseService {
  ・・・
      @Override
      protected void onMainServiceConnected() {
          //control code
      }
 
      @Override
      public void onCreate() {
          register(SampleService.class);
          super.onCreate();
      }
  ・・・   
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
* 음성 출력 [**SpeechManager**]
  ~~~
  public OperationResult startSpeak(String text)
  ~~~
* 음성 입력  [**SpeechManager**]
  - 음성 입력 대기 상태 요청
  ~~~
  speechManager.doWakeUp();
  ~~~
  - 음성 입력 중지 상태 요청
  ~~~
  speechManager.doSleep();
  ~~~
  - 음성 입력
  ~~~
  speechManager.setOnSpeechListener(new RecognizeListener() {
    @Override
    public boolean onRecognizeResult(Grammar grammar) {
      String speechToText = grammer.getText() // 음성입력 처리 콜백
    }
    @Override
    public void onRecognizeVolume(int i) {  
      // 음성입력 볼륨 콜백
    }
  }
  ~~~ 
* 하드웨어 컨트롤 [**HardWareManager**]
   - LED 켜기/끄기
   ~~~
   hardWareManager.switchWhiteLight(true); // turn on
   hardWareManager.switchWhiteLight(false); // turn off
   ~~~
   - 터치 센서 이벤트
   ~~~
   hardWareManager.setOnHareWareListener(new TouchSensorListener() {
    @Override
    public void onTouch(int part) {
        if (part == 11 || part == 12 || part == 13) {
            touchTv.setText(‘Head had been touched’);
        }
      }
    });
   ~~~
   - PIR센서 이벤트
   ~~~
   hardWareManager.setOnHareWareListener(new PIRListener() {
    @Override
    public void onPIRCheckResult(boolean isCheck, int part) {
        System.out.print((part == 1 ? ‘Front of the body’ : ‘Back of the body’) + ‘PIR has been triggered’);
      }
    });
   ~~~
* Movement 컨트롤 [**HeadMotionManager**, **HandMotionManager**, **WheelMotionManager**]
   - 머리 움직임(**HeadMotionManager**)
   ~~~
   /**
     * 각도 조작이 없는 머리 움직임
     *  new NoAngleHandMotion(Action, Speed,  Part)
     *  Action :
     *  		RelativeAngleHandMotion.ACTION_UP //hands up 
     *  		RelativeAngleHandMotion.ACTION_DOWN //hands down
     *  Speed : 1~8
     *  Part :
     *  		RelativeAngleHandMotion.PART_LEFT  //control left hand
     *  		RelativeAngleHandMotion.PART_RIGHT  //control right hand
     *         RelativeAngleHandMotion.PART_BOTH  //control both**
     *   
     */ 
     
	   NoAngleHandMotion motion =new NoAngleHandMotion(NoAngleHandMotion._PART_BOTH_, 5 ,NoAngleHandMotion._ACTION_UP_);
	   handMotionManager.doNoAngleHandMotion(motion);
   
   // 각도설정에 따른 머리 움직임
   
   ~~~
   - 팔 움직임(**HandMotionManager**)
   ~~~
   public OperationResult doNoAngleMotion(NoAngleHandMotion noAngleHandMotion)
   ~~~

* 얼굴인식22
* 카메라2
---
