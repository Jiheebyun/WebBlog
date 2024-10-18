<details>
  <summary>Testing Hooks</summary>

#### beforeAll(), beforeEach(), afterEach(), afterAll()
 각 메서드는 테스트 실행 전후에 특정 작업을 수행할 수 있게 해준다. 


#### 1. beforeAll()
beforeAll()은 모든 테스트가 실행되기 전에 한 번만 실행된다. 보통 데이터베이스 초기화, API 연결 등을 설정하는 데 사용된다.

#### 2. beforeEach()
beforeEach()는 각 테스트가 실행되기 전에 매번 실행된다. 테스트에 필요한 초기 상태나 데이터를 설정하는 데 사용된다.


#### 3. afterEach()
afterEach()는 각 테스트가 실행된 후에 매번 실행된다. 테스트 후에 필요한 정리 작업을 수행하는 데 사용된다.

#### 4. afterAll()
afterAll()은 모든 테스트가 실행된 후에 한 번만 실행된다. 주로 리소스를 해제하거나 종료 작업을 수행하는 데 사용된다.

```javascript

describe("User Service", () => {
    beforeAll(async () => {
        await initializeDatabase(); // 데이터베이스 초기화
    });

    beforeEach(() => {
        resetMockData(); // 각 테스트 전에 모의 데이터 초기화
    });

    afterEach(() => {
        cleanupMocks(); // 각 테스트 후 모의 데이터 정리
    });

    afterAll(async () => {
        await closeDatabase(); // 데이터베이스 연결 종료
    });

    it("사용자를 성공적으로 생성해야 한다", () => {
        const user = createUser("testUser");
        expect(user.name).toBe("testUser"); // 사용자 이름이 올바른지 확인
    });

    it("사용자를 성공적으로 삭제해야 한다", () => {
        createUser("testUser"); // 사용자 생성
        const result = deleteUser("testUser"); // 사용자 삭제
        expect(result).toBe(true); // 삭제 결과가 true인지 확인
    });
});


```

</details>