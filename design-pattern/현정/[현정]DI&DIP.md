# DI & DIP

- 의존성주입(DI, Dependency Injection)은 메인 모듈이 직접 하위 모듈에 대한 의존성을 주지 않고, 중간에 의존성 주입자(dependency injector)가 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식
- 메인과 하위 모듈간의 의존성을 느슨하게 만들어 모듈을 쉽게 교체할 수 있는 구조로 만든다.

## 의존한다는 의미

- A가 B에 의존한다
- B가 변하면 A에 영향을 미치게 된다.
- A -> B

```java
import java.util.*;

class B {
    public void go() {
        System.out.println("B의 go()함수"); 
    }
}
class A {
    public void go() { 
        new B().go();  // 의존
    }
}
public class main {
    public static void main(String args[]) {
        new A().go(); 
    }
}
```

### DI 예시

- 적용하지 않은 사례

```java
import java.util.*;

class BackendDeveloper { 
    public void writeJava() {
        System.out.println("자바가 좋아 인터네셔널~"); 
    }
}
class FrontEndDeveloper {
    public void writeJavascript() {
        System.out.println("자바스크립트가 좋아 인터네셔널~"); 
    }
}
public class Project {
    private final BackendDeveloper backendDeveloper; 
    private final FrontEndDeveloper frontEndDeveloper;
    
    public Project (BackendDeveloper backendDeveloper, FrontEndDeveloper frontEndDeveloper) {
        this.backendDeveloper = backendDeveloper;
        this.frontEndDeveloper = frontEndDeveloper; 
    }

    public void implement() { 
        backendDeveloper.writeJava(); 
        frontEndDeveloper.writeJavascript();
    }

    public static void main(String args[]) {
        Project a = new Project (new BackendDeveloper(), new FrontEndDeveloper());
        a.implement();
    }
}
```

Project 클래스는 BackendDeveloper와 FrontEndDeveloper 클래스의 구체적인 인스턴스에 직접적으로 의존하고 있다.
Project가 구체적인 클래스가 아닌 추상화에 의존하도록 설계되어야 하지만, 코드 상에서는 직접적인 클래스 타입에 의존하고 있기 때문에 DIP를 완전히 준수하지 않는다.

- 적용한 사례

```java
import java.util.*;

interface Developer { 
    void develop();
}

class BackendDeveloper implements Developer { 
    @Override
    public void develop() { 
        writeJava();
    }

    public void writeJava() { 
        System.out.println("자바가 좋아~ 새삥새삥");
    } 
}

class FrontendDeveloper implements Developer { 
    @Override
    public void develop() { 
        writeJavascript();
    }

    public void writeJavascript() { 
        System.out.println("자바스크립트가 좋아~ 새삥새삥");
    } 
}

public class Project {
    private final List<Developer> developers;
    
    public Project (List<Developer> developers) { 
        this.developers = developers;
    }

    public void implement() { 
        developers.forEach(Developer::develop);
    }

    public static void main(String args[]) { 
        List<Developer> dev = new ArrayList<>(); 
        dev.add(new BackendDeveloper()); 
        dev.add(new FrontendDeveloper()); 
        
        Project a = new Project(dev); a.implement();
    } 
}
```

Developer라는 dependency injector를 두어 다른 모듈을 추가하거나 쉽게 교체할 수 있는 구조로 변경되었다.
Project에서 BackendDeveloper, FrontEndDeveloper로 향하던 화살표를 모든 모듈이 Developer로 향하게 하면서 의존성을 역전시켰다.

### 의존관계역전원칙

의존성을 주입할 때는 의존관계역전원칙(DIP, Dependency Inversion Principle)이 적용되며 아래 2가지 규칙을 지키는 상태를 의미한다.

- 상위 모듈을 하위 모듈에 의존하면 안되며 둘 다 추상화에 의존해야 한다.
- 추상화는 세부 사항에 의존하면 안되며 세부 사항은 추상화에 따라 달라져야 한다.

### 장점

- 외부에서 모듈을 생성하여 주입하기 때문에 모듈을 쉽게 교체할 수 있는 구조가 된다.
- 단위 테스팅과 마이그레이션이 쉬워진다.
- 의존성 방향이 일관되어 코드를 추론하기 쉬워진다.

### 단점

- 모듈이 더 추가되므로 복잡도가 증가한다.
- 종속성 주입 자체가 런타임때 일어나기 때문에 컴파일 할 때 종속성 주입에 관한 에러를 잡기가 어려울 수 있다.