---
id: css_3
title: ※ GraphQL
sidebar_label: ※ GraphQL
---

## GraphQL VS REST API

### 1-1 장점

-   REST API와는 다르게 원하는 데이터만 손쉽게 가지고 올 수 있다.
-   여러 컬렉션을 사용할때도 직관적으로 표현 가능하다.
-   리팩토링하기 쉽다.

### 1-2 단점

-   Request 캐싱이 안된다.

## 2. 예시

### 2-1. 스키마 정의

```js
// 스키마 정의
type QUERY {
    movie(name: String!): Movie
}

type Movie {
    title: String!,
    category: String!,
    time: String!,
    age: Number!,
    info: String!,

}
```

### 2-2. 쿼리문

```js
query {
    movie(name: "아이언맨"){
        category
    }
}

// 결과
dat{
    movie:{
        category: 'hero'
    }
}

// 다른 컬렉션 데이터 추가
query {
    movie(name: "아이언맨"){
        category
    }
    actor(name: "아이언맨"){
        name,
        profile,
        age,
        sex,
    }
}
```
