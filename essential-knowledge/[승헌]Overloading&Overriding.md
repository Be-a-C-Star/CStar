## **1. 오버로딩 (Overloading)**

- **같은 함수 이름, 매개변수의 수나 타입에 따라 다른 기능을 수행하도록 정의**
- 자바스크립트에서는 기본적으로 함수 오버로딩을 지원하지 않음
    - 매개변수의 **타입이나 개수를 조건문으로 구분해서** 비슷하게 구현 해야함

```jsx
function greet(name, age) {
  if (typeof age === "undefined") {
    console.log(`Hello, ${name}!`);
  } else {
    console.log(`Hello, ${name}. You are ${age} years old.`);
  }
}

greet("Adam");        // Hello, Adam!
greet("Adam", 27);    // Hello, Alice. You are 27 years old.
```

- **TypeScript**에서는 함수 오버로딩이 가능
    - 인자에 따라 함수가 다르게 동작하도록 구현할 수도 있어.

```tsx
// 타입 정의
function add(a: number, b: number): number;
function add(a: string, b: string): string;

// 함수 구현
function add(a: any, b: any): any {
  return a + b;
}

// 사용 예시
const sum1 = add(5, 10); // 숫자 더하기
console.log(sum1); // 15

const sum2 = add("Hello, ", "World!"); // 문자열 더하기
console.log(sum2); // "Hello, World!"
```

## **2. 오버라이딩 (Overriding)**

- **상속받은 클래스에서 부모 클래스의 메서드를 재정의하는 것**
- 자바스크립트는 ES6에서 클래스 문법이 도입, 상속과 메서드 오버라이딩을 지원
- 덮어쓰기 같은것..

```jsx
class car {
  run() {
    console.log("skrr");
  }
}

class motorcycle extends car {
  run() {
    console.log("vroom~");
  }
}

const myCar = new motorcycle();
myCar.run(); // vroom~
```

## **3. 추상화 (Abstraction)**

- **세부 사항을 감추고 필요한 기능만을 외부에 노출하는 것**
- 클래스를 사용해 추상화를 구현할 수 있음
- ES6에서 abstract 키워드는 제공하지 않기 때문에 간접적으로 추상화를 구현해야 함

```jsx
class Shape {
  calculateArea() {
    throw new Error("calculateArea must be implemented by subclass");
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }

  calculateArea() {
    return Math.PI * this.radius ** 2;
  }
}

const myCircle = new Circle(5);
console.log(myCircle.calculateArea()); // 78.5398...
```

- Shape 클래스는 추상 개념으로만 존재
- 실제 사용은 상속받은 Circle과 같은 구체적인 클래스
- calculateArea 메서드는 자식 클래스에서 구현하게 함으로써 추상화를 구현함
