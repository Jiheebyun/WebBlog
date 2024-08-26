
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



## Web:
<details>
  <summary>rss서비스가 제공하는 Xml 파싱</summary>
  ## Xml json Yaml 데이터

  
아래는 fetch를 사용하여 RSS 피드를 가져오고, TextDecoder를 사용하여 인코딩 


```jsx

// RSSItem 생성 함수
const createRSSItem = (title, link, description, pubDate) => ({
    title,
    link,
    description,
    pubDate,
    toString() {
        return `RSSItem(title=${this.title}, link=${this.link}, description=${this.description}, pubDate=${this.pubDate})`;
    }
});

// XML 데이터를 파싱하여 RSSItem 배열로 변환하는 함수
const parseRSS = (xml) => {
    const parser = new DOMParser();
    const xmlDoc = parser.parseFromString(xml, "application/xml");
    const items = Array.from(xmlDoc.getElementsByTagName("item"));

    return items.map(item => createRSSItem(
        item.getElementsByTagName("title")[0].textContent,
        item.getElementsByTagName("link")[0].textContent,
        item.getElementsByTagName("description")[0].textContent,
        item.getElementsByTagName("pubDate")[0].textContent
    ));
};

// URL에서 RSS 피드를 가져오고 파싱된 RSSItem 배열을 반환하는 함수
const fetchRSSFeed = async (url) => {
    try {
        const response = await fetch(url);
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        const buffer = await response.arrayBuffer();
        const decoder = new TextDecoder('utf-8');
        const xml = decoder.decode(buffer);

        return parseRSS(xml);
    } catch (error) {
        console.error("Failed to fetch RSS feed:", error);
        return [];
    }
};

// 사용 예제
const rssUrl = 'https://example.com/rss';
fetchRSSFeed(rssUrl).then(rssItems => {
    rssItems.forEach(item => console.log(item.toString()));
});


```
</details>


<details>
  <summary>iframe이란 </summary>
## iframe

iframe은 한 웹 페이지 안에 다른 웹 페이지를 포함시킬 수 있는 HTML 요소이다. 이를 통해 외부 콘텐츠를 현재 페이지에 표시할 수 있으며, 주로 다음과 같은 목적으로 사용된다
즉, 하나의 웹 페이지 내부에 다른 웹 페이지를 불러와서 보여줄 수 있는 기능을 제공한다는 의미이다.


현재 페이지 (main.html)
```jsx
<!DOCTYPE html>
<html>
<head>
    <title>Main Page</title>
</head>
<body>
    <h1>이것은 메인 페이지입니다.</h1>
    <iframe src="iframe-content.html" width="600" height="400"></iframe>
</body>
</html>

```

포함된 페이지 (iframe-content.html)

```jsx
<!DOCTYPE html>
<html>
<head>
    <title>Iframe Content</title>
</head>
<body>
    <h1>이것은 iframe 내부의 페이지입니다.</h1>
</body>
</html>
```
설명
- main.html 파일에는 iframe 태그가 있으며, src 속성을 통해 iframe-content.html 파일을 불러온다.
- iframe-content.html 파일은 독립된 HTML 문서로, 자신의 <html>, <head>, <body> 태그를 가진다.
- 브라우저는 main.html을 렌더링할 때, iframe을 만나면 iframe-content.html을 별도로 로드하여 iframe 영역에 표시한다.
- iframe은 현재 페이지 내에서 별도의 브라우저 창처럼 동작한다.
- iframe 태그 안에 불러온 문서는 현재 페이지의 일부로 보이지만, 사실은 독립적인 HTML 문서이다.


이와 같이 iframe을 사용하면 하나의 HTML 문서 내에 또 다른 HTML 문서를 포함시키는 효과를 얻을 수 있지만, 이는 단순히 두 개의 HTML 문서를 포함하는 것이 아니라, 현재 페이지 내에 별도의 브라우저 창을 생성하여 다른 HTML 문서를 로드하는 것이다. 각 문서는 독립적으로 존재하며, 각자의 <html>, <head>, <body> 태그를 가진다.

그럼 iframe을 왜사용하는 걸까? 

iframe을 사용하는 이유는 다양한 외부 콘텐츠를 현재 웹 페이지에 간편하게 포함시키고, 이러한 콘텐츠를 독립적으로 관리할 수 있기 때문이다.

