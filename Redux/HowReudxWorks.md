<details>
  <summary>How Redux Works</summary>

####  데이터 흐름

- Action: "무슨 일이 일어났다"는 사실을 설명하는 객체 (type 필수)

- Reducer: state와 action을 받아 새로운 state를 만드는 순수 함수

- Store: state를 보관하고, dispatch/getState/subscribe를 제공하는 중앙 저장소

##### Action
- { type: string, payload?: any } 형태의 평범한 JS 객체
- "사용자가 버튼을 클릭했다", "서버에서 데이터를 받았다" 같은 **이벤트의 종류(type)와 데이터(payload)**를 표현

##### Reducer
- (state, action) => newState 형태의 순수 함수
- 순수성: 외부에 영향 X, 같은 입력 → 같은 출력
- 불변성 유지: 기존 state를 직접 변경하지 말고, 새 객체/배열을 만들어 반환

##### Store

- Redux에서 제공하는 중앙 저장소. createStore (전통), Redux Toolkit의 configureStore(권장)
- 현재 state 보관
- dispatch(action)으로 변경 트리거
- getState()로 현재 state 조회
- subscribe(listener)로 변경 구독


</details>