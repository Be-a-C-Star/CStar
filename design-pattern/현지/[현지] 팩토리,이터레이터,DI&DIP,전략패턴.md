# 팩토리패턴

팩토리 메소드 패턴은 객체 생성을 공장 클래스로 캡슐화 처리하여 대신 생성하게 하는 생성 디자인 패턴이다. 사용처에서 직접 `new` 연산자를 사용해서 제품 객체를 생성하는 것이 아니라, 제품 객체들을 도맡아 생성하는 공장 클래스를 만들고, 이를 상속하는 서브 공장 클래스의 메서드에서 여러가지 제품 객체 생성을 각각 책임지는 것이다. 또한 객체 생성에 필요한 과정을 미리 구성해놓고, 객체 생성에 관한 전후처리를 통해 생성 과정을 다양하게 처리하여 객체를 유연하게 정할 수 있는 특징도 있다.

정리하자면, 팩토리 메소드 패턴은 객체를 만들어내는 공장을 만드는 패턴이라고 볼 수 있다. 이렇게 하는 이유는 객체간의 결합도가 낮아지고 유지보수에 용이해지기 때문이다.

```js
// CoffeeFactory 클래스: 팩토리 패턴의 기본 클래스 역할을 수행
// `createCoffee` 메서드는 정적 메서드로, 전달받은 타입에 따라 적절한 팩토리의 `createCoffee` 메서드를 호출
class CoffeeFactory {
  static createCoffee(type) {
    // type에 맞는 팩토리를 factoryList에서 가져옴
    const factory = factoryList[type];
    // 해당 팩토리에서 커피 객체를 생성해 반환
    return factory.createCoffee();
  }
}

// Latte 클래스: 구체적인 커피 제품 클래스 중 하나
// 커피의 이름을 속성으로 가짐
class Latte {
  constructor() {
    this.name = 'latte'; // 커피 이름을 'latte'로 설정
  }
}

// Espresso 클래스: 또 다른 구체적인 커피 제품 클래스
// 마찬가지로 커피 이름을 속성으로 가짐
class Espresso {
  constructor() {
    this.name = 'Espresso'; // 커피 이름을 'Espresso'로 설정
  }
}

// LatteFactory 클래스: Latte 객체를 생성하는 팩토리
// CoffeeFactory를 상속받아 팩토리 역할을 수행
class LatteFactory extends CoffeeFactory {
  static createCoffee() {
    // Latte 객체를 생성하여 반환
    return new Latte();
  }
}

// EspressoFactory 클래스: Espresso 객체를 생성하는 팩토리
// CoffeeFactory를 상속받아 팩토리 역할을 수행
class EspressoFactory extends CoffeeFactory {
  static createCoffee() {
    // Espresso 객체를 생성하여 반환
    return new Espresso();
  }
}

// factoryList 객체: 팩토리 클래스들을 key-value 쌍으로 관리
// key는 팩토리의 이름, value는 해당 팩토리 클래스
const factoryList = { LatteFactory, EspressoFactory };

// main 함수: 실제로 팩토리 패턴을 이용해 커피를 생성하고 출력하는 예제
const main = () => {
  // 'LatteFactory'라는 타입으로 커피를 주문
  const coffee = CoffeeFactory.createCoffee('LatteFactory'); // LatteFactory의 createCoffee 메서드 호출
  console.log(coffee.name); // 생성된 커피의 이름(latte)을 출력
};

main();
```

# 이터레이터 패턴

반복자(Iterator) 패턴은 일련의 데이터 집합에 대하여 순차적인 접근(순회)을 지원하는 패턴이다.

데이터 집합이란 객체들을 그룹으로 묶어 자료의 구조를 취하는 컬렉션을 말한다. 대표적인 컬렉션으로 리스트나 트리, 그래프, 테이블 ..등이 있다. 보통 배열이나 리스트 같은 경우 순서가 연속적인 데이터 집합이기 때문에 간단한 for문을 통해 순회할수 있다.

그러나 해시, 트리와 같은 컬렉션은 데이터 저장 순서가 정해지지 않고 적재되기 때문에, 각 요소들을 어떤 기준으로 접근해야 할지 애매해진다. 복잡하게 얽혀있는 자료 컬렉션들을 순회하는 알고리즘 전략을 정의하는 것을 이터레이터 패턴이라고 한다. 컬렉션 객체 안에 들어있는 모든 원소들에 대한 접근 방식이 공통화 되어 있다면 어떤 종류의 컬렉션에서도 이터레이터만 뽑아내면 여러 전략으로 순회가 가능해 보다 다형적인 코드를 설계할 수 있게 된다.

## 패턴 사용 시기

