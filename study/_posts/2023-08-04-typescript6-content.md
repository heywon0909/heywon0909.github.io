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

1. 추론이 구체적일때는 type 선언을 자제하자.
2. type,interface
3. 빈배열은 type이 never이므로 type 선언을 해주자.
4. Enum과 Object
5. string과 String은 다른 객체이다!
6. & 와 | 의 차이
7. 객체에서의 좁은 타입과 넓은 타입
8. void 타입 선언의 방식 
9. declare
10. any와 unknown

## typescript 기초 문법 정리하기

기초 문법을 정리해보자 👍

### type과 interface

type과 interface는 쉽게 말해 어떤 변수나 객체등의 template을 만드는 것이다.
이런 template 선언으로 해당 변수나 객체의 타입으로 선언해줄 수 있는데, 둘이 동일한 기능을 수행하지만, 약간의 차이가 존재한다.  

```js
const a:string = 'hello';
const b:number = 5;
```



### 추론이 구체적일때는 type 선언을 자제하자.

아래 예시 처럼 간단한 const 변수 선언일때, typescript가 타입 추론을 해준다. 
이때 a는 const로 선언되어 변수에 값이 재할당될 수 없는 아이로 'hello'값만 가지게 된다. 이러한 상황에서 string으로 타입을 선언해버리면 'hello'이외의 string 문자열을 타입으로 갖는다는 의미가 되므로 타입이 확장되어버린다. 타입 추론이 구체적으로 이루어 질때는 최대한 type 선언을 자제하는 것이 좋다. 

1. interface는 확장이 가능하다.

type은 불가능하나 interface는 동일한 이름으로 여러번 선언한 후 한번에 묶을 수 있다. => 이를 확장성이 좋다는 것을 의미한다. 대부분의 라이브러리에서 interface를 사용하여 확장성을 넓히는 방식을 택한다.

```js
interface Animal {
    eat: () =>void;
}
interface Animal {
    sleep:()=>void;
}
interface Animal {
    play:()=>void;
}

const cat:Animal = {eat:()=>{},sleep:()=>void,play:()=>void;}

```

2. interface는 extends, type은 intersection 으로 둘다 상속이 가능하다. 

```js
type A = {gender:string};
type B = {height: string};

type AB = A | B;
const human:AB = {genger:'woman'};

type C = A & B;
const anotherHuman:C ={gender:'woman',height:'150m'}; 

```



### 빈배열은 type이 never이므로 type 선언을 해주자.

배열을 선언할때 무조건 어떠한 타입의 배열인지 구체적으로 적어주어야한다. 타입을 명시하지 않으면 never[]이 되어버리므로 구체적으로 명시해주자.


```js
const arr:string[]=['1','2','3'];
```

- 중첩된 객체의 키값이 존재하지 않을 경우 오류가 발생할 수 있기때문에 확인한 후에 작업을 해야한다.


### string과 String은 다른 객체이다!

String 대신 string을 쓰자. 문자열 변수를 선언할때, new String() 생성자가 아닌 string문자열로 선언하므로, 타입을 String이 아닌 string으로 선언해주자. 

```js
const a:string ='👍';
const b:String = '💩';

```

### & 와 | 의 차이

&는 교집합 |는 합집합의 의미라고 생각하면 된다. 

```js
type A = {name:string};
type B = {age:string};

type AB = A | B // {name:string} 이거나 {age:string} 이거나 {name:string,age:string} 이거나
type C =  A & B // {name:string,age:string}

```

### 객체에서의 좁은 타입과 넓은 타입

좁은 객체란 더 구체적인 객체를 의미하고 넓은 타입이란 그와 반대의 경우라고 생각하면 된다. 
여기서 보면 C ⊂ A, C ⊂ B 의미 즉, A보다 C가 더 많은 속성을 가지고 있으므로 구체적이다. A가 C보다는 넓은 타입, C는 A보다 좁은 타입이라고 얘기한다.


```js
type A = {name:string};
type B = {age:string};

type AB = A | B // {name:string} 이거나 {age:string} 이거나 {name:string,age:string} 이거나
type C =  A & B // {name:string,age:string}

```

typescript를 쓰다보면 넓은 타입을 좁은 타입에 넣거나, 좁은 타입을 넓은 타입에 넣어야하는 경우가 생기는데, 
넓은 타입을 좁은 타입에 대입 (💩)
좁은 타입을 넓은 타입에 대입 (👍) 으로 
좁은 타입을 넓은 타입에 대입하는 것만 가능하다.

변수를 객체에 바로 대입할때는 잉여속성 검사를 한다. 잉여속성 검사란 변수의 타입에 키이외의 속성이 있는지 검사하는 것을 의미한다. 

이러한 상황에서, 변수에 바로 객체를 대입하지 말고, 다른 변수의 값을 할당해주면 에러가 나지 않는다. 


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

### void 타입 선언의 방식

void 타입 선언의 방식은 다음과 같다.

1. 함수에서 return 값의 타입이 void
- 여기서의 void의 의미는 반환값이 없다의 의미 
2. 매개변수에 return 값이 void인 함수 넣기
3. 객체의 메서드에 void 함수
- 2,3번에서의 void의 값은 반환값이 없다의 의미가 아닌, 반환값을 사용하지 않겠다는 의미로 return 값이 있어도 에러가 나지 않는다.


```js
const add:(x:number,y:number)=>void = (x,y)=>{
    // return x+y ->  에러가 남 💩
    // return ; -> 에러가 나지 않음 👍
}

function example(callback:()=>void):void{

}
exmaple(()=>{return 3}) // 에러가 나지 않음 👍

interface Study {
    coding: () => void;
}

const study:Study = {
    coding: () => {return 'typescript'} // 에러가 나지 않음 👍
}

```

### declare

declare는 타입만 만들어준다는 의미로 이미 다른  파일에서 선언되어있는데 현재 파일에서 사용하려고 할때, 다른 파일에 선언되있음을 보증할때 사용한다.

```js
declare add:(x:number,y:number)=>void

declare study:Study;

```

### any와 unknown

any는 모든 타입이 다 해당 되는 타입으로 ts 파일에서 any를 선언한다는 것은 type 선언을 포기한다는 의미이다.
unknown은 지금 당장은 type을 몰라도 나중에 직접 typing 해서 쓰려고 할때 선언하는 타입이다.

```js
interface Animal {
    eat:()=>void;
}
const cat:Animal = {
    eat:()=>{return 3};
}

const c:unknown = cat.eat(); // eat은 객체의 메세드이므로 return 이 어떤 타입이든 간에 상관없이 void이다.
c.method() // c가 any타입이면 에러가 나지 않는다.

(c as Animal).eat(); // as를 사용해서 type을 바꿔준다.

```

🤷‍♀️ 단, as는 unknown 사용시 type을 설정해 줄때에만 사용해야한다...!