iframe의 사용 목적<br/>
1. 외부 콘텐츠 포함
- 다른 웹사이트나 서비스에서 제공하는 콘텐츠를 현재 웹 페이지에 포함시킬 때 유용하다.
- 예: 유튜브 비디오, 구글 맵, 외부 광고 배너 등.
2. 독립적인 콘텐츠 관리
- iframe으로 포함된 콘텐츠는 현재 페이지와 독립적으로 작동하므로, 스타일이나 스크립트 충돌을 피할 수 있다.
- 예: 다양한 소스에서 가져온 데이터를 표시할 때 각 소스의 스타일이나 스크립트가 충돌하지 않도록 하기 위해 사용.
3. 보안과 격리
- iframe은 포함된 콘텐츠를 현재 페이지와 격리시키므로 보안상의 이유로도 사용된다.
- 예: 외부 콘텐츠가 현재 페이지에 영향을 미치지 않도록 하기 위해 사용.

</details>





## JavaScript:



<details>
  <summary>디바운스</summary>
### 디바운스(Debounce)란?<br>
디바운스(Debounce)는 특정 시간 동안 연속적으로 발생하는 이벤트를 제어하여, 마지막 이벤트만 처리하도록 하는 기술이다. 예를 들어, 사용자가 키보드로 텍스트를 입력할 때, 키를 누를 때마다 이벤트가 발생하는데, 디바운스를 사용하면 사용자가 입력을 멈추고 일정 시간이 지난 후에만 이벤트가 처리되도록 할 수 있다. 이를 통해 불필요한 연속적인 이벤트 처리를 방지하고, 성능을 최적화할 수 있다.

```jsx
function debounce(func, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    }
}

window.addEventListener('input', debounce(function(e) {
    console.log("Input value:", e.target.value);
}, 200));
```

1. 페이지 로드 시

- 브라우저가 window.addEventListener('input', debounce(...)) 코드를 실행한다. 이때 debounce 함수가 호출된다.
- debounce 함수는 내부적으로 timer 변수를 초기화하고, 새로운 함수를 반환한다. 이 반환된 함수가 input 이벤트 리스너로 등록된다.
- 따라서, input 이벤트가 발생할 때마다 반환된 함수가 실행되게 된다.
  
2. 사용자가 입력 필드에 값을 입력할 때

- 사용자가 키보드를 통해 입력 필드에 텍스트를 입력하면 input 이벤트가 발생한다. 이때, 등록된 디바운스 함수가 실행된다.
- 디바운스 함수가 실행되면, 먼저 clearTimeout(timer)를 통해 이전에 설정된 타이머가 있는 경우 이를 취소한다. 이를 통해 이전 이벤트에 대한 처리가 무효화된다.
- 이후, setTimeout을 통해 새로운 타이머를 설정한다. 이 타이머는 delay(여기서는 200ms) 이후에 func 함수(이 경우 console.log)를 호출하도록 설정된다.
3. 사용자가 입력을 멈추고 일정 시간(200ms)이 지나면

- 사용자가 입력을 멈추고 새로운 input 이벤트가 발생하지 않으면, 마지막으로 설정된 타이머가 만료된다.
- 타이머가 만료되면, setTimeout에 의해 설정된 함수가 실행되고, 이 시점에서 console.log("Input value:", e.target.value);가 호출되어 현재 입력된 값이 콘솔에 출력된다.
  
</details>

<details>
  <summary>Throttling </summary>
"스로틀링(throttling)"은 특정 함수가 일정 시간 간격으로만 실행되도록 제어하는 기법이다. 이는 사용자가 특정 이벤트를 반복적으로 발생시키는 경우(예: 스크롤 이벤트, 리사이즈 이벤트 등) 과도한 함수 호출로 인해 성능 저하가 발생하는 것을 방지하기 위해 사용된다.

스로틀링의 동작 방식
스로틀링은 첫 번째 함수 호출을 허용한 후 일정 시간이 지나기 전까지 추가적인 함수 호출을 무시한다. 예를 들어, 사용자가 100ms 간격으로 스크롤 이벤트를 발생시킨다고 가정하자. 이 경우 스로틀링을 사용하면, 지정된 간격(예: 500ms) 내에서 함수는 한 번만 실행되고, 나머지 호출은 무시된다. 이후 간격이 지나면 다시 함수가 실행될 수 있다.

