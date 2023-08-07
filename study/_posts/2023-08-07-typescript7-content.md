---
layout: post
title: typescript 제대로 공부하기 - typescript 기초 문법
description: >
  typescript 를 SPA 프레임워크에 적용하기위한 공부
image:
sitemap: false
---


zerocho의 타입스크립트 올인원:Part1.기본문법 편1,2를 듣고 정리한 내용이다.


## topic 🚀

1. 커스텀 타입 가드
2. {},Object,Object 🤷‍♀️
3. 인덱스드 시그니처, 맵드 타입스
4. js 클래스 !== ts 클래스
5. 옵셔널 
6. 제네릭
7. 기본값 타이핑


## typescript 기초 문법 정리하기

기초 문법을 정리해보자 👍

### 커스텀 타입 가드

is 문법은 타입을 확인하는 가드를 직접 구현할때 등장하는 문법이다.


```js
interface Human {think:string};
interface Animal {notThink:string};

function humanOrAnimal(a:Human | Animal): a is Animal { // custom Guard function
    if((a as Animal).notThink) return true;
    else return false;
}

function pet(a:Human | Animal){
    if(humanOrAnimal(a)){
        console.log('animal',a.notThink);
    }else{
        console.log('Human',a.think);
    }
}

```

zerocho님께서 실제 사용하는 코드와 비슷하게 예시로 들어준걸 보면 이해가 빠르다.

```js
const isRejected = (input:PromisedSettledReulst<unknown>):input is PromiseRejectedResult => {
    return input.status ==='rejected';
}

const isFulfilled = <T>(input:PromisedSettledResult<T>):input is PromiseFulfilledResult<T> => {
    return input.status='fulfilled';   
} 

const promises = await Promise.allSettled([Promise.resolve('a'),Promise.resolve('b')]);


// const errors = promises.filter(promise=>promise.status==='rejected') -> 이렇게 타이핑시, errors 값이 PromisedSettledResult로 나올때.. 
// PromiseRejectedResult라는 값으로 추론안될때 💩


const errors = promises.filter(isRejected); // 👍

export {};


```



### {},Object,Object 🤷‍♀️

typeScript v4 이후로 등장한 문법이다. 
1. {},Object => any와 같은 모든 타입 (단, null 과 undefined는 제외) ‼
2. object => real 객체를 의미한다.


```js
const x:{}=123;
const y:Object='hi';
const zz:object={isStudying:true};
const z:unknown = 'hi';
if(z){// unknown을 if 문안에 넣으면 true일때 any가 된다.
    z; // {},any;
}else{
    z; // null | undefined
}

```



### 인덱스드 시그니처, 맵드 타입스

인덱스드 시그니처는 key들의 타입을 하나로, value의 타입을 하나로 정할 수 있다. 


```js
interface A {
    readonly a:string;
    b:string;
}
type a = {[key:string]:number};
```

맵드타입스는 타입을 구체적으로 제한하여 설정할 수 있다.

```js
type B = 'javascript' | 'python' |'typescript';

type b = {[key in B]:B};
const realB = {javascript:'python'};
```




### js 클래스 !== ts 클래스

protected, private와 같은 문법은 typescript에서만 제공하는 기능이기때문에 ts를 js로 변환시 사라지게 된다.

#### 클래스 tip

```js
class A {
    private a:string;
    b:number;
    constructor(a:string,b:number=123){
        this.a = a;
        this.b = b;
    }

}
const a = new A('hi',123);
type a = A; // 클래스 자체의 이름은 인스턴스를 가리킨다.
const b:a = new A('123'); 
const d:typeof A = A; // typeof 클래스는 클래스 자체의 타입을 가리킨다.

```


#### private, protected


```js
interface A{
    readonly a:string
    b:string
}

class B implements A {
    a:string ='hi';
    b:string='hello';
    private e:string='hihi'
    protected c:string='wow'; 
    method(){
        console.log('a',this.a);
        console.log('b',this.b);
        console.log('c',this.c);
    }
}

class C extends B{
    method(){
        console.log(this.e); // 💩 ->private이므로 접근 불가
        console.log(this.a);
        console.log(this.b);
        console.log(this.c);
    }
}

new B().a;
new B().b;
new B().c; // 💩 ->protected 접근 불가
new B().e; // 💩 ->private 접근 불가
```

사용가능 여부를 표로 정리해보면 다음과 같다.


||public|protected|private|
|---|------|---|---|
|클래스내부|O|O|O|
|인스턴스에서(new)로 생성시|O|X|X|
|상속클래스에서|O|O|X|




```js
type A = {name:string};
type B = {age:string};

type AB = A | B // {name:string} 이거나 {age:string} 이거나 {name:string,age:string} 이거나
type C =  A & B // {name:string,age:string}

const a:AB = {name:'heywon0909',age:26};

// 💩
const c:C = {name:'heywon0909',age:26,height:'167'}; // height는 C 타입의 키값에 없는 키이므로 에러가 난다 👩‍🎤

// 👍
const obj = {name:'heywon0909',age:26,height:'167'}
const c_solution:C = obj;

```

### 옵셔널

옵셔널은 필수로 있어야하는 값이 아니라 있어도 되고 없어도 되는 선택적인 값을 의미한다.


```js
const add:(x:number,y?:number)=>number = (x,y)=>{
    return x+y;
}

add(1); // 👍-> y가 optional이므로 없어도 가능 
add(1,2); // 👍 -> y가 optional이므로 있어도 가능 
add(1,2,3); // 💩

```

### 제네릭

변수처럼 쓸 수 있는 타입으로 함수를 실제로 사용할때 타입이 정해진다.

```js
function add<T>(x:T,y:T){}
add(1,2); // x,y -> number 타입
add('1','2'); // x,y -> string 타입

```

제네릭 타입을 쓰는 방법은 다음과 같다. 👩‍🎤
1. <T extends {...}>
2. <T extends any[]>
3. <T extends (...args:any)=>any>
- 모든 함수를 나타낼때 사용한다.
4. <T extends abstract new (...args:any)=>any>
- 생성자만 뽑고 싶을때!, 클래스 자체로 제한하고 싶을떄 사용한다.

### 기본값 타이핑

React에서는 jsx문법으로인해 <>이 기본 typescript문법이 작동하지 않을때가 있다. 이때, 기본 값을 타이핑해주면 에러를 막을 수 있다.

```js
const add= <T>(x:T,y:T)=>({x,y}); // 기본 ts에서 동작 잘됨... React에서는 안됨...


// React에서 기본값 타이핑 하는 방법 🔎
const add = <T,>(x:T,y:T)=>({x,y});
const add = <T=unknown>(x:T,y:T)=>({x,y});
const add = <T extends unknown>(x:T,y:T)=>({x,y});


const result = add(1,2)
```

