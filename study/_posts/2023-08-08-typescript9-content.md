---
layout: post
title: typescript 제대로 공부하기 - Utility Types
description: >
  typescript 를 SPA 프레임워크에 적용하기위한 공부
image:
sitemap: false
---


zerocho의 타입스크립트 올인원:Part1.기본문법 편1,2를 듣고 정리한 내용이다.


## topic 🚀

1. Utility Types



## Utility Types 분석

Utility Types에 대해서 알아보자 👩‍🎤

### Utility Types란


Utility Types란, Types 변환을 쉽게 해주는 추가 아이템이다. 유틸리티는 전역으로도 사용이 가능하다. 


강의를 듣고 강의자료와 typeScript 공식 문서를 활용하여 작성하였다.


### Partial<Type>

주어진 프로퍼티를 선택적으로 생성하도록 만든다.

```js
// 타입
type Partial<T> = {
    [P in keyof T]?: T[P];
};

// 예시

interface Buying {
  water:boolean;
  cookie:boolean;
  makeup:boolean;
  food:boolean;
}

function isBuying(buying: Buying, fieldsToUpdate: Partial<Buying>) {
  return { ...buying, ...fieldsToUpdate };
}

const buying1 = {
  water:true,
  cookie:false,
  makeup:true,
  food:true
};

const todo2 = isBuying(buying1, {
  water:false
});

```


### Required<Type>

주어진 프로퍼티중 선택적으로 택할 수 있는 프로퍼티를 필수로 바꾼다.

```js
// 타입
type Required<T> = {
    [P in keyof T]-?: T[P];
};


// 예시
interface User{
    age?: number;
    name: string;
    addr?: string;
}

const human: Required<User> = {age:23,name:'her',addr:'seoul'}


```

### Readonly<Type>

주어진 프로퍼티들은 읽기전용이다. 쓰기 전용 아님..!

```js
// 타입
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};


// 예시
interface User{
    age?: number;
    name: string;
    addr?: string;
}

const human: Readonly<User> = {age:23,name:'her',addr:'seoul'};

human.age = 33 // 💩 읽기전용이므로 쓰기 안됨


```



### Record<Type>

Type의 프로퍼티의 Key들의 집합으로 타입을 생성한다. 타입의 프로퍼티를 다른 타입에 매핑시키는데 사용된다.

```js
// 타입
type Record<T, K extends keyof T> = {
    [P in K]: T[P];
};


// 예시
type Student ="high" | "middle" |"element";
interface User{
    age?: number;
    name: string;
    addr?: string;
}

const student:Record<Student,User> = {
  high:{age:12,name:'exam1',addr:'seoul'},
  middle:{name:'exam2',addr:'incheon'},
  element:{name:'exam3'}

}



```

### Pick<Type, Keys>

Type에서 프로퍼티의 Keys들중 선택해서 타입을 생성한다.

```js
// 타입
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};


// 예시
type Student ="high" | "middle" |"element";
interface User{
    age?: number;
    name: string;
    addr?: string;
}

const student:Record<Student,User> = {
  high:{age:12,name:'exam1',addr:'seoul'},
  middle:{name:'exam2',addr:'incheon'},
  element:{name:'exam3'}
}

const onlyAge:Pick<User,"age"> ={
  age:123
}

```

### Exclude<Type, ExcludedUnion>

ExcludedUnion에 속하지 않는 Type으로 타입을 생성한다.

```js
// 타입
type Exclude<T, U> = T extends U ? never : T;


// 예시
type T = Exclude< "high"|"middle"|"element","middle" >;
// "high" | "element"


```

### Extract<Type, Union>

Union에 속하는 Type을 가져와서 타입을 생성한다.


```js
// 타입
type Extract<T, U> = T extends U ? T : never;


// 예시
type T = Extract<"high"|"middle"|"element","element"|"high">;
// "element" | "high"

```




### Omit<Type, Keys>

Type에서 모든 프로퍼티를 선택하고 key를 제거한 타입생성한다.

```js
// 타입
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;


// 예시
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
// 1.
type TodoPreview = Omit<Todo, "description">; 
// 2. 
type TodoPreviewEx = Pick<Todo,Exclude<keyof Todo,"description">>;

//  1번과 2번은 같다.

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
const todoEx: TodoPreviewEx = {
  title:"shower",
  completed:true
}

todo;
// ^?

```



### NonNullable<Type>

Type에서 null과 undefined을 제거하고 타입을 생성한다.

```js
// 타입
type NonNullable<T> = T & {};


// 예시
type T = NonNullable<string|number|undefined>;
// string | number 

```