```jsx
javascript
코드 복사
function throttle(func, delay) {
    let lastCall = 0;
    return function (...args) {
        const now = new Date().getTime();
        if (now - lastCall < delay) {
            return;
        }
        lastCall = now;
        return func(...args);
    };
}

// 사용 예시
window.addEventListener('scroll', throttle(() => {
    console.log('스크롤 이벤트 실행');
}, 1000));
```
### 코드 설명
- throttle 함수는 두 개의 인자를 받는다. 첫 번째 인자는 실행하려는 함수 func이고, 두 번째 인자는 함수가 실행된 후 다음 실행까지 대기할 시간 delay이다.
- lastCall 변수는 함수가 마지막으로 호출된 시간을 저장한다.
- 반환되는 함수는 이벤트가 발생할 때마다 호출되며, 현재 시간(now)과 마지막 호출 시간(lastCall)의 차이를 계산한다.
- 이 차이가 delay보다 작으면 함수 호출을 무시하고, delay를 초과하면 함수를 실행하고 lastCall을 현재 시간으로 갱신한다.



  
</details>


<details>
  <summary>클래스 호이스팅</summary>
  
- 자바스크립트에서 어떤 방법으로 클래스를 선언하든, 클래스는 호이스팅되지 않는다.
### 클래스 선언에는 두 가지 주요 방법이 있다:
- **클래스 선언문**과 **클래스 표현식**이다. 
- 이 두 가지 방법 모두 호이스팅되지 않는다는 점에서 동일하다.

1. **클래스 선언문**:
   ```javascript
   class MyClass {
     constructor() {
       this.name = 'Example';
     }
   }
   ```
   클래스 선언문은 `class` 키워드를 사용하여 클래스를 선언하는 방식이다. 이 방식으로 선언된 클래스는 코드에서 선언한 이후에만 접근할 수 있다. 앞서 설명한 것처럼, 이 경우 호이스팅이 발생하지 않아 선언 전에 클래스를 사용하려고 하면 `ReferenceError`가 발생한다.

2. **클래스 표현식**:
   ```javascript
   const MyClass = class {
     constructor() {
       this.name = 'Example';
     }
   }
   ```
   클래스 표현식은 변수에 클래스를 할당하는 방식으로, 이는 익명 클래스나 이름이 있는 클래스 모두 가능하다. 이 경우에도 클래스는 선언 이후에만 사용할 수 있으며, 선언 이전에 접근하려고 하면 역시 `ReferenceError`가 발생한다.

예를 들어, 클래스 표현식을 사용한 다음 코드를 보자:

```javascript
const instance = new MyClass(); // ReferenceError: Cannot access 'MyClass' before initialization

const MyClass = class {
  constructor() {
    this.name = 'Example';
  }
};
```

- 여기서도 동일하게 클래스가 선언되기 전에 인스턴스화하려고 하면 오류가 발생한다.

- 따라서, 클래스 선언 방식이 클래스 선언문이든 클래스 표현식이든 관계없이, 자바스크립트에서는 클래스가 호이스팅되지 않으며, 선언 이후에만 접근이 가능하다. 
- 이 점은 코드의 실행 순서를 명확히 하고, 잠재적인 오류를 방지하는 데 도움을 준다.

### 호이스팅이 되지않는 그이유는 ?
- 자바스크립트는 다른 언어들과 달리 **인터프리터** 방식으로 작동하지만, 실행 전에 코드의 구조를 분석하고, 변수 및 함수 선언 등을 미리 처리하는 **컴파일링 단계**를 거친다. 
- 이 과정에서 이루어지는 작업을 "평가"라고 한다.

### 자바스크립트의 실행 과정

자바스크립트 코드가 실행되기 전에 다음과 같은 단계들이 진행된다:

1. **코드 분석 (파싱)**:
   - 자바스크립트 엔진은 소스 코드를 읽어 들여 이를 구문 트리(Abstract Syntax Tree, AST)로 변환한다. 
   
2. **평가 (컴파일링 과정)**:
   - 평가 단계에서는 코드에 포함된 모든 선언문(변수, 함수, 클래스 등)을 처리하여 메모리에 등록한다.
   - 함수 선언문의 경우, 이 단계에서 함수 객체가 생성되며, 이 함수는 코드의 어느 위치에서든지 호출 가능하게 된다. 이것이 함수 호이스팅이다.
   - 클래스 선언문도 평가 과정에서 처리되지만, **초기화되지 않은 상태**로 메모리에 등록된다. 이 때문에 클래스는 선언문이 위치한 이후에만 사용할 수 있으며, 그 이전에 접근하려고 하면 `ReferenceError`가 발생한다.

