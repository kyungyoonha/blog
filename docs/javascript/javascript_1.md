---
id: javascript_1
title: ※ 자바스크립트의 자료구조
sidebar_label: ※ 자료구조
---

## 1 배열

-   배열의 경우 자료들이 메모리 주소에 순서대로 정렬되어 있다.
-   특정 데이터를 순차적으로 iterate 해야하는 경우 최상의 자료구조형
-   시간복잡도. Push => O(1)
-   시간복잡도. Pop => O(1)
-   시간복잡도. unShift => O(n): 앞에 추가 => 뒤에 모두 새로운 인덱스
-   시간복잡도. Splice => O(n): 중간에 추가 => index 재부여 필요

| ![img](/img/javascript/javascript_1_1.png) |
| ------------------------------------------ |


## 2 해쉬 테이블(Hash Table)

-   Key와 value가 쌍을 이룬 형태로 데이터가 저장되어있는 자료구조형
-   객체는 해쉬 테이블 자료구조 중의 하나이다.
-   Key가 input 되면 Hash Function이 무작위 주솟값 생성하고 key, value 저장
-   장점. 시간복잡도가 O(1)으로 search, lookup, indsert, delete 계산이 빠르다
-   단점. 구조가 정렬되어 있지 않고 메모리 공간에서 랜덤한 곳에 저장된다.
-   단점. 해쉬충돌. 다른키값인데 hash function을 거치면서 같은 값이 나오면 같은 곳에 저장해버리는 문제점이 있다.

| ![img](/img/javascript/javascript_1_2.png) |
| ------------------------------------------ |


## 3 연결 리스트(Linked List)

-   Value와 pointer가 한쌍이 노드가 모여있는 자료구조형
-   자료의 순서가 중요하지 않고 삽입과 삭제가 쉬워야 하는경우 사용
-   시간복잡도
    -   Prepend(맨앞에 삽입): O(1)
    -   Append(맨뒤에 삽입): O(1)
    -   search: O(n)
    -   Insert: O(1)
    -   Delete: O(1)

| ![img](/img/javascript/javascript_1_3.png) |
| ------------------------------------------ |


## 4 스택과 큐

-   ### 4-1 스택 (Stacks)
    -   LIFO: Last In First Out 구조
    -   마지막으로 삽입된 element가 가장 먼저 제거되는 방식
    -   스택은 브라우저 히스토리(이전, 다음페이지) 또는 Ctrl + Z 에 쓰임
    -   배열이나, linked list 어떤걸로 만들어도 상관없음
-   ### 4-2 큐 (Queues)
    -   FIFO: First In First Out
    -   큐는 대기줄
    -   먼저 들어온 element가 먼저 사용된다.
    -   배열은 O(n)의 시간복잡도를 가지므로 Linked List로 만들어야 한다.

| ![img](/img/javascript/javascript_1_4.png) |
| ------------------------------------------ |


## 5 트리

-   ### 5-1 Binary Trees

    -   각 노드가 하나 혹은 2개의 자식 노드만을 가지고 있는 상태의 트리구조
    -   현재보다 오른쪽에 있으면 현재 값보다 크고 왼쪽은 현재값 보다 작다.
    -   Search의 경우 시간복잡도는 O(log n)으로 O(n)보다 효율적이다
    -   모든 노드를 볼필요 없이 왼쪽 오른쪽만 선택해서 값을 찾으므로 빠르다
    -   값이 한쪽으로만 추가되서 불균형한 경우 배열과 거의 동일해진다

    | ![img](/img/javascript/javascript_1_5.png) | ![img](/img/javascript/javascript_1_5_1.png) |
    | ------------------------------------------ | -------------------------------------------- |


-   ### Binary Heap

    -   상위층에서 아래층으로 내려갈수록 값이 작아진다
    -   Priority Queue에 사용 우선순위가 높은 것을 먼저

    | ![img](/img/javascript/javascript_1_6.png) |
    | ------------------------------------------ |


-   ### Trie

        -   Element가 텍스트인 경우 seaching작업을 수행할 때 주로 쓰인다.
        -   사전 또는 검색어 자동완성 같은 알고리즘에 쓰인다.
        -   찾고자하는 단어의 길이만 알면 되기 때문에 O(length of word)

    | ![img](/img/javascript/javascript_1_7.png) |
    | ------------------------------------------ |


## 6 출처

-   https://soldonii.tistory.com/70
-   https://velog.io/@raram2/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%ED%86%B5%ED%95%9C-%EA%B5%AC%ED%98%84-Linked-List
-   https://soldonii.tistory.com/75?category=862199
-   https://junwoo45.github.io/2020-05-09-data_structure/
