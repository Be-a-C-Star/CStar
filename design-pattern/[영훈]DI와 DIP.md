### 의존성주입(DI, Dependency Injection)

###### 메인 모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자(dependency injector)가 이 부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식

- 메인 모듈과 하위모듈간의 의존성을 조금 더 느슨하게 만들 수 있으며 모듈을 쉽게 교체 가능한 구조로 만듬

> 의존한다의 의미❓
> A가 B에 의존한다 = B가 변하면 A에 영향을 미치는 관계 = A - > B
> 코드로는 이러한 것을 A가 B에 의존한다고 함

##### 의존관계역전원칙(DIP, Dependency Inversion Principle)

###### 의존성 주입을 할 때는 의존관계역전원칙(DIP, Dependency Inversion Principle)이 적용됨

###### 이는 2가지의 규칙을 지키는 상태를 말함

1. 상위 모듈은 하위 모듈에 의존해서는 안 된다. 둘 다 추상화에 의존해야함
2. 추상화는 세부사항에 의존해서는 안 된다. 세부 사항은 추상화에 따라 달라져야함

##### 의존성 주입의 장점

- 외부에서 모듈을 생성하여 집어넣는 구조가 되기 때문에 모듈들을 쉽게 교체할 수 있는 구조가 됨
- 단위 테스팅과 `마이그레이션이` 쉬워짐
  - `마이그레이션` : 다른 운영환경으로 이동하는 것(DB이동, 데이터 이동 등)
- 애플리케이션 의존성 방향이 좀 더 일관되어 코드를 추론하기가 쉬워짐

##### 의존성 주입의 단점

- 결국에는 모듈이 더 생기게 되므로 복잡도가 증가함
- 종속성 주입자체가 컴파일을 할 때가 아닌 런타임 때 일어나기 때문에 컴파일을 할
  때 종속성 주입에 관한 에러를 잡기가 어려워질 수 있음

##### DI가 적용되지 않았을 때

```javascript
class EmailService {
  sendEmail(message) {
    console.log(`이메일 전송: ${message}`);
  }
}

class Notification {
  constructor() {
    this.emailService = new EmailService(); // 의존성을 내부에서 직접 생성
  }

  notify(message) {
    this.emailService.sendEmail(message);
  }
}

// 사용
const notification = new Notification();
notification.notify("의존성 주입 없이 작동 중!");
```

- Notification 클래스가 EmailService의 구체적인 구현에 강하게 결합되어 있음
- EmailService를 다른 구현체(예: SMSService)로 변경하려면 Notification 코드를 수정해야 함
- 테스트 시 EmailService를 Mock 객체로 교체하기 어려움

```javascript
class EmailService {
  sendEmail(message) {
    console.log(`이메일 전송: ${message}`);
  }
}

class SMSService {
  sendSMS(message) {
    console.log(`SMS 전송: ${message}`);
  }
}

class Notification {
  constructor(service) {
    this.service = service; // 외부에서 의존성 주입
  }

  notify(message) {
    this.service.sendEmail
      ? this.service.sendEmail(message) // EmailService 처리
      : this.service.sendSMS(message); // SMSService 처리
  }
}

// 사용
const emailService = new EmailService();
const smsService = new SMSService();

const emailNotification = new Notification(emailService);
emailNotification.notify("이메일 의존성 주입!");

const smsNotification = new Notification(smsService);
smsNotification.notify("SMS 의존성 주입!");
```

- EmailService 대신 SMSService를 쉽게 교체할 수 있음

```javascript
class MockService {
  sendEmail(message) {
    console.log(`Mock 이메일 전송: ${message}`);
  }
}

const mockNotification = new Notification(new MockService());
mockNotification.notify("테스트 중입니다!");
```

- 테스트 시 MockService를 주입하여 실제 네트워크 요청 없이 동작을 검증할 수 있음
- 새로운 서비스를 추가해도 Notification 클래스는 수정 없이 동작함
