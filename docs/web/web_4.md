---
id: web_4
title: ※ HTTP 통신 vs Socket 통신
sidebar_label: ※ HTTP 통신 vs Socket 통신
---

## ※ 비동기 서버통신의 모듈 axios, fetch 비교

-   ### 1-1 Fetch

    -   #### 장점
        -   import 하지 않아도 사용할 수 있다.
        -   라이브러리 업데이트에 따른 에러 방지가능
    -   #### 단점
        -   요청을 취소하는 abort 메서드를 지원하지 않는다.
        -   IE 지원하지 않는다.
        -   Response timeout API 제공이 안된다.

-   ### 1-2 Axios

    -   #### 장점
        -   API URL의 기본 인스턴스를 만들 수 있다.
        -   JSON 데이터 자동변환이 가능하다
        -   구형 브라우저를 지원한다.
        -   요청 취소, 응답 시간 초과 설정 가능하다
        -   CSRF 보호 기능이 내장
        -   Node.js에서도 사용가능하다.
    -   #### 단점
        -   라이브러리가 업데이트 에러가 발생할 수 있음

-   ### 출처
    -   https://velog.io/@ejchaid/JS-axios
    -   https://donghunee.github.io/study/2019/10/21/react/
