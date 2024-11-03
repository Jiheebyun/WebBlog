
<details>
  <summary> DOM: Side Effects Test</summary>


#### sample code 


```javascript

export function showError(message) {
  const errorContainerElement = document.getElementById('errors');
  const errorMessageElement = document.createElement('p');
  errorMessageElement.textContent = message;
  errorContainerElement.innerHTML = '';
  errorContainerElement.append(errorMessageElement);
}
```


#### Test Code 


```javascript
import {expect, it, vi} from 'vitest'
import { Window } from 'happy-dom';

import { showError } from './dom';

const htmlDocPath = path.join(process.cwd(), 'index.html');
const htmlDoccumentContent = fs.readFileSync(htmlDocPath).toString();

// provided by HappyDom
const window = new Window();
const document = window.document;

// render index.html into the virtual browser 
document.write(htmlDoccumentContent);
vi.stubGlobal('document', document);


it('should add an error paragraph to the id="errors" element', () => {
    showError('Test');

    const errorsEl = document.getElementById('errors')
    const errorParagraph = errorsEl.firstElementChild;

    expect(errorParagraph).not.toBeNull();
})

it('should not contain an error paragraph initially', () => {
  const errorsEl = document.getElementById('errors');
  const errorParagraph = errorsEl.firstElementChild;

  expect(errorParagraph).toBeNull();
});

```

</details>
