---
id: react_6
title: ※ contextAPI
sidebar_label: ※ contextAPI
---

## 1. 정의

-   리덕스와 마찬가지로 상태를 중앙에서 관리하기 위한 상태 관리 도구이다.
-   (상태 관리도구가 아니라 prop drilling 문제를 해결하기 위한 솔루션 이라는 말도 있음)
-   리덕스와는 다르게 여러개의 저장소가 존재할 수 있다.

## 2. 장점

-   부모 - 자식 관계가 아니여도 데이터를 쉽게 전달 할 수 있음

## 3. 여러개의 context 한번에 사용하는 방법

-   https://stackoverflow.com/questions/53346462/react-multiple-contexts

## 4. 사용법

```jsx
// [정의] 정의할 때 초기 값을 넣어준다.
export const PostsDispatchContext = createContext(null);

// [뿌림] Provider로 뿌려준다. 이때 value 값으로 저장할 값을 지정해준다.
<PostsDispatchContext.Provider value={postsDispatch}>

// [사용] 검포넌트 안에서 사용
const PostsDispatch = useContext(PostsDispatchContext);
```