3. **실행**:
   - 평가가 완료된 후, 코드가 순차적으로 실행된다. 이때 평가 과정에서 처리된 변수 및 함수, 클래스가 실제로 사용된다.

### 평가와 클래스

평가 단계에서 클래스 선언문이 처리되면, 클래스가 메모리에 등록되지만 **초기화되지 않는다**는 점이 중요하다. 
이 초기화되지 않은 상태 때문에 클래스 선언문은 마치 호이스팅되지 않은 것처럼 보인다. 
반면, 함수는 평가 단계에서 객체로 생성되어 바로 사용할 수 있다.

즉,클래스의 경우 평가 후 초기화되지 않기 때문에 선언 전에 접근할 수 없다
</details>


<details>
  <summary>클래스 싱글톤 패턴: Singleton Pattern</summary>
  
### 싱글톤 패턴 

싱글톤 패턴은 소프트웨어 디자인 패턴중 하나로, 특정 클래스의 인스턴스를 단 하나만 생성하고, 그 인스턴스에 접근할수 있는 전역적인 접근 방법을 제공하는 패턴이다. 

```javascript
class Singleton {
    private static instance: Singleton;

    private constructor() {
        // 생성자를 private으로 선언하여 외부에서 접근할 수 없게 합니다.
    }

    static getInstance(): Singleton {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }
        return Singleton.instance;
    }

    public someMethod() {
        console.log("Singleton method called");
    }
}

// 싱글톤 인스턴스를 가져옵니다.
const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();

// 하나의 인스턴스만 존재
// 싱클턴 객체에 접근할떄는 static 함수로 접근해해야한다
// 하나의 파일안에서 싱글톤 클래스를 참조하면서 **instance1과 instance2**라는 두 개의 인스턴스를 만들 수 없다.

// 두 변수는 같은 인스턴스를 참조합니다.
console.log(singleton1 === singleton2); // true

```

### 그럼언제 싱글톤을 패턴을 사용해? 
- 하나의 인스턴스만 필요할경우
- 여러 인스턴스를 생성하는 것이 의미가 없을 경우
- 인스턴스에 대한 전역적 접근이 필요할 경우

### 싱글톤 패턴의 주요 목적
1. 전역적인 상태 관리:

애플리케이션의 여러 부분에서 동일한 상태를 공유하고 유지하려는 경우, 싱글톤 패턴은 효과적입니다.
예를 들어, 설정 정보, 데이터베이스 연결, 로그 기록, 캐시 데이터 등을 관리하는 경우 싱글톤 패턴을 사용하여 하나의 인스턴스에서 이 모든 상태를 관리할 수 있습니다.

2. 자원 절약:

인스턴스가 하나만 생성되므로, 메모리와 자원의 낭비를 줄일 수 있습니다.
예를 들어, 데이터베이스 연결 풀과 같은 경우 싱글톤 패턴을 사용하여 여러 곳에서 같은 연결을 재사용할 수 있습니다.

3. 전역 접근:

애플리케이션의 어디에서나 동일한 인스턴스에 접근할 수 있게 되어, 코드의 일관성과 접근성을 유지할 수 있습니다.
설정이나 로그 기록과 같은 자원을 전역적으로 접근할 수 있습니다.
  
</details>


<details>
  <summary>팩토리 패턴: Factory Pattern</summary>

### 팩토리 패턴이란? 

### 팩토리 패턴의 개념

- 팩토리 패턴은 특정 인터페이스를 통해 객체를 생성하는 패턴이다. 즉, 어떤 객체를 생성해야 할지 결정하고, 객체를 생성하는 과정을 추상화하여 별로의 함수를 통해 이를 처리한다
- 클라이언트 코드에서는 객체 생성에 관한 세부사항을 몰라도 객체를 생성할 수 있게 된다.
- 즉, 팩토리 클래스는 주로 인스턴스를 생성하는 역할을 담당하는 클래스이고 팩토리 클래스의 핵심 목적은 특정 조건이나 입력값에 따라 적절한 클래스의 인스턴스를 생성하고 반환하는 것이다

### 팩토리 패턴의 장점

1. 객체 생성 로직을 한 곳에 집중: 객체 생성 로직이 한 곳에 집중되므로 유지보수가 쉽습니다.
2. 유연성: 어떤 타입의 객체를 생성할지 런타임에 결정할 수 있어 유연성이 높습니다.
3. 재사용성: 객체 생성에 필요한 로직이 중앙 집중화되어 재사용 가능성이 높아집니다.
4. 코드 간결화: 클라이언트 코드에서 객체 생성에 대한 세부 사항을 감출 수 있어 코드가 간결해집니다.


