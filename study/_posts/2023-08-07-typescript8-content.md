---
layout: post
title: typescript 제대로 공부하기 - lib.es5.d.ts분석
description: >
  typescript 를 SPA 프레임워크에 적용하기위한 공부
image:
sitemap: false
---


zerocho의 타입스크립트 올인원:Part1.기본문법 편1,2를 듣고 정리한 내용이다.


## topic 🚀

1. 공변성과 반공변성
2. 타입오버로딩
3. 타입스크립트는 건망증이 심하다 😥


## lib.es5.d.ts분석


### 공변성과 반공변성


zerocho님이 알려주신 바대로 정리해보면 리턴값은 넓은 타입으로 대입이 가능하며, 반대로 매개변수는 좁은 타입으로 대입이 가능하다 👩‍🎤



```js

function a(x:string|number):number{
    return +x;
}

type b = (x:string)=> number | string;

const c:b=a;

```



### 타입오버로딩

타입오버로딩이란, 케이스별(상황에 따라) 똑같은 이름의 메서드를 작성해도 해당 케이스별 메서드가 동작할 수 있다는 의미이다. 

lib.es5.d.ts 파일에서 보면 filter함수가 두번 작성되어있는 것을 볼 수 있다. 이는 두가지 케이스에 의해 filter 함수가 쓰일 수 있다는 것을 의미한다.

```js
interface ReadonlyArray<T> {
//
 filter<S extends T>(predicate: (value: T, index: number, array: readonly T[]) => value is S, thisArg?: any): S[];
filter(predicate: (value: T, index: number, array: readonly T[]) => unknown, thisArg?: any): T[];

}

```



### 타입스크립트는 건망증이 심하다 😥

타입스크립트는 타입캐스팅 해주었다는 사실을 잊어먹는다. 이때, 이를 해결하기위해서는 as로 매번 타입캐스팅을 해주는 방법이 있는데 이는 매우 번거롭고 코드가 더러워질 수 있다. 이를 해결하기위해 확실한 typeGuard를 설정해야한다.


```js
interface Axios{
    get():void;
}
declare const axios:Axios;
class CustomError extends Error{
    response?:{
        data:any;
    }
}

(async()=>{
    try{
        await axios.get();
    }catch(error:unknown){
       // 1. as로 타입 캐스팅후 변수에 넣어주기 
       const customError = error as CustomError;
       // 2. if 문으로 확실하게 검사
       if(error instanceof CustomError){
            const customError = error;
            //....
       }
    }
})

```




