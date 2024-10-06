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


</details>