---
id: web_6
title: ※ 웹팩이란 무엇인가?
sidebar_label: ※ 웹팩이란 무엇인가?
---

## 1 웹팩이란?

-   프래임워크에서 가장 많이 사용되는 모듈 번들러
-   (HTML, CSS Javascript, image 등)을 모두 모듈로 보고 조합하여 결과물을 만들어줌
-   여러개로 나누어져있는 파일을 하나의 자바스크립트 코드로 압축하고 최적화

## 2 웹팩장점

-   분리된 파일을 모듈 단위로 사용가능하다
-   각 파일끼리 서로 스코프를 침범하지 않게 해준다.
-   여러개 파일을 하나의 파일로 변경하여 네트워크 비용을 최소화해준다.
-   ES6문법을 지원하지 않는 브라우저에서 사용할 수 있는 코드로 변환

## 3 주요 속성 및 기능

### 3-1 Entry

-   웹팩이 빌드할 파일의 시작 위치를 나타낸다.
-   시작점으로부터 필요한 모듈을 로딩하고 하나의 파일로 묶는다.

### 3-2 Output

-   번들링된 결과물을 저장할 위치와 파일의 이름을 지정한다.

### 3-3 Loader

-   웹팩은 자바스크립트 외에 파일도 전부 모듈로 관리한다,
-   하지만 웹팩은 오직 Javascript와 JSON만 이해할 수 있기 때문에 변환이 필요.
-   다른 타입의 파일(img, font, stylesheet 등) 웹팩이 이해 가능한 모듈로 변환한다.
-   Test: 변환해야 하는 파일 / use: 로더 설정
-   Babel-loader: ES6문법을 ES5 문법으로 변환하여 모든 브라우져에서 사용가능
-   Css-loader, style-loader : CSS파일을 자바스크립트로 변환

### 3-4 PlugIn

-   로더가 파일단위로 처리하는 반면, 플러그인은 번들된 결과물을 처리한다.
-   번들된 자바스크립트 코드를 난독화 한다거나 특정 텍스트를 추출
-   Bundle optimiztaion, asset, management, injection for enviroment

### 3-5 Mode

-   Development(빠르게 빌드) / production(최적화) / none

### 3-6 출처

-   https://velog.io/@hih0327/Webpack-%EA%B8%B0%EC%B4%88
-   http://jeonghwan-kim.github.io/js/2017/05/15/webpack.html
-   https://medium.com/@woody23/js-webpack-1-%EC%9B%B9%ED%8C%A9%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-f29ebca31da4
