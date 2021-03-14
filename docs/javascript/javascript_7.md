---
id: javascript_7
title: ※ 클로저
sidebar_label: ※ 클로저
---

## 클로저란?
- 생성될 당시의 환경을 기억하는 함수를 말한다. 
- 외부함수가 실행되고 나면 그 지역변수들은 메모리에서 지워져야함
- 하지만 외부함수 안에 있는 내부함수는 클로저를 통해여 외부함수의 지역변수를 기억하고 있게된다
- 즉 외부함수가 종료되고 나서도 내부함수는 외부함수의 지역변수에 접근할 수 있다.

## 클로저 구조
- 클로져 구조: 함수를 즉시호출하는 구조를 만들고 리턴 값으로 함수를 정의해준다.
```js
// case1: 뒤에 괄호는 즉시실행함수를 실행시켜주기 위함
cosnt result = (function(){ return function(){} })();

// case2:
const outFunc = funcion( return function innerFunc(){} ){} 
const result = outFunc()
```

## 클로저 사용하는 이유
- 함수를 실행하고나서 그 값을 기억하고 싶은 경우
- 아래 예시에서보면 render는 자체가 함수이므로 실행 값을 변수로 저장하여(prevVdom) 다음 값(nextVdom)을 비교할 수없다.
- 이러한 경우 클로저를 만들어줘서 변수에 기존 값을 저장 시킬 수 있다.


```js
// 적용전
export function render(nextVdom, container) {
    if (prevVdom !== nextVdom) { 
        // 불가능 
        // prevVdom 을 저장할 수 없음
    }
    container.appendChild(renderRealDom(vdom));
}

// 적용 후
export const render = (function () {
    let prevVdom = null;

    return function (nextVdom, container) {
        if (prevVdom === null) {
            prevVdom = nextVdom;
        }
        // diff 로직 구현해주면 됌
        // 기존 돔과 새로운 돔이 일치하지 않는경우 rerendering 시켜줌
        if (prevVdom !== nextVdom) {
        }

        container.appendChild(renderRealDom(vdom));
    };
})();
```
