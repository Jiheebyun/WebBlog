<details>
  <summary>Proxy</summary>

### Proxy

프락시는 자바스크립트의 객체를 감싸는 기능이다. 프락시는 객체의 기본 동작을 가로채고 수정할 수 있는 메커니즘을 제공한다.
프락시는 new Proxy(target, handler) 구문으로 생성된다. 여기서 target은 감싸고자 하는 원본 객체이고, handler는 동작을 가로채는 메소드를 정의한 객체이다.

프락시는 객체에 접근할 때 추가적인 로직을 삽입할 수 있다. 예를 들어, 속성에 접근할 때 로그를 남기거나, 속성 값을 검증하는 등의 작업을 수행할 수 있다.

### 예시

```javascript

const person = {};
const handler = {
    set: function(target, property, value) {
        if (property === 'age') {
            if (value < 0 || value > 120) {
                throw new RangeError('Age must be between 0 and 120.');
            }
        }
        target[property] = value;
        return true;
    }
};

const proxyPerson = new Proxy(person, handler);

proxyPerson.name = 'Alice'; // 정상
proxyPerson.age = 30; // 정상
// proxyPerson.age = 150; // 오류 발생: RangeError: Age must be between 0 and 120

```

해당 예제에서는 age 속성에 값을 설정 할때 유효성 검사를 하게 된다. 프록시는 데이터를 보호하고, 유효하지 않은 잘못된 값을 
설정할 경우에 오류를 발생 시킨다. 객체안에  

-  위의 코드를 보면 new Proxy를 사용하여 person 객체를 감싸는 proxyPerson이라는 프락시 객체를 생성한다. 이 프락시는 handler를 통해 동작을 제어한다.
- proxyPerson에 name 속성을 'Alice'로 설정한다. 이 경우 set 메소드가 호출되지만, name은 유효성 검사를 필요로 하지 않으므로 정상적으로 속성이 설정된다.
- proxyPerson에 age 속성을 30으로 설정한다. set 메소드가 호출되고, 값이 유효하므로 정상적으로 속성이 설정된다.
- proxyPerson에 age 속성을 150으로 설정하려고 한다. 이 경우 set 메소드에서 유효성 검사가 실패하고, RangeError 오류가 발생한다.


### 프록시 동작 원리 

```javascript
const proxyPerson = new Proxy(person, handler);
```
- 이 코드로 proxyPerson이라는 프락시 객체가 생성된다. 이 프락시는 person 객체를 감싸고, handler 객체에 정의된 동작을 사용할 수 있다.
- proxyPerson은 이제 person 객체를 감싸는 프락시 객체가 된다. 이 객체는 handler에 정의된 동작을 사용할 수 있게 된다.

```javascript
proxyPerson.name = 'Alice';

```
- 이 라인에서 proxyPerson에 name 속성을 'Alice'로 설정하려고 한다. 일반 객체라면 그냥 속성을 직접 설정하지만, 프락시의 경우에는 다르게 동작한다.

### ??? 일반객체랑 프록시 객체인지 어떻게 구별을 해서 다른게 동작을 한다는거야 ??? 

프락시 객체인지 아닌지 구별하는 것은 자바스크립트 엔진의 내부 동작 방식에 의해 진다. 일반 객체와 프락시 객체는 특정 속성이나 메소드로 구별되지는 않지만, 엔진은 프락시의 특별한 프로토콜을 통해 이를 처리한다. 

### 프록시 객체 구별 과정

1. 객체의 유형 
자바스크립트 엔진은 객체에 접근할 때 해당 객체가 기본 객체인지 프락시 객체인지 확인한다. 프락시 객체는 Proxy 생성자 함수로 생성된 객체이므로, 내부적인 타입 체크를 통해 구별된다.

2. 프락시 메소드의 존재

프락시 객체는 기본적으로 handler로 지정된 메소드를 통해 동작한다. set, get, apply와 같은 메소드는 프락시가 작동하는 방식의 핵심이다. 자바스크립트 엔진은 이러한 메소드가 있는지를 확인하고, 해당 객체가 프락시인지 판단한다.

3. 내부 메커니즘

프락시가 설정된 경우, 자바스크립트 엔진은 객체에 대한 기본 동작을 수행하기 전에 해당 객체가 프락시인지 확인한다. 필요에 따라 handler의 메소드를 호출하여 동작을 조정한다. 일반 객체는 이러한 메소드가 없기 때문에 기본 동작이 직접 실행된다.

</details>