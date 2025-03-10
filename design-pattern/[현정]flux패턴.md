# flux 패턴
데이터 흐름을 단방향으로 관리하는 디자인패턴. 기존에 MVC 패턴을 사용했을 때, 어플리케이션이 커질수록 View와 Model의 관계가 복잡해져 데이터 흐름을 읽기가 어렵고 디버깅하는 것에도 문제가 생겨 facebook에서 만들어진 패턴이다. flux 패턴은 action, dispatcher, store, view라는 계층으로 구성된다.

## flux 패턴의 구조

- Action
    - 사용자 이벤트 담당
    - 이벤트에 관한 객체를 만들어 dispatcher에게 전달
- Dispatcher
    - Action 객체 정보를 기반으로 어떤 행위를 할 것인지 결정
    - Action 객체의 type을 기반으로 미리 만들어 놓은 로직을 수행하고 이를 store에 전달한다.
- Store
    - 애플리케이션의 상태를 관리하고 저장
    - 도메인의 상태, 사용자의 인터페이스 등 상태를 모두 저장한다.
- view
    - 데이터를 기반으로 표출되는 사용자 인터페이스

### Flux의 단방향 데이터 흐름

1. 사용자가 View를 통해 이벤트를 트리거
2. Action이 생성되어 Dispatcher로 전달
3. Dispatcher는 이 Action을 Store로 전달
4. Store는 Action을 처리하여 상태를 업데이트하고, 변경된 상태를 View에 알림
5. View는 새로운 상태를 기반으로 UI를 업데이트

### 장점
- 데이터 일관성의 증대
    - 데이터는 항상 단방향으로 흐르기 때문에, 상태 변경의 원인을 쉽게 추적할 수 있다.
    - 디버깅이 쉬워지고, 애플리케이션의 동작이 예측 가능해진다.
- 상태 관리의 중앙 집중화
    - 모든 상태 변경은 Store를 통해 관리되므로, 애플리케이션 상태를 한 곳에서 쉽게 파악할 수 있다.
    - 전역 상태 관리에 유리하며, 대규모 애플리케이션에서도 일관성을 유지할 수 있다.
- 유연한 구조
    - Flux는 특정 라이브러리에 종속적이지 않으며, 다양한 프로젝트에 쉽게 적용 가능하다.
    - Dispatcher, Store 등을 사용자가 원하는 방식으로 커스터마이징할 수 있다.
- 테스트 용이성
    - Store와 Action을 별도로 테스트할 수 있어, 애플리케이션의 각 부분을 독립적으로 검증할 수 있다.

### 단점

- 보일러플레이트 코드 증가
    - Flux 패턴을 적용하면 Action, Dispatcher, Store 등 여러 구성 요소를 추가로 작성해야 하므로 코드가 장황해질 수 있다.
    - 단순한 애플리케이션에는 오히려 과도한 설계가 될 수 있습니다.
- 학습 곡선
    - Flux의 구조와 각 구성 요소 간의 상호작용을 이해하는 데 시간이 걸릴 수 있다.
    - 초심자에게는 MVC와 같은 전통적인 패턴보다 복잡하게 느껴질 수 있다.
- 상태 간 의존성이 강한 경우 디버깅이 까다로워질 수 있다. 
- Action과 Store 간의 반복적인 코드 작성이 필요하며, 작은 데이터 변경에도 여러 파일을 수정해야 할 수 있다.

### 대표적인 사례
- React 기반 애플리케이션
    - Flux는 React와 함께 대규모 애플리케이션에서 상태 관리를 단순화하고 데이터 흐름을 예측 가능하게 만드는 데 사용된다.
- Redux
    - Flux의 아이디어를 기반으로 발전된 상태 관리 라이브러리.
    - Flux의 Dispatcher를 제거하고 Store를 단일화하여 Flux의 단점을 보완함.