- 컬렉션에 상관없이 객체 접근 순회 방식을 통일하고자 할 때
- 컬렉션을 순회하는 다양한 방법을 지원하고 싶을 때
- 컬렉션의 복잡한 내부 구조를 클라이언트로 부터 숨기고 싶은 경우 (편의 + 보안)
- 데이터 저장 컬렉션 종류가 변경 가능성이 있을 때

  💥 클라이언트가 집합 객체 내부 표현 방식을 알고 있다면, 표현 방식이 달라지면 클라이언트 코드도 변경되어야 하는 문제가 생긴다.

## 패턴의 장점

- 일관된 이터레이터 인터페이스를 사용해 여러 형태의 컬렉션에 대해 동일한 순회 방법을 제공한다.
- 컬렉션의 내부 구조 및 순회 방식을 알지 않아도 된다.
- 집합체의 구현과 접근하는 처리 부분을 반복자 객체로 분리해 결합도를 줄 일 수 있다.
- Client에서 iterator로 접근하기 때문에 ConcreteAggregate 내에 수정 사항이 생겨도 iterator에 문제가 없다면 문제가 발생하지 않는다.
- 순회 알고리즘을 별도의 반복자 객체에 추출하여 각 클래스의 책임을 분리하여 단일 책임 원칙(SRP)를 준수한다.
- 데이터 저장 컬렉션 종류가 변경되어도 클라이언트 구현 코드는 손상되지 않아 수정에는 닫혀 있어 개방 폐쇄 원칙(OCP)를 준수한다.

## 패턴 단점

- 클래스가 늘어나고 복잡도가 증가한다.
- 만일 앱이 간단한 컬렉션에서만 작동하는 경우 패턴을 적용하는 것은 복잡도만 증가할 수 있다.
- 이터레이터 객체를 만드는 것이 유용한 상황인지 판단할 필요가 있다.
- 구현 방법에 따라 캡슐화를 위배할 수 있다.

```js
class Iterator {
  constructor(collection) {
    this.collection = collection; // 순회할 대상 컬렉션
    this.index = 0; // 현재 위치를 나타내는 인덱스
  }

  next() {
    // 다음 요소를 반환
    if (this.hasNext()) {
      return this.collection[this.index++]; // 현재 요소를 반환하고 인덱스를 증가
    }
    return null; // 더 이상 요소가 없으면 null 반환
  }

  hasNext() {
    // 다음 요소가 있는지 확인
    return this.index < this.collection.length;
  }

  reset() {
    // 순회를 처음부터 다시 시작
    this.index = 0;
  }
}

// 사용 예시
const items = ['apple', 'banana', 'cherry'];
const iterator = new Iterator(items);

console.log(iterator.next()); // apple
console.log(iterator.next()); // banana
console.log(iterator.hasNext()); // true
console.log(iterator.next()); // cherry
console.log(iterator.hasNext()); // false

iterator.reset(); // 순회를 초기화
console.log(iterator.next()); // apple
```

# DI & DIP

## DI(Dependency Injection): 의존성 주입

DI는 객체가 의존하는 객체(의존성)를 직접 생성하지 않고 외부에서 주입받는 방식을 말한다. 이렇게 하면 의존성을 더 쉽게 교체하거나 테스트할 수 있다.

```js
class Engine {
  start() {
    return 'Engine started!';
  }
}

class Car {
  constructor() {
    this.engine = new Engine(); // Car가 Engine에 강하게 결합됨
  }

  drive() {
    return this.engine.start();
  }
}

const car = new Car();
console.log(car.drive()); // Engine started!
```

Car 클래스가 Engine 객체를 직접 생성하므로 강한 결합이 발생한다. 이를 DI를 이용해 개선할 수 있다.

```js
class Engine {
  start() {
    return 'Engine started!';
  }
}

class Car {
  constructor(engine) {
    this.engine = engine; // 의존성을 외부에서 주입받음
  }

  drive() {
    return this.engine.start();
  }
}

// 의존성을 외부에서 주입
const engine = new Engine();
const car = new Car(engine);

console.log(car.drive()); // Engine started!
```

    •	Car 클래스는 더 이상 Engine에 강하게 결합되지 않음.
    •	Engine을 교체하거나 Mock 객체로 대체해 테스트하기 쉬움.

## DIP(Dependency Inversion Principle): 의존성 역전 원칙

DIP는 상위 모듈이 하위 모듈에 의존하지 않고, 둘 다 추상화에 의존해야 한다는 원칙이다. 즉, 구체적인 구현체가 아니라 인터페이스나 추상화에 의존하도록 설계한다.

#### DIP를 위반한 코드

