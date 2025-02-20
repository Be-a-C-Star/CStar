## 1. Factory Pattern

객체 생성 로직을 캡슐화, 클라이언트가 객체 생성 로직 없이 객체 생성할 수 있음

객체 생성 로직을 분리해서 재사용성, 유지보수성 향상

- 구조
    - 상속관계의 클래스에서 상위클래스에서 생성자 함수로 구체적인 구조 결정
    - 하위 클래스에서 조건문 등으로 객체 생성에 대한 구체적인 내용 설정
- 특징
    - 생성의 책임을 팩토리함수(클래스)에 위임함
    - 다양한 객체를 동적으로 생성, 유연성

```jsx
class Whisky {
  constructor(name, years) {
    this.type = "Whisky";
    this.name = name;
    this.years = years;
  }
}

class Vodka {
  constructor(name, ABV) {
    this.type = "Vodka";
    this.name = name;
    this.ABV = ABV;
  }
}

class Factory {
  static createBottle(type, options) {
    switch (type) {
      case "Whisky":
        return new Whisky(options.name, options.years);
      case "Vodka":
        return new Vodka(options.name, options.ABV);
      default:
        throw new Error("Unknown type");
    }
  }
}

const myFavDrink = Factory.createBottle("Whisky", {
  name: "Talisker",
  years: "12",
});

```

## 2. Iterator Pattern

객체 내 요소들에 순차적으로 접근할 수 있도록 인터페이스를 제공

컬렉션 내부 구조 노출 없이 탐색이 가능하도록 함

- 특징
    - 반복 가능한 객체 구현
    - `next()` 메서드로 요소를 하나씩 탐색
- JavaScript에서의 순회
    - 자바스크립트에서는 이터레이터를 기본적으로 제공함  `for - of 문`

```jsx
class Iterator {
  constructor(data) {
    this.data = data;
    this.index = 0;
  }

  next() {
    if (this.index < this.data.length) {
      return { value: this.data[this.index++], done: false };
    } else {
      return { done: true };
    }
  }
}

const bottles = ["whisky", "vodka", "gin", "rum", "tequila"]
const bottlesIterator = new Iterator(items);

let result = bottlesIterator.next();
while (!result.done) {
  console.log(result.value);
  result = bottlesIterator.next();
} // whisky, vodka, gin, rum, tequila
```
