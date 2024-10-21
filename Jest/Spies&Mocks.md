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
모의 객체는 실제 객체의 동작을 시뮬레이션하는 테스트용 객체이다. 테스트를 할 때, 실제 객체를 사용하면 외부 의존성(예: 파일 시스템, 데이터베이스 등)에 영향을 받을 수 있기때문에 이를 피하기 위해 모의 객체를 사용한다.

##### vi.mock('fs')의 역할

- 모듈 모킹: vi.mock('fs')를 호출하면, Node.js의 fs 모듈을 모의 객체로 변환한다. 이제 테스트 코드에서 fs 모듈의 메서드를 호출해도 실제 파일 시스템에 접근하지 않게 된다.
- 메서드 스텁(stub): 모의 객체는 기본적으로 모든 메서드가 빈 함수로 설정되고, 따라서 fs.writeFile 같은 메서드를 호출해도 실제로 파일에 쓰지 않지만, 호출 여부를 기록할 수 있다.
- 상태 관리: 모의 객체는 호출 횟수, 호출된 인수, 반환 값 등을 추적할 수 있는 기능을 제공한다. 이를 통해 테스트가 어떻게 작동하는지, 원하는 대로 동작하는지를 확인할 수 있다.


</details>