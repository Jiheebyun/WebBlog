<details>
  <summary>옵저버 패턴 : Observer Pattern</summary>

  ### 옵저버 패턴??
  옵저버 디자인 패턴(Observer Design Pattern)은 객체의 상태가 변경될 때, 그 객체에 의존하는 다른 객체들에게 자동으로 통보하고 업데이트할 수 있도록 하는 패턴이다. 감시자 패턴은 커스텀 이벤트라고 부르기도 하는데, 이는 브라우저가 발생시키는 이벤트가 아닌 프로그램에 의해 만들어진 이벤트를 뜻한다. 또 다른 이름으로 구독자/발행자 패턴이라고도 한다. 이 패턴은 주로 객체 간의 일대다(one-to-many) 관계를 설정하여, 하나의 객체(주체)의 상태 변화가 여러 다른 객체들(옵저버)에게 전파되도록 한다.

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