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



</details>