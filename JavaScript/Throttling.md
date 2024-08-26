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
