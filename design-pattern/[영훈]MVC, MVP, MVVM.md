### MVC, MVP, MVVM

#### MVC 패턴

###### 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴

![MVC 패턴](<스크린샷 2024-11-25 오후 4.24.50.png>)

##### 모델(Model)

- 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등
  - ex. 사용자가 네모박스에 글자를 적는다고 할 때 모델은 네모박스의 크기정보, 글자내용, 글자의 위치, 글자의 포맷 정보 등이 됨

##### 뷰(View)

- inputbox, checkbox, textarea 등 사용자 인터페이스 요소를 나타내며 모델을 기반으로 사용자가 볼 수 있는 화면
- 모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 변경이 일어나면 컨트롤러에 이를 전달해야 함

##### 컨트롤러(Controller)

- 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당함
- 모델과 뷰의 생명주기도 관리하며, 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려줌

#### MVC패턴의 장점

##### 장점

- 재사용성과 확장성이 용이함
- 애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있음

##### 단점

- 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해짐

#### MVP 패턴

###### Controller가 P(프레젠터, presenter) 로 교체된 패턴. View와 P는 1:1 관계이므로 MVC보다 더 강한 결합을 지닌 디자인 패턴

##### Model

- 데이터와 비즈니스 로직을 담당

##### View

- UI를 담당. 수동적으로 작동하며, 이벤트를 Presenter로 전달함

##### Presenter

- View와 Model 간의 모든 로직을 처리함

#### MVVM 패턴

###### MVC의 C가 VM(뷰모델, view model)로 바뀐 패턴. VM은 뷰를 추상화한 계층이며 VM : V = 1 : N 이라는 관계를 가짐

![MVVM 패턴](<스크린샷 2024-11-25 오후 4.27.18.png>)

##### View

- UI를 담당. 양방향 데이터 바인딩으로 ViewModel과 연결됨

##### View Model

- Model과 View 사이의 데이터 변환 및 상태 관리를 담당
- 커맨드와 데이터바인딩을 가짐
  - 커맨드 : 여러 요소에 대한 처리를 하나의 액션으로 처리할 수 있는 기법
  - 데이터바인딩 : 화면에 보이는 데이터와 브라우저 상의 메모리 데이터를 일치시키는 방법
  - ex. 뷰(Vue.js)

##### Model

- 데이터와 비즈니스 로직을 담당

```javascript
// Model: 데이터와 비즈니스 로직을 담당
class TodoModel {
  constructor(id, text, completed = false) {
    this.id = id;
    this.text = text;
    this.completed = completed;
  }

  toggle() {
    this.completed = !this.completed;
  }
}

// ViewModel: Model과 View를 연결하고 데이터를 가공하는 역할
import { useState, useCallback } from "react";

const useTodoViewModel = () => {
  const [todos, setTodos] = useState([]);

  const addTodo = useCallback((text) => {
    const newTodo = new TodoModel(Date.now(), text);
    setTodos((prev) => [...prev, newTodo]);
  }, []);

  const toggleTodo = useCallback((id) => {
    setTodos((prev) =>
      prev.map((todo) => {
        if (todo.id === id) {
          const newTodo = new TodoModel(todo.id, todo.text, !todo.completed);
          return newTodo;
        }
        return todo;
      })
    );
  }, []);

  const remainingTodos = todos.filter((todo) => !todo.completed).length;
  const completedTodos = todos.filter((todo) => todo.completed).length;

  return {
    todos,
    addTodo,
    toggleTodo,
    remainingTodos,
    completedTodos,
  };
};

// View: UI를 담당하는 컴포넌트
const TodoApp = () => {
  const { todos, addTodo, toggleTodo, remainingTodos, completedTodos } =
    useTodoViewModel();
  const [newTodoText, setNewTodoText] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    if (newTodoText.trim()) {
      addTodo(newTodoText);
      setNewTodoText("");
    }
  };

  return (
    <div>
      <h1>Todo List</h1>
      <div>
        <p>남은 할 일: {remainingTodos}개</p>
        <p>완료된 할 일: {completedTodos}개</p>
      </div>

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={newTodoText}
          onChange={(e) => setNewTodoText(e.target.value)}
          placeholder="새로운 할 일을 입력하세요"
        />
        <button type="submit">추가</button>
      </form>

      <ul>
        {todos.map((todo) => (
          <li
            key={todo.id}
            onClick={() => toggleTodo(todo.id)}
            style={{
              textDecoration: todo.completed ? "line-through" : "none",
              cursor: "pointer",
            }}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TodoApp;
```

##### 차이점

![차이점](<스크린샷 2024-11-25 오후 4.28.37.png>)

##### View의 역할

###### MVC

- View는 사용자 입력 처리와 화면 표시

###### MVP

- View는 단순히 UI 업데이트만 수행

###### MVVM

- View는 ViewModel과 데이터 바인딩만 수행

##### 데이터의 흐름

###### MVC, MVP

- 데이터가 View → (Controller 또는 Presenter) → Model로 흘러가고, 다시 Model → View로 업데이트됨
- View와 Model은 직접 연결되지 않거나, 최소한의 상호작용을 가짐

###### MVVM

- View ↔ ViewModel ↔ Model 간 데이터가 자동으로 동기화됨
- 사용자가 View에서 데이터를 변경하면 ViewModel에 즉시 반영되고, Model의 상태 변화도 View에 자동 업데이트됨

##### 로직 처리 담당

###### MVC

- Controller에 분산
  - 사용자의 입력을 받아 Model을 갱신하고, View를 업데이트함

###### MVP

- Presenter가 View와 Model 간의 모든 로직을 담당

###### MVVM

- Model과 View 사이의 데이터 변환 및 상태 관리 로직을 담당
