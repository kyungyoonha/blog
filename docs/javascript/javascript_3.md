---
id: javascript_3
title: ※ Async - Await란 무엇인가
sidebar_label: ※ Async Await
---

-   ### 정의

    -   비동기 처리는 그 결과가 언제 반환될지 알 수 없기 때문에 동기식으로 처리해야함
    -   대표적으로 setTimeout, callback, promise가 있으나 약간의 문제점이 있음
    -   Promise를 편하게 쓰기 위한 방법이며 promise를 반환한다.

-   ### 장점

    -   장점. 코드가 간편해지고 가독성이 높아진다.
    -   장점. Try / catch 로 에러를 핸들링 가능하며 에러가 어디서 발생했는지 파악이 쉬움

-   ### 사용예제
    -   함수에 async를 붙여주고 내부 로직중 http 통신을 하는 비동기 처리 코드 앞에 await붙인다
    -   Fetch()로 API를 요청하여 데이터가 반환될 때까지 대기(await)하고 다음 코드가 진행된다.

```javascript
function async fetchItems(){
    var url = 'https://jsonplaceholder.typicode.com/users/1'
    const response = await fetch(url).then( response => response.json() );
    console.log(response)
}
```

-   ### Promise와 비교
    -   에러 찾기가 쉽고 코드도 간결하고 명확함

```javascript
function getData() {
    return promise1()
        .then((response) => {
            return response;
        })
        .then((response2) => {
            return response2;
        })
        .catch((err) => {
            // Todo: error handling
            // 에러가 어디서 발생했는지 알기 어렵다.
        });
}
```

```javascript
async function getData() {
    const response = await promise1();
    const response2 = await promise2(response);
    return response2;
}
```

-   ### 출처
    -   https://blueshw.github.io/2018/02/27/async-await/
    -   https://ithub.tistory.com/223
    -   https://joshua1988.github.io/web-development/javascript/js-async-await/
