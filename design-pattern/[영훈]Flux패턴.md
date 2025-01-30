### flux 패턴

###### 단방향으로 데이터 흐름을 관리하는 디자인패턴

##### 기존 MVC 패턴의 문제점

![MVC 패턴의 문제점](<스크린샷 2024-12-01 오전 11.44.22.png>)

- 데이터를 일관성 있게 뷰에 공유하기가 어려워짐
- 버그 수정, 데이터 흐름 파악에 어려움
- flux 패턴은 페이스북에서 만듦
  - 기존에 MVC 패턴을 사용중, 모델과 뷰의 관계가 너무 복잡해짐
  - 특정 페이지에서는 메세지 읽음 표시가 정상적으로 동작하고 특정 페이지에서는 정상적으로 동작하지 않은 메세지 읽음 표시(mark seen)에 대한 기능장애를 겪음
  - 해결방법으로 데이터가 단방향으로 흐르게하는 flux 패턴이 등장함

#### flux패턴의 구조

##### Action

###### 사용자의 이벤트를 담당. 마우스 클릭이나, 글을 쓴다던가 등을 의미하며 해당 이벤트에 관한 객체를 만들어내 dispatcher에게 전달함

##### Dispatcher

###### 들어오는 Action 객체 정보를 기반 어떠한 “행위”를 할 것인가를 결정함. 보통 action객체의 type를 기반으로 미리 만들어 놓은 로직을 수행하고 이를 Store에 전달함

- 보통 switch문을 사용하여 action에 따라 결정된 내용을 store에 전달함

##### Store

###### 애플리케이션 상태를 관리하고 저장하는 계층. 도메인의 상태, 사용자의 인터페이스 등의 상태를 모두 저장함

##### View

###### 데이터를 기반으로 표출이 되는 사용자 인터페이스(UI)

#### flux 패턴의 장점

##### 장점

- 데이터가 한 방향으로만 흐르기 때문에 애플리케이션 상태를 예측 가능하게 유지할 수 있음
- 상태 변경 과정이 Action과 Dispatcher를 통해 명시적으로 처리되므로, 상태가 어떻게 변했는지 추적하기 쉽고, 상태 변경 로직이 Store에 집중되어 있어 디버깅과 유지보수가 간편화됨
- Action, Store, View가 독립적으로 설계되어 재사용성과 테스트 용이성이 좋음
- 유일하게 Store에서 상태를 갱신하고 데이터를 처리하여, 상태 관리의 일관성을 유지함

##### 단점

- 러닝 커브 높음
- Action, Dispatcher, Store 등 다양한 구성 요소를 만들어야 하므로 간단한 앱에서도 코드 양이 많아질 수 있음
- Store가 커질수록 상태 변경 감지와 데이터 업데이트에 비용이 증가할 수 있음. 특히 대규모 애플리케이션에서 최적화가 필요함

#### flux패턴이 적용된 Redux

```javascript
// redux 내부 코드

const initialState = {
  visibilityFilter: "SHOW_ALL",
  todos: [],
};

function appReducer(state = initialState, action) {
  switch (action.type) {
    case "SET_VISIBILITY_FILTER": {
      return Object.assign({}, state, {
        visibilityFilter: action.filter,
      });
    }
    case "ADD_TODO": {
      return Object.assign({}, state, {
        todos: state.todos.concat({
          id: action.id,
          text: action.text,
          completed: false,
        }),
      });
    }
    case "TOGGLE_TODO": {
      return Object.assign({}, state, {
        todos: state.todos.map((todo) => {
          if (todo.id !== action.id) {
            return todo;
          }
          return Object.assign({}, todo, {
            completed: !todo.completed,
          });
        }),
      });
    }
    case "EDIT_TODO": {
      return Object.assign({}, state, {
        todos: state.todos.map((todo) => {
          if (todo.id !== action.id) {
            return todo;
          }

          return Object.assign({}, todo, {
            text: action.text,
          });
        }),
      });
    }
    default:
      return state;
  }
}
```

- 대표적으로 JS 상태 관리 라이브러리 Redux에 flux 패턴이 적용돼있음
- redux 내부에서는 위 코드와 같이 switch문으로 action.type에 따라서 특정 동작을 수행하는 로직이 쓰이고 있음

##### Redux 동작 방식 예시

![redux](<스크린샷 2024-12-02 오전 10.50.57.png>)

```javascript
import React from "react";
import { createRoot } from "react-dom/client";
import App from "./App";
import { Provider } from "react-redux";
import { legacy_createStore as createStore } from "redux";

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

const count = 1;

// Reducer
// Reducer를 생성할 때에는 초기 상태를 인자로 요구함
const counterReducer = (state = count, action) => {
  // Action 객체의 type 값에 따라 분기하는 switch 조건문
  switch (action.type) {
    //action === 'INCREASE'일 경우
    case "INCREASE":
      return state + 1;

    // action === 'DECREASE'일 경우
    case "DECREASE":
      return state - 1;

    // action === 'SET_NUMBER'일 경우
    case "SET_NUMBER":
      return action.payload;

    // 해당 되는 경우가 없을 땐 기존 상태를 그대로 리턴
    default:
      return state;
  }
  // Reducer가 리턴하는 값이 새로운 상태가 됨
};

// Action
export const increase = () => {
  return {
    type: "INCREASE",
  };
};

export const decrease를 = () => {
  return {
    type: "DECREASE",
  };
};

// Store
const store = createStore(counterReducer);

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);

// Dispatch

// Action 객체를 직접 작성하는 경우
dispatch({ type: "INCREASE" });
dispatch({ type: "SET_NUMBER", payload: 5 });

// 액션 생성자(Action Creator)를 사용하는 경우
dispatch(increase());
dispatch(setNumber(5));
```

1. 상태가 변경되어야 하는 이벤트가 발생하면, 변경될 상태에 대한 정보가 담긴 Action 객체 생성
2. Action 객체는 Dispatch 함수의 단일인자로 전달
3. Dispatch 함수는 Action 객체를 Reducer 함수로 전달
4. Reducer 함수는 Action 객체의 값을 확인해서 전역 상태 저장소 Store의 상태를 변경
5. 상태가 변하면 React는 리렌더링을 함

### MVC 패턴과 Flux 패턴의 차이점

#### 구조적 차이

###### MVC 패턴

- Model, View, Controller로 구성

###### Flux 패턴

- Dispatcher, Store, View, Action으로 구성

#### 데이터 흐름

###### MVC 패턴

- 양방향 데이터 흐름을 지원. View와 Model이 상호 작용하며 데이터 변경 시 서로에게 영향을 미침

###### Flux 패턴

- 단방향 데이터 흐름을 강제함. Action → Dispatcher → Store → View 순서로 데이터가 흐르며, View는 Store의 상태를 읽기만 함

#### 상태 관리

###### MVC 패턴

- Controller가 Model과 View 사이의 로직 관리

###### Flux 패턴

- Dispatcher를 통해 중앙 집중식으로 상태 변경 관리

#### 복잡성 관리

###### MVC 패턴

- 데이터 흐름이 복잡한 애플리케이션에서는 Model과 View 간 의존성이 증가하여 유지보수가 어려워질 수 있음

###### Flux 패턴

- 데이터 흐름을 단방향으로 제한하여 상태 관리가 명확하며, 대규모 애플리케이션에서도 상태 변경 추적이 쉬움
