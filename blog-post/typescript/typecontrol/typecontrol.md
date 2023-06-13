---
slug: "/typescript/typecontrol"
date: "2023-06-12"
title: "타입 조작하기"
categories: ["Typescript"]
desc: "원래 타입에서 새로운 타입으로 조작하기"
topbg: "../topbg.png"
thumbnail: "../thumbnail.png"
---

## 1. 타입 조작이란?
- 원래 있던 타입들을 상황이나 조건에 따라 유동적으로 다른 타입으로 변경하는 타입스크립트의 특수한 기능
- 제네릭도 타입을 조작할 수 있는 기능중에 하나이다.

## 2. 인덱스드 엑세스 타입(Indexed access type)
- 인덱스를 이용하여 다른 타입내에 특정 프로퍼티 타입을 추출하는 타입

 ### 객체 프로퍼티 타입 추출

```ts {numberLines}
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
  };
}

const post: Post = {
  title: "post title",
  content: "main",
  author: {
    id: 1,
    name: "user1",
  },
};
```

- author의 프로퍼티를 출력하는 함수
```ts {numberLines}
function printAuthorInfo(author: { name: string; id: number;}) {
  console.log(`${author.name}-${author.id}`);
}
```
만일 author의 프로퍼티를 새로 추가한다면 author 타입도 추가해야 하므로 불편하다.

```ts {numberLines}
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number // new property
  };
}

function printAuthorInfo(author: { name: string; id: number; age: number}) {}
```

이러한 번거로움을 해결하기 위해 indexed access type을 이용하면 된다.

```ts {numberLines}
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number // new property
  };
}

function printAuthorInfo(author: Post["author"]) {
  console.log(`${author.name}-${author.id}`);
  // author 타입은 {id:number; name:string; age:number}
}
```
- `Post["author]` Post타입으로부터 author 프로퍼티 타입을 추출
- author에 새로운 프로퍼티를 선언해도 Post의 author 프로퍼티로부터 타입을 자동적으로 추론한다.

#### 주의
- 객체에서 indexed aceess type을 사용할때 대괄호에 변수나 해당하지 않는 프로퍼티를 사용하면 에러가 발생한다.

```ts {numberLines}
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number;
  };
}

const authorKey = "author";

function printAuthorInfo(author: Post[authorKey]) { // ❌
  console.log(`${author.id} - ${author.name}`);
}

function printAuthorInfo(author: Post["what"]) { // ❌
  console.log(`${author.id} - ${author.name}`);
}
```

#### 중첩 사용

```ts {numberLines}
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number;
  };
}


function printAuthorInfo(author: Post["author"]["id"]) {
  console.log(`${author.id} - ${author.name}`);
}
// author의 매개변수 타입은 객체 타입에서 number 타입으로 변경 된다.
```


### 배열 요소에서 타입 추출

```ts {numberLines}
type Postlist = {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number;
  };
}[];
```
배열의 한 요소의 객체 author의 타입을 추출
```ts {numberLines}
function printAuthorInfo(author: Postlist[number]["author"]) {
  console.log(`${author.id} - ${author.name}`);
} 
```

배열 한 요소의 타입 추출
```ts {numberLines}
const post: Postlist[number] = {
  title: "post title",
  content: "main",
  author: {
    id: 1,
    name: "user1",
    age: 18
  },
};  // ✅

const post: Postlist[0] = {
  title: "post title",
  content: "main",
  author: {
    id: 1,
    name: "user1",
    age: 18,
  },
}; // ✅
```
index에 들어가는 것은 타입만 올 수 있고 나머지는 올 수 없음
```ts {numberLines}
const num = 0;

const postlist3: Postlist[num] = {
  title: "post title",
  content: "main",
  author: {
    id: 1,
    name: "user1",
    age: 18,
  },
}; // ❌ 
```


### 튜플 요소에서 타입 추출
튜플의 index literal number 타입 추출
```ts {numberLines}
type Tup = [number, string, boolean];

type Tup0 = Tup[0]; // Tup0은 number type으로 추출
type Tup1 = Tup[1]; // Tup1은 string type으로 추출
type Tup2 = Tup[2]; // Tup2은 boolean type으로 추출
type Tup3 = Tup[3]; // ❌ Tup[3]의 요소가 없음
```

튜플의 index number 타입 추출
```ts {numberLines}
type Tup = [number, string, boolean];
type TupNum = Tup[number]; // string | number | boolean의 유니언타입으로 최적의 공통 타입 추출
```


## referance

- [한입 타입스크립트 핸드북](https://ts.winterlood.com/)