# 3장, lib.es5.d.ts 분석하기

### 3.1 Partial, Required, Readonly, Pick, Record

1. Partial

- 기존 객체의 속성을 전부 **옵셔널**로 만들어 버리는 함수

```jsx
type Partial<T>={
	[P in keyof T]?:T[P]
}
```

1. Required

- 반대로 모든 속성값을 필수로 만들어버리는 함수

```jsx
type Required<T>={
	[P in keyof T]-?:T[P];
}
```

1. Readonly

- 모든 속성을 readonly로 만들어버리기

```jsx
type Readonly<T>={
	readonly [P in keyof T]:T[P];
}
```

- 모든 속성을 readonly가 아니게 만들어버리기

```jsx
type NotReadonly<T>={
	-readonly [P in keyof T]: T[P];
}
```

1. Pick

- 지정한 속성 값만 추리기

```jsx
type Pick<T,K extends keyof T> = {
	[P in K]:T[P];
}
```

- K값이 T안에 속하지 않은 key 값이 들어갈 때, 지정한 속성 값만 추리기

```jsx
	type Pick<T,K> = {
		[P in (K extends keyof T ? K : never)]:T[P]
 }
```

1. Record

- 모든 속성의 타입이 동일한 객체의 타입만들기

```jsx
type Record<K extends keyof any,T> = {
	[P in K]: T;
}
```

### 3.2 Exclude, Extract, Omit, NonNullable

- 컨디셔널 타입일 때, 유니언인 기존 타입과 제네릭이 만나면 분배법칙이 실행된다

1. Exclude

- 어떠한 타입에서 지정한 타입을 제거하는 함수

```jsx
type Exclude<T,U> = T extends U ? never : T;
type Result = Exclude<1|'2'|3,string>;
```

1. Extract

- 어떠한 타입에서 지정한 타입만 추리는 함수

```jsx
type Extract<T,U> = T extends U ? T : never;
type Result<1|'2'|3,string>; // '2'
```

1. Omit

- 특정 객체에서 지정한 속성을 제거하는 함수
  - Pick 과 Exclude 타입을 활용

```jsx
type Omit<T,K extends keyof any> = Pick<T,Exclude<keyof T,K>>;

type Result = Omit<{a:'1',b:2,c:true},'a'|'c'>;
```

1. NonNullable

- 예전 버전

```jsx
type NonNullable<T> = T extends null | undefined ? never : T;
```

- 현재
  - {} 는 null과 undefined를 포함하지 않는 타입
  - T 와 {}의 교집합

```jsx
type NonNullable<T> = T & {};
```

### 3.3 Parameters, ConstructorParameters, ReturnType, InstanceType

1. Parameters

- 함수 매개변수 타입

```jsx
type Parameters<T extends (...args:any)=>any> =
T extends (...args:infer P)=> any ? P : never;
```

1. ConstructorParameters

- 생성자 함수의 매개변수 타입
  ```jsx
  type ConstructorParameters<T extends abstract new (...args:any)=>any> =
  T extends abstract new (...args:infer P)=> any ? P : never;
  ```

1. ReturnType

- 함수 반환 값 타입

```jsx
type ReturnType<T extends (...args:any)=>any> =
T extends (...args:any)=> infer R ? R : any;
```

1. InstaceType

- 생성자 함수로 만든 인스턴스 타입

```jsx
type InstanceType<T extends abstract new (...args:any)=>any> =
T extends abstract new (...args:any)=> Infer R ? R : any;
```

### 3.4 ⚪ ThisType

- 메서드들에 **this**를 정해주는 타입
  - lib.ex5.d.ts에 정의되어있지 않음 ⇒ 내부 구현이 특별하게 처리되어 있음

```jsx
type Data = {money:number};
type Methods = {
	addMoney(amount:number):void;
	useMoney(amount:number):void;
}
type Obj = {
	data: Data;
	methods: Methods & **ThisType<Data & Methods>**;
}
const obj:Obj = {
	data:{
		money: 0,
	},
	methods:{
		addMoney(amount){
			this.money += amount;
		}
		useMoney(amount){
			this.money -= amount;
		}
	}
}
```

