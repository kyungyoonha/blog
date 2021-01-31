---
id: css_2
title: ※ mvvm 패턴
sidebar_label: ※ mvvm 패턴
---

## 1 MVVM 구조

### 1-1 정의

-   Model-View-ViewModel 패턴
-   데이터 모델 부분과 화면을 보여주는 뷰 그리고 뷰에서 발생되는 이벤트 행등(뷰모델)로 나눔

### 1-2 분류

1. Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
2. View: 사용자가 상호 작용하는UI 계층, 유저가 보는 화면,
3. View Controller: View를 나타내 주기 위한 Model이며 데이터 처리를 하는 부분

| ![img](/img/etc/etc_2_1.png) |
| ---------------------------- |


### 1-3 MVVM 동작

1. View를 통해서 사용자의 Action이 들어온다.
2. Command 패턴으로 View Model에 Action을 전달한다.
3. View Model은 Model에게 데이터를 요청한다.
4. Model은 요청을 확인하고 View Model에게 데이터를 응답한다.
5. View Model은 응답 받은 데이터를 가공하여 저장한다.
6. View는 View Modle과 Data Binding을 하여 화면을 나타낸다

### 1-4. MVVM 장점

-   View 와 Model 사이의 의존성이 없다.
-   Command 패턴과 Data Binding을 사용하여 View와 ViewModel 사이의 의존성 또한 없앰
-   각 부분이 독립적이기 때문에 모듈화하여 개발할 수 있다.

### 1-5 출처

-   https://beomy.tistory.com/43
-   https://goodteacher.tistory.com/191
