


#  Web을 위한 블로그 입니다.

## React:  

<details>
  <summary>useState</summary>
  
- useState 어떤 구조가지고 있는지 고민해보자

[참조]: 모던리액트 Deep Dive
```jsx
function useState*(initialValue){
  let internalState = initialValue

  function setState(newValue){
    internalState = newValue
  }

  return [ internalState, setState ]
}
```


- 이 코드에서 `setValue`를 호출해도 `value` 값이 변경되지 않는 이유는 `useState`가 React 컴포넌트의 상태를 관리하는 방식 때문이다.

```jsx
const [value, setValue] = useState(0);
setValue(1);
console.log(value); // 0
```

1. **비동기 상태 업데이트**: React의 상태 업데이트 함수(`setValue`)는 비동기적으로 작동한다. 즉, 상태를 업데이트하라는 요청을 보낸 후 바로 상태가 변경되지 않고, React가 다음 렌더링 사이클에서 상태를 업데이트한다. 그래서 `setValue`를 호출한 직후에는 상태가 즉시 변경되지 않은 상태로 유지된다.

2. **구조 분해 할당**: 코드에서 `const [value, setValue] = useState(0);`는 현재 상태 값과 상태 업데이트 함수를 반환한다. 이때 반환된 `value`는 초기 값(0)을 가지게 된다. 이후 `setValue(1)`을 호출해도, 이 시점에서는 아직 상태 업데이트가 반영되지 않았기 때문에 `value`는 여전히 0이다.

3. **동기식 로그 호출**: `console.log(value)`는 `setValue` 호출 직후에 실행되므로, 상태 업데이트가 완료되기 전에 `value`를 출력하게 된다. 따라서 여전히 초기 값인 0을 출력하게 되는 것이다.

이를 해결하기 위해서는 상태 업데이트 후 다시 렌더링된 컴포넌트에서 새로운 상태 값을 확인해야 한다. React는 상태가 업데이트되면 자동으로 컴포넌트를 다시 렌더링하여 최신 상태 값을 반영한다.
  
</details>



## CS:

<details>
  <summary>Xml Json Yaml 데이터</summary>

## Xml json Yaml 데이터


### XML (eXtensible Markup Language)
XML은 데이터를 구조화하기 위한 마크업 언어이다. 태그를 사용하여 데이터를 계층적으로 표현한다. 주로 문서 저장 및 전송에 사용된다.

**예시:**
```xml
<book>
    <title>Learning XML</title>
    <author>John Doe</author>
    <year>2021</year>
</book>
```
위 예시에서는 `<book>` 태그 안에 책의 제목, 저자, 출판연도를 태그로 감싸서 구조화하고 있다.

### JSON (JavaScript Object Notation)
JSON은 데이터를 저장하고 전송하기 위한 경량 데이터 교환 형식이다. 자바스크립트 객체 표기법을 사용하여 데이터를 표현한다. 주로 웹 애플리케이션에서 데이터 교환에 사용된다.

**예시:**
```json
{
    "title": "Learning JSON",
    "author": "Jane Doe",
    "year": 2022
}
```
위 예시에서는 JSON 객체 안에 책의 제목, 저자, 출판연도를 키-값 쌍으로 표현하고 있다.

### YAML (YAML Ain't Markup Language)
YAML은 사람이 읽기 쉬운 데이터 직렬화 형식이다. 들여쓰기를 사용하여 데이터를 계층적으로 표현한다. 주로 설정 파일에 사용된다.

**예시:**
```yaml
title: Learning YAML
author: Alice Doe
year: 2023
```
위 예시에서는 YAML 형식으로 책의 제목, 저자, 출판연도를 들여쓰기를 통해 계층적으로 표현하고 있다.

### 요약
- XML은 데이터를 태그로 감싸서 구조화한다.
- JSON은 데이터를 키-값 쌍으로 표현하며 주로 웹에서 사용된다.
- YAML은 들여쓰기를 통해 데이터를 구조화하며 사람이 읽기 쉽다.

이들 포맷은 각각의 장점과 사용 사례가 다르므로 상황에 맞게 선택해서 사용하면 된다.

</details>







