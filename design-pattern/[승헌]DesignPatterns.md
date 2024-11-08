## 1. 디자인 패턴?

- 반복적으로 등장하는 설계 문제를 효율적으로 해결하기 위해 만들어진 ‘문제 해결 템플릿’
- 주로 객체 지향 프로그래밍에서 코드 구조를 개선하고 유지보수성을 높이기 위해 사용됨
- 소프트웨어를 설계하는 과정에서 발생할 수 있는 특정 상황에 대한 **Best Practice**

## 2. 장점

- **코드의 가독성** 향상
- **유지보수성** 향상
- 협업 시 **이해하기 쉬운 코드**를 작성할 수 있음
- 특정 문제 해결에 최적화된 구조를 가져다 사용함, **시간 절약**

## 3. 디자인 패턴의 분류

### **생성 패턴 (Creational Patterns)**

- **싱글톤(Singleton) 패턴**
    - 특정 클래스의 인스턴스를 하나만 생성, 시스템 전체에서 공유할 때 사용
    - *e.g.* 데이터베이스 연결 객체, 설정 객체 등 하나만 존재해야 하는 객체에 유용.
    - 클래스 내에서 하나의 인스턴스만 만들어짐, 모든 코드에서 같은 인스턴스를 사용
    ⇒  자원 절약
    
    ```jsx
    class Singleton {
      private static instance: Singleton;
    
      private constructor() {} // 외부에서 인스턴스 생성 불가
    
      public static getInstance(): Singleton {
        if (!Singleton.instance) {
          Singleton.instance = new Singleton();
        }
        return Singleton.instance;
      }
    }
    
    // 사용
    const instance1 = Singleton.getInstance();
    const instance2 = Singleton.getInstance();
    console.log(instance1 === instance2); // true
    ```
    
- **팩토리(Factory) 패턴**
    - 객체 생성의 책임을 별도의 팩토리 클래스로 분리
    - 객체를 생성하는 로직을 유연하게 변경할 수 있게 함
    - 팩토리 메서드가 필요한 객체의 타입에 따라 적절한 인스턴스를 반환.
    - *e.g.* 다양한 버튼을 생성하는 GUI 애플리케이션, 특정 버튼의 스타일에 따라 인스턴스를 리턴
    
    ```jsx
    interface Button {
      name: string;
      color: string;
    }
    
    class WhiteButton implements Button {
      name = "Button_White";
      color = "white";
    }
    
    class BlackButton implements Button {
      name = "Button_Black";
      color = "black";
    }
    
    class ButtonPainter {
      public static createButton(type: string): Button {
        if (type === "W") return new WhiteButton();
        if (type === "B") return new BlackButton();
        throw new Error("Unknown Button type");
      }
    }
    
    // 사용
    const product = ProductFactory.createProduct("W");
    console.log(Button.name); // Button_White
    ```
    
- **빌더(Builder) 패턴**
    - 복잡한 객체를 단계별로 조립할 때 사용
    - 객체 생성의 유연성을 높임
    - 여러 속성을 설정해주어야 할 때, 빌더 클래스를 통해 속성값을 설정하면서 인스턴스를 생성
    - *e.g.* 여러 옵션을 가진 복잡한 HTML 요소 생성, JSON 객체 생성
    
    ```jsx
    class Car {
      engine: string;
      wheels: number;
      color: string;
    
      constructor(builder: CarBuilder) {
        this.engine = builder.engine;
        this.wheels = builder.wheels;
        this.color = builder.color;
      }
    }
    
    class CarBuilder {
      engine: string;
      wheels: number;
      color: string;
    
      setEngine(engine: string): CarBuilder {
        this.engine = engine;
        return this;
      }
    
      setWheels(wheels: number): CarBuilder {
        this.wheels = wheels;
        return this;
      }
    
      setColor(color: string): CarBuilder {
        this.color = color;
        return this;
      }
    
      build(): Car {
        return new Car(this);
      }
    }
    
    // 사용
    const myCar = new CarBuilder().setEngine("V8").setWheels(4).setColor("red").build();
    console.log(myCar);
    // Car {
    //   engine: "V8",
    //   wheels: 4,
    //   color: "red"
    // }
    ```
    

### **구조 패턴 (Structural Patterns)**

