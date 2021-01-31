---
id: react_5
title: ※ 리액트 Hooks
sidebar_label: ※ 리액트 Hooks
---

## 1. 장점

-   함수형 컴포넌트에서 클래스형 기능을 구현 가능하게 한다.
-   State, LifeCycle, Reference등의 기능을 사용가능하다.
-   클래스형 컴포넌트가 가지고 있던 복잡성, 재사용성의 단점을 해결

## 2. 종류
### 2-1. useState

-   함수형 컴포넌트에서도 상태값을 관리할 수 있다.
-   첫번째 배열 원소는 상태값(state), 두번째 원소는 상태값을 변경하는 함수
-   Const = [count, setCount] = useState(0)

### 2-2. useEffect

-   Life-cycle 함수
-   마운트 : 나타난다.
-   마운트. 예시. 초기 props 값을 컴포넌트의 state로 정하는경우
-   마운트. 예시. REST API 이용하는 경우
-   마운트. 예시. setInterval, setTimeout
-   언마운트. return () => { 실행 }
-   언마운트. 예시. clearInterval, clearTimeout
-   언마운트. 예시. 라이브러리, 인스턴스 제거
-   deps. 두번째 값으로 deps (dependence) 값을 넣어줘야한다.
-   deps. deps = [] 인경우 처음 한번만 실행됌
-   deps. deps = [user] 인경우, user 값이 처음 생기거나(마운트), 변경될때마다 실행된다.

### 2-3. useCallback

-   '함수'를 재사용할때 사용한다.
-   이벤트 핸들러 함수를 필요할 때만 생성해 줄 수 있다.
-   함수를 내부에 선언해놓으면 리렌더링 될때마다 VirtualDOM에 매번 다시 선언됌
-   VirtualDOM에 하는 rerendering까지 줄일 수 있는 방법
-   함수를 다시 선언하는 것은 그렇게 큰 메모리를 잡아먹는 것은 아니지만. 재사용 하는 것이 좋음
-   deps. 참고하고 있는 값을 넣어줘야한다.

### 2-4. useReducer

-   상태값을 리덕스처럼 관리할 수 있다.

## 3 출처

-   https://velog.io/@_jouz_ryul/-%EB%A6%AC%EC%95%A1%ED%8A%B8-hooks-%ED%95%A8%EC%88%98%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8
-   https://gist.github.com/ninanung/25bdbf78a720846e4dc4c30ac1c9ec9b