### 컴포넌트 클래스 정의
```javascript
class Button {
    constructor(label, color) {
        this.label = label;
        this.color = color;
    }

    render() {
        const button = document.createElement('button');
        button.textContent = this.label;
        button.style.backgroundColor = this.color;
        return button;
    }
}

class Card {
    constructor(title, content) {
        this.title = title;
        this.content = content;
    }

    render() {
        const card = document.createElement('div');
        card.className = 'card';
        
        const titleElem = document.createElement('h2');
        titleElem.textContent = this.title;

        const contentElem = document.createElement('p');
        contentElem.textContent = this.content;

        card.appendChild(titleElem);
        card.appendChild(contentElem);
        return card;
    }
}

class Modal {
    constructor(message) {
        this.message = message;
    }

    render() {
        const modal = document.createElement('div');
        modal.className = 'modal';

        const messageElem = document.createElement('p');
        messageElem.textContent = this.message;

        modal.appendChild(messageElem);
        return modal;
    }
}

```
### 팩토리 클래스 구현
```javascript
class ComponentFactory {
    createComponent(type, options) {
        switch (type) {
            case 'button':
                return new Button(options.label, options.color);
            case 'card':
                return new Card(options.title, options.content);
            case 'modal':
                return new Modal(options.message);
            default:
                throw new Error('Unknown component type');
        }
    }
}
```

### 웹페이지에서의 사용 예시
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const factory = new ComponentFactory();

    // 동적으로 버튼 생성
    const button = factory.createComponent('button', { label: 'Click Me', color: 'blue' });
    document.body.appendChild(button.render());

    // 동적으로 카드 생성
    const card = factory.createComponent('card', { title: 'My Card', content: 'This is a card component.' });
    document.body.appendChild(card.render());

    // 동적으로 모달 생성
    const modal = factory.createComponent('modal', { message: 'This is a modal window.' });
    document.body.appendChild(modal.render());
});

```

웹페이지 구현에서 팩토리 패턴을 사용하는 것은 다양한 UI컴포넌트의 생성과 관리에 매우 유용하다
컴포넌트 생성로직을 한 곳에 모아 관리함으로써 코드의 가독성을 높이고, 유지보수 및 확장성을 쉽게 할수 있다
이러한 패턴을 통해 웹 애플리케이션을 더ㄱ 효율적으로 모듈화된 구조로 설계할 수 있다.

</details>

<details>
  <summary>옵저버 패턴 : Observer Pattern</summary>

  ### 옵저버 패턴??
  옵저버 디자인 패턴(Observer Design Pattern)은 객체의 상태가 변경될 때, 그 객체에 의존하는 다른 객체들에게 자동으로 통보하고 업데이트할 수 있도록 하는 패턴이다. 이 패턴은 주로 객체 간의 일대다(one-to-many) 관계를 설정하여, 하나의 객체(주체)의 상태 변화가 여러 다른 객체들(옵저버)에게 전파되도록 한다.

- 주체(Subject): 상태를 가지고 있으며, 상태가 변경될 때 이를 감지하고, 등록된 옵저버들에게 알리는 역할을 한다. 주체는 옵저버의 목록을 관리하고, 상태 변경 시 notifyObservers 메소드를 통해 모든 옵저버에게 통보한다.
- 옵저버(Observer): 주체의 상태 변화에 반응하는 객체이다. 옵저버는 주체의 상태가 변경되었을 때 update 메소드를 호출받아 자신의 상태를 업데이트하거나 적절한 작업을 수행한다.

### 주요 구성 요소
1. Subject (주체):
- 주체는 상태를 저장하고 있으며, 상태 변화가 발생할 때 옵저버들에게 알린다.
- 주체는 옵저버를 추가하거나 제거할 수 있는 메소드를 제공한다.
2. Observer (옵저버):
- 옵저버는 주체의 상태 변화에 반응하여 자신의 상태를 업데이트하거나 특정 동작을 수행한다.
- 옵저버는 주체의 상태가 변경되었을 때 호출되는 update 메소드를 정의한다.


```javascript
// Subject (주체)
class WeatherStation {
    constructor() {
        this.temperature = null;
        this.observers = [];
    }

    addObserver(observer) {
        this.observers.push(observer);
    }

    removeObserver(observer) {
        this.observers = this.observers.filter(obs => obs !== observer);
    }

