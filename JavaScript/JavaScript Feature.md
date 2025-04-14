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


##  
```javascript

```

```javascript


```




</details>