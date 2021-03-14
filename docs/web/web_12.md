---
id: web_12
title: ※ Babel이란 무엇인가?
sidebar_label: ※ Babel이란 무엇인가?
---

## 1 바벨이란

- 자바스크립트 컴파일러
- 최신버전의 자바스크립트 문법을 브라우저가 이해할 수 있도록 변경해준다.
- 덕분에 ES6나 ES7문법 들도 사용가능하다

## 2. @babel/cli 
- 커멘드라인에서도 컴파일 가능하게 해준다.
- 웹펙으로 빌드할 수도 있으나 cli로도 가능하다
```js
// build라는 명령어를 통해 babel 실행
// 컴파일된 파일은 build 파일 안에 만들어짐
// -w: src에 변화가 감지되면 자동 컴파일 다시
"script": {
    "build": "babel src -d build -w"
}
```

## babel.config.js
- 바벨 자체로는 아무것도 할 수 없고 프리셋이나 플러그인을 설치해줘야한다.
- 프리셋 > 플러그인: 프리셋은 플러그인을 모아놓은 것
- 예전에는 연도별로 프리셋을 제공 (babel-reset-es2015, babel-reset-es2016, babel-reset-es2017, babel-reset-latest)
- 지금은 preset-env하나면 다 포함
```js
module.exports = {
    presets: [
      "@babel/preset-env"
      "@babel/preset-react", // 리액트를 변환하기 위한 프리셋
      "@babel/preset-typescript", // 타입스크립트를 변환하기 위한 프리셋
      "@babel/preset-flow"
    ],
    plugins: ['@babel/plugin-transform-arrow-functions'] 
    // 프리셋보다 더 작은 단위
    // 여러 가능중 한가지 기능만 추가하고 싶은 경우 플러그인에 추가해주면 된다.
}
```