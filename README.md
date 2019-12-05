# Android 이론

### Activity

- Activity?
  - 사용자에게 UI가 있는 화면을 제공하는 앱 컴포넌트
  - 어플리케이션은 무조건 하나이상의 액티비티가 존재해야함

* Activity LifeCycle

  * ![ActivityLifeCycle](/Users/txxbro/workspace/AndroidTheory/image/ActivityLifeCycle.png)
  * onCreate(): 액티비티가 생성될 때 호출되며 사용자 인터페이스 초기화에 사용됨
  * onStart(): 액티비티가 사용자에게 보여지기 바로 직전에 호출됨
  * onResume(): 액티비티가 사용자와 상호작용하기 바로 전에 호출됨
  * onPause(): 다른 액티비티가 보여질 때 호출됨. 데이터 저장, 스레드 중지 등의 처리하기에 적당한 메소드
  * onStop(): 액티비티가 더이상 사용자에게 보여지지 않을 때 호출됨. 메모리가 부족할 경우에는 onStop() 메소드가 호출되지 않을 수도 있음
  * onDestroy() 액티비티가 소멸될 때 호출됨. finish() 메소드가 호출되거나 시스템이 메모리 확보를 위해 액티비티를 제거할 때 호출됨
  * onRestart(): 액티비티가 멈췄다가 다시 시작되기 바로 전에 호출됨. onStop() 에서 호출되며 onStart()를 호출함

  > 추가 tip
  >
  > - 사용자가 홈 키를 눌렀는지를 파악하는 방법
  >   - Activity 클래스에서 제공하는 onUserLeavHint()는 사용자가 홈 키를 눌렀을 때 호출되는 메소드이다.
  >   - 해당 함수를 오버라이딩하면 사용자가 홈 키를 눌렀는지 파악이 가능하다.

* Intent?

  * 액티비티간에 데이터를 전송할 때 사용
  * putExtra(), getExtra()

* Activity가 종료되는 경우?

  * 사용자가 Back 키를 눌러 Activity를 종료한 경우
  * Activity가 백그라운드에 있을때 시스템 메모리가 부족해진 경우(OS가 강제 종료시키는 경우)
  * 언어 설정을 변경한 경우
  * 화면을 가로/세로 회전할 때
  * 폰트 크기나 폰트를 변경했을 때

* onSaveInstanceState()?

  * 해당 메소드를 이용하면 Activity가 종료될 때 데이터를 저장할 수 있다.
  * Activity가 종료되는 경우?
    * 사용자가 Back 키를 눌러 Activity를 종료한 경우
    * Activity가 백그라운드에 있을때 시스템 메모리가 부족해진 경우(OS가 강제 종료시키는 경우)
    * 언어 설정을 변경한 경우
    * 화면을 가로/세로 회전할 때
    * 폰트 크기나 폰트를 변경했을 때
  * 화면 회전시 LifeCycle
    * onPause() -> onSaveInstanceState() -> onStop() -> onDestroy() -> onCreate() -> onStart() -> onResume()

### Fragment

- Fragment?
  - 하나의 액티비티가 여러개의 화면을 가지도록 만들기 위한 개념
  - 액티비티처럼 레이아웃, 동작 처리, 생명주기를 가지는 독립적인 모듈

- Fragment LifeCycle
  - ![FragmentLifeCycle](/Users/txxbro/workspace/AndroidTheory/image/FragmentLifeCycle.png)
  - onAttach(): 프래그먼트가 액티비티에 붙을 때 호출됨 (완벽하게 생성된 상태 아님)
  - onCreate(): 프래그먼트가 액티비티에 호출을 받아 생성되는 시점. 액티비티의 onCreate()에선 View나 UI관련 작업을 할 수 있으나, 프래그먼트의 onCreate()에서는 할 수 없음
  - onCreateView(): 프래그먼트에 속한 각종 View나 ViewGroup에 대한 UI 바인딩 작업이 가능한 시점(Layout을 inflater하여 View 작업을 함)
  - onActivityCreated(): 액티비티에서 프래그먼트를 모두 생성하고 난 다음에 호출됨. 즉 액티비티의 onCreate() 다음에 호출됨. 액티비티와 프래그먼트가 연결되는 시점
  - onStart(): 프래그먼트가 사용자에게 보여지기 전에 호출되는 함수
  - onResume(): 프래그먼트가 비로소 화면에 보여지는 단계. 사용자에게 포커스를 잡은 상태로 사용자와 상호 작용이 가능함
  - onPause(): 프래그먼트와 사용자간의 상호작용이 중지된 상태 (다른 프래그먼트가 add된 상태)
  - onStop(): 프래그먼트는 더이상 보여지지 않게되며 프래그먼트 기능이 중지됨
  - onDestroyView(): 프래그먼트에 view들을 제거. backstack을 사용했다면, 다시 해당 프래그먼트로 돌아올 때 onCreateView()가 호출됨
  - onDestroy(): 프래그먼트를 제거하기 직전 상태
  - onDetach(): 프래그먼트를 제거하고 액티비티와의 연결도 해제됨
- bundle
  - 프래그먼트 간 데이터를 전송할 때 사용
  - setArguments(), getArguments()
- Framgent에 대한 궁금점
  - 굳이 newInstance()를 사용하는 이유 와 굳이 bundle을 사용하는 이유?
    - 재생성 때문이다. 안드로이드에서는 메모리가 부족하게되면 액티비티를 파기하여 메모리를 확보하는데, 액티비티 뿌만 아니라 메모리가 부족하면 프래그먼트도 파기되며 필요시 재생성 되게 된다. 재생성시에 필요한 생성자는 아무것도 매개변수가 없는 생성자인데, `AFragment a = new AFragment(param);` 이와 같이 생성자를 만든다면 비어있는 생성자를 만들어주지 않을 경우 에러가 발생하게 된다.
    - 프래그먼트가 재생성될 때에 호출 되는 메소드는  `Fragment[#instantiate(Context](https://m.blog.naver.com/BlogTagView.nhn?blogId=tpgns8488&pushNavigation=true&tagName=instantiate(Context) context, String fname, Bundle args)` 이다. 매개변수 중에 Bundle이 존재한다. newInstance()를 통해서 셋팅했던 Bundle이 재생성될 때 다시 셋팅되어 넘어오게 된다. **그렇기 때문에 Bundle을 통해 설정한 데이터들은 액티비티가 파기될 때 따로 onSaveInstanceState()를 하지 않아도 복구가 된다.**
- Fragment에서의 onSaveInstanceState()?
  - Fragment 종료 시, onPause() -> onSaveInstanceState() 순서로 호출됨
  - Fragment 전환시 LifeCycle
    - onPause() -> onSaveInstanceState() -> onStop() -> onDestroyView() -> onDestroy() -> onDetach() -> onAttach() -> onCreate() -> onCreateView() -> onActivityCreated() -> onStart() -> onResume()