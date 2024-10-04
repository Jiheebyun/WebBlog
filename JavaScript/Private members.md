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

### Using WeakMaps

WeakMap은 JavaScript에서 제공하는 특별한 종류의 맵이다. 이 맵은 객체를 키로 사용하며, 그 키에 대한 참조가 약한 형태로 유지된다. 즉, WeakMap에 저장된 키가 더 이상 사용되지 않으면, 가비지 컬렉터에 의해 자동으로 메모리에서 제거될 수 있다. 이로 인해 메모리 누수를 방지하는 데 유용하다.

#### 특징
1. 키와값 : 
- WeakMap의 키는 반드시 객체여야 하며, 기본 데이터 타입(문자열, 숫자 등)은 키로 사용할 수 없다.
- 값은 어떤 타입도 가능하지만, 키와 값 간의 관계는 강한 참조가 아니다.
2. 약한 참조:
- WeakMap의 키에 대한 참조는 약하므로, 다른 코드에서 해당 키의 참조를 모두 제거하면 해당 키-값 쌍은 자동으로 제거된다. 이는 메모리 관리를 효율적으로 해준다.
3. 메소드:
WeakMap은 다음과 같은 메소드를 제공한다:
- set(key, value): 키와 값을 추가한다.
- get(key): 주어진 키에 해당하는 값을 반환한다. 키가 존재하지 않으면 undefined를 반환한다.
- has(key): 주어진 키가 존재하는지 여부를 반환한다.
- delete(key): 주어진 키와 해당 값을 제거한다. 키가 존재하지 않으면 false를 반환한다.
4. 반복 불가:
- WeakMap은 이터러블(iterable)하지 않기 때문에, forEach나 for...of와 같은 반복문으로 순회할 수 없다. 이는 내부적으로 저장된 데이터의 구조를 보호하기 위한 설계이다.

```javascript
const weakMap = new WeakMap();

let obj1 = {};
let obj2 = {};

weakMap.set(obj1, 'value1');
weakMap.set(obj2, 'value2');

console.log(weakMap.get(obj1)); // 'value1'
console.log(weakMap.has(obj2)); // true

obj1 = null; // obj1에 대한 참조를 제거

// obj1에 대한 WeakMap의 키는 이제 가비지 컬렉션의 대상이 된다.


```

#### 그럼 언제 사용하는데?

- 캡슐화: 객체의 내부 상태를 보호하고 외부에서 접근하지 못하도록 할 때 유용하다.
- 메모리 관리: 메모리 누수를 방지하고 싶을 때, 특히 DOM 요소와 같은 짧은 수명을 가진 객체를 다룰 때 적합하다.
</details>