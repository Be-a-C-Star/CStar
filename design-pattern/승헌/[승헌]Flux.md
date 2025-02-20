# Flux pattern

### Flux는 **Facebook이** 개발한 애플리케이션 아키텍쳐 패턴

### 데이터가 **한방향**으로만 흐르도록 구성

## 1. 구성 요소

1. Action
    1. 상태를 변경하는 “**이벤트**”, “**명령**”
    2. 데이터를 포함할 수 있음(payload)
    3. 이벤트에 관한 객체를 만들어 Dispatcher에 전달
    4. Action Creator를 구성해서 Action객체 생성 전에 데이터를 준비, 비동기 로직 실행도 가능
2. Dispatcher
    1. 받은 Action을 Store에 전달
    2. MiddleWare로 Action을 가로채 데이터 가공 후 Dispatcher로 전달할 수도 있음
3. Store
    1. 상태(state)저장, Action을 기반으로 상태 변경
    2. 변경된 상태를 View에 전달
4. View
    1. Store에서 상태를 구독
    2. 상태 변경 시 렌더링

## 2. 데이터 흐름 예시

1. 사용자가 **View**에서 이벤트를 트리거함 (예: 버튼 클릭)
2. **Action** 생성 후 **Dispatcher**로 전달
3. **Dispatcher**가 **Store**로 Action을 전달
4. **Store**는 상태를 업데이트하고 **View**에 알림
5. **View**가 새로운 상태를 기반으로 다시 렌더링

## 3. 장점

- 데이터 일관성 증대
- 디버깅
- 단위 테스트

### **단방향 데이터 흐름**과 **상태 구독**이 핵심!

## 4. Redux

- Flux 아이디어를 기반으로, 단점을 개선하고 사용성을 높임
- 예측 가능성과 디버깅 용이성을 강조
- Action Dispatching → 상태변경 → View에 반영
    - Dispatcher가 제거됨
    - Reducer 순수 함수가 Action을 직접 처리함
        - Action을 보내면 Reducer가 새로운 상태를 리턴
- 전역 상태를 하나의 Store에서 관리
