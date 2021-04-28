---
id: javascript_10
title: ※ 클래스
sidebar_label: ※ 클래스
---

## supper

-   클래스가 상속 받았을 경우 부모 클래스에 접근할때 이용한다.
-   클래스의 상속의 경우 동일한 함수명으로 함수를 정의할 경우 부모의 함수를 overwriting할 수 있는데 기존의 부모함수의 값을 super로 접금할 수 있다.

```js
class Parent {
    constructor() {}
    static speak() {
        return "부모";
    }
}

class Child extends Parent {
    constructor() {}
    static speak() {
        return super.speak() + " 자식";
    }
}
Child.speak(); // '부모 자식'
```
