## **1. 클래스(Class)**

- 객체를 만들기 위한 **설계도** 역할, 추상적 개념
- 클래스는 데이터(속성)와 메서드(기능)를 정의
    - **속성(Attributes)**
        - 객체가 가지는 데이터, 상태를 나타냄
        - 변수로 정의됨
        - *e.g.* `color`, `fuelLevel`
    - **메소드(Methods)**
        - 특정 동작을 수행하는 함수
        - *e.g.* `drive()`, `stop()`
- 이를 기반으로 여러 객체를 생성함

```jsx
class Car {
  constructor(name, fuelLevel = 100) {
    this.color = color;
    this.fuelLevel = fuelLevel;
  }
  drive(distance) {
    console.log(`The ${this.name}'s fuel ${fuelLevel - distance} left`);
}

class ElectricCar extends Car { // Car클래스 상속받음
  constructor(color, batteryLevel = 100) {
    super(name); // 부모 클래스의 생성자 호출
    this.batteryLevel = batteryLevel;
  }
  drive(distance) { // 부모 클래스의 메소드 오버라이딩
    console.log(`The ${this.name}'s battery ${batteryLevel - distance} left`);
  }
}
```

## **2. 객체(Object)**

- 클래스의 인스턴스를 통해 실제로 만들어진 데이터, **구체적 사례**
- 속성과 메소드를 포함하는 기본 데이터 구조
    - **클래스 없이도 생성 가능**
- e.g. Car클래스 → Truck, Bus처럼 특정 속성과 기능을 가진 객체

## **3. 인스턴스(Instance)**

- 특정 클래스에서 생성된 객체
- 객체는 클래스에 기반한 인스턴스의 한 형태
- AWS 클라우드의 가상서버를 지칭하는 용어로도 쓰임

```jsx
const myCar = new Car("진짜차");
myCar.drive(50);   // The 진짜차's fuel 50 left

const myElectricCar = new ElectricCar("가전제품");
myElectricCar.drive(30);   // The 가전제품's battery 70 left
```

## 4. 차이점

- 클래스는 설계도, 객체와 인스턴스는 실제 만들어진 구체적 데이터
- 인스턴스는 객체와 거의 같은 의미이지만, 특정 클래스에 속한 객체를 지칭하는 구체적 표현에 사용됨

### **인스턴스**는 **클래스**에서 만들어진 **객체**이다~

## 5. 상속

- 객체 지향 프로그래밍(Object-Oriented Programming, OOP)의 중요 개념
- 클래스의 재사용 및 클래스의 책임 분리, 계층구조를 구성할 수 있음(종속)
- 주요 키워드
    - `super()`
        - 부모클래스의 생성자, 메소드를 호출할 때 사용
        - 부모클래스 생성자 실행, 부모클래스로 부터 상속받아 자식클래스에 할당되는 속성이 초기화됨
    - 메소드 오버라이딩(Overriding)
        - 자식 클래스에서 부모 클래스와 동일한 이름의 메서드를 다시 정의하는것
        - 덮어쓰기

---


---

## 1. Static?

- 클래스 자체에 속하는 메소드나 속성을 정의
- 인스턴스 생성 없이 클래스명으로 직접 호출, 접근 가능
    - 인스턴스에서 접근 불가
    - 클래스 자체에 귀속

```jsx
class Counter {
  static count = 0;
  constructor() {
    Counter.count += 1;
  }
  
  static getCount() {
    return Counter.count;
  }
}

new Counter();
new Counter();
console.log(Counter.getCount()); // 2 
```

```jsx
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}
console.log(MathUtils.add(5, 3)); // 8
const mathInstance = new MathUtils();
console.log(mathInstance.add(5, 3)); // error: add is not a function
```

## 2. 왜 쓸까?

- 유틸함수, 공용 데이터에 사용
    - 특정 객체(인스턴스)말고 **클래스 자체**에 속해야 하는 경우
    - 모든 인스턴스가 동일하게 **공유**해야 하는 **속성**, **데이터**를 static으로 정의할 수 있음
- 메모리 절약
    - 공통 기능이나 속성을 클래스에서 관리
    - 활용이 필요한 경우 인스턴스를 인수로 전달해서 사용

## 3. 단점

1. 인스턴스 데이터에 접근이 어려움
    1. 인수로 인스턴스 주면 되지만 객체지향 원칙 충돌, 코드 더러워짐
    2. 직관성 부족, 본래 목적에서 너무 벗어나는 접근
2. 상속의 유연성 부족
    1. 상속은 되지만 재정의가 불가능함
3. 객체지향 위반 가능성
    1. static은 인스턴스 중심이 아님
    2. 다형성, 캡슐화 특성 위배
4. 테스트 어려움
    1. 전역적으로 의존성 분리가 어려움
    2. 메소드를 mocking 하기 어려움
5. 메모리 누수
    1. 인스턴스 생성하지 않아도 메모리에 상주함(정적데이터)
    2. 애플리케이션 규모가 커지면서 static 데이터 증가
