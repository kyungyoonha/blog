---
id: web_2
title: ※ REAL DOM (VS) VIRTUAL DOM
sidebar_label: REAL DOM (VS) VIRTUAL DOM
---

## 1. DOM

### 1-1 정의

-   Document Object Model
-   HTML 문서의 내용을 객체 기반의 표현 방식으로 나타내준다.
-   따라서 HTML 내용이 객체 모델로 변환되므로 다양한 프로그램에서 사용가능해진다.
-   이때 객체 구조를 ‘노드 트리’ 구조를 갖는 객체들의 계층적 구조로 표현

| 정의         | 예시                               |
| ------------ | ---------------------------------- |
| **HTML**     | ![web_2_1](/img/web/web_2_1.png)   |
| **노드트리** | ![미션6 GIF](/img/web/web_2_2.png) |

### 1-2 장점

-   계층적 구조로 변형해주기 때문에 프로그래밍 언어가 접근가능해진다.
-   구조, 스타일, 내용등을 변경 가능할 수 있도록 API를 제공한다.
-   자바스크립트를 사용해 DOM에 새로운 노드를 추가하는 등, 동적 역할도 가능
-   노드 트리를 자바스크립트 객체로 변환 가능

```js
{
    type: 'body',
    props: {},
    children: [
        {
            type: 'h1',
            props: {},
            children:['Hello, World!']
        },
        {
            type: 'p',
            props: {},
            children:['How are you?']
        }
    ]
}
```

### 1-3 단점

-   DOM조작이 많이 일어나는 경우 비효율 적이다.
-   DOM에 변화가 생기는 경우 랜더트리를 다시 재생성
-   레이아웃을 만들고 페인팅 하는 과정이 다시 반복된다. (빈번한 repaint와 reflow 문제)
-   다만, 대규모 SPA의 웹 페이지가 아니라 소개페이지 같은 용도라면 DOM이 더 빠름

## 2. VIRTURL DOM

### 2-1 정의

-   변화가 일어나면 오프라인 Virturl DOM트리에 적용
-   in-memory에 존재해서 실제 렌더링 되지 않는다. => 연산 비용이 적다
-   연산이 끝나고 최종 변화를 실제DOM에 반영하여 딱 한번만 랜더트리를 재생성.
-   이때 알고리즘을 통하여 변경된 부분만 새로그려서 repaint와 reflow 연산도 줄여줌

### 2-2 장점

-   최소한의 DOM 조작을 통해 repaint, reflow 문제를 줄여준다.
-   DOM의 상태를 메모리에 저장, 변경전, 후를 비교하여 최소한의 내용만 반영

### 2-3 동작방법

-   1. 데이터가 업데이트 되면, 전체 UI를 Virtual DOM에 리렌더링 한다.
-   2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교
-   3. 바뀐 부분만 실제 DOM에 적용

## 3. 제이쿼리 돔 직접참조에 비해서 무엇이 개선되었는가.

-   DOM은 동적인 UI에 최적화되어 있지 않기 때문에 제이쿼리를 통해 동적 효과를 준다
-   하지만 수많은 데이터가 로딩되고 표현되고 있는 웹 어플리케이션에서 DOM에 직접 접근하여 변화를 주면 성능상 이슈가 발생한다.
-   DOM의 변화가 일어나면 CSS를 다시 연산하고(repaint) 레이아웃을 구성(reflow)하기 때문에 문제가 일어난다.

## 4. 출처

-   https://velog.io/@sbinha/React%EC%97%90%EC%84%9C-Virtual-DOM
-   https://wit.nts-corp.com/2019/02/14/5522
-   http://blog.drakejin.me/React-VirtualDOM-And-Repaint-Reflow/
-   https://velopert.com/3236
-   https://jw910911.tistory.com/41

브라우저 뷰포트 vs DOM. 뷰 포트에 보이는 것은 렌더트리이다(DOM 과 CSSOM 조합)
브라우저 뷰포트 vs DOM. 렌더 트리는 오직 스크린에 그려지는 것으로 구성되어 있음
브라우저 뷰포트 vs DOM. {‘display’:’none’} 속성을 주면 뷰포트에서는 안보이고 DOM에선 보임
