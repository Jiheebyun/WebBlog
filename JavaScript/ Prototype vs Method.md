<details>
  <summary>프로토타입과 메서드</summary>


### 프로토타입

- 프로토타입은 객체가 다른 객체로부터 속성과 메서드를 상속받을 수 있도록 해준다.
- 프로토타입을 사용하여 여러 객체가 동일한 메서드를 공유할 수 있다.
- 프로토타입은 객체의 상속 구조를 정의하며, 객체가 생성될 때 프로토타입 체인에서 메서드와 속성을 찾는다.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function() {
  console.log(`Hello, my name is ${this.name}`);
};

const person = new Person('Alice');
person.sayHello(); // "Hello, my name is Alice"

```


```javascript
//참고
const c1 = new Person('jihee')
console.log(Object.keys(c1)) // Returns instance members
// {name}
for (let key in c1) console.log(key) // Returns all members (instance + prototype)
// {name, sayHello}

```


- 프로토 타입은 각각의 객체에서 같은 프로토타입을 참조하고 있으므로, 여러 객체에서 하나의 프로토타입에 접근을 한다 



### 메서드 

- 메서드는 객체에 정의된 함수로, 객체의 행동이나 동작을 정의한다.
- 메서드는 객체의 인스턴스와 관련된 특정 작업을 수행하며, 객체의 속성으로서 동작을 한다.
- 메서드는 객체나 클래스 내에서 직접 정의되며, 객체의 기능을 구현한다.

```javascript
const dog = {
  name: 'Rex',
  bark: function() {
    console.log(`${this.name} says woof!`);
  }
};
dog.bark(); // "Rex says woof!"
```


- 즉, 프로토타입을 사용하여 객체의 상속과 메서드 공유를 구현할 수 있고 메서드는 객체의 행동을 정의하고, 프로토타입에 정의된 메서드는 모든 인스턴스에서 공유된다.

</details>