<details>
  <summary>Private members</summary>


  ### Using Symbol

JavaScript의 Symbol은 ES6에서 도입된 원시 데이터 타입으로, 고유하고 변경 불가능한 값을 생성하는 데 사용된다. Symbol은 주로 객체의 프로퍼티 키로 사용되며, 프로퍼티의 이름이 충돌하지 않도록 보장한다. 이로 인해 같은 이름의 프로퍼티가 다른 객체에 존재하더라도 서로 영향을 주지 않는다.

#### 예시

```javascript

const _radius = Symbol();
const _draw = Sysbol();

class Circle {
    constructor(radius) {
        this[_radius] = radius;
    }

    [_draw]() {

    }
}
const c = new Circle(1);
console(c[key])
//Circle {Sysbol():1}
    //Sysbol():1
    //__proto__:
        //constructor: class Circle
        //Symbol(): f ()
        //__proto__: Object
```
- 각각의 Symbol은 고유하며, 같은 설명을 가진 심볼도 다른 심볼과는 다르다
 ex> console.log(Symbol() === Symbol())  // false

- Symbol은 객체의 프로퍼티 키로 사용될 수 있어, 일반적인 문자열 키와는 달리 다른 프로퍼티와 충돌하지 않는다.
- Symbol로 정의된 프로퍼티는 for ...in 루프나 Object.key()메서드로 열거되지 않아 숨겨진 프로퍼티를 만들수 있다.

  ### Using WakMaps



```javascript

```
</details>