- **어댑터(Adapter) 패턴**
    - 다른 인터페이스를 가진 클래스들이 함께 동작할 수 있게 중간 어댑터 객체를 통한 호환성을 제공
    - *e.g.* 다양한 라이브러리의 기능을 통합할 때 유용, 다양한 결제 서비스의 호출 방식을 하나로 통일
    
    ```jsx
    // 공통 인터페이스 정의
    class PaymentProcessor {
      processPayment(amount) {
        throw new Error("This method should be overridden.");
      }
    }
    
    // Toss 어댑터
    class TossAdapter extends PaymentProcessor {
      constructor(TossInstance) {
        super();
        this.Toss = stripeInstance;
      }
    
      processPayment(amount) {
        // Toss의 결제 API 호출 방식에 맞춰 호출
        return this.Toss.makePayment(amount);
      }
    }
    
    // PayPal 어댑터
    class PayPalAdapter extends PaymentProcessor {
      constructor(paypalInstance) {
        super();
        this.paypal = paypalInstance;
      }
    
      processPayment(amount) {
        // PayPal의 결제 API 호출 방식에 맞춰 호출
        return this.paypal.sendPayment(amount);
      }
    }
    
    // 사용 예시
    const TossAdapter = new TossAdapter(new Toss());
    const paypalAdapter = new PayPalAdapter(new PayPal());
    
    stripeAdapter.processPayment(100); // Stripe API 사용
    paypalAdapter.processPayment(100); // PayPal API 사용
    ```
    
- **브리지(Bridge) 패턴**
    - 추상화와 구현을 분리, 서로 독립적으로 확장할 수 있게 하는 구조 패턴
    - *e.g.* 크로스플랫폼 애플리케이션 개발 시, 각 환경에 종속적인 기능을 브리지로 나누어 구현
    
    ```jsx
    // 구현 계층: 플랫폼 인터페이스 정의
    class Platform {
      send(message) {
        throw new Error("This method should be overridden.");
      }
    }
    
    class AndroidPlatform extends Platform {
      send(message) {
        console.log(`Sending "${message}" to Android device.`);
      }
    }
    
    class iOSPlatform extends Platform {
      send(message) {
        console.log(`Sending "${message}" to iOS device.`);
      }
    }
    
    // 추상화 계층: 알림 타입 정의
    class Notification {
      constructor(platform) {
        this.platform = platform;
      }
    
      send(message) {
        throw new Error("This method should be overridden.");
      }
    }
    
    class EmailNotification extends Notification {
      send(message) {
        console.log("Email Notification:");
        this.platform.send(message);
      }
    }
    
    class SMSNotification extends Notification {
      send(message) {
        console.log("SMS Notification:");
        this.platform.send(message);
      }
    }
    
    // 사용 예시
    const emailNotificationForAndroid = new EmailNotification(new AndroidPlatform());
    const smsNotificationForiOS = new SMSNotification(new iOSPlatform());
    
    emailNotificationForAndroid.send("Hello via Email on Android!");
    smsNotificationForiOS.send("Hello via SMS on iOS!");
    ```
    
- **컴포지트(Composite) 패턴**
    - 객체를 트리 구조로 구성, 부분-전체 계층을 표현하는데 사용
    - 개별 객체와 객체의 모음을 동일하게 처리할 수 있음, 트리 구조의 유연한 관리 가능
    - *e.g.* 파일 시스템에서, 디렉토리와 파일을 계층구조로 표현할 수 있도록 할 수 있음
        - 디렉토리는 다른 디렉토리와 파일을 모두 포함할 수 있음 → 파일과 동일한 동작으로 처리
    
    ```jsx
    // 컴포넌트 인터페이스 정의
    class FileSystemComponent {
      constructor(name) {
        this.name = name;
      }
    
      display(indent = 0) {
        throw new Error("This method should be overridden.");
      }
    }
    
    // 파일 클래스 (Leaf)
    class File extends FileSystemComponent {
      display(indent = 0) {
        console.log(`${' '.repeat(indent)}- File: ${this.name}`);
      }
    }
    
    // 디렉토리 클래스 (Composite)
    class Directory extends FileSystemComponent {
      constructor(name) {
        super(name);
        this.children = [];
      }
    
      add(component) {
        this.children.push(component);
      }
    
      remove(component) {
        this.children = this.children.filter(child => child !== component);
      }
    
      display(indent = 0) {
        console.log(`${' '.repeat(indent)}+ Directory: ${this.name}`);
        this.children.forEach(child => child.display(indent + 2));
      }
    }
    
    // 사용 예시
    const rootDir = new Directory("root");
    const documentsDir = new Directory("Documents");
    const photosDir = new Directory("Photos");
    
    const file1 = new File("resume.pdf");
    const file2 = new File("photo.jpg");
    const file3 = new File("project.docx");
    
    documentsDir.add(file1);
    documentsDir.add(file3);
    photosDir.add(file2);
    
    rootDir.add(documentsDir);
    rootDir.add(photosDir);
    
    // 전체 파일 시스템 출력
    rootDir.display();
    // + Directory: root
    //   + Directory: Documents
    //     - File: resume.pdf
    //     - File: project.docx
    //   + Directory: Photos
    //     - File: photo.jpg
    ```
    

### **행위 패턴 (Behavioral Patterns)**

