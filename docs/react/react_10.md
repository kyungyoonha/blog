---
id: react_10
title: ※ 웹 성능 최적화
sidebar_label: ※ 웹 성능 최적화
---


## 1. 로딩 성능 최적화

-   이미지 사이즈 최적화
-   code split
-   텍스트 압축

## 2. 렌더링 성능 최적화

-   Bottleneck 코드 최적화

## 3. Lighthouse 사용

### 3-1. Opportunities - 리소스의 관점에서 가이드 제시 (로딩 성능 최적화)
-   Properly size images
    -   실제 렌더링 되는 사이즈 보다 2배로 최적화 하는 것이좋다
    -   120 x 120 pixels 이라면 240 x 240 pixels로 사용
    -   API를 통해서 이미지 받아오는 경우 이미지 cdn을 사용하는 것이 좋다.
    -   Image CDN 사용
        -   CDN: Contents Delivery Network
        -   CDN: 물리적 거리의 한계를 극복하기 위해 사용자와 가까운 곳에 컨텐츠 서버를 두는 기술
        -   Image CDN: 원본 형태의 이미지를 가공해서 사용자에게 전달해준다.
        -   Image CDN: 사이즈를 변경하거나 포멧을 변경할 수 있다.
        -   Image CDN: http://cnd.image.com?src=[img src]&width=200&height=100
    -   예시)
        -   브런치에서 실제 로딩되는 이미지 주소를 보면
        -   //img1.daumcdn.net/thumb/C320x520.fjpg/?fname=https://t1.daumcdn.net/section/oc/5ee6d6bff0214a3086fa8825708a43b1
        -   daumcdn을 이용하여 image를 넘겨주고 있다.
    -   사용
        -   cdn 직접 구현 어려울 경우
        -   Image CDN 솔루션 => img Ix
        -   https://www.imgix.com/
-   Remove unused Javascript
-   Remove duplicate modules in Javascript bundles
-   Aviod serving legach Javascript to modern browsers
### 3-2. Diagnostics - 페이지 실행 관점에서 가이드 제시 (렌더링 성능 최적화)
-   Reduce JavaScript execution time
    -   Lighthouse로는 알 수 없음
    -   Performance
        -   record 할 필요 없이 옆에 리프레시 버튼 누르면 됌
        -   Article 컴포넌트 밑에 removeSpecialCharactor 함수가 여러번 끊어져서 실행되고 있음
        -   너무 많은 리소스를 차지하다 보니 가비지 컬렉터가 중간중간 끊었음
        -   Minor GC를 보면 가비지 컬렉터