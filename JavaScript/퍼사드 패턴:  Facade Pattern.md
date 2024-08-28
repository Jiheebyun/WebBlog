<details>
  <summary>퍼사트 패턴: Facade Pattern</summary>


### 퍼사드 패턴이란?
퍼사드 패턴(Facade Pattern)은 객체 지향 디자인 패턴 중 하나로, 복잡한 시스템이나 서브시스템에 대해 간단한 인터페이스를 제공하여 클라이언트가 시스템을 더 쉽게 사용할 수 있도록 하는 패턴이다. 


#### 다음과 같은 상황에서 사용

1. 복잡한 서브시스템의 통합: 다양한 클래스나 서브시스템이 서로 상호작용하는 복잡한 구조를 간소화하기 위해 사용된다.
2. 시스템의 간단한 접근 제공: 클라이언트가 복잡한 내부 구조를 알 필요 없이 단순한 인터페이스를 통해 시스템의 기능을 사용할 수 있도록 한다.


#### 퍼사드 패턴 구성요소 

- 퍼사드(Facade) 클래스: 클라이언트가 상호작용하는 간단한 인터페이스를 제공하는 클래스이다. 이 클래스는 복잡한 서브시스템의 기능을 사용하기 위해 여러 서브시스템 클래스를 조합하여 통합된 메서드를 제공한다.
- 서브시스템(Subsystem) 클래스: 실제로 복잡한 작업을 수행하는 클래스들이다. 퍼사드 클래스는 이들 서브시스템의 메서드를 호출하여 클라이언트가 원하는 작업을 수행한다.


```javascript
// 서브시스템 클래스 1
class Subsystem1 {
  operation1() {
    console.log("Subsystem1: Operation 1");
  }
}

// 서브시스템 클래스 2
class Subsystem2 {
  operation2() {
    console.log("Subsystem2: Operation 2");
  }
}

// 서브시스템 클래스 3
class Subsystem3 {
  operation3() {
    console.log("Subsystem3: Operation 3");
  }
}

// 퍼사드 클래스
class Facade {
  constructor() {
    this.subsystem1 = new Subsystem1();
    this.subsystem2 = new Subsystem2();
    this.subsystem3 = new Subsystem3();
  }

  simpleOperation() {
    console.log("Facade: Simple Operation");
    this.subsystem1.operation1();
    this.subsystem2.operation2();
    this.subsystem3.operation3();
  }
}

// 클라이언트 코드
const facade = new Facade();
facade.simpleOperation();
// 출력:
// Facade: Simple Operation
// Subsystem1: Operation 1
// Subsystem2: Operation 2
// Subsystem3: Operation 3



```




</details>