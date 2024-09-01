
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

옮기기



