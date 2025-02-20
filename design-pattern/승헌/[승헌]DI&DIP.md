## 1. DI (의존성 주입)

DI는 객체가 직접 의존성을 생성하지 않고 외부에서 주입받도록 설계하는 방식

객체 간 결합도를 낮춰 유연성, 테스트 편의성 향상

## 2. DIP (의존성 역전 원칙)

고수준 모듈이 저수준 모듈에 의존하지 않고, 추상화된 인터페이스에 의존해야 한다는 원칙

의존성 주입으로 DIP를 구현할 수 있음

```jsx
// 설명서 같은 역할, send 메서드 없이 상속하면 오류 발생
// 어떤 클래스인지 몰라도 일관된 방식으로 메서드 호출 가능하도록 함
class NotificationService {
  send(message) {
    throw new Error("send() must be implemented!");
  }
}

// 구현클래스, 상속을 통해 동일한 인터페이스(메서드, 속성 이름) 가지도록함
class EmailNotification extends NotificationService {
  send(message) {
    console.log(`new Email: ${message}`);
  }
}
class SMSNotification extends NotificationService {
  send(message) {
    console.log(`[[ ${message} ]]`);
    // 여기서 다양한 알람 전달 로직 구성할 수 있음
  }
}

// 새로운 알림서비스가 필요할 때 이 manager 객체 수정 없이 새로운 notification 클래스만 정의하면 됨
class NotificationManager {
  // 어떤 알림 서비스를 사용할 지 외부에서 주입받아 동작
  constructor(notificationService) {
    this.notificationService = notificationService;
  }

  // 매니저 객체는 단순히 외부로부터 주입받은 서비스 객체의 send 메서드를 호출하는 방식으로 동작
  notify(message) {
    this.notificationService.send(message);
  }
}

const emailService = new EmailNotification();
const smsService = new SMSNotification();

const manager1 = new NotificationManager(emailService);
manager1.notify("Hello via Email"); // new Email : Hello

const manager2 = new NotificationManager(smsService);
manager2.notify("Hello via SMS"); // [[ Hello ]]

```

## 3. Strategy Pattern

알고리즘을 클래스로 분리, 런타임에 알고리즘을 교체할 수 있도록 설계

클라이언트가 특정 알고리즘에 의존하지 않도록 만듦

의존성 주입의 원래를 활용한 패턴

- 특징
    - 여러 알고리즘을 정의하고 교체할 수 있도록 함
    - 새로운 알고리즘 추가 시 코드 수정이 필요없음

```jsx
// 전략 인터페이스, 포맷 안내
class PaymentStrategy {
  pay(amount) {
    throw new Error("pay() must be implemented");
  }
}

// PayPal 전략
class PayPalPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid $${amount} using PayPal`);
  }
}

// CreditCard 전략
class CreditCardPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid $${amount} using Credit Card`);
  }
}

// PaymentProcessor는 전략을 실행
class PaymentProcessor {
  constructor(strategy) {
  // 외부에서 의존성 주입받음
    this.strategy = strategy;
  }

  processPayment(amount) {
    this.strategy.pay(amount);
  }
}

// 사용
const paypal = new PayPalPayment();
const creditCard = new CreditCardPayment();

const processor1 = new PaymentProcessor(paypal);
processor1.processPayment(100); // Paid $100 using PayPal

const processor2 = new PaymentProcessor(creditCard);
processor2.processPayment(200); // Paid $200 using Credit Card
```

## 4. 전략패턴과 의존성 주입의 차이점

- 특정 알고리즘이나 동작을 선택해 사용할 수 있도록 하기 위한 디자인패턴이라는 공통점
- 차이점
    - 목적
        - 전략패턴: 동적인 알고리즘 할당, 변경
        - DI: 결함도 감소 및 유연성 향상
    - 적용 방식
        - 전략패턴: 객체 내부에서 전략을 교체
        - DI: 객체 외부에서 의존성을 주입, 원하는 로직으로 동작하도록 함

