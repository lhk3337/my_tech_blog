---
slug: "/typescript/class"
date: "2023-06-23"
title: "class type"
categories: ["Typescript"]
desc: "타입스크립트에서 class 타입 선언하기"
topbg: "../topbg.png"
thumbnail: "../thumbnail.png"
---


## Javascript에서 클래스란?

클래스는 자바스크립트의 같은 프로퍼트의 객체를 여러개 생성할 수 있는 문법

만일 똑같은 프로퍼티 형태의 여러개의 객체를 생성하면 코드가 비효율적이고 중복이 발생한다.

```js {numberLines}
let studentA = {
  name: "user1",
  grade: "A",
  age: 22,
  study() {
    console.log("study hard");
  },
  introduce() {
    console.log("Hello");
  },
};

let studentB = {
  name: "user2",
  grade: "B",
  age: 20,
  study() {
    console.log("study hard");
  },
  introduce() {
    console.log("Hello");
  },
};

let studentC = {
  name: "user3",
  grade: "C",
  age: 21,
  study() {
    console.log("study hard");
  },
  introduce() {
    console.log("Hello");
  },
};
```
객체 studentA, studentB, studentC 객체는 프로퍼티형태가 같고 코드도 길어지고 중복이 발생한다.  

### 클래스 선언
하지만 클래스를 사용하면 간결하고 여러개의 객체를 생성할 수 있다.

```ts {numberLines}
class Student {
  // 필드
  name;
  grade;
  age;

  // 생성자
  constructor(name, grade, age) {
    this.name = name;
    this.grade = grade;
    this.age = age;
  }

  study() {
    console.log("study hard");
  }
  introduce() {
    console.log(`Hello I am ${this.name}.`);
  }
}

// 클래스를 이용하여 만든 객체 -> 인스턴스
// student instance
let studentA = new Student("user1", "A", 22);
let studentB = new Student("user2", "B", 20);
let studentC = new Student("user3", "C", 21);
```
객체로 생성한 것보다 코드가 더 간결해졌다. <br /><br />


### 클래스 상속
또한 상속을 사용하여 중복된 필드를 재사용할 수 있어 중복으로 같은 필드를 생성하지 않아도 된다.


```js {numberLines}
class Student {
  // 필드
  name;
  grade;
  age;

  // 생성자
  constructor(name, grade, age) {
    this.name = name;
    this.grade = grade;
    this.age = age;
  }

  study() {
    console.log("study hard");
  }
  introduce() {
    console.log(`Hello I am ${this.name}.`);
  }
}


// 클래스 상속
class StudentDeveloper extends Student {
  // 필드
  favoriteSkill;

  // 생성자
  constructor(name, grade, age, favoriteSkill) {
    super(name, grade, age); 
    // Student 클래스의 필드를 상속 받고 있기 떄문에 현재의 생성자 함수에서 3개의 생성자를 선언하지 않음
    this.favoriteSkill = favoriteSkill;
  }

  // 메서드
  study() {
    console.log("study hard");
  }
  introduce() {
    console.log(`Hello I am ${this.name}.`);
  }
  programming() {
    console.log(`my best program-lang is ${this.programming}`);
  }
}

const studentDeveloper = new StudentDeveloper("users", "A+", 30, "Typescript");
console.log(studentDeveloper);
```


## class에서 타입 선언하기
 ```ts {numberLines}
class Employee {
  // 필드
  name: string;
  age: number;
  position: string;

  // 생성자
  constructor(name: string, age: number, position: string) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log("working");
  }
}

const employeeB = new Employee("user2", 22, "UI");
console.log(employeeB);
 ```
클래스에서  필드와 생성자의 매개변수에 타입을 선언하면 된다. <br /> <br />

또한 클래스는 타입으로 선언할 수 있어서 아래와 같이 사용할 수 있다.

```ts {numberLines}
const employeeC: Employee = {
  name: "",
  age: 0,
  position: "",
  work() {},
};
```
이때  객체의 `name`, `age`, `position`, `work`의 프로퍼티를 모두 선언하지 않을 경우 에러가 발생한다.


### 상속 시 타입 선언하기

```ts {numberLines}
class Employee {
  // 필드
  name: string;
  age: number;
  position: string;

  // 생성자
  constructor(name: string, age: number, position: string) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log("working");
  }
}

class ExecutiveOfficer extends Employee {
  officeNumber: number;
  constructor(name: string, age: number, position: string, officeNumber: number) {
    super(name, age, position); 
    // ts에서 super를 선언하지 않으면 에러가 발생한다. 
    // js에서는 생략해도 에러가 발생하지 않는다.
    this.officeNumber = officeNumber;
  }
}

const executiveOfficer = new ExecutiveOfficer("own", 40, "ceo", 1);
```

## referance

- [한입 타입스크립트 핸드북](https://ts.winterlood.com/)