---
id: react_2
title: ※ 컴포넌트의 각 생명주기 & 호출 시기
sidebar_label: ※ 컴포넌트의 각 생명주기 & 호출 시기
---


## 1. 생성 및 마운트

-   컴포넌트가 처음 실행될 때를 Mount라고 표현한다. 아래 순서대로 실행된다.
    1. constructor(): this.state 초기값 적용 및 이벤트 메서드 바인딩
    2. static getDerivedStateFromProps(): props에 있는 값을 state에 동기화
    3. render(): UI 렌더링 메서드
    4. componentDidMount(): 컴포넌트가 DOM tree에 삽입된 직후에 호출

## 2. 업데이트

-   리액트 컴포넌트가 업데이트 될때이며 아래 4가지 경우에 업데이트 된다.
-   Props가 변경될 때 / State가 바뀔 때 /부모 컴포넌트 리렌더링 / this.forceUpdate
-   업데이트시 호출하는 메서드 4가지
    1. Static getDerivedStateFromProps
    2. shouldComponentUpdate: 컴포넌트릐 레더링 여부 결정
    3. Render
    4. getSnapshotBeforeUpdate: DOM에 변화를 반영하기 직전에 호출
    5. componentDidUpdate: 리렌더링 후에 실행

## 3 언마운트
-   리액트 컴포넌트가 DOM 상에서 제거되는 것
-   언마운트시 호출하는 메서드
    1. componentWillUnmount
## 4 LifeCycle Method

-   componentWillMount() => 컴포넌트 마운트 전
-   componentDidMount() => 컴포넌트 마운트 후(Render 실행 완료)
-   componentWillRecieveProps(newProps) => 새로운 props 받은 후
-   shouldComponentUpdate(newProps, newState) => True: 리렌더링, False: 렌더링 x
-   componentWillUpdate(newProps, nextState) => 컴포넌트 업데이트 전
-   componentDidUpdate(prevProps, prevState) => 컴포넌트 업데이트 후(Render 실행)
-   componentWillUnMount() => 컴포넌트 언마운트 전

![img](/img/react/react_2.png)

5. ## 출처
-   https://medium.com/humanscape-tech/react-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0-c7f45ef2d0be
-   https://niceman.tistory.com/58
