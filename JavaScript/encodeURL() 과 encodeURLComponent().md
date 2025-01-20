<details>
  <summary>encodeURL() & encodeURLComponent()</summary>


###  인코딩(Encoding)과 디코딩(Decoding)을 하는 이유

- 특수 문자 및 공백 처리

URL(Uniform Resource Locator)은 주소 체계 내에서 특정 문자가 특별한 의미를 가진다. 예를 들어 ?, =, & 등은 쿼리 파라미터를 구분하는 문자로 쓰이며, 공백은 직접 표현할 수 없다. 이러한 문자들을 그대로 사용하면 URL 구조가 깨지거나 의도하지 않은 파라미터 구분이 생길 수 있다.
따라서 특수 문자나 공백, 혹은 일부 언어(예: 한글)은 % 기호와 숫자로 이루어진 형태(%20, %ED%95%9C%EA%B8%80)로 인코딩하여 URL에서 안전하게 전달한다.

- 안전하고 일관성 있는 데이터 전송
  
웹 표준에서는 URL에 들어갈 수 있는 문자를 제한한다. 다른 언어권에서 오는 다양한 문자나, 브라우저마다 조금씩 달라질 수 있는 해석 문제를 방지하고, 일관성 있는 방식으로 데이터가 전송되도록 하기 위해 인코딩 과정을 거친다.

- 원본 데이터를 복원하기 위해
  
이렇게 인코딩된 문자열을 다시 원본 형태로 확인하고 사용할 수 있어야 하므로, 반대로 디코딩 과정을 거쳐 사람이 읽을 수 있는 문자열, 또는 실제 비즈니스 로직에서 필요한 값으로 복원한다. 즉, 서버나 자바스크립트(클라이언트) 단에서 파라미터를 실제로 사용하기 전에, 인코딩된 데이터(%ED%95%9C%EA%B8%80)를 디코딩하여 한글처럼 제대로 된 문자열로 만든다.


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
