
<details>
  <summary>Simple Test3</summary>


  #### Sample Code
  ```javascript
import { sendDataRequest } from '../util/http.js';
import { validateNotEmpty } from '../util/validation.js';

export function savePost(postData) {
postData.created = new Date();
return sendDataRequest(postData);
}

export function extractPostData(form) {
const title = form.get('title');
const content = form.get('content');

validateNotEmpty(title, 'A title must be provided.');
validateNotEmpty(content, 'Content must not be empty!');

return { title, content };
}

```

#### Test Code
```javascript
import { describe, expect, it } from 'vitest';
import { extractPostData } from './posts';


const testTitle = 'Test title';
const testContent = 'Test content';
let testFormData 
describe('extractPostData()', () => {
    beforeEach(() => {
        testFormData = {
            title: testTitle,
            content: testContent,
            get(identifier) {
                return this[identifier]
            }
        }
    })


    it('should extract title and content from the provied form data', () => {
        const data = extractPostData(testFormData);

        expect(data.title).toBe(testTitle);
        expect(data.content).toBe(testContent);

    })
})

```
</details>