    setTemperature(newTemperature) {
        this.temperature = newTemperature;
        this.notifyObservers();
    }

    notifyObservers() {
        this.observers.forEach(observer => observer.update(this.temperature));
    }
}

// Observer (옵저버)
class TemperatureDisplay {
    update(temperature) {
        console.log(`The temperature is now ${temperature}°C`);
    }
}

// 사용 예제
const weatherStation = new WeatherStation();
const display = new TemperatureDisplay();

weatherStation.addObserver(display);

weatherStation.setTemperature(25);
// 출력: The temperature is now 25°C

weatherStation.setTemperature(30);
// 출력: The temperature is now 30°C


```
  - update 메소드는 옵저버 디자인 패턴에서 옵저버 객체가 상태 변경에 반응하기 위해 정의하는 메소드이다. 이 메소드는 주체(Subject)의 상태가 변경될 때 호출되며, 옵저버는 이 메소드를 통해 상태의 새로운 값을 받고 그에 맞는 작업을 수행한다.

### 예시
```javascript

<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MutationObserver 클래스 예제</title>
</head>
<body>
    <h1>MutationObserver로 엘리먼트 감시하기</h1>

    <!-- 감시할 엘리먼트 -->
    <div id="observedElement">
        <p>이곳의 내용이 변경될 것입니다.</p>
    </div>

    <!-- 버튼들 -->
    <button id="changeTextBtn">텍스트 변경</button>
    <button id="addChildBtn">자식 요소 추가</button>
    <button id="changeAttributeBtn">속성 변경</button>

    <script>
        class ElementObserver {
            constructor(targetNode) {
                this.targetNode = targetNode;
                this.observer = new MutationObserver(this.handleMutations.bind(this));
            }

            // 감지된 변화를 처리하는 메소드
            handleMutations(mutationsList) {
                for (let mutation of mutationsList) {
                    if (mutation.type === 'childList') {
                        console.log('자식 노드가 변경되었습니다.');
                    } else if (mutation.type === 'attributes') {
                        console.log(`엘리먼트 속성 ${mutation.attributeName}이(가) 변경되었습니다.`);
                    }
                }
            }

            // 감시를 시작하는 메소드
            startObserving() {
                const config = { attributes: true, childList: true, subtree: true };
                console.log(config)
                this.observer.observe(this.targetNode, config);
            }

            // 감시를 중단하는 메소드
            stopObserving() {
                this.observer.disconnect();
            }
        }

        // 감시할 대상 엘리먼트 선택
        
        const targetNode = document.getElementById('observedElement');

        // ElementObserver 인스턴스 생성 및 감시 시작
        const elementObserver = new ElementObserver(targetNode);
        elementObserver.startObserving();

        // 버튼 클릭 이벤트 핸들러
        document.getElementById('changeTextBtn').addEventListener('click', () => {
            targetNode.innerHTML = '<p>새로운 텍스트입니다.</p>';
        });

        document.getElementById('addChildBtn').addEventListener('click', () => {
            const newElement = document.createElement('p');
            newElement.textContent = '추가된 자식 요소입니다.';
            targetNode.appendChild(newElement);
        });

        document.getElementById('changeAttributeBtn').addEventListener('click', () => {
            targetNode.setAttribute('data-status', 'changed');
        });
    </script>
</body>
</html>

