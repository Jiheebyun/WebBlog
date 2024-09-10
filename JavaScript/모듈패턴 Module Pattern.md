<details>
  <summary>모듈 패턴</summary>

#### 모듈 패턴? 

모듈 패턴은 자바스크립트에서 코드의 캡슐화와 재사용성을 향상시키기 위해 사용되는 디자인 패턴이다. 이 패턴은 내부적으로 숨기고 싶은 데이터와 메소드를 감추고, 외부에서 접근할 수 있는 공용 API를 제공한다. 주로 즉시 실행 함수 표현식(IIFE: Immediately Invoked Function Expression)을 사용하여 모듈을 생성하며, 이를 통해 독립적인 코드 블록을 만들 수 있다.

#### 모듈 패턴 장점

- 데이터 캡슐화: 내부 데이터를 외부로부터 보호하고, 내부 상태를 감추기 위해 사용한다. 이렇게 하면 내부 데이터가 직접 변경되는 것을 방지할 수 있다.
- 코드의 조직화: 코드의 기능별로 모듈을 분리하여 더 나은 구조와 가독성을 제공한다. 모듈 별로 기능을 나누어 관리하면 코드의 유지보수와 확장이 용이하다.
- 재사용성 향상: 재사용 가능한 코드 블록을 정의하여 여러 곳에서 동일한 기능을 사용하도록 한다. 모듈을 통해 한 번 정의한 기능을 여러 곳에서 활용할 수 있다.
- 네임스페이스 충돌 방지: 전역 네임스페이스를 오염시키지 않고, 각 모듈의 범위를 독립적으로 유지한다. 이로 인해 전역 변수 충돌 문제를 방지할 수 있다.


```javascript
const UserModule = (function() {
    // 내부 상태 (히든)
    let username = null;

    // 공개 API
    return {
        setUsername: function(name) {
            username = name;
        },
        getUsername: function() {
            return username;
        },
        isUserLoggedIn: function() {
            return username !== null;
        }
    };
})();

// 사용 예
UserModule.setUsername('Alice');
console.log(UserModule.getUsername()); // 출력: Alice
console.log(UserModule.isUserLoggedIn()); // 출력: true
```



#### 사용자 인증 모듈 예 
### 함수형
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Authentication Module</title>
</head>
<body>
    <h1>User Authentication Module</h1>
    <button id="loginBtn">Login</button>
    <button id="logoutBtn">Logout</button>
    <p id="status">Not logged in</p>

    <script>
        // 사용자 인증 모듈
        const AuthModule = (function() {
            // 내부 상태 (히든)
            let currentUser = null;

            // 히든 함수: 사용자 인증 확인
            function isAuthenticated() {
                return currentUser !== null;
            }

            // 공개 API
            return {
                login: function(username) {
                    if (isAuthenticated()) {
                        return 'User is already logged in.';
                    }
                    currentUser = username;
                    return `User ${username} logged in.`;
                },
                logout: function() {
                    if (!isAuthenticated()) {
                        return 'No user is logged in.';
                    }
                    const username = currentUser;
                    currentUser = null;
                    return `User ${username} logged out.`;
                },
                getCurrentUser: function() {
                    if (isAuthenticated()) {
                        return `Current user: ${currentUser}`;
                    }
                    return 'No user is logged in.';
                },
                isLoggedIn: function() {
                    return isAuthenticated();
                }
            };
        })();

        // HTML 요소와 이벤트 리스너
        document.getElementById('loginBtn').addEventListener('click', function() {
            const username = prompt('Enter username to login:');
            const message = AuthModule.login(username);
            document.getElementById('status').textContent = message;
        });

        document.getElementById('logoutBtn').addEventListener('click', function() {
            const message = AuthModule.logout();
            document.getElementById('status').textContent = message;
        });

        // 초기 상태 업데이트
        document.getElementById('status').textContent = AuthModule.getCurrentUser();
    </script>
</body>
</html>

```

### 클래스형 

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Authentication Module</title>
</head>
<body>
    <h1>User Authentication Module</h1>
    <button id="loginBtn">Login</button>
    <button id="logoutBtn">Logout</button>
    <p id="status">Not logged in</p>

    <script>
        // 사용자 인증 모듈 클래스
        class AuthModule {
            constructor() {
                // 내부 상태 (히든)
                this.currentUser = null;
            }

            // 히든 함수: 사용자 인증 확인
            isAuthenticated() {
                return this.currentUser !== null;
            }

            // 공개 API
            login(username) {
                if (this.isAuthenticated()) {
                    return 'User is already logged in.';
                }
                this.currentUser = username;
                return `User ${username} logged in.`;
            }

            logout() {
                if (!this.isAuthenticated()) {
                    return 'No user is logged in.';
                }
                const username = this.currentUser;
                this.currentUser = null;
                return `User ${username} logged out.`;
            }

            getCurrentUser() {
                if (this.isAuthenticated()) {
                    return `Current user: ${this.currentUser}`;
                }
                return 'No user is logged in.';
            }

            isLoggedIn() {
                return this.isAuthenticated();
            }
        }

        // 인스턴스 생성
        const authModule = new AuthModule();

        // HTML 요소와 이벤트 리스너
        document.getElementById('loginBtn').addEventListener('click', function() {
            const username = prompt('Enter username to login:');
            const message = authModule.login(username);
            document.getElementById('status').textContent = message;
        });

        document.getElementById('logoutBtn').addEventListener('click', function() {
            const message = authModule.logout();
            document.getElementById('status').textContent = message;
        });

        // 초기 상태 업데이트
        document.getElementById('status').textContent = authModule.getCurrentUser();
    </script>
</body>
</html>

```
</details>


