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
### async test



#### test example
```javascript
export function generateToken(userEmail, doneFn){
  jwt.sign({email: userEmail}, 'sevret123', doneFn)
}

export function  generateTokenPromise(userEmail) {
  const promise = new Promise(( resolve, reject ) => {
      jwt.sign({email: userEmail}, 'sevret123', (error, token) => {
        if (error) {
          reject(error)
        } else {
          resolve(token)
        }
      } )
  })
}

```

#### test
```javascript
import { it } from 'vitest';

it('should generate a token value', (done) => {
  const testUserEmail = 'test@test.com';
  
  generateToken(testUserEamil, (err, token) => {
    expect(token).toBeDefined();
    done();
  })
})

```

- 여기서 done()은 테스트를 수행할 콜백 함수를 인자로 받는다. 비동기 테스트가 끝났을을 알리는 콜백 할수이다
- toBedefined()는 매처 중 하나로, 주어진 값이 절의 되어 있는지 확인하는데 사용된다. 즉, 검사할 변수가 undefined가 아닌지 체크하고, 변수에 값이 할당되었거나, 함수의 반환값이 있을경우 매처를 통과한다 반대로, value가 null, 숫자, 문자열, 객체 등 어떤 값이든 undefined가 아니면 테스트는 통과하게 된다 
- done 함수는 Jest나 Vitest와 같은 테스트 프레임워크에서 제공되는 기능이지만, 직접적으로 정의되지는 않는다. 대신, 테스트 함수의 두 번째 인자로 자동으로 제공되는 콜백이다.


#### async test : if a return value is existed


```javascript

import { it } from 'vitest';

it('should generate a token value', (done) => {
  const testUserEmail = 'test@test.com';
  
  generateToken(testUserEamil, (err, token) => {
    
    try {
      expect(token).toBeDefined();
      //or
      expect(token).toBe(2);
      done();
    } catch (err) {
      done();
    }
    
  })
})

```

1. toBe()
- 특정한 값과 정확히 일치하는지 검사한다.
- 사용 예: 비동기 함수가 예상하는 특정 값을 반환할 때 유용하다.
- 예를 들어, generateToken 함수가 사용자 이메일에 대해 특정한 토큰 값을 생성해야 한다고 가정하자. 이 경우, 생성된 토큰이 예상하는 값인지 확인하려면 toBe를 사용할 수 있다.

2. toBeDefined()
- 값이 undefined가 아닌지 확인 확인한다.
- 사용 예: 비동기 함수가 정상적으로 값을 반환했는지 여부를 확인하고 싶을 때 유용하다. 즉, 반환된 값이 존재해야 할 때 사용한다.
- 사용 상황: 예를 들어, 사용자의 이메일로부터 생성된 토큰이 반드시 존재해야 한다면 toBeDefined를 사용하여 값이 정의되어 있는지 체크할 수 있다.


#### Promise

```javascript

import { it } from 'vitest';

it('should generate a token value', () => {
  const testUserEmail = 'test@test.com';
  
  return expect(generateTokenPromise(testUserEmail)).resolves.toBeFefined();
  // Don't need to return when using async/await (since a function annotated with async returns a promise implicitly)
})

it('should generate a token value',  async () => {
  const testUserEmail = 'test@test.com';
  const token = await generateTokenPromise(testUserEmail);
  
  expect(token).toBeFefined();
})


```

</details>