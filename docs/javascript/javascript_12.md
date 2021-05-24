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

## prototype으로 상속 구현하기

### 예제1

```js
const Person = function (name, birthYear) {
    this.name = name;
    this.birthYear = birthYear;
};

Person.prototype.calAge = function () {
    console.log(2021 - birthYear);
};

const Student = function (name, birthYear, course) {
    Person.call(this, name, year);
    this.course = course;
};

// 부모의 propertype 객체를 Object.create() 로 생성해주면 Stududent의 객체가  __propto__ 값으로 Person.prototype을 갖는다
// mike.__proto__ === Studuent.prototype
// mike.__proto__.__proto__ === Person.prototype
// mike.__proto__.__proto__.__proto__ === Object.prototype
// mike.__proto__.__proto__.__proto__.__proto__ === null
Student.prototype = Object.create(Person.prototype); // __proto__값이 가르킬 값을 지정
// Student.prototype === mike.__proto__
Student.prototype.constructor = Student; // 자기 자신을 가르켜야함
Student.prototype.introduce = function () {
    console.log(`My name is ${this.name} and my course is ${this.course}`);
};

const mike = new Student("Mike", 2010, "Computor");
mike.introduce(); // Student의 메소드
mike.calAge(); // Person의 메소드
```

### 예제2

```js
const Car = function (maker, speed) {
    this.maker = maker;
    this.speed = speed;
};

Car.prototype.accelerate = function () {
    this.speed += 20;
    console.log(`${this.maker} going at ${this.speed} km/h`);
};

const Ev = function (maker, speed, charge) {
    Car.call(this, maker, speed);
    this.charge = charge;
};
Ev.prototype = Object.create(Car.prototype);
Ev.prototype.constructor = Ev;
Ev.prototype.chargeBattery = function (chargeTo) {
    this.charge = chargeTo;
};
// 다향성
Ev.prototype.accelerate = function () {
    this.speed += 20;
    this.charge -= 1;
    console.log(
        `${this.maker} going at ${this.speed} km/h with a charge of ${this.charge}%`
    );
};

const tesla = new Ev("Tesla", 120, 23);
tesla.chargeBattery(90);
tesla.accelerate();
tesla.accelerate();
```

### 예제3

-   Es6 클래스 문법보다 Object.create()로 만들면 좀더 명확함
-   ES6 클래스는 실제 클래스가 아님

```js
const PersonProto = {
    calcAge(){
        console.log(2037 - this.birthYear);
    }

    init(fullName, birthYear){
        this.fullname = fullName;
        this.birthYear = birthYear;
    }
};

const StudentProto = Object.create(PersonProto);
StudentProto.init = function(firstName, birthYear, course){
    PersonProto.init.call(this, firstName, birthYear);
    this.course = course;
}

StudentProto.introduce = function(){
    console.log(`My name is ${this.fullName} and I study ${this.course}`)
}

const jay = Object.create(StudentProto);
jay.init('Jay', 2010, 'Computor');
jay.introduce();
jay.calcAge();
```

## ES6 상속

```js
class Person {
    constructor(name, birthYear) {
        this.name = name;
        this.birthYear = birthYear;
    }

    calcAge() {
        console.log(2021 - this.birthYear);
    }
}

class Student extends Person {
    // [생략가능]
    // this를 새로 정의하려면 super도 같이 써줘야한다
    // this를 새로 정의하지 않으면 모두 생략해도 됌

    // constructor(name, birthYear, course) {
    //     super(name, birthYear);
    //     this.course = course;
    // }
    introduce() {
        console.log(`My name is ${this.name} and my course is ${this.course}`);
    }
}

const mike = new Student("Mike", 1991, "Computor");
mike.introduce();
mike.calcAge();
```
