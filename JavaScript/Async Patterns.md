<details>
  <summary>Async Patterns: Parallel and Sequential Async</summary>


### Parallel Async?
여러 개의 비동기 작업을 동시에(병렬로) 수행하여, 각 작업이 모두 끝날 때 처리 결과를 받을 수 있는 방식

### Sequential Async?
여러 개의 비동기 작업을 순서대로(앞 작업이 끝난 후에) 실행하는 방식


## Parallel Async
```javascript
const result = [];

async function getData1(url) {
    const res = await fetch(url);
    result.push(res);
    console.log('res1')
}
async function getData2(url) {
    const res = await fetch(url);
    result.push(res);
    console.log('res2')
}
async function getData3(url) {
    const res = await fetch(url);
    result.push(res);
    console.log('res3')
}


getData1()
getData2()
getData3()
console.log('DONE')
// 동시에 실행

```

##  Sequential Async

```javascript

async function getData() {
    const res1 = await fetch('');
    console.log('res1');
    const res2 = await fetch('');
    console.log('res2');
    const res3 = await fetch('')
    console.log('res3');
}

getData();
// 순차적으로 실행

```

##  Promise.allSettled()

```javascript

async function allSettledDemo() {
    const GIT_BASE_URL = "https://api.github.com";

    let elieP = fetch(`${GIT_BASE_URL}/users/elis`);
    let joelP = fetch(`${GIT_BASE_URL}/users/jeolburton`);
    let badUrl = fetch(`${GIT_BASE_URL}/badurlbadurl`);
    let coltP = fetch(`${GIT_BASE_URL}/users/colt`);
    let antherbadUrl = fetch(`${GIT_BASE_URL}/anotherBadUrlBadUrl`);

    let results = await Promise.allSettled([
        elieP,
        joelP,
        badUrl,
        coltP,
        antherbadUrl
    ])

    console.log(results);
    const fulfilled = results.filter((r)=> r.status === 'fulfilled');
    const rejected = results.filter((r)=> r.status === 'rejected');
    console.log(fultilled);
    console.log(rejected);
}

// Promise.all()처럼  여러 개의 비동기 작업 중 하나라도 실패하면 나머지 결과를 확인할 수 없고, 에러가 발생하는 즉시 제어 흐름이 catch로 넘어가게 되는데,
// Promise.allSettled()는 JavaScript에서 모든 Promise가 성공(fulfilled)하거나 실패(rejected)하더라도, 모두 ‘settled’(결과가 확정된 상태) 될 때까지 기다렸다가 그 최종 결과를 배열 형태로 반환

```



##  Custom Promise
```javascript

class MyPromise extends Promise {
  constructor(executor) {
    super(executor);
  }

  // 새로운 메서드를 추가할 수도 있습니다.
  myCustomMethod() {
    console.log('커스텀 메서드 실행!');
    return this;
  }
}

const mp = new MyPromise((resolve, reject) => {
  setTimeout(() => resolve('Hello'), 1000);
});
//mp.  MyPromise 인스턴스( mp )가 생성
mp.then(value => { //.then(...)을 호출하는 즉시 시점에 “새로운 MyPromise 인스턴스”가 만들어져 반환, promise의 상태에 따라서 콜백이 실행됨 
  console.log('기본 then:', value);
})
.myCustomMethod() // 여기서 mp의 객체가 아닌, than() 호출로 인해 생성된 객체의 .myCustomMethod()가 실행된다.
.then(() => {
  console.log('연결된 then');
})
.catch((error) => {
    console.log('에러 발생:', error);
});

```

```javascript
function randomRejectResolve() {
    const p = new Promise(function (resolve, reject) {
        setTimeout(function () {
            const randomNum = Math.dandom();
            if(randomNum < 0.5){
                resolve("Resolve!!")
            } else {
                reject("ERROR!")
            }
        },3000)
    })
    return p
}
randomRejectResolve
    .then((val) => {
        console.log(val)
    })
    .catch((err)=>{
        console.log(err)
    });

```




</details>