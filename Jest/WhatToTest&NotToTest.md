
<details>
  <summary>What to test & Not to test</summary>

### What to test & Not to test

#### what should be tested
- only test your code and test one thing.
  - What is "one thing" refer to ? : it is one feature or one behavior such as validate input OR transform it. 

- write separated tests for your backend or frontend code.
- Do test your client-side reaction to different responses & errors.
- wirte AAA Pattern's tests.
- focus on the essence of a test when arranging.
- keep your number assertion low (ex. minimize expect statement)

#### what should not be tested
- Don't test thired-party code !
** Because of the third-party API, which has bugs that we cannot fix ** 
- ex. fetch() API - Don't test if it works as intended.
- Don't test your server-side code implicitly via your client-side code.





</details>