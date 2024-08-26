<details>
  <summary>encodeURL() & encodeURLComponent()</summary>

### encodeURL()

### encodeURI

- 목적: 전체 URL을 인코딩할 때 사용된다.
- 동작: URL의 구조를 유지하면서, URL의 값에 포함된 특수 문자만 인코딩한다. 즉, URL의 구분자와 구조를 나타내는 문자는 인코딩하지 않는다.
- 구분자: 인코딩하지 않는 주요 문자는 /, :, ?, #, &, = 등이 있다.

### 예제:

```javascript
const url = "https://example.com/page?name=John Doe&age=25";
const encodedURL = encodeURI(url);
console.log(encodedURL);  // "https://example.com/page?name=John%20Doe&age=25"
```
- 여기서, 공백은 %20으로 인코딩되지만, ?, &, = 같은 구분자는 그대로 유지된다.

### encodeURIComponent
- 목적: URL의 특정 구성 요소를 인코딩할 때 사용된다. 주로 쿼리 파라미터, 경로의 일부 또는 값을 인코딩할 때 유용하다.
- 동작: URL의 모든 특수 문자를 인코딩한다. URL의 구분자도 인코딩하므로, 결과적으로 모든 문자가 안전한 형식으로 변환된다.
### 예제:

```javascript

const paramName = "John Doe";
const encodedParamName = encodeURIComponent(paramName);
console.log(encodedParamName);  // "John%20Doe"
```
- 이 경우, 공백은 %20으로 인코딩되며, &, =, ? 같은 문자가 포함되어도 인코딩된다.

### 예제 URL
```javascript
const baseURL = "https://example.com/page";
const queryParams = "name=John Doe&age=25";
const fullURL = `${baseURL}?${queryParams}`;
```
- 이 URL은 https://example.com/page?name=John Doe&age=25이다. 여기에서 쿼리 문자열 부분이 중요하다.

### encodeURI 사용 예제
목적: 전체 URL을 인코딩할 때 사용된다. URL의 구조를 유지하면서 값에 포함된 특수 문자만 인코딩한다.
- 여기서 encodeURI는 URL의 구분자(예: ?, &, =)를 그대로 유지하고, 공백만 %20으로 인코딩한다.
```javascript
const encodedURLWithEncodeURI = encodeURI(fullURL);
console.log(encodedURLWithEncodeURI);
// 출력: "https://example.com/page?name=John%20Doe&age=25"
```


### encodeURIComponent 사용 예제
목적: URL의 특정 구성 요소를 인코딩할 때 사용된다. 쿼리 파라미터나 URL의 부분 문자열을 인코딩할 때 유용하다.
- 여기서 encodeURIComponent는 쿼리 파라미터 문자열의 모든 특수 문자를 인코딩한다. 구분자(예: =, &)도 인코딩되어 %3D와 %26으로 변환된다.
```javascript
const encodedFullURLWithEncodeURIComponent = encodeURIComponent(fullURL);
console.log(encodedFullURLWithEncodeURIComponent);
// 출력: "https%3A%2F%2Fexample.com%2Fpage%3Fname%3DJohn%20Doe%26age%3D25"
```

- 참고: <a> 태그에 encodeURI의 값을 그대로 사용해도 영향이 없지만, 
- encodeURIComponent의 값의 그대로 사용하게 되면 유효하지 않는 링크가 된다.

</details>