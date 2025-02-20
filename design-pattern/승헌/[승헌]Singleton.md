# 싱글톤!
### 하나의 클래스에서 단 하나의 인스턴스만 생성되도록 보장하는 디자인 패턴

- 앱 내의 공유 자원을 관리
- 설정 값 등 단일 리소스 관리

## 1. 활용

### 앱에서 단일 리소스 관리가 필요할 때

1. **전역 설정 관리**
    
    설정 파일이나 환경 변수에 접근, 필요한 정보 제공
    
2. **로깅 시스템**
    
    여러 컴포넌트에서 하나의 로그 시스템을 사용할 때
    
3. **캐싱**
    
    데이터베이스나 API 호출을 최소화
    

#### 남용하면 의존성을 전역에 퍼뜨려 객체 간의 독립성을 낮출 수 있음


## 2. 싱글톤을 만드는 핵심 원리

1. **단일 인스턴스 보장**
    - 같은 클래스에서 여러 인스턴스를 생성할 수 없도록 제한 → **전역 상태 공유,** 시스템 **일관성** 향상
    - 리소스 절약 → **재사용 및 성능 개선**
2. **전역 접근성**
    - 어디서든 접근할 수 있는 전역 객체처럼 사용
    - but, 직접 클래스 인스턴스를 새로 만들지 않음
        - `constructor()` : 객체(인스턴스) 생성 시 자동으로 호출되는 특수 메소드, *a.k.a 생성자 함수*
        
        ```jsx
        class Singleton {
          constructor() {
            // 이미 인스턴스가 존재, 새로운 인스턴스를 생성하지 않고 기존 인스턴스를 반환
            if (Singleton.instance) {
              return Singleton.instance;
            }
            // 인스턴스가 없으면, 새 인스턴스를 만들고 static 변수에 저장
            Singleton.instance = this;
            // 기타 초기화 작업
            this.value = "Hello, Singleton!";
          }
        
          getValue() {
            return this.value;
          }
        }
        
        // 싱글톤 인스턴스 획득
        const instanceA = new Singleton();
        const instanceB = new Singleton();
        
        console.log(instanceA === instanceB); // true - 동일한 인스턴스임
        ```
        
3. **지연 초기화(Lazy Initialization)**
    - 필요할 때 인스턴스를 생성 및 초기화
    - 리소스 효율적으로 사용
    
    ```jsx
    class Singleton {
      constructor() {
        if (!Singleton.instance) {
          this.resource = null;  // 초기화하지 않고 null로만 설정
          Singleton.instance = this;
        }
        return Singleton.instance;
      }
    
      getResource() {
        // 처음 호출 시에만 초기화
        if (this.resource === null) {
          this.resource = "Heavy Resource Loaded";
          console.log("Resource initialized.");
        }
        return this.resource;
      }
    }
    
    const instance1 = new Singleton();
    console.log(instance1.getResource()); // Resource initialized. "Heavy Resource Loaded"
    console.log(instance1.getResource()); // "Heavy Resource Loaded" (재초기화 없이 반환)
    ```
    

## **3. JavaScript에서의 싱글톤 구현**

### **클로저 기반 구현**

- 클래스 내부에 instance 변수를 두고 이를 통해 하나의 인스턴스만 존재하도록 하는 거야.

```jsx
class Singleton {
  constructor() {
    if (Singleton.instance) { // 생성된 인스턴스가 있으면 동일한 인스턴스 반환
      return Singleton.instance;
    }
    Singleton.instance = this; // 인스턴스 변수

    // 초기화 로직
    this.config = "Singleton Configuration";
  }

  getConfig() {
    return this.config;
  }
}

const singletonA = new Singleton();
const singletonB = new Singleton();

console.log(singletonA === singletonB); // true, 같은 인스턴스를 가리킴
```

### **모듈 패턴으로 싱글톤 구현**

- JavaScript의 모듈 시스템(export와 import)을 이용해 싱글톤처럼 구현 가능
- 모듈 로딩 시점에 객체가 딱 한 번 생성
    - 이후 해당 모듈을 여러 곳에서 가져와도 같은 객체를 공유함

```jsx
const Singleton = (() => {
  let instance;  // 인스턴스를 저장할 변수

  function createInstance() {
    return {
      config: "Singleton Configuration",  // 인스턴스의 속성
    };
  }

  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();  // 인스턴스가 없으면 생성
      }
      return instance;  // 이미 생성된 인스턴스를 반환
    },
  };
})();

const singletonA = Singleton.getInstance();
const singletonB = Singleton.getInstance();

console.log(singletonA === singletonB); // true
```

- instance를 즉시실행함수 안에 숨겨 외부에서 직접 접근하지 못하게하고
- getInstance 메서드를 통해서만 인스턴스를 얻을 수 있게 함

### **ES6 모듈을 이용한 싱글톤 구현**

- ES6 모듈 시스템은 그 자체로 싱글톤처럼 동작할 수 있음
- 불필요한 메서드 없이 변수에 접근 가능

```jsx
// Singleton.js
export default new class Singleton {
  constructor() {
    this.config = "Singleton Configuration";
  }
  getConfig() {
    return this.config;
  }
}();
```

- new 키워드로 인스턴스를 미리 만들어서 바로 내보내는 방식
- 다른 파일에서 import Singleton from './Singleton'로 가져올 때마다 같은 인스턴스를 공유함

## 4. 클로저

### 함수와 그 함수가 선언된 렉시컬 환경을 함께 기억하는 구조를 말함

- JavaScript 에서는 함수 생성 시 scope 내의 변수들을 함께 기억함
- 클로저는 이를 통해 외부 변수에 접근 가능

```jsx
function outer() { // outer는
  let outerVar = 'I am outside!';

  function inner() { // inner가 클로저
    console.log(outerVar); // outerVar에 접근 가능
  }

  return inner;
}

const closureFunc = outer(); // outer의 실행 결과인 inner를 반환
closureFunc(); // 'I am outside!' 출력
```

### 활용

- 콜백함수, 이벤트 핸들러, 비동기 함수 등

### 주의할 점

- 메모리 관리
    - 클로저는 해당 함수, 변수를 계속 유지하도록 하기 때문
- 변수 값의 참조
    - 반복문에서 클로저가 제대로 동작하기 위해
    - 블록스코프나 즉시 실행 함수로 클로저의 스코프를 고정하는게 좋음
    - 스코프 고정 → 생성 당시의 환경(스코프, 상위 함수 변수 , …)을 기억, 이후에도 접근할 수 있도록 함

## 5. 렉시컬 환경(스코프, 스코프 체인)

### 코드가 작성될 당시의 환경

- 어떤 변수를 찾을 때 규칙과 방식을 정의한 환경
- 스코프와 스코프 체인을 포함

### 구성 요소

1. 환경 기록
    - 현재 스코프에서 선언된 변수와 함수들이 저장되는 공간
2. 외부 렉시컬 환경 참조
    - 해당 스코프의 상위 스코프를 가리키는 참조
    - 현재 스코프에 변수가 없으면 상위 스코프를 차례대로 탐색함 → 스코프체인
