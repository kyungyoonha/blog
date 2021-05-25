실행. 컴파일 이후 실행된다.
실행. 콜스택. 코드는 콜스택의 실행컨택스트 안에서 실행된다.
실행. 콜스택. 실행컨택스트들이 쌓임, 나중에 들어오는게 먼저나감
실행. 콜스택. 콜스택 덕분에 현재 어디를 처리중인지 알 수 있음
실행. 콜스택. 실행컨택스트. 글로벌 실행컨택스트. Top-level code
실행. 콜스택. 실행컨택스트. 글로벌 실행컨택스트. 어떤 함수도 실행되기 전임

실행. 콜스택. 실행컨택스트. 어떤 코드가 실행되기위해 필요한 환경으로 필요한 정보를 담고 있다.
실행. 콜스택. 실행컨택스트. argument나 로컬변수등을 담고 있음

실행. 콜스택. 실행컨택스트. Variable Enviroment / Scope Chain / this가 생김
실행. 콜스택. 실행컨택스트. 단, 화살표 함수는 this가 없음
실행. 콜스택. 실행컨택스트. Variable Enviroment. (let / const /var), Function, arguments
실행. 콜스택. 실행컨택스트. Scope Chain. 지금 함수 밖에 참조 가능한 변수의 위치를 담음
실행. 콜스택. 실행컨택스트. Scope Chain. 지금 함수 밖에 참조 가능한 실행컨택스트들을 가짐
실행. 콜스택. 실행컨택스트. THIS

실행컨택스트. scope. 유효범위
실행컨택스트. scope. 렉시컬 스코프. 함수를 어디에 선언하였느냐에 따라 결정
실행컨택스트. scope. chain. 스코프는 자기 + 모든 부모의 스코프를 가짐
실행컨택스트. scope. chain. nested 되도 자기 부모의 모든 변수나 함수의 정보를 가진다
실행컨텍스트. scope. Global / Funcion / Block
실행컨텍스트. scope. 스코프는 자신 밖의 스코프에 접근할 수 있게 해준다.
실행컨텍스트. scope. 오직 상위 스코프만 참고할 수 있다

함수의 경우 스코프와 variable Enviroment가 같다
var 는 function 스코프
let, const 는 block스코프이다.
블락 안에서 var 정의해도 밖의 function에 정의되어 있는 것과 마찬가지임

콜스택의 variable Enviroment vs 스코프
콜스택은 함수가 호출된 순서에 따라 실행 컨택스트가 쌓아며 각각의 로컬 변수나 함수만 저장
스코프는 선언된 위치에 따라서 접근할 수 있는모든 정보를 다 가짐
스코프는 함수의 호출 순서와는 관계 없다
![image](https://user-images.githubusercontent.com/29701385/119355171-eaec0f80-bcdf-11eb-81ce-7a0d7027f1bc.png)

## validation

```js
const validInputs = (...inputs) => inputs.every((inp) => Nummber.isFinite(inp));
const allPositive = (...inputs) => inputs.every((inp) => inp > 0);

if (!validInputs(321, 3222, 321) || !allPositive(321, -321, 321)) {
    return alert("wrong input");
}
```
