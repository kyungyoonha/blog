---
id: javascript_5
title: ※ AJAX란 무엇인가?
sidebar_label: AJAX란
---

### 1. 정의

-   비동기식 자바스크립트와 xml (Asynchronous Javascript And Xml)
-   자바스크립트를 통해서 서버에 데이터를 요청하는 것
-   클라이언트와 서버간에 XML 데이터를 주고받는 기술
-   브라우저가 가지고 있는 XMLHttpRequest 객체를 이용하여 서버에 request를 요청한다.
-   전체 페이지를 새로 고치지 않고 페이지 일부만을 위한 데이터를 로드하여 자원과 시간을 아낌
-   JSON이나 XML 형태로 필요한 데이터만 받아서 갱신할 수 있다.
-   기본적으로 HTTP 프로토콜은 클라이언트 쪽에서 Request를 보내고 서버쪽에서 Response를 받으면 이어졌던 연결이 끊어지게 되어있다. 따라서 화면의 내용을 갱신하기 위해서는 다시 request를 하고 response를 하며 페이지 전체를 갱신하게 된다. 하지만 이렇게 할 경우 엄청난 자원낭비와 시간낭비를 초래하게 된다.

### 2. 예제

```javascript
$.ajax({
    url: "/examples/media/request_ajax.php", // 클라이언트가 요청을 보낼 서버의 URL 주소
    data: { name: "홍길동" }, // HTTP 요청과 함께 서버로 보낼 데이터
    type: "GET", // HTTP 요청 방식(GET, POST)
    dataType: "json", // 서버에서 보내줄 데이터의 타입
})

    // HTTP 요청이 성공하면 요청한 데이터가 done() 메소드로 전달됨.
    .done(function (json) {
        $("<h1>").text(json.title).appendTo("body");
        $('<div class="content">').html(json.html).appendTo("body");
    })

    // HTTP 요청이 실패하면 오류와 상태에 관한 정보가 fail() 메소드로 전달됨.
    .fail(function (xhr, status, errorThrown) {
        $("#text")
            .html("오류가 발생했습니다.<br>")
            .append("오류명: " + errorThrown + "<br>")
            .append("상태: " + status);
    })

    // HTTP 요청이 성공하거나 실패하는 것에 상관없이 언제나 always() 메소드가 실행됨.
    .always(function (xhr, status) {
        $("#text").html("요청이 완료되었습니다!");
    });
```

### 3. 장점

-   웹페이지의 속도 향상
-   서버의 처리가 완료 될때까지 기다리지 않고 처리 가능하다.
-   서버에서 Data만 전송하면 되므로 전체적인 코딩의 양이 줄어든다.
-   기존 웹에서는 불가능했던 다양한 UI를 가증하게 해준다.

### 4. 단점

-   히스토리 관리가 되지 않는다.
-   페이지 이동없는 통신으로 보안상의 문제가 있다.
-   연속으로 데이터를 요청할 경우 서버 부하가 증가된다.
-   XMLHttpRequest를 통해 통신하는 경우 사용자에게 진행 정보가 주어지지 않는다.
-   AJAX를 쓸수 없는 브라우저에 대한 이슈가 있다.
-   Cross Domain

### 출처

-   https://coding-factory.tistory.com/143
-   https://velog.io/@surim014/AJAX%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
-   http://tcpschool.com/ajax/ajax_jquery_ajax
-   https://araikuma.tistory.com/640
