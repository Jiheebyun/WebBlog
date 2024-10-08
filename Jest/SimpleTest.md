<details>
  <summary>Practice Simple Test</summary>


####  해당 함수에 대한 test 코드 작성
```javascript
export function transformToNumber(value) {
  return +value;
}
```


#### Test
```javascript

import { it, expect } from 'vitest';
import { transformToNumber } from './numbers.js';

it('should be a number type', () => {
    const num = '123'

    const result = typeof(transformToNumber('123'))

    expect(result).toBe('number')
})

it("should be a number type", () => {
    const input = '1';

    const result = transformToNumber(input);

    expect(result).toBeTypeOf('number')
})

it("should yield NaN for non-transformable values", () => {
    const input = 'invalid';

    const result = transformToNumber(input);

    expect(result).toBeNaN()
})

```

</details>