```


</details>




## Docker:  


<details>
  <summary>Attach 모드  &  Detach 모드</summary>

### Attach 모드
attach 모드는 도커 컨테이너의 표준 입력(stdin), 표준 출력(stdout), 표준 오류(stderr) 스트림에 연결하여 직접 상호작용하는 모드입니다. 이 모드를 사용하면 실행 중인 컨테이너의 콘솔에 접속하여 실시간으로 로그를 확인하거나 입력을 전달할 수 있습니다.

사용 예시: docker attach <container_id> 또는 docker attach <container_name>을 사용한다.<br/>
특징:
컨테이너의 현재 실행 상태를 확인하고, 실시간으로 로그를 볼 수 있다.
터미널에서 직접 명령어를 입력할 수 있다.
Ctrl+C를 누르면 컨테이너가 종료된다.




<details>
  <summary>장식자 패턴: Decorator Pattern</summary>

### 장식자 디자인 패턴이란?
  
장식자(Decorator) 디자인 패턴은 객체에 새로운 기능을 동적으로 추가할 수 있게 해주는 구조적 디자인 패턴이다. 
이 패턴은 객체에 기능을 추가하거나 변경할 때, 상속을 사용하지 않고도 유연하게 처리할 수 있게 해준다. 객체의 기본 기능을 그대로 유지하면서 추가적인 책임을 덧붙일 수 있는 방법을 제공한다.

### 장식자 패턴의 주요 요소
  
- Component (컴포넌트):
기본 기능을 정의하는 인터페이스나 추상 클래스이다. 이 컴포넌트는 장식자와 실제 객체가 공통적으로 구현해야 하는 메소드를 선언한다.
- ConcreteComponent (구체적인 컴포넌트):
Component 인터페이스를 구현하는 실제 클래스이다. 기본 기능을 구현하며, 장식자 패턴을 통해 기능이 확장될 수 있는 대상이다.
- Decorator (장식자):
Component 인터페이스를 구현하며, 다른 Component 객체를 포함하는 추상 클래스이다. 기본 기능을 구현하는 ConcreteComponent 객체를 감싸고 있으며, 기능을 추가하는 방법을 정의할 수 있다.
- ConcreteDecorator (구체적인 장식자):
Decorator를 상속받아, 실제로 기능을 추가하는 클래스이다. 추가적인 책임이나 상태를 유지하며, 기본 기능을 확장할 수 있다.
### 장식자 패턴의 동작 방식
- 기본 컴포넌트 정의:
Component 인터페이스를 정의한다. 기본적인 메소드나 기능을 선언하여, 장식자와 실제 객체가 공통적으로 따라야 할 계약을 설정한다.
- 구체적인 컴포넌트 구현:
ConcreteComponent 클래스를 통해 Component 인터페이스를 실제로 구현한다. 기본 기능을 제공하며, 장식자 패턴을 통해 이 클래스의 기능을 확장할 수 있다.
- 장식자 클래스 구현:
Decorator 추상 클래스를 정의하여, Component 객체를 감싸는 구조를 만든다. ConcreteComponent 객체를 포함하고, 기능을 추가할 수 있는 기초를 제공한다.
- 구체적인 장식자 구현:
ConcreteDecorator 클래스를 통해 Decorator를 상속받아, 실제로 추가적인 기능을 구현한다. 추가된 기능이나 상태를 유지하며, 기본 기능을 확장하는 역할을 한다.
  
</details>








### Detach 모드
detach 모드는 컨테이너를 백그라운드에서 실행하며, 사용자가 직접 컨테이너의 콘솔에 접속할 필요가 없는 모드입니다. 이 모드는 주로 서버 애플리케이션이나 백그라운드 작업을 실행할 때 사용된다.

사용 예시: docker run -d <image> 또는 docker run --detach <image>를 사용한다.<br/>
특징:
컨테이너가 백그라운드에서 실행되므로 터미널을 점유하지 않는다.
컨테이너의 로그를 확인하거나 상호작용하려면 docker logs, docker exec 등을 사용해야 한다.
Ctrl+C를 눌러도 컨테이너는 계속 실행된다.
</details>

<details>
  <summary>도커 실행시 options -a -i</summary>

도커를 실행할 때 사용하는 -a와 -i 옵션은 컨테이너의 입출력 연결 방식에 영향을 준다.

-a 옵션
-a 옵션은 --attach의 약어로, 도커 컨테이너의 표준 스트림(표준 입력, 표준 출력, 표준 오류)에 연결하는 데 사용한다. 예를 들어, -a stdout은 컨테이너의 표준 출력 스트림에 연결한다는 의미이다.

-i 옵션
-i 옵션은 --interactive의 약어로, 표준 입력(standard input, stdin) 스트림을 열어둔다. 이 옵션은 보통 컨테이너 내에서 상호작용이 필요한 경우에 사용한다.

-a와 -i를 함께 사용하는 경우
이 두 옵션을 함께 사용하는 경우, 다음과 같은 효과가 있다:

docker run -a stdin -a stdout -a stderr -i <image> 또는 docker run -a -i <image>로 실행하면, 표준 입력, 표준 출력, 표준 오류 스트림이 모두 연결되고, 표준 입력이 열려있다. 이는 사용자가 컨테이너와 상호작용할 수 있도록 한다.
-a만 사용하는 경우
-a 옵션만 사용하는 경우에는 표준 입력이 기본적으로 열려있지 않다. 예를 들어, docker run -a stdout <image>로 실행하면 표준 출력 스트림에만 연결된다. 이 경우 사용자는 컨테이너의 표준 출력만 볼 수 있으며, 표준 입력을 통해 상호작용할 수 없다.

정리
-a -i 옵션을 함께 사용하면 컨테이너의 표준 입출력 스트림에 모두 연결되고, 표준 입력이 열려 있어 상호작용이 가능하다.
-a 옵션만 사용하면 특정 스트림(표준 출력 등)에만 연결되며, 상호작용은 불가능하다.   
</details>
<details>
  <summary>
    중지된 컨테이너 자동 제거하기 
  </summary>
### 컨데이너 실행할때 --rm 옵션을 붙이면 컨테이너가 중지되면 자동으로 제거 된다.<br/>
   docker run -p 3000:80 -d --rm 2ddf2*****fed
</details>

<details>
  <summary>이미지 검사</summary>
  
도커(Docker) 이미지를 검사하는 방법은 다음과 같다:

### 1. 이미지 레이어 확인
- 도커 이미지는 여러 레이어로 구성된다. 각 레이어는 도커 파일에서 작성된 명령의 결과로 생성된다.
- `docker history` 명령어를 사용하면 이미지의 레이어와 각 레이어의 크기를 확인할 수 있다.

  ```bash
  docker history <이미지 이름 또는 ID>
  ```

### 2. 이미지 메타데이터 확인
- `docker inspect` 명령어를 사용하면 이미지의 메타데이터(예: 환경 변수, 명령어, 볼륨, 네트워크 설정 등)를 확인할 수 있다.

  ```bash
  docker inspect <이미지 이름 또는 ID>
  ```

### 3. 이미지 콘텐츠 확인
- 이미지에 포함된 파일들을 확인하고 싶다면, `docker run` 명령어를 사용하여 임시 컨테이너를 생성하고 그 내부를 탐색할 수 있다.

  ```bash
  docker run --rm -it <이미지 이름 또는 ID> /bin/sh
  ```

  또는 컨테이너 내부의 특정 경로를 확인하고 싶을 때는 다음과 같이 명령을 실행할 수 있다:

  ```bash
  docker run --rm -it <이미지 이름 또는 ID> ls /경로
  ```

### 4. 보안 검사
- 도커 이미지의 보안 취약점을 검사하기 위해 `Clair`, `Anchore`, `Trivy`와 같은 도구를 사용할 수 있다.
  - 예를 들어 `Trivy`를 사용하여 보안 취약점을 검사할 수 있다.

  ```bash
  trivy image <이미지 이름 또는 ID>
  ```

### 5. 이미지 빌드 로그 확인
- 도커 이미지를 빌드할 때 어떤 단계에서 문제가 발생했는지 확인하려면, 빌드 로그를 분석할 수 있다.
- `docker build` 명령어를 사용할 때 `--progress=plain` 옵션을 추가하면 빌드 로그를 더 자세히 확인할 수 있다.

  ```bash
  docker build --progress=plain -t <이미지 이름> .
  
  ```

이 방법들을 사용하여 도커 이미지를 상세히 검사하고 필요한 정보를 얻을 수 있다.

</details>

<details>
  <summary>Docker hub: push</summary>

### 도커 허브 로그인
- 먼저, 로컬 컴퓨터에서 도커 허브에 로그인한다. docker login 명령어를 터미널에서 실행하고, 도커 허브 계정의 사용자 이름과 비밀번호를 입력한다. 이 과정을 통해 도커 허브에 인증된다.
### 이미지 생성 또는 태그 추가
- 도커 이미지를 로컬에서 생성하거나 이미 생성된 이미지에 태그를 추가한다. 이미지가 이미 있는 경우 docker tag 명령어를 사용하여 해당 이미지에 도커 허브로 푸시할 때 사용할 태그를 추가한다. 예를 들어, docker tag my-image username/my-image:tag와 같이 한다.
### 도커 이미지 푸시
- 도커 이미지를 도커 허브에 푸시한다. docker push username/my-image:tag 명령어를 사용하여 로컬에 있는 이미지를 도커 허브에 업로드한다. 이 과정은 이미지의 크기에 따라 시간이 걸릴 수 있다.
### 푸시 확인
- 푸시가 완료된 후, 도커 허브에 로그인하여 업로드된 이미지를 확인한다. 도커 허브 웹사이트에서 저장소(repository)를 열어 푸시된 이미지가 제대로 올라갔는지 확인할 수 있다.

</details>




<details>
  <summary>form</summary>
</details>




