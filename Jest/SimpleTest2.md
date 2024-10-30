<details>
  <summary>Simple test2</summary>


##### http error sample
```javascript
export class HttpError {
  constructor(statusCode, message, data) {
    this.statusCode = statusCode;
    this.message = message;
    this.data = data;
  }
}

export class ValidationError {
  constructor(message) {
    this.message = message;
  }
}

```

##### Test Code 
```javascript
import { it, expect, describe } from 'vitest';
import { HttpError, ValidationError } from './errors';


describe('class HttpError', () => {
    it('should contain the provide status code, message and data', () => {
        const testStatus = 1;
        const testMessage = 'test';
        const testData = {key:'test'};

        const testError = new HttpError(testStatus,testMessage, testData);

        expect(testError.statusCode).toBe(testStatus);
        expect(testError.message).toBe(testMessage);
        expect(testError.data).toBe(testData);
    })

    it('should contain undefined as data if no dada is provided', () => {
        const testStatus = 1;
        const testMessage = 'test';

        const testError = new HttpError(testStatus,testMessage);

        expect(testError.statusCode).toBe(testStatus);
        expect(testError.message).toBe(testMessage);
        expect(testError.data).toBeUndefined();
    })
})

describe('class ValidationError', () => {
    it('should contain the provide message', () => {
        const testMessage = 'test';

        const testValidationError = new ValidationError(testMessage);

        expect(testValidationError.message).toBe(testMessage)
    })

    it('should contain the provide message', () => {
        const testMessage = 'test';

        const testValidationError = new ValidationError(testMessage);

        expect(testValidationError.message).toBe(testMessage)
    })
    
    it('should ', () => {

    })
})
```



##### Sample Code 
```javascript
import { ValidationError } from './errors.js';

export function validateNotEmpty(text, errorMessage) {
  if (!text || text.trim().length === 0) {
    throw new ValidationError(errorMessage);
  }
}
```

```javascript
import { it, expect } from 'vitest';
import { validateNotEmpty } from './validation';



it('should throw an error if an an empty string is provided as a value', () => {
    const testInput = '';
    const validationFn = () => validateNotEmpty(testInput);

    expect(validationFn).toThrow();
})


it('should throw an error if an an empty string is provided as a value', () => {
    const testInput = '     ';
    const validationFn = () => validateNotEmpty(testInput);

    expect(validationFn).toThrow();
})

it('should throw an error with the provided error message', () => {
    const testInput = '';
    const testErrorMessage = 'Test';
    const validationFn = () => validateNotEmpty(testInput, testErrorMessage);

    expect(validationFn).toThrow(testErrorMessage);
})
```
</details>