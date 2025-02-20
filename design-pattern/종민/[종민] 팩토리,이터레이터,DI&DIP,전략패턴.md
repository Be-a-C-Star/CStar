### 1. **팩토리 패턴 (Factory Pattern)**

팩토리 패턴은 객체 생성 로직을 캡슐화하여, 객체 생성의 책임을 팩토리로 위임하는 패턴입니다. 이 패턴은 객체 생성 코드를 중앙화하여 코드 중복을 줄이고 유지보수성을 높이는 데 유용합니다.

#### **예시 (JavaScript):**
```javascript
class Dog {
  constructor(name) {
    this.name = name;
    this.sound = "Woof";
  }
}

class Cat {
  constructor(name) {
    this.name = name;
    this.sound = "Meow";
  }
}

class AnimalFactory {
  static createAnimal(type, name) {
    switch (type) {
      case "dog":
        return new Dog(name);
      case "cat":
        return new Cat(name);
      default:
        throw new Error("Unknown animal type");
    }
  }
}

// 사용 예시
const myDog = AnimalFactory.createAnimal("dog", "Buddy");
console.log(myDog); // Dog { name: 'Buddy', sound: 'Woof' }

const myCat = AnimalFactory.createAnimal("cat", "Whiskers");
console.log(myCat); // Cat { name: 'Whiskers', sound: 'Meow' }
```

---

### 2. **이터레이터 패턴 (Iterator Pattern)**

이터레이터 패턴은 컬렉션(배열, 리스트 등)을 순회하면서 내부 요소에 접근하는 방법을 제공하는 패턴입니다. JavaScript에서는 `for of`가 대표적인 예시


---

### 3. **DI (Dependency Injection)와 DIP (Dependency Inversion Principle)**

#### **DI (Dependency Injection):**
DI는 객체가 의존성을 직접 생성하지 않고 외부에서 주입받도록 설계하는 방법입니다. 이를 통해 객체 간의 결합도를 낮추고 테스트 및 확장성을 높일 수 있습니다.

#### **DIP (Dependency Inversion Principle):**
DIP는 고수준 모듈이 저수준 모듈에 의존하지 않고, 둘 다 추상화된 인터페이스에 의존하도록 설계해야 한다는 원칙입니다.

#### **예시 (JavaScript):**
```javascript
// 추상화된 인터페이스
class NotificationService {
  send(message) {
    throw new Error("send() must be implemented");
  }
}

// 저수준 모듈
class EmailService extends NotificationService {
  send(message) {
    console.log(`Sending email: ${message}`);
  }
}

// 저수준 모듈
class SMSService extends NotificationService {
  send(message) {
    console.log(`Sending SMS: ${message}`);
  }
}

// 고수준 모듈
class NotificationManager {
  constructor(service) {
    this.service = service; // DI를 통해 의존성을 주입받음
  }

  notify(message) {
    this.service.send(message);
  }
}

// 사용 예시
const emailService = new EmailService();
const smsService = new SMSService();

const emailManager = new NotificationManager(emailService);
emailManager.notify("Hello via Email!");

const smsManager = new NotificationManager(smsService);
smsManager.notify("Hello via SMS!");
```

---

### 4. **전략 패턴 (Strategy Pattern)**

전략 패턴은 행위를 캡슐화하여 교체 가능한 전략으로 정의하고, 런타임에 동적으로 전략을 바꿀 수 있도록 만드는 패턴입니다. 

#### **예시 (JavaScript):**
```javascript
// 전략 인터페이스
class PaymentStrategy {
  pay(amount) {
    throw new Error("pay() must be implemented");
  }
}

// 전략 1: 카드 결제
class CardPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid ${amount} using Card.`);
  }
}

// 전략 2: 페이팔 결제
class PayPalPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid ${amount} using PayPal.`);
  }
}

// 전략을 사용하는 컨텍스트
class PaymentProcessor {
  constructor(strategy) {
    this.strategy = strategy;
  }

  setStrategy(strategy) {
    this.strategy = strategy; // 런타임에 전략 변경 가능
  }

  processPayment(amount) {
    this.strategy.pay(amount);
  }
}

// 사용 예시
const cardPayment = new CardPayment();
const payPalPayment = new PayPalPayment();

const paymentProcessor = new PaymentProcessor(cardPayment);
paymentProcessor.processPayment(100); // Paid 100 using Card.

paymentProcessor.setStrategy(payPalPayment);
paymentProcessor.processPayment(200); // Paid 200 using PayPal.
```

--- 
