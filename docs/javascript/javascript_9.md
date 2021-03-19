## 자바스크립트 엔진

-   자바스크립트 코드를 실행시켜주는 프로그램
-   모든 브라우저는 자바스크립트 엔진을 가진다.
-   구글 크롬의 경우 V8 Engine

## 자바스크립트 Engine

-   모든 자바스크립트 엔진은 콜스택과 힙을 가진다
-   콜스택. 자바스크립트 코드가 실제로 실행 되는 곳
-   콜스택. Exceution Context 를 이용하여 실행된다.
-   힙. 비구조된 메모리 풀로 어플리케이션에 필요한 모든 object들이 저장되는곳
-   힙. Object가 저장되는 곳
-   힙.

## 코드는 어떻게 기계코드(0과 1)로 바뀔까?

-   Compilation. 1단계. 전체 코드가 한번에 컴퓨터로 실행될 수 있는 바이너리(기계코드) "파일"로 변경
-   Compilation. 2단계. 프로세스의 CPU에 의해 실행된다.
-   Compilation. 변경과 실행 2단계로 나뉘며 실행은 나중에 될 수 도있음
-   Interpretation. 코드를 한줄 한줄 해석한다.
-   Interpretation. 해석과 동시에 실행 (1단계만 있음)
-   Interpretation. Compilation보다 느리다는 단점이 있음

## Just-In-Time Compilation (JIT) (Compilation + Interpretation)

-   전체 코드가 한번에 변경되고난 후에 바로 실행된다.
-   코드를 한줄한줄 해석하는 Interpretation방식에 비해 빠르다
-   최적화 되지 않은 버전을 먼저 빠르게 띄우고 백그라운드에서 코드가 리컴파일되면 기존껄 대체해가면서 최적화 한다
