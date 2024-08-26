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
