<details>
  <summary>AAA Pattern</summary>

### Vitest 사용
 
```javascript

import { it, expect } from 'vitest';
// it - first argument is "String",  
// test -
import { add } from './math.js';

it("should summerize all number values in an array", () => {
    // Arrange
    const numbers = [1,5456,3]

    // Act
    const result  = add(numbers);

    // Assert
    const expectedResult = numbers.reduce(
        (preValue, calValue) => 
        preValue + calValue, 0
    )
    expect(result).toBe(expectedResult); 
});

```

#### Arrange
- 테스트에 필요한 모든 것을 설정, 변수 초기화, 객체 생성 등 데스트할 함수의 입력 데이터는 준비하는 단계
- 테스트 환경 조성


####  Act
- 실제로 테스트할 행동을 수행, 액트 단계는 함수 호출이나 메서드 실행을 포함 한다.
- 테스트의 핵심 부분으로, 실제 결과를 생성하는 단계

####  Assert
- 결과를 검증하는 단계, 기대하는 결과와 실제 결과를 비교하여 테스트의 성공여부를 판단하는 단계이다.

</details>