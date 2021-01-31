---
id: web_11
title: ※ ESLint
sidebar_label: ※ ESLint
---

## 1 ESLint

### 1-1 정의

-   ES + Lint
-   ES => EcsmScript (자바스크립트)
-   Lint => 보푸라기
-   사용자가 직접 정의한대로 코드를 점검하고, 에러가 있으면 표시해준다.
-   문법 에러뿐만 아니라 코딩 스타일도 정의할 수 있어서 팀원끼리 협업할 때 좋다.

### 1-2 장점

-   확장성 때문에 많이 쓰인다
-   (다양한 플러그인을 사용할 수 있다.)
-   예를들어 eslint-config-airbnb-base 를 설치할 경우 airbnb 사의 코딩 스타일을 적용할 수 있음

### 1-3 .eslintrc

-   parserOption
    -   자바스크립트 버전 / 모듈 사용 여부
-   extends
    -   확장 설정
    -   예시에서는 airbnb 사용
-   env
    -   프로젝트의 사용 환경을 넣어줌
-   rules
    -   extends 와 plugins 세부 설정
    -   0 => 에러 검출 x
    -   1 => 경고
    -   2 => 에러 표시
    -   https://eslint.org/docs/rules/
-   plugins
    -   플러그인 넣어줌
    -   html, import, react 등
-   globals
    -   전역 변수 넣어줌
    -   ESlint는 기본적으로 전역변수 사용을 에러 처리함
    -   만약 JQuery 같은 외부 라이브러리 사용하려면 여기에 넣어줘야함

```JSX
{
"parserOptions": {
  "ecmaVersion": 6,
  "sourceType": "module"
},
"extends": "airbnb-base",
"env": {
  "browser": true,
  "node": true,
  "mocha": true
},
  "rules": {
  "max-length": 0 // 0으로 설정하면 에러 검출 X
},
"plugins": ['import'],
"globals": {
  "jQuery": true,
  "$": true
}
}
```

### 1-4 출처

-   https://www.zerocho.com/category/JavaScript/post/583231719a87ec001834a0f2#:~:text=%EC%A6%89%20ESLint%EB%8A%94%20%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8,%ED%98%91%EC%97%85%EC%9D%84%20%ED%95%A0%20%EB%95%8C%20%EC%A2%8B%EC%8A%B5%EB%8B%88%EB%8B%A4.
