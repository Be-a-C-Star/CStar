

### 클래스와 객체 인스턴스 (자바스크립트 관점)

1. **클래스 (Class)**:
   - 자바스크립트에서 클래스는 객체를 생성하기 위한 **템플릿**으로 `class` 키워드로 정의합니다.
   - ES6(ECMAScript 2015)부터 `class` 문법이 도입되었지만, 실제로 클래스는 함수로 구현되며 프로토타입 기반으로 작동합니다.
   - 클래스에는 객체의 속성(property)과 동작(method)을 정의할 수 있습니다.

   ```javascript
   class Car {
       constructor(brand, color) {
           this.brand = brand;
           this.color = color;
       }
       
       drive() {
           console.log(`${this.brand} is driving`);
       }
   }
   ```

2. **객체 인스턴스 (Instance)**:
   - 클래스는 **new 키워드**를 통해 **객체 인스턴스**로 생성됩니다.
   - 각 인스턴스는 클래스에 정의된 속성과 메서드를 독립적으로 갖게 됩니다.
   
   ```javascript
   const myCar = new Car('Toyota', 'Red');
   const yourCar = new Car('Honda', 'Blue');
   
   myCar.drive(); // "Toyota is driving"
   yourCar.drive(); // "Honda is driving"
   ```

이처럼 **클래스는 설계도**, **객체 인스턴스는 실제 메모리에 생성된 객체**입니다. 자바스크립트의 클래스는 프로토타입 체인을 이용하여 생성된 모든 인스턴스가 메서드와 속성을 공유하는 방식으로 작동합니다.

### `static` 키워드의 단점 (자바스크립트 관점)
자바스크립트에서도 `static` 키워드를 사용하여 클래스의 인스턴스가 아닌 **클래스 자체에 귀속되는 메서드나 속성**을 정의할 수 있습니다. 그러나 `static`을 사용할 때 몇 가지 단점이 있습니다.

1. **객체 지향 원칙 위반 가능성**:
   - `static` 메서드는 인스턴스에 속하지 않고 클래스에 직접 속하기 때문에 **다형성**과 **상속**을 약화시킬 수 있습니다.
   - 여러 객체가 공유해야 하는 속성이나 메서드를 정의하는 경우 외에는 클래스 인스턴스에 속하지 않는 `static` 메서드와 속성의 사용을 제한해야 합니다.

   ```javascript
   class Car {
       static totalCars = 0;
       
       constructor(brand, color) {
           this.brand = brand;
           this.color = color;
           Car.totalCars++; // 모든 Car 객체가 공유하는 속성
       }
   }
   ```

2. **프로토타입 체인의 단절**:
   - 자바스크립트의 `static` 메서드는 **프로토타입 체인**에 포함되지 않기 때문에 인스턴스에서 접근할 수 없습니다.
   - 이는 객체지향 설계에서 **다형성**을 구현하는 데 장애가 될 수 있습니다.

3. **메모리 관리의 어려움**:
   - `static` 속성은 프로그램이 실행되는 동안 메모리에 유지되기 때문에, 필요 이상으로 메모리를 점유할 수 있습니다.
   - 주의하지 않으면 **메모리 누수**의 위험이 커집니다.

4. **테스트의 어려움**:
   - `static` 속성과 메서드는 프로그램 전체에서 공유되므로, 특정 상황에 따라 독립적으로 테스트하기 어렵습니다. 이는 **유닛 테스트**에서 종종 문제가 됩니다.