**참고**

---

1. UpperCase, LowerCase, Capitalize, UnCapitalize 타입도 내부적으로 따로 구현되어있음

### 3.9 flat 분석하기

- flat은 한 배열의 차원을 한 단계 씩 낮추는 메서드

```jsx
type FlatArray<Arr,Depth extends number>={
	"done" : Arr,
	"recur" : Arr extends ReadonlyArray<infer InnerArr>
	? FlatArray<InnerArr,[-1,0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20][Depth]>
	: Arr
}[Depth extends -1 ? "done" : "recuer"];
interface Array<T>{
	flat<A,D extends number = 1>(this:A,depth?:D):FlatArray<A,D>[]
}
```

- A는 this 타입으로 원본 배열을 의미
- D는 flat 메서드의 매개변수인 낮출 차원 수
  - 차원이 몇 차원이다.. 의미가 아님!!
  - 재귀적으로 FlatArray 타입을 계속 호출하여 배열의 차원 수를 낮춰감..
  - Depth를 낮춰가면서 인덱스 접근 타입[-1,0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]의 인덱스 값에 접근
  - Depth 값이 -1이 되면 flat 재귀를 멈추고 “done” // 완료

### 3.10 Promise, Awaited 타입 분석하기

- lib.es5.d.ts 파일을 살펴보자

```jsx
type Awaited<T> =
	T extends null | undefined ? T :
	T extends object & { then(onfulfilled: infer F,...args: infer -) : any } ?
	F extends ((value:infer V,...args: infer _) => any) ?
	Awaited<V> : never : T;

```

- 1. **규칙 1번. Awaited<객체가 아닌 값> === 객체가 아닌 값**
  - Awaited<string> 일 때,
    - T extends null | undefined 는 false 이고,
    - T extends object & {then(onfulfilled:infer F,…args: infer\_):any ⇒ 객체이며 then 메서드를 가지고 있지 않으므로 false 가 되어 T 가 된다.
- 2. **규칙 2번. Awaited(Promise<T>> === Awaited<T>**
  - Awaited<Promise<string>> 일 때,
    - T extends null | undefined 는 false 이고,
    - T extends object & {then(onfulfilled:infer F,…args:infer\_):any ⇒ 객체이며 Promise 인스턴스 타입이 then 메서드를 가지고 있으므로 true이고 infer F 는 then메서드 안의 콜백함수를 가리킨다
    - F extends ((value:infer V,…args:infer\_)⇒any) 콜백함수와 같게 생겼으므로 true이며,
    - then메서드 안의 콜백함수의 인자 값 V를 추론하여 Awaited<V> 가 된다
    - 사실.. Promise<string> 이므로 then 콜백메서드 안의 인자값도 string이 된다.

### 3.11 lib.es5.d.ts 분석하기

- CallableFunction은 호출할 수 있는 함수, NewableFunction 은 new를 붙여 호출할 수 있는 함수
  - 클래스에서의 bind : 클래스에서는 this 를 bind 하는 것을 무시하므로, OmitThisParameter를 반환하지 않음..
- ThisParameterType
  - this 값 가져오는 타입
  - 만약 this값이 없다면 unknown 이다..

```jsx
inferface CallableFunction extends Function {
	bind<T>(this:T,thisArg: ThisParameterType<T>): OmitThisParameter<T>;
}

this ThisParameterType<T> = T extends (this: infer U,...args:never)=> any ?
U : unknown;

type OmitThisParameter<T> = unknown extends ThisParameterType<T> ? T : T extends
(...args:infer A) => infer R ? (...args:A) => R : T;
```

- OmitThisParameter
  - this 값이 없으면 unknown
  - this 값이 있고 this가 함수 형태 일 때, this값을 제거한 함수 형태만 반환

**참고**

---

- bind, call, apply 의 타입은 strict 옵션이 활성화 있을 때에만 검사한다..
  - strictBindCallApply 옵션임
  - 이 옵션을 해제하면 bind,call,apply 타입 검사를 하지 않음
