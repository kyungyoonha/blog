---
id: web_5
title: ※ Asynchronous 비동기
sidebar_label: ※ Asynchronous
---

## 1 Async 비동기식

-   Asynchronous
-   병력적으로 수행하며 테스크가 종료되지 않은 상태에도 다음 테스크 실행
-   DOM 이벤트, setTimeout, setInterval, Ajax 요청
-   요청을 병렬로 처리하여 다른 요청이 블로킹 되지 않는다

## 2 Promise

### 2-1 정의

-   비동기 연산을 수행하고 연산이 종료되면 결과를 알려주기 위해 사용
-   비동기식 연산결과를 알려주겠다고 약속하는 것

### 2-2 장점

-   비동기 처리 시점을 명확하게 해준다.
-   가독성이 좋아지고, 에러 예외처리가 쉬워진다.
-   전통적인 방법인 Callback 패턴의 단점을 보완함

### 2-3 상태

-   Primise는 생성하고 종료될 때까지 3가지 상태를 가진다.

1. Peding(대기): 비동기 처리 로직이 완료되지 않은 상태.
2. Fulfilled(이행): 비동기 처리가 완료되어 프라미스 결과값 반환
3. Rejected(실패): 비동기 처리가 실패하거나 오류 발생

### 2-4 사용방법

-   생성자 함수인 new Promise()로 promise 객체를 만들 수 있다.
-   이때 인자로는 Executor가 들어가는데 resolve와 reject라는 두 개의 함수를 매게변수로 받는 실행함수 값을 넣어준다 로직은 작업이 성공했을 때 resove(), 실패했을때 reject() 함수를 실행할 수 있게 해준다.

-   #### 실행

    -   promise.then(successCallback, failureCallback)의 형태로 then() 메소드를 이용한다.
    -   successCallback() => resove()
    -   failureCallback => reject()

    | ![img](/img/web/web_5_1.png) |
    | ---------------------------- |
    | ![img](/img/web/web_5_2.png) |

### 2-5 출처

-   https://limboy.tistory.com/382
-   https://velog.io/@ejchaid/JS-axios
-   https://donghunee.github.io/study/2019/10/21/react/
-   https://poiemaweb.com/es6-promise
-   https://2dubbing.tistory.com/77
-   https://velog.io/@cyranocoding/2019-08-02-1808-%EC%9E%91%EC%84%B1%EB%90%A8-5hjytwqpqj
