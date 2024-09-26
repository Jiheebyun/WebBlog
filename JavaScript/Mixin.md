<details>
  <summary>Mixin</summary>

  ### Mixin

Mixin은 객체 지향 프로그래밍에서 여러 클래스나 객체의 기닝을 결합하여 새로운 기능을 가진 객체를 생성하는 패턴이다 

```javascript
const canFly = {
    fly() {
        console.log("Flying!");
    }
};

const canSwim = {
    swim() {
        console.log("Swimming!");
    }
};

const duck = Object.assign({}, canFly, canSwim);

duck.fly(); // "Flying!"
duck.swim(); // "Swimming!"
```


```javascript
function Flyable(Base) {
    return class extends Base {
        fly() {
            console.log("Flying!");
        }
    };
}

function Swimmable(Base) {
    return class extends Base {
        swim() {
            console.log("Swimming!");
        }
    };
}

class Animal {}

class Duck extends Flyable(Swimmable(Animal)) {}

const duck = new Duck();
duck.fly(); // "Flying!"
duck.swim(); // "Swimming!"

```




```javascript
function Flyable(Base) {
    return class extends Base {
        fly() {
            console.log("Flying!");
        }
    };
}


```
- 위의 코드는 클래스 상속: class extends Base는 Base 클래스를 상속받아 새로운 클래스를 정의합니다. 이 클래스는 Base의 모든 속성과 메서드를 가지게 된다.
- fly() 메서드는 새로 생성된 클래스에 추가되는 메서드입니다. 따라서 Base 클래스에는 이 메서드가 없더라도, 상속된 클래스에서는 사용할 수 있다.




</details>