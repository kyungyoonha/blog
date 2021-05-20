---
id: javascript_10
title: ※ prototype
sidebar_label: ※ prototype
---

## 1. 생성자 함수

-   new operator를 이용하여 객체를 만드는 함수
-   그냥 평범한 함수와 똑같으며, new를 이용하여 객체를 만들면 생성자 함수라고 한다.

### 1-1 new 오퍼레이터

-   new를 통하여 객체를 생성하면 다음과 같은 4단계가 이루어진다
-   1. 빈객체를 생성
-   2. 함수를 실행
-   3. 생성자 함수의 property값을 저장 => **proto** : (생성자함수).prototype
-   4. 생성된 객체를 리턴

### 2. prototype chain

-   객체는 **proto**값을 통해 상속받은 메소드나 필드에 접근 가능하다.

```js
// 생성자 함수.
const Person = function (firstName, age) {
    this.firstName = firstName;
    this.age = age;
};

Person.prototype.species = "human";
Person.prototype.printAge = function () {
    console.log(this.age);
};

const jack = new Person("jack", 21);
const yoon = new Person("yoon", 22);

// Prototype Chain
console.log(yoon.__proto__ === Person.prototype); // true
console.log(yoon.__proto__.__prodto__ === Object.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true

// hasOwnProperty를 통해 객체에 직접 정의된건지 상속된 건지 확인 가능
console.log(yoon.hasOwnProperty("firstName")); // true
console.log(yoon.hasOwnPRoperty("species")); // false

// Array 생성자 함수의 prototype에 정의된 메소드들은 전부 접근가능
const testArray = [1, 2, 3, 4];
console.log(testArray.__proto__ === Array.prototype); // true
console.log(testArray.__proto__.__proto__ === Object.prototype); // true
```
