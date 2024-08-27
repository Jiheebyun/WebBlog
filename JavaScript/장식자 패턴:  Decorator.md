<details>
  <summary>장식자 패턴: Decorator Pattern</summary>

### 장식자 패턴이란?

장식자 패턴(Decorator Pattern)은 객체의 기능을 동적으로 추가하거나 변경할 수 있는 디자인 패턴입니다. 이 패턴은 기존의 객체에 새로운 기능을 추가하면서도 객체의 구조를 변경하지 않고 유지할 수 있도록 해준다

### 장식자 패턴의 기본 개념

1. 컴포넌트(객체) 인터페이스: 장식자를 적용할 객체의 기본 인터페이스를 정의한다.
2. 구체적인 컴포넌트: 실제로 기능을 구현한 객체
3. 장식자(Decorator): 기능을 추가하거나 변경할 수 있는 클래스 또는 함수이다. 기본 컴포넌트를 래핑(wrapping)하여 새로운 기능을 추가한다


```javascript

// 클래스 정의
class Beverage {
  cost() {
    return 5; // 기본 음료의 가격
  }
}

// 변수에 클래스의 인스턴스를 할당
const myDrink = new Beverage(); // myDrink는 Beverage 클래스의 인스턴스

// 장식자 클래스 정의
class BeverageDecorator {
  constructor(drink) {
    this._drink = drink; // Beverage 인스턴스를 저장
  }

  cost() {
    return this._drink.cost(); // 저장된 Beverage 인스턴스의 cost() 메서드를 호출
  }
}

// 우유 장식자 정의
class MilkDecorator extends BeverageDecorator {
  cost() {
    return super.cost() + 2; // 우유 추가 비용
  }
}

// 사용 예시
const milkDrink = new MilkDecorator(myDrink);
console.log(milkDrink.cost()); // 우유가 추가된 음료의 가격: 7

```


### 자 그럼 여기서 드는 의문!, 그냥 생성한 클래서에서 상속을 하면 되지 굳히 장식자를 만들어서 써야되?

### 장식자 패턴
장식자 패턴은 객체의 기능을 동적으로 추가할 때 사용된다. 이 패턴은 기존 객체의 구조를 변경하지 않고도 새로운 기능을 덧붙일 수 있다. 장식자는 기본 객체를 래핑하여 추가적인 기능을 제공한다.

#### 장점
유연성: 기능을 런타임에 동적으로 추가하거나 수정할 수 있다. 기존 객체를 변경하지 않고도 새로운 기능을 덧붙일 수 있다.
조합 가능성: 여러 장식자를 조합하여 다양한 기능을 추가할 수 있다. 예를 들어, 우유와 설탕을 동시에 추가할 수 있다.
코드 재사용: 동일한 기본 객체를 사용하여 다양한 장식자를 적용할 수 있다. 장식자는 기본 객체의 기능을 재사용하면서 추가적인 기능을 더할 수 있다.
단일 책임 원칙: 각 장식자는 하나의 책임만 가지고 있어, 코드의 유지보수가 용이하다.
#### 단점
복잡성: 많은 장식자를 조합하면 객체 구조가 복잡해질 수 있다. 복잡한 장식자 체인은 이해하기 어려울 수 있다.
성능: 여러 장식자를 사용할 경우, 메서드 호출이 중첩되어 성능이 저하될 수 있다.
### 상속
상속은 기본 클래스를 확장하여 새로운 기능이나 속성을 추가하는 방법이다. 상속을 사용하면 기본 클래스의 기능을 그대로 가져와서 추가적인 기능을 구현할 수 있다.

#### 장점
간단함: 기본 클래스에서 새로운 클래스를 확장하여 기능을 추가하는 것이 직관적이고 간단하다.
명확한 관계: 상속 구조는 클래스 간의 관계를 명확하게 나타낸다. 기본 클래스와 자식 클래스 간의 관계가 직관적이다.

