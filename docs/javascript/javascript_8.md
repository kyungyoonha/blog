---
id: javascript_9
title: ※ Curring
sidebar_label: ※ Curring
---

## Curring이란
- 함수를 분해함
- 인자를 넘겨주고, 그 다음 인자를 넘겨주는 타이밍을 함수 밖에서 제어할 수 있다.
- f(a, b, c)처럼 단일 호출로 처리하는 함수를 f(a)(b)(c)와 같이 각각의 인수가 호출 가능한 프로세스로 호출된 후 병합되도록 변환



## Curring 장점
- 인자가 두개일 경우 두번째 인자 값을 원할때 실행할 수 있음
- 리덕스의 경우 원하는 시점에서 dispatch안에 action을 넣을 수 있다.

```js
    const middleware = dispatch => action => {}
    // case 1
    middleware(dispatch)(action) 
    // case 2
    const fn = middleware(dispatch)
    fn(action)
    ```