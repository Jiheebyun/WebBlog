<details>
  <summary>More Key Testing Library APIs</summary>


### toBe() and toEqual()


```javascript

describe('',() => {
    it('should return an array of number values if an array of string number values is provided',() => {
        const numberValues = ['1', '2'];

        const cleanedNumbers = cleanNumbers(numberValues);

        // expect(cleanedNumbers).toBe([1,2])
        //결과 값은 [1,2] , [1,2] 보기엔 같지만, 자스크립트에서는 참조타입이기 때문에 다르게 취급한다. 이럴때는 toEqual()을 사용하여 값이 같은지 비교해야한다
        //The result is [1, 2] and [1, 2]. Although they appear the same, in JavaScript, they are treated differently because they are reference types. In this case, you should use toEqual() to compare their values.

        expect(cleanedNumbers).toEqual([1,2])
    })
})

```

</details>