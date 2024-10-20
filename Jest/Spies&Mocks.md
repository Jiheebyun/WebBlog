<details>
  <summary>Spies & Mocks</summary>

#### Spies

- Spy는 기존의 함수를 감시하고, 해당 함수가 어떻게 호출되었는지를 기록하는 특별한 도구이다. Spy는 함수의 구현을 수정하지 않으면서도 호출된 인자, 호출 횟수, 반환값 등을 추적한다.


##### 호출 여부 확인
```javascript

const originalFunction = jest.fn();
originalFunction(); // 함수 호출

expect(originalFunction).toHaveBeenCalled(); // 호출 여부 확인
//Spy를 사용하면 특정 함수가 호출되었는지 여부를 검증

```

##### 호출인자 검증
```javascript
const originalFunction = jest.fn();
originalFunction(1, 2);

expect(originalFunction).toHaveBeenCalledWith(1, 2); // 특정 인자로 호출됐는지 확인

//Spy를 사용하여 함수가 어떤 인자로 호출되었는지를 확인

```

##### 호출횟수 확인
```javascript
const originalFunction = jest.fn();
originalFunction();
originalFunction();

expect(originalFunction).toHaveBeenCalledTimes(2); // 호출 횟수 확인
//Spy를 통해 함수가 몇 번 호출되었는지를 추적

```

##### 반환값 확인
```javascript
const sum = (a, b) => a + b;
const spySum = jest.spyOn({ sum }, 'sum').mockImplementation(sum);

expect(spySum(1, 2)).toBe(3); // 원래 함수의 동작 확인

//원래 함수의 반환값도 확인


#### Mocks

```

</details>