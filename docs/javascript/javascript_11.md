---
id: javascript_10
title: ※ THIS
sidebar_label: ※ THIS
---

## 1. 4가지 바인딩

-   this는 호출부가 어디냐에 따라 다음과 같은 4가지로 바인딩된다.

### 1-1. Default Binding

```js
let a = 1;
const func = function () {
    console.log(this.a);
};

func(); // 호출부: window => window.a = 1
```

### 1-2. Implicit Binding(암묵적 바인딩)

```js
const func = function () {
    consle.log(this.a);
};
const obj = {
    a: "2",
    func: func,
};
obj.func(); // 호출부: obj => obj.a = 2
```

### 1-3. Explicit Binding(명시적 바인딩)

```js
const func = function () {
    console.log(this.a);
};

const obj = {
    a: 3,
};

func.call(obj); // 3 => this를 obj로 바인딩해줬음
```

### 1-4 Arrow Function

-   this는 무조건 Lexical scope를 가르킨다.
-   (호출시가 아닌 선언될때 가장 가까운 변수)

```js
const obj = {
    a: 4,
    lst: ["a", "b", "c"],
    func() {
        this.lst.forEach((item) => {
            console.log(this.a);
        });
    },
};
obj.func(); // 4 4 4
```

## 2 주의

### 2-1 Event Callback의 호출부는 window이다.

```js
const testObj = {
    name: "Yoon",
    print: console.log(`Hello ${this.name}`),
};
$("#test").click(testObj.print()); // Hello undefined => 호출부는 window
$("#test").click(testObj.print.bind(testObj)); // Hello Yoon => bind로 this 고정
```

### 2-2 변수로 선언된 곳이 호출부가 된다.

-   암묵적 바인딩(x) -> default binding

```js
const testObj = {
    name: "Yoon",
    print: console.log(`Hello ${this.name}`),
};

const testPrint1 = testObj.print;
const testPring2 = testObj.print.bind(testObj);
testPrint1(); // Hello undefined
testPring2(); // Hello Yoon
```
