<details>
  <summary>form</summary>

### Error

.not는 Jest에서 사용되는 matcher의 하나로 특정 조건이 발생하지 않기를 기대할때 사용한다
.toThrow는 특정 함수가 호출될 때 오류를 발생시키는지를 확인하는 데 사용된다



```javascript
it("should throw an error if no value is passed into the function", () => {
    const resultFn = () => {
        add();
    };
 
    expect(resultFn).toThrow();
})

```
#### resultFn
- resultFn은 return하는 값을 없지만, add()를 호출을 통해서 오류를 발생시키는지 테스트하기 위해 존재한다
- 즉, 함수의 경과를 사용할 필요가 없고, 단지 오류가 발생하지 않는지 확인하기 위한 것이다. 

##### .not.toThrow() & .toThrow()

- expect(resultFn).not.toThrow() -> resultFn이 오류를 던지지않는것을 예상, 오류를 던지지않으면 통과
- expect(resultFn).thThrow() -> resultFn이 오류를 던지는것을 예상, 오류를 던지면 통과



</details>