```js
class GasEngine {
  start() {
    return 'Gas engine started!';
  }
}

class Car {
  constructor() {
    this.engine = new GasEngine(); // Car가 구체적인 GasEngine에 의존
  }

  drive() {
    return this.engine.start();
  }
}

const car = new Car();
console.log(car.drive()); // Gas engine started!
```

Car 클래스가 GasEngine에 의존하므로 다른 종류의 엔진(예: 전기 엔진)으로 변경하려면 Car를 수정해야 한다.

```js
class GasEngine {
  start() {
    return 'Gas engine started!';
  }
}

class ElectricEngine {
  start() {
    return 'Electric engine started!';
  }
}

// 인터페이스 역할: 엔진의 공통 동작을 정의
class Car {
  constructor(engine) {
    this.engine = engine; // Car는 추상화(인터페이스)에 의존
  }

  drive() {
    return this.engine.start();
  }
}

// 의존성을 주입하면서 유연하게 사용할 수 있음
const gasEngine = new GasEngine();
const electricEngine = new ElectricEngine();

const car1 = new Car(gasEngine);
console.log(car1.drive()); // Gas engine started!

const car2 = new Car(electricEngine);
console.log(car2.drive()); // Electric engine started!
```

    •	Car 클래스는 이제 특정 엔진 구현에 의존하지 않고, 공통된 인터페이스를 따르는 객체를 받아 동작.
    •	새로운 엔진을 추가하려면 Car를 수정할 필요 없이 엔진 클래스를 구현하기만 하면 됨.

# 전략패턴

전략 패턴은 동작(알고리즘)을 캡슐화하고, 이를 사용하는 클라이언트 코드에서 동적으로 교체할 수 있게 설계하는 디자인 패턴이다. 즉, 여러 알고리즘이 있을 때, 이를 각각 클래스로 정의하고 런타임에 적절한 알고리즘을 선택해 사용할 수 있다.

이 패턴은 다음과 같은 상황에서 유용하다:

    •	여러 개의 유사한 동작이 있고, 이를 교체 가능하게 하고 싶을 때.
    •	코드의 조건문/분기문이 많아질 때 이를 줄이고 싶을 때.
    •	동작을 쉽게 확장할 수 있도록 설계하고 싶을 때.

## 구성 요소

    1.	Context: 전략을 사용하는 클라이언트 클래스.
    2.	Strategy: 공통 인터페이스(또는 메서드)를 가진 동작 클래스들.
    3.	Concrete Strategies: 구체적인 전략을 구현한 클래스들.

### 예시: 결제 방식 선택

쇼핑몰에서 다양한 결제 방식을 지원한다고 가정했을 때, 전략 패턴을 사용해 이를 구현한다.

```js
// 1. 전략 인터페이스 역할: 모든 결제 방식에서 공통으로 사용할 메서드
class PaymentStrategy {
  pay(amount) {
    throw new Error('pay() 메서드는 반드시 구현해야 합니다.');
  }
}

// 2. 구체적인 전략들
class CreditCardPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`신용카드로 ${amount}원을 결제했습니다.`);
  }
}

class PayPalPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`PayPal로 ${amount}원을 결제했습니다.`);
  }
}

class BitcoinPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`비트코인으로 ${amount}원을 결제했습니다.`);
  }
}

// 3. Context: 결제를 관리하는 클래스
class PaymentContext {
  constructor(strategy) {
    this.strategy = strategy; // 사용할 결제 전략
  }

  setStrategy(strategy) {
    this.strategy = strategy; // 전략 변경 가능
  }

  executePayment(amount) {
    this.strategy.pay(amount); // 선택된 전략으로 결제
  }
}

// 4. 사용 예시
const creditCard = new CreditCardPayment();
const paypal = new PayPalPayment();
const bitcoin = new BitcoinPayment();

const payment = new PaymentContext(creditCard); // 초기 전략: 신용카드
payment.executePayment(10000); // "신용카드로 10000원을 결제했습니다."

payment.setStrategy(paypal); // 전략 변경: PayPal
payment.executePayment(20000); // "PayPal로 20000원을 결제했습니다."

payment.setStrategy(bitcoin); // 전략 변경: 비트코인
payment.executePayment(30000); // "비트코인으로 30000원을 결제했습니다."
```

## 장점

    •	유연성: 알고리즘을 동적으로 변경 가능.
    •	확장성: 새로운 전략을 추가해도 기존 코드를 수정할 필요 없음.
    •	코드 분리: 알고리즘 구현과 사용하는 코드를 분리.

## 단점

    •	클래스가 늘어나서 코드가 복잡해질 수 있음.
    •	단순한 상황에서는 오히려 과도한 설계가 될 수 있음.

전략 패턴을 활용하면 다양한 동작을 유연하게 관리할 수 있고, 조건문/분기문을 대폭 줄일 수 있다.
