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