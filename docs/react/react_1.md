---
id: react_1
title: ※ Component
sidebar_label: ※ Component
---


## 1. 컴포넌트란

### 1-1 정의

-   재사용 할 수 있는 단위
-   리액트는 컴포넌트들의 합으로 UI를 구성한다.
-   Input으로 Props를 받고, output으로 Element를 반환한다.

### 1-2. 컴포넌트 설계 장점

-   재사용성, 생산성이 좋다.
-   기능별로 분리하여 개발하고 이를 재사용함으로써 시간과 작업을 줄임
-   쉽게 수정 및 보수가 용이하다.

### 1-3. 컴포넌트 설계방법

-   재사용 할 수 있도록 구현이 완료되어 있어야한다.
-   코드들이 독립적인 단위로 패키지화되어 있어야한다.
-   독립 배포가 가능해야한다.
-   특정 기능을 수행하는 모듈로써 한 시스템이 종속적이지 않고 재사용 가능
-   단일책임 원칙으로 컴포넌트가 한가지 작업만 하는 것이 이상적이다.
-   컴포넌트가 점점 커진다면 작은 서브 컴포넌트들로 분리하는 것이 좋다.
-   앱에서 필요로하는 변경가능한 state의 최소 집합을 생각해야한다.
-   불필요한 state가 없는지 확인
-   어떤 컴포넌트가 state를 변경하거나 소유할지 찾아야한다.
-   역방향 데이터 흐름 고려 자식 컴포넌트에서 부모 컴포넌트의 state 변경

### 1-4 클래스형

-   컴포넌트와 함수형 컴포넌트 2가지로 나뉜다.
-   State, 라이프사이클 기능, 임의 메서드를 정의 가능
-   Render() 함수가 반드시 필요하며 그 안에 JSX를 반환
-   단점. 함수형 컴포넌트보다 메모리 자원 결과물 크기가 크다.

```jsx
import React from 'react';
import './App.css';

class App extends Component {
    render() {
        return (
            <div>
                클래스형 컴포넌트
            </div>
        );
    }
}
export default App;
```

    

### 1-5 함수형

-   메모리자원 결과물의 크기가 작고 선언이 편함
-   Hooks를 통해 state, 라이프사이클 API도 사용가능

```jsx
import React from 'react';
import './App.css';

function App() {
return <div>함수형 컴포넌트</div>;
}

export default App;
```

    

### 2. Props

### 2-1. 정의
-   props는 컴포넌트의 속성을 설정할때 사용된다.
-   컴포넌트에서 사용할 데이터 중 변경되지 않는 데이터를 다룰 때 사용되며 내부에서는 수정되지 않고 변경이 불가능 하다.
-   Props 값을 변경하려면 컴포넌트 내부가 아닌 부모 컴포넌트에서 변경이 되어야한다.
-   부모 컴포넌트

```jsx
import React from 'react';
import ChildComponent from './ChildComponent';

function Parent(){
    return(
        <div className="parent">
            <ChildComponent name="전달값">
        </div>
    )
}
```

### 2-2. 자식 컴포넌트

```jsx
import React from "react";

function ChildComponent({ name }) {
    return <div className="childComponent">{name}</div>;
}
/*
name 값으로 "전달값" 출력됌
*/
```

## 3. State

### 3-1. 정의

-   컴포넌트에서 관리하는 상태값으로 유동적인 데이터를 다룰 때 사용한다.
-   변경이 가능하고 변경할 때는 setState 메서드를 사용하여 변경한다.
-   setStat()는 비동기로 동작하며 동작완료에 대한 콜백을 할 수 있다.

### 3-2.  예제

```jsx
import React, { useState } from "react";

function StateExample() {
    // state
    const [isShow, setShow] = useState(false);

    // 버튼 클릭 시, isShow가 true <-> false 로 전환됌
    const handleClickToggle = () => {
        setShow(!isShow);
    };

    // isShow가 true 일때만 <div>보입니다.</div> 가 나타난다.
    return (
        <div className="stateExample">
            <button onClick={handleClickToggle}>클릭</button>
            {isShow && <div>보입니다.</div>}
        </div>
    );
}
```

## 4. 컴포넌트 리렌더링

1. Props가 변경되었을 때
2. State가 변경되었을 때
3. forceUpdate()를 실행하였을 때
4. 부모 컴포넌트가 렌더링 되었을 때

## 5. 참고

-   https://reactjs-kr.firebaseapp.com/docs/thinking-in-react.html
-   https://yzzzzun.tistory.com/36
-   https://boxfoxs.tistory.com/401
-   https://trustyoo86.github.io/react/2017/11/18/props-state-react.html