#### 단점
강한 결합: 자식 클래스는 부모 클래스에 강하게 결합되어 있어, 부모 클래스의 변경이 자식 클래스에 영향을 미칠 수 있다.
상속 계층의 복잡성: 깊은 상속 계층은 코드의 복잡성을 증가시키고, 유지보수에 어려움을 줄 수 있다.
다중 상속 문제: 자바스크립트는 다중 상속을 지원하지 않으며, 복잡한 상속 구조를 사용하는 것은 문제를 일으킬 수 있다.
선택의 기준
- 장식자 패턴을 사용할 때:

1. 기능을 동적으로 추가하거나 수정하고 싶을 때.
2. 여러 기능을 조합하여 사용할 때.
3. 객체의 구조를 변경하지 않고도 기능을 확장하고 싶을 때.
- 상속을 사용할 때:

1. 기본 클래스의 기능을 확장하거나 변경하고 싶을 때.
2. 클래스 간의 명확한 상속 관계를 표현하고 싶을 때.
3. 상속 계층이 간단하고, 클래스 간의 관계가 명확할 때.


### 장식자 패턴 사용
```javascript
// 기본 음료 클래스
class Drink {
  cost() {
    return 5; // 기본 음료의 가격
  }

  description() {
    return "Basic Drink"; // 기본 음료의 설명
  }
}

// 장식자 기본 클래스
class DrinkDecorator {
  constructor(drink) {
    this._drink = drink; // Drink 인스턴스를 저장
  }

  cost() {
    return this._drink.cost(); // 기본 음료의 cost() 메서드를 호출
  }

  description() {
    return this._drink.description(); // 기본 음료의 description() 메서드를 호출
  }
}

// 우유 장식자
class MilkDecorator extends DrinkDecorator {
  cost() {
    return super.cost() + 2; // 기본 음료 가격에 우유 추가 비용을 더함
  }

  description() {
    return `${super.description()} with Milk`; // 기본 설명에 우유 추가
  }
}

// 설탕 장식자
class SugarDecorator extends DrinkDecorator {
  cost() {
    return super.cost() + 1; // 기본 음료 가격에 설탕 추가 비용을 더함
  }

  description() {
    return `${super.description()} with Sugar`; // 기본 설명에 설탕 추가
  }
}

// 사용 예시
const myDrink = new Drink(); // 기본 음료 객체 생성
const milkDrink = new MilkDecorator(myDrink); // 우유를 추가한 음료
const sugarMilkDrink = new SugarDecorator(milkDrink); // 설탕과 우유가 추가된 음료

console.log(sugarMilkDrink.cost()); // 8 (기본 가격 5 + 우유 2 + 설탕 1)
console.log(sugarMilkDrink.description()); // "Basic Drink with Milk with Sugar"
```

### 상속
```javascript
// 기본 음료 클래스
class Drink {
  cost() {
    return 5; // 기본 음료의 가격
  }

  description() {
    return "Basic Drink"; // 기본 음료의 설명
  }
}

// 우유가 추가된 음료 클래스
class MilkDrink extends Drink {
  cost() {
    return super.cost() + 2; // 기본 음료 가격에 우유 추가 비용을 더함
  }

  description() {
    return `${super.description()} with Milk`; // 기본 설명에 우유 추가
  }
}

// 설탕이 추가된 음료 클래스
class SugarMilkDrink extends MilkDrink {
  cost() {
    return super.cost() + 1; // 우유가 추가된 음료 가격에 설탕 추가 비용을 더함
  }

  description() {
    return `${super.description()} with Sugar`; // 우유가 추가된 설명에 설탕 추가
  }
}

// 사용 예시
const sugarMilkDrink = new SugarMilkDrink(); // 설탕과 우유가 추가된 음료

console.log(sugarMilkDrink.cost()); // 8 (기본 가격 5 + 우유 2 + 설탕 1)
console.log(sugarMilkDrink.description()); // "Basic Drink with Milk with Sugar"
```

</details>
