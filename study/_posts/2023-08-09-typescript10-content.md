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

1. never 타입은 distributed가 안된다.
2. 공변성과 반공변성
3. 잉여속성검사
4. ambiant
5. 데코레이터 



## 보너스 분석 

이제 올인원:Part1을 완강했다 🤗

### never 타입은 distributed가 안된다.


Optional 타입에서 보면 

```js
// 타입
type IsNever<T> = T extends never ? true : false;
type A = IsNever<never> 

```
여기서 A의 값이 true가 아닌 never가 된다. 이는 never는 공집합이므로 distributed(분배)가 일어나지 않아서 never extends never을 true 나 false 값으로 분배하지 못하기 때문에 아무일도 일어나지 않아서 never 값이 되는 것이다. 

그러면 어떻게 써야 이를 해결 할 수 있을까? 

```js
// 타입
type IsNever<T> = [T] extends [never] ? true : false;
type A = IsNever<never> 

```
이렇게 써줘야지만 A의 타입이 원하는 결과의 값을 얻어낼 수 있다.


### 공변성과 반공변성

> 리턴값은 넓은 타입으로 대입이 가능하고(공변성),매개변수는 좁은 타입으로 대입이 가능하다.(반공변성)

이를 활용하여 교집합과 합집합 타입을 만들어 낼 수 있다.

이 예시는 공변성을 활용한 예시로, infer로 추론해보도록 만든 U의 값은 둘의 합집합인 1|2|3이 된다.
```js
// 타입
type Union<T>  = T extends {a:()=>infer U,b:()=>infer U} ? U :never;
type Result = Union<{a:()=>1|2,b:()=>2|3}>;
```

이 예시는 반공변성을 활용한 예시로, infer로 추론해보도록 만든 U의 값은 둘의 교집합인 2가 된다.

```js
// 타입
type Intersection<T>  = T extends {a:(pa:infer U)=>void,b:(pb:infer U)=>void} ? U :never;
type Result = Intersection<{a:(pa:1|2)=>void,b:(pb:2|3)=>void}>;
```


### 잉여속성검사

잉여속성검사란 해당 타입을 만족하는 객체를 생성하면, 타입이외의 속성을 객체가 가지고 있는지 검사를 하므로 바로 선언을 하면 에러가 난다.
이때 객체를 만든 후, 타입을 만족하는 변수에 객체를 참조하게 하면 에러가 나지 않는다.

```js
// 타입
interface VO{
    value:any;
}
const obj:VO = {value:'hi',name:'sdf'} // 💩 잉여속성검사로 에러가 나게됨

// 👍 => 에러가 나지 않음
const ex = {value:'hi',name:'sdf'}
const obj1:VO = ex;



```




### ambiant

ambiant는 declare 선언을 의미한다. 이때 구현부를 직접 구현하지 않아도 되며, 외부에 이를 구현한 javaScript가 존재할때, d.ts파일을 만들어서 존재여부를 확증하기 위해서 사용한다.


ambiant는 동일한 이름으로 선언이 가능한 경우가 존재한다. => 이를 선언을 병합할 수 있다고 말한다.

```js
// 타입
declare class A{
    constructor(name:string);
}
function A(name:string){
    return new A(name);
}
new A('zerocho');
A('zerocho');



```

### 데코레이터

데코레이터는 `@데코레이터이름` 형식으로 함수나 클래스를 장식하는 것을 도와주는 장치이다. 쉽게 말해 함수나 클래스를 좀더 유연하게 쓸 수 있는 공통적인 기능을 제작할 수 있게 해주는 도구이다.


```js
function startAndEnd<This,Args extends any[],Return>(originalMethod:(this:This,...args:Args)=>Return,context:ClassMethodDecoratorContext<This,(...args:Args)=>Return>){
    if(context.kind==='method'){
      return function replacementMethod(this:This,...args:Args){
        console.log(context.name);
        const result = originalMethod.call(this,...args);
        return result;
      }
    }
    return originalMethod;
  }

export class A{
    @startAndEnd
    eat(){
        console.log('eat');
    }
    @startAndEnd
    work(){
        console.log('work');
    }
    @startAndEnd
    sleep(){
        console.log('sleep');
    }
}

```

이렇게 쓰면 eat,work,sleep 시작할때마다  console.log('start'); => originalMethod => console.log('end'); 로 끝나게 된다.

이렇게 도 쓸 수 있다. 

대신 start과 end값을 주지 않으면 원래대로 console.log('start'); => originalMethod => console.log('end'); 로 끝나게 되고 start 와 end 값을 주면 준 값으로 변경되어 호출한다.


```js
function startAndEnd(start='start',end='end'){
  return function DealDecorator<This,Args extends any[],Return>(originalMethod:(this:This,...args:Args)=>Return,context:ClassMethodDecoratorContext<This,(...args:Args)=>Return>){
    if(context.kind==='method'){
      return function replacementMethod(this:This,...args:Args){
        console.log(context.name);
        const result = originalMethod.call(this,...args);
        return result;
      }
    }
    return originalMethod;
  }
}
export class A{
    @startAndEnd('start','end')
    eat(){
        console.log('eat');
    }
    @startAndEnd('시작','끝')
    work(){
        console.log('work');
    }
    @startAndEnd()
    sleep(){
        console.log('sleep');
    }
}

```

bind라는 데코레이터를 통해서 class 내부의 constructor에서 bind하는 반복작업을 줄여줄 수도 있다. 

log 데코레이터를 활용해서 class 만들어서 C클래스의 eat,work,sleep 기능과 log 메서드 기능을 쓸 수 있게 할 수도 있다. 

```js
function bound(originalMethod:unknown,context:ClassMethodDecoratorContext<any>){
  const methodName = context.name;
  if(context.kind==='method'){
    context.addInitializer(function(){
      this[methodName] = this[methodName].bind(this);
    })
  }
}

function log<Input extends new(...args:any[])=>any>(value:Input,context:ClassDecoratorContext){
  if(context.kind==='class'){
    return class extends value {
      constructor(...args:any[]){
        super(args);
      }
      log(msg:string):void{
        console.log(msg);
      }
    }
  }
}

export @log class C{
  @startAndEnd()
  @bound
  eat(){
    console.log('eat');
  }
  @bound
  @startAndEnd('시작','끝')
  work(){
    console.log('work');
  }
  @startAndEnd()
  sleep(){
    console.log('sleep');
  }
}

```