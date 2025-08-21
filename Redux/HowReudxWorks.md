<details>
  <summary>How To Use Sotre API</summary>

#### actions.ts

```javascript
// actions.ts
export const INCREMENT = 'counter/increment' as const
export const ADD = 'counter/add' as const

export type IncrementAction = { type: typeof INCREMENT }
export type AddAction = { type: typeof ADD; payload: number }
export type CounterActions = IncrementAction | AddAction

export const increment = (): IncrementAction => ({ type: INCREMENT })
export const add = (value: number): AddAction => ({ type: ADD, payload: value })

```

#### reducer.ts
```javascript
// reducer.ts
import { INCREMENT, ADD, CounterActions } from './actions'

export type CounterState = { value: number }
const initialState: CounterState = { value: 0 }

export function counterReducer(
  state: CounterState = initialState,
  action: CounterActions
): CounterState {
  switch (action.type) {
    case INCREMENT:
      return { value: state.value + 1 }
    case ADD:
      return { value: state.value + action.payload }
    default:
      return state
  }
}

```
#### store.ts
```javascript
// store.ts
import { legacy_createStore as createStore, combineReducers } from 'redux'
import { counterReducer } from './reducer'

const rootReducer = combineReducers({
  counter: counterReducer,
})

export const store = createStore(rootReducer)

// 타입 유틸
export type RootState = ReturnType<typeof rootReducer>
export type AppDispatch = typeof store.dispatch
```


#### 사용 예시
```javascript
// demo.ts (또는 App 초기화 코드 어딘가)
import { store } from './store'
import { increment, add } from './actions'

// 현재 상태 스냅샷 얻기
let state = store.getState()
console.log('초기값:', state.counter.value) // 0

// 상태 변경
store.dispatch(increment())
store.dispatch(add(5))

// 다시 읽기 (스냅샷이므로 재호출 필요)
state = store.getState()
console.log('변경 후:', state.counter.value) // 6
```

</details>