- **옵저버(Observer) 패턴**
    - 한 객체의 상태가 변경되면 이를 **관찰**하고 있는 다른 객체들에게 자동으로 알림
    - 주로 이벤트 중심 시스템에 사용, 객체들 간의 느슨한 결합을 유지하며 상태 변화를 공유 가능
    - *e.g.* Social media 알림 시스템, **observer**: 알림을 받을 사용자, **subject**: 알림을 보내는 시스템
    
    ```jsx
    // 옵저버 인터페이스
    interface Observer {
      update(message: string): void;
    }
    
    // 주제 클래스 (알림 시스템)
    class NotificationSystem {
      private observers: Observer[] = [];
    
      // 옵저버 추가
      addObserver(observer: Observer) {
        this.observers.push(observer);
      }
    
      // 옵저버 제거
      removeObserver(observer: Observer) {
        this.observers = this.observers.filter((obs) => obs !== observer);
      }
    
      // 변화가 생겼을 때 모든 옵저버들에게 알림 전송
      notifyObservers(message: string) {
        this.observers.forEach((observer) => observer.update(message));
      }
    
      // 예를 들어 새로운 게시글이 올라왔을 때
      newPost(post: string) {
        this.notifyObservers(`새로운 게시글: ${post}`);
      }
    }
    
    // 구체적인 옵저버 클래스 (팔로워)
    class Follower implements Observer {
      constructor(private name: string) {}
    
      update(message: string) {
        console.log(`${this.name}님, 알림: ${message}`);
      }
    }
    
    // 사용 예시
    const notificationSystem = new NotificationSystem();
    
    // 팔로워 추가
    const follower1 = new Follower("Adam");
    const follower2 = new Follower("James");
    
    notificationSystem.addObserver(follower1);
    notificationSystem.addObserver(follower2);
    
    // 새로운 게시글 추가 (알림이 전송됨)
    notificationSystem.newPost("옵저버 패턴을 활용한 알림 시스템");
    // Adam님, 알림: 새로운 게시글: 옵저버 패턴을 활용한 알림 시스템
    // James님, 알림: 새로운 게시글: 옵저버 패턴을 활용한 알림 시스템
    ```
    
- **전략(Strategy) 패턴**
    - 다양한 알고리즘을 정의, **런타임에** 필요에 따라 알고리즘을 선택할 수 있도록 함
    - *e.g.* 정렬 방식(퀵 정렬, 버블 정렬 등)을 전략 패턴으로 구성, 상황에 따라 효율적인 알고리즘 선택
    
    ```jsx
    interface Strategy {
      execute(a: number, b: number): number;
    }
    
    class AddStrategy implements Strategy {
      execute(a: number, b: number): number {
        return a + b;
      }
    }
    
    class MultiplyStrategy implements Strategy {
      execute(a: number, b: number): number {
        return a * b;
      }
    }
    
    class Context {
      private strategy: Strategy;
    
      setStrategy(strategy: Strategy) {
        this.strategy = strategy;
      }
    
      executeStrategy(a: number, b: number) {
        return this.strategy.execute(a, b);
      }
    }
    
    // 사용
    const context = new Context();
    context.setStrategy(new AddStrategy());
    console.log(context.executeStrategy(5, 2)); // 7
    context.setStrategy(new MultiplyStrategy());
    console.log(context.executeStrategy(5, 2)); // 10
    ```
    
- **커맨드(Command) 패턴**
    - 실행할 기능을 요청하는 객체와 실행하는 객체를 분리
    - 요청을 캡슐화하고 실행을 지연하거나 취소할 수 있도록 함
    - *e.g.* 리모컨의 특정 버튼을 눌러서 명령을 TV로 전달함
    
    ```jsx
    // Command 인터페이스
    interface Command {
      execute(): void;
      undo(): void;
    }
    
    // Receiver 클래스 (TV)
    class TV {
      on() {
        console.log("TV가 켜졌습니다.");
      }
    
      off() {
        console.log("TV가 꺼졌습니다.");
      }
    }
    
    // 실제 실행될 명령 클래스들
    class TVOnCommand implements Command {
      constructor(private tv: TV) {}
    
      execute() {
        this.tv.on();
      }
    
      undo() {
        this.tv.off();
      }
    }
    
    class TVOffCommand implements Command {
      constructor(private tv: TV) {}
    
      execute() {
        this.tv.off();
      }
    
      undo() {
        this.tv.on();
      }
    }
    
    // Invoker(호출자) 클래스 (리모컨)
    class RemoteControl {
      private history: Command[] = [];
    
      submit(command: Command) {
        command.execute();
        this.history.push(command);
      }
    
      undo() {
        const command = this.history.pop();
        if (command) {
          command.undo();
        }
      }
    }
    
    // 사용 예시
    const tv = new TV();
    const remoteControl = new RemoteControl();
    
    const tvOnCommand = new TVOnCommand(tv);
    const tvOffCommand = new TVOffCommand(tv);
    
    // TV 켜기
    remoteControl.submit(tvOnCommand); // TV가 켜졌습니다.
    
    // TV 끄기
    remoteControl.submit(tvOffCommand); // TV가 꺼졌습니다.
    
    // 마지막 명령 취소 (TV 켜짐)
    remoteControl.undo(); // TV가 켜졌습니다.
    ```
