### PART1: Challenges due to the nature of mobile applications

앱 개발 경험이 없는 사람들은 네이티브 앱 개발이 웹 개발과 비슷할 것이라 생각하지만 그렇지 않다.

웹 혹은 백엔드와 다르게, 앱의 이진 배포(binary distribution of mobile apps), 낮은 전력 소모(low connectivity setup), 앱 푸시, 딥 링크, 인앱 결제 등 기능을 통합해야 한다.

#### 1. State Management

네이티브 개발에 있어 상태관리는 복잡하다. 모던 웹, 백엔드도 비슷하지만, 네이티브 개발에서는 앱의 `app lifeCycle events`, `화면 전환`을 신경써야 한다. 라이프 사이클과 관련한 예시로, 
앱이 일시정지 또는 백그라운드로 이동한 후, 포그라운드로 돌아오거나 중단되는 경우가 있다. (the app pausing and going to the background, the returning to the foreground or being suspened)

`이벤트`는 대부분의 앱에서 상태 변화를 일으키며, '상태 변경', '네트워크 요청', '유저 인풋' 같은 비동기 방식으로 트리거 된다. 대부분의 버그 또는 앱 크래쉬는 `이벤트`와 `앱 상태 붕괴`로 발생하며, 상태가 붕괴되는 문제는
전역(global) 또는 지역(local) 상태가 서로 알려지지 않은 여러 구성 요소에 의해 조작되는 앱의 일반적인 문제이다. 이러한 문제를 겪는 팀은 컴포넌트와 앱의 상태를 분리하고(isolate component and application state),
반응형 상태 관리(reactive state management)를 도입해야 한다.

`반응형 프로그래밍(Reactive Programming)`은 크고, 상태를 관리하기 위한 앱에서, 상태 변경을 격리시키기 위해 선호되는 프로그래밍 방식이다. 상태를 가능한한 변하지 않도록 유지하고, 모델을 상태 변화를 방출하는 불변의 객체로 저장한다.
`컴포넌트 트리 아래(down a tree of components)`방향으로 상태를 변경해 나가는 과정이 지루할 수 있지만, 지루함은 컴포넌트에서 의도하지 않은 상태 변경을 만들기 어렵게 한다.

권한, 블루투스와 같은 전역적인 앱 상태는 도전 과제를 제시한다. 하나의 전역 상태가 바뀔 때마다 네트워크 연결이 끊어지면 앱의 다른 부분은 다르게 반응해야할 수도 있다.
전역 상태와 함께 특정 컴포넌트가 상태 변화를 듣도록 결정되어야 한다. 하나의 스팩트럼의 앱의 화면 또는 컴포넌트는 전역 상택의 변화를 들을 수 있다. 이로 인해 많은 코드 중복이 발생하지만, 컴포넌트는 전역 상태를 핸들링 한다(components handling all of the global state concerns).
결과적으로 컴포넌트는 특정 글로벌 상태 변화를 듣고 이를 앱의 특정 부분으로 전달할 수 있다. 이것은 덜 복잡한 코드의 결과가 되지만, 전역 상태 관리와 그것을 알고 있는 컴포넌트 사이 심한 결합(tight coupling)을 가지고 있다. 

딥링크, 또는 내부 네비게이션(internal shortcut navitaion points) 또한 앱 상태 관리에 복잡성을 더한다. 딥링크를 예로 들면, 딥링크가 활성화 된 후 앱의 상태를 설정해야 할 수도 있다. 
