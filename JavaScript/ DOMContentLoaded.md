<details>
  <summary>DOMContentLoaded VS window.onload</summary>

#### DOMContentLoaded 이벤트

- 발생 시점: HTML 문서가 완전히 로드되고, DOM 트리가 완성된 상태에서 발생한다. 즉, HTML의 구조와 요소가 모두 준비된 시점이다. 외부 리소스(이미지, 스타일시트 등)가 로드되지 않았더라도 DOM은 준비된 상태이다.
- 특징:이 시점에서 DOM 요소에 접근하거나 초기화 작업을 할 수 있다. 외부 리소스의 로딩 여부와는 관계없이 문서의 구조만 준비되면 된다.
페이지가 빠르게 반응할 수 있도록, 외부 리소스의 로드를 기다리지 않고 DOM이 준비되면 즉시 실행된다.


- 사용 예: 페이지의 HTML 구조를 기반으로 초기화 작업을 수행하거나, DOM 요소에 이벤트 리스너를 추가하는 등의 작업에 적합하다.
```javascript
코드 복사
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM fully loaded and parsed');
  // 이 시점에서 DOM 요소에 접근하거나 초기화할 수 있다.
});
```

#### window.onload 이벤트 

- 발생 시점: 문서와 모든 외부 리소스(이미지, 스타일시트, 프레임 등)가 모두 로드된 후에 발생한다. 즉, 문서와 관련된 모든 리소스가 준비된 상태에서 발생한다.
- 특징:모든 외부 리소스가 로드된 후에 실행되므로, 페이지의 최종 상태를 확인하거나 모든 리소스가 준비된 후의 작업을 수행할 수 있다.
문서와 모든 외부 리소스의 로드가 완료된 후에 작업을 수행하는 데 적합하다.
- 사용 예:이미지나 외부 스타일시트가 모두 로드된 후에 작업을 수행하거나, 최종 페이지 상태에 대한 작업이 필요할 때 사용된다.
```javascript
코드 복사
window.addEventListener('load', () => {
  console.log('All resources including images are loaded');
  // 이 시점에서 모든 리소스가 로드된 후 작업을 수행할 수 있다.
});
```


- DOMContentLoaded:
1. 문서의 DOM이 완전히 로드된 시점에 발생한다.
2. 외부 리소스의 로딩 여부와는 관계없이 DOM에 접근할 수 있다.
3. DOM 초기화와 구조 조작에 적합하다.
- window.onload:
1. 문서와 모든 외부 리소스가 로드된 시점에 발생한다.
2. 페이지의 모든 리소스가 준비된 상태를 확인할 수 있다.
3. 모든 리소스가 필요할 때 작업을 수행하는 데 적합하다.

</details>