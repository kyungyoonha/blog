---
id: web_7
title: ※ 서버API 통신 & REST API란 무엇인가?
sidebar_label: ※ 서버API 통신 & REST API
---

## 1. 서버 API 통신

### 1-1 정의

-   Application Programming Interface의 약자
-   API는 한 프로그램이 다른 프로그램을 이용할 때 쓰는 인터페이스로 입출력이 데이터로 된다.
-   어떤 특정 사이트에서 특정 데이터를 공유할 경우 어떤 방식으로 요청해야 하는지 어떤 데이터를 받을 수 있는지 등의 규격을 말한다.
-   서버와 데이터베이스에 대한 출입구 역할로 허용된 사람들에게만 접근성 부여할 수 있다.
-   UI (User Interface)와 비교하면 이해가 편함 (사용자 인터페이스)
-   UI는 일반 사용자는 버튼이나 링크, 이미지등 특정 ui를 통해 조작함으로써 시스템을 제어한다.
-   API는 개발자가 특정 프로그램에서 제공하는 규격을 이용하여 응용프로그램을 제어하고 구축

| ![img](/img/web/web_8_1.png) |
| ---------------------------- |


### 1-2 출처

-   https://www.youtube.com/watch?v=Z4kH0IZVT-8&t=95s
-   http://blog.wishket.com/api%EB%9E%80-%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85-%EA%B7%B8%EB%A6%B0%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8/
-   https://m.blog.naver.com/azure0777/220749852024

## 2. REST API

### 2-1 정의

-   Representational State Transfer의 약자로 HTTP 통신에서 어떤 자원에 대한 CRUD(Create, Read, Update, Delete) 요청을 Resource와 Method(GET, POS, DELETE등)로 표현하여 특정한 형태로 전달하는 방식을 말한다. HTTP 프로토콜 (링크를 기반으로 데이터를 요청하고 받음)의 장점을 최대한 활용 가능한 방법이다.

-   resource. 모든 자원에는 고유한 ID가 존재하고 이 자원은 Server에 존재한다.
-   resource. 자원을 구분하는 ID는 /groups/:group_id 와 같은 HTTP URI이다.
-   method. GET / POST / PUT /DELTE
-   Representaion of Resource. JSON, XMS, TEXT, RSS등 다양한 형태로 데이터를 보낼 수 있다.
-   Representaion of Resource. JSON을 많이 쓴는 추새

| ![img](/img/web/web_8_2.png) |
| ---------------------------- |


-   moives라는 Resource에 다양한 메소드(GET, POST, PUT, DELETE)로 접근이 가능해짐

### 2-2 출처

-   https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
-   https://mangkyu.tistory.com/46
-   https://sookmyunglion.tistory.com/9
