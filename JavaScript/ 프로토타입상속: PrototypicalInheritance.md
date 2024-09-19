<details>
  <summary>프로토타입 상속: Prototypical Inheritance</summary>


### 프로토타입 상속 

- 객체 간의 상속을 구현 하는 기법중 하나 

```javascript

function Shape(){}

Shape.prototype.duplicate = function() {
    console.log('duplicate')
}

function Circle(radius) {
    this.radius = radius
}

// 이 과정은 circle의 인스턴스가 Shape의 메서드에 접근 할수 있도록 한다
Circle.prototype = Object.create(Shape.prototype);

Circle.prototype.draw = function() {
    console.log('draw')
}

const s = new Shape();
const c = new Circle(1);

c.duplicate(); // 'duplicate' 출력
c.draw();      // 'draw' 출력

```

- s.draw()를 호출할 수 없다.
- c 는 Shape를 상속하지만, s는 Circle의 메서드에 접근할수 없다.

### 사용 될수 있는 상황 
1. 코드 재사용
- 공통된 속성과 메서드를 여러 객체에서 공유할 수 있다. 이를 통해 중복 코드를 줄이고 유지보수를 쉽게 한다.
- 예: 여러 도형(원, 사각형 등)에서 공통적인 메서드(예: draw, area 등)를 정의할 때.
2. 객체 지향 프로그래밍
- JavaScript에서 객체 지향 프로그래밍(OOP) 패러다임을 구현할 수 있게 한다. 객체 간의 관계를 설정하고, 클래스처럼 행동할 수 있다.
- 예: 여러 종류의 사용자(User, Admin 등)를 정의할 때, 기본 사용자 클래스를 상속하여 특정 기능을 추가할 수 있다.
3. 유연한 구조
- 상속을 통해 객체의 구조를 유연하게 만들 수 있다. 새로운 기능이 필요할 때 기존 객체를 수정하지 않고 새로운 객체를 추가할 수 있다.
- 예: 게임 개발에서 기본 캐릭터 클래스를 만들고, 이를 상속받아 다양한 캐릭터를 구현할 때.
4. 상속 계층 구축
- 복잡한 상속 구조를 통해 객체의 계층을 만들 수 있다. 이를 통해 특정 기능을 가진 객체들을 그룹화하고 관리할 수 있다.
- 예: 동물 클래스를 만들고, 이를 상속하여 포유류, 조류 등을 정의할 수 있다.
5. 플러그인 시스템
- 상속을 통해 기능을 확장할 수 있는 플러그인 시스템을 구현할 수 있다.
- 예: 기본 웹 애플리케이션을 만들고, 다양한 플러그인(예: 차트, 데이터 시각화)을 상속받아 추가할 수 있다.
</details>