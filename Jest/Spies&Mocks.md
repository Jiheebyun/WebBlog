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

##### ex
```javascript

import { describe, it, expect, vi } from 'vitest';

describe('generateReportData', () => {

    it('should execute logFn if provided', () => {
        const logger = vi.fn(); // 빈 function을 만들어  

        generateReportData(logger);
        //generateReportData함수는 인자로 함수를 받아 안에서 실행하고 있다.
        //generateReportData함수가 인자를 실행이 되었는지 안되었는지 확인 할수 있다.

        expect(logger).toBeCalled();
    })

})
```

##### 반환값 확인
```javascript
const sum = (a, b) => a + b;
const spySum = jest.spyOn({ sum }, 'sum').mockImplementation(sum);

expect(spySum(1, 2)).toBe(3); // 원래 함수의 동작 확인

//원래 함수의 반환값도 확인

```

#### Mocks

Mock은 기존 함수의 동작을 완전히 대체하는 가짜 함수이다. Mock 함수는 호출된 횟수, 인자 등을 추적할 뿐만 아니라, 특정 값을 반환하도록 설정할 수 있다.


```javascript
import { it, expect,vi } from 'vitest';

import writeData from './io';
import { promises as fs } from 'fs';

vi.mock('fs')

it('should excute the writeFile method',()=>{
    const testData = 'Test';
    const testFilename = 'test.txt';

    // writeData를 호출 (이 안에서 fs.writeFile을 사용한다고 가정)  
    writeData(testData,testFilename)

    // fs.writeFile이 호출되었는지 확인
    expect(fs.writeFile).toBeCalled();
})

```
##### 모의 객체(mock object)
Mocking은 테스트를 독립시키기 위해 의존성을 개발자가 컨트롤하고 검사할 수 있는 오브젝트로 변환하는 테크닉입이다. 테스트를 할 때, 실제 객체를 사용하면 외부 의존성(예: 파일 시스템, 데이터베이스 등)에 영향을 받을 수 있기때문에 이를 피하기 위해 모의 객체를 사용한다.
** 참고:  mock은 호이스팅이 된다 **
여기서 “오브젝트로 변환한다”는 말은 Mocking을 통해 실제 의존성을 대체하는 가짜 객체를 생성한다는 의미이다. 구체적으로 설명하자면:

##### 실제 의존성 vs. Mock 객체
실제 의존성: 예를 들어, 데이터베이스나 외부 API와 같은 실제 객체는 코드가 실행될 때 실제 데이터를 가져오거나 특정 동작을 수행한다. 이런 객체들은 실제 상태나 동작에 의존하므로, 테스트를 복잡하게 만들거나 불안정하게 만들 수 있다.
Mock 객체: Mocking을 통해 개발자는 이러한 실제 의존성의 행동을 시뮬레이션할 수 있는 객체를 만든다. 이 Mock 객체는 실제 의존성과 같은 인터페이스를 가지고 있지만, 실제 데이터베이스나 API와 연결되어 있지 않고, 대신 사전에 정의된 방식으로 동작한다.

##### vi.mock('fs')의 역할

- 모듈 모킹: vi.mock('fs')를 호출하면, Node.js의 fs 모듈을 모의 객체로 변환한다. 이제 테스트 코드에서 fs 모듈의 메서드를 호출해도 실제 파일 시스템에 접근하지 않게 된다.
- 상태 관리: 모의 객체는 호출 횟수, 호출된 인수, 반환 값 등을 추적할 수 있는 기능을 제공한다. 이를 통해 테스트가 어떻게 작동하는지, 원하는 대로 동작하는지를 확인할 수 있다.

##### 참고 : 메서드 스텁과 모킹의 차이 
- 메서드 스텁(stub): 모의 객체는 기본적으로 모든 메서드가 빈 함수로 설정되고, 따라서 fs.writeFile 같은 메서드를 호출해도 실제로 파일에 쓰지 않지만, 호출 여부를 기록할 수 있다.
모의 객체는 호출된 정보와 결과를 추적하는 데 중점을 두고, 스텁은 특정 값을 반환하는 데 초점을 맞춘다는 점




</details>


















<details>
  <summary>Custom Mocking Logic</summary>

  ##### Mocking Logic


##### EX 
```javascript
//io.js
import path from 'path';
import { promises as fs } from 'fs';

export default function writeData(data, filename) {
  const storagePath = path.join(process.cwd(), 'data', filename);
  return fs.writeFile(storagePath, data);
}
```

```javascript
//Test Code


import { it, expect,vi } from 'vitest';

import writeData from './io';
import { promises as fs } from 'fs';


vi.mock('fs')
vi.mock('path', () => {
    //own implementation
    return {
        default: {
            join: (...args) => {
                return args[args.length -1]
            }
        }
    }
});
// 이 부분은 path 모듈을 모킹하는 코드이다. 여기서 path.join 메서드를 사용자 정의 구현으로 대체합니다.
// join 메서드는 일반적으로 여러 경로를 결합하는 데 사용되지만, 이 모킹에서는 모든 인수 중 마지막 인수만 반환하도록 구현되었다. 즉, path.join('a', 'b', 'c')를 호출하면 'c'만 반환된다.
//모킹된 구현에서 마지막 인수만 반환하면, 이 경우 filename만 반환된다. 예를 들어 path.join(process.cwd(), 'data', 'test.txt')는 'test.txt'를 반환하게 된.
// 이렇게 하는 이유는 테스트 환경에서 경로 처리를 간단하게 하여 테스트의 의도를 명확하게 하고, 복잡한 경로 결합 로직으로 인한 오류를 피하기 위함이다.
//참고 :  default라는 이름은 path 모듈을 default방식으로 임포트했기 때문에 default라고 명시되었다.
// 만약 path as abc 라고 모듈을 임포트 했다면, 모킬을 리털할때, {abc: ...} 모킹을 내보내야한다.
it('should excute the writeFile method',()=>{
    const testData = 'Test';
    const testFilename = 'test.txt';


    writeData(testData,testFilename)

    expect(fs.writeFile).toBeCalledWith(testFilename, testData)
})
```


</details>




<details>
  <summary>__mocks__</summary>

Jest에서 __mocks__ 디렉토리는 모듈의 자동 또는 수동으로 설정한 모의(mock) 구현을 제공하는 곳이다. Jest는 테스트를 수행할 때 종속성을 격리하고, 특정 모듈의 동작을 제어하기 위해 모의 기능을 많이 사용한다. __mocks__ 디렉토리를 사용하면 보다 쉽게 모듈을 모의할 수 있다.

```javascript
// src/myModule.js
export function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("real data");
    }, 1000);
  });
}

// src/__mocks__/myModule.js
export function fetchData() {
  return Promise.resolve("mocked data");
}

// src/myComponent.js
import { fetchData } from './myModule';

export async function getData() {
  const data = await fetchData();
  return data;
}

```

##### Test Code
```javascript
// src/myComponent.test.js
jest.mock('./myModule'); // myModule을 모킹

import { getData } from './myComponent';
import * as myModule from './myModule';

test('getData should return mocked data', async () => {
  const data = await getData(); // 비동기 함수 호출
  expect(data).toBe('mocked data'); // 모의 데이터 확인
  expect(myModule.fetchData).toHaveBeenCalled(); // 호출 여부 확인
});

```

</details>