<details>
  <summary>모듈 패턴 예제</summary>


  ```javascript
  // mathUtils.js
export class MathUtils {
  // Private fields
  #history = [];

  static add(a, b) {
    return a + b;
  }

  static subtract(a, b) {
    return a - b;
  }

  static multiply(a, b) {
    return a * b;
  }

  static divide(a, b) {
    if (b === 0) {
      throw new Error('Cannot divide by zero');
    }
    return a / b;
  }

  // Instance methods
  constructor() {
    this.#history = [];
  }

  // Private methods
  #logOperation(operation) {
    this.#history.push(operation);
  }

  getHistory() {
    return this.#history.slice(); // Return a copy of the history
  }

  performAndLog(operation, ...args) {
    let result;
    switch (operation) {
      case 'add':
        result = MathUtils.add(...args);
        break;
      case 'subtract':
        result = MathUtils.subtract(...args);
        break;
      case 'multiply':
        result = MathUtils.multiply(...args);
        break;
      case 'divide':
        result = MathUtils.divide(...args);
        break;
      default:
        throw new Error('Invalid operation');
    }
    this.#logOperation({ operation, args, result });
    return result;
  }
}
```

```javascript
// stringUtils.js
export class StringUtils {
  #privatePrefix = 'Prefix: ';

  static capitalize(str) {
    if (typeof str !== 'string') throw new Error('Input must be a string');
    return str.charAt(0).toUpperCase() + str.slice(1);
  }

  static truncate(str, length) {
    if (typeof str !== 'string') throw new Error('Input must be a string');
    if (str.length <= length) return str;
    return str.slice(0, length) + '...';
  }

  // Instance methods
  constructor(prefix) {
    this.#privatePrefix = prefix;
  }

  #addPrefix(str) {
    return this.#privatePrefix + str;
  }

  formatString(str) {
    return this.#addPrefix(StringUtils.capitalize(str));
  }
}
```

```javascript
// dateUtils.js
export class DateUtils {
  #timezone = 'UTC';

  static formatDate(date, format) {
    if (!(date instanceof Date)) throw new Error('Input must be a Date object');
    const options = { year: 'numeric', month: 'short', day: 'numeric' };
    return new Intl.DateTimeFormat('en-US', options).format(date);
  }

  static daysBetween(date1, date2) {
    if (!(date1 instanceof Date && date2 instanceof Date)) {
      throw new Error('Both inputs must be Date objects');
    }
    const diffTime = Math.abs(date2 - date1);
    return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
  }

  constructor(timezone) {
    this.#timezone = timezone;
  }

  #convertToTimezone(date) {
    // Dummy implementation for timezone conversion
    return `${date.toISOString()} ${this.#timezone}`;
  }

  getCurrentTime() {
    const now = new Date();
    return this.#convertToTimezone(now);
  }
}
```


```javascript
// main.js
import { MathUtils } from './mathUtils.js';
import { StringUtils } from './stringUtils.js';
import { DateUtils } from './dateUtils.js';

function runExamples() {
  try {
    // MathUtils example
    console.log('MathUtils:');
    const math = new MathUtils();
    console.log('Add: 3 + 4 =', MathUtils.add(3, 4));
    console.log('Subtract: 10 - 5 =', MathUtils.subtract(10, 5));
    console.log('Multiply: 2 * 6 =', MathUtils.multiply(2, 6));
    console.log('Divide: 8 / 2 =', MathUtils.divide(8, 2));
    
    // Perform and log operations
    console.log('Perform and log operations:');
    console.log('Result:', math.performAndLog('add', 1, 2));
    console.log('History:', math.getHistory());

    // StringUtils example
    console.log('\nStringUtils:');
    const stringUtil = new StringUtils('Mr. ');
    console.log('Capitalize and add prefix:', stringUtil.formatString('hello world'));

    // DateUtils example
    console.log('\nDateUtils:');
    const dateUtil = new DateUtils('GMT');
    console.log('Current time:', dateUtil.getCurrentTime());
    console.log('Days between dates:', DateUtils.daysBetween(new Date('2024-01-01'), new Date('2024-12-31')));
    
  } catch (error) {
    console.error('Error:', error.message);
  }
}

runExamples();
```

</details>