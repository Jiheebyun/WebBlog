<details>
  <summary>JavaScript Feature</summary>




## Nullish 

- ?? (Nullish Coalescing Operator)는 null이나 undefined인 경우에만 오른쪽 값을 반환하고, 그 외의 값이면 그대로 왼쪽 값을 반환
- || (Logical OR)는 “falsy” 값이라면(즉 false, 0, '', null, undefined, NaN 등) 오른쪽 값을 반환하고, “truthy” 값이라면 왼쪽 값을 반환

```javascript
const value1 = 0 || 5;    // 0은 falsy이므로, 결과는 5
const value2 = 0 ?? 5;    // 0은 null도 undefined도 아니므로, 결과는 0

console.log(value1); // 5
console.log(value2); // 0

const str1 = '' || '기본값';   // ''는 falsy이므로, 결과는 '기본값'
const str2 = '' ?? '기본값';   // ''는 null도 undefined도 아니므로, 결과는 ''

console.log(str1); // '기본값'
console.log(str2); // ''

let userName = null;
const displayName1 = userName || 'Guest'; // null은 falsy이므로, 'Guest'
const displayName2 = userName ?? 'Guest'; // null이므로, 'Guest'

console.log(displayName1); // 'Guest'
console.log(displayName2); // 'Guest'

```


##  numeric seperators
```javascript

```

```javascript


```

## Map Object vs Object
- Object (일반 객체): 프로그램의 데이터 모델과 행위(메서드) 를 묶는 범용 컨테이너이다.
- Map (ES2015 도입): 키-값 컬렉션만을 빠르고 예측 가능하게 다루기 위한 전용 자료구조이다.
### Object
```javascript

const user = { name: 'Jihee' };

// 추가/갱신
user.age = 30;               // dot notation
user['job'] = 'Frontend';    // bracket notation

// 조회
console.log(user.age);       // 30
console.log('job' in user);  // true

// 삭제
delete user.job;

// 반복
for (const [k, v] of Object.entries(user)) {
  console.log(k, v);
}

```
### Map
```javascript
const userMap = new Map();

// 추가/갱신
userMap.set('name', 'Jihee')
       .set('age', 30);

// 조회
userMap.get('age'); 
userMap.has('name');

// 삭제·비우기
userMap.delete('age');
userMap.clear();

// 반복 (삽입순 유지)
for (const [k, v] of userMap) {
  console.log(k, v);
}

```
### Type

```javascript
const o = {};
const m = new Map();

console.log(typeof o, typeof m);                 // object, object
console.log(o instanceof Map, m instanceof Map); // false, true
console.log(Object.prototype.toString.call(m));  // [object Map]
```



## IIFE: Immediately Invoked Function Expression


</details>