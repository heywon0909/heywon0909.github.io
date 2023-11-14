# 2장, 기본 문법 익히기

### 2.1 변수, 매개변수, 반환 값에 타입을 붙이면 된다.

- 타이핑: 변수,매개변수,반환 값에 타입을 붙이는 행위

### 2.2 타입 추론을 적극 활용하자.

- 타입스크립트는 어느 정도 변수와 반환 값의 타입을 추론하는 능력을 갖추고 있다.
- **단, 매개변수에는 타입을 부여해야 한다.**
- 암묵적 타입 추론
  직접 타입을 표기하지 않아서 타입스크립트가 타입을 추론했다는 의미!!
- implicitAny
  암묵적 추론했더니 타입이 any 가 되어 발생하는 에러

<aside>
💡 타입을 제대로 추론하면 그대로 쓰고, 틀리게 추론할 때는 올바른 타입을 표기하자

</aside>

**참고**

---

1. {} 타입은 객체를 의미하는 것이 아니라 null 과 undefined 를 제외한 모든 타입을 의미한다.
2. let 으로 선언한 변수는 타입을 넓게, const 으로 선언한 변수는 타입을 좁게 추론한다.

### 2.3 값 자체가 타입인 리터럴 타입이 있다

- 값이 변하지 않는 다면, as const 붙여서 타입으로 만들 수 있다 === 리터럴 타입
  - readonly 가 됨 → 변경 불가능한 값이 됨

### 2.4 배열 말고 튜플도 있다

- 타입[] 또는 Array<타입> 표기
- 길이가 고정된 배열(❌) 각 요소 자리에 타입이 고정되어 있는 배열(⭕)
- []안에 정확한 타입을 입력해야 함. 표기하지 않은 자리는 undefined( 배열 push 사용 가능 ‼)
- 단, readonly 수식어를 붙여주면, push 사용 막을 수 있음 🚫
- spread 연산자를 활용하여 튜플 배열 타입 정의 가능

### 2.5 타입으로 쓸 수 있는 것을 구분하자

- 타입 = 값으로 사용할 수 없는 것
- String ,Object, Number, Boolean, Symbol → string, object, number, boolean, symbol 을 타입으로 사용하자 ‼
- 변수를 타입으로 쓰고 싶다면 변수 앞에 typeof 붙이자

**참고**

---

- 함수 반환값을 타입으로 쓰고 싶다면, ReturnType(3.3)
- 클래스는 typeof 없이도 타입으로 사용 가능

### 2.6 유니언 타입으로 OR 관계를 표현하자

- OR = 이거 또는 저거
- 타입 좁히기
  유니언 타입으로부터 정확한 타입을 찾아내는 기법
- 타입 사이 | , 타입 앞 | 쓸 수 있다

### 2.7 타입스크립트에만 있는 타입을 배우자

1. any

   - 모든 동작을 허용한다.
   - any 쓰면 타입스크립트가 타입을 검사하지 못하므로 쓰는 의미가 없어짐… 😥

   <aside>
   💡 any 타입은 타입 검사를 포기한다는 선언과 같다… 
   타입스크립트가 any로 추론하는 타입이 있다면 타입을 직접 표기해야 한다.

   </aside>

   - 명시적으로 any 반환하는 경우 : JSON.parse, fetch

1. unknown

   - any 와 비슷하게 모든 타입을 대입할 수 있지만, 그 후 어떤 동작도 수행할 수 없게 됨
   - unknown 인 변수를 사용한 모든 동작이 에러로 처리됨… → as 써서 강제로 타입 주장 가능
   - cf) !(non-null assertion) = null, undefined 이 아님을 주장하는 연산자

1. void
   - 타입스크립트에서는 타입으로 쓰임
   - 함수의 반환 값이 없는 경우, 반환값 void로 추론됨 → but, 반환값이 void 타입이라고 해서 함수가 undefined 가 아닌 다른 값을 반환하는 것을 막지는 못함 ‼
   - 함수의 반환 값이 void | 다른 타입 일 경우, 반환값을 무시하지 않음
   - 함수의 반환값이 void 로 정의하려 할 때
     - 사용자가 함수의 반환값을 사용하지 못하도록
     - 반환값을 사용하지 않는 콜백함수 타이핑 (ex: forEach…)
1. {}, Object
   - 객체(❌) null 과 undefined 제외한 모든 값(⭕)
   - {}는 실제로 사용할 수 없음,object 타입도 마찬가지
   - unknown 타입을 if 문으로 걸러보면 {}이 나옴
1. never
   - 어떠한 타입도 대입할 수 없음
   - 함수 선언문은 throw 하더라도, 반환값의 타입이 void, 함수 표현식은 never
   - 무한 반복문도 함수 표현식일 때 never 타입, 함수 선언문일 때 void 타입

### 2.7.6 타입 간 대입 가능 표

- **타입 간 대입 가능 표**(https://www.typescriptlang.org/docs/handbook/type-compatibility.html)

**참고**

---

1. undefined 와 null 값은 별개의 값이다.

### 2.8 타입 별칭으로 타입에 이름을 붙이자

- 타입 별칭(type alias) : 기존 타입에 새로 이름을 붙인 것 → type 키워드를 사용해 선언 가능
- 주로 타입 표기가 너무 길어질 때, 붙임

### 2.9 인터페이스로 객체를 타이핑하자

- 객체 타입에 이름을 붙이는 방법
- **인덱스 시그니처**
  interface Arr{
  length: number;
  [key:number]: string;
  }
  const arr:Arr = [’3’,’5’,’7’];
  - 이 객체의 length 제외한 속성 key가 모두 number 이다
- 이름만 있고, 정의되지 않은 인터페이스 = = {} 타입( null 과 undefined 제외한 모든 타입)

### 2.9.1 인터페이스 선언 병합

- 인터페이스는 서로 합쳐질 수 있다
- 객체에 대한 타입을 수정하기 위해 → 인터페이스 병합되어 합쳐짐
- 인터페이스 간에 속성이 겹치는데 타입이 다르면 에러 발생…(타입을 맞추자)

### 2.9.2 네임스페이스

- 원하지 않게 인터페이스가 병합되는 것을 막아준다
- 네임스페이스 내부 타입을 사용하려면 export 를 써준다
- 동일한 이름으로 정의한 네임스페이스는 병합된다

### 2.10 객체의 속성과 메서드에 적용되는 특징을 알자

- 객체의 속성에 옵셔널(optional)이나 readonly 수식어 가능
- **잉여 속성 검사 🤔**
  인수 자리나 객체를 변수에 대입할 때, 객체 리터럴(타입에 맞춰 만든 애)를 대입 하면 에러 남‼
  객체 리터럴을 변수에 넣은 후 변수를 대입할 때는 에러가 나지 않음 ‼
  타입스크립트가 객체리터럴을 대입할 때와 변수를 대입할 때 다르게 처리한다…
  → 객체 리터럴을 대입하면 잉여 속성 검사를 진행(타입에 선언되지 않은 속성들 걸러냄)

### 2.10.1 인덱스 접근 타입

- 인덱스 접근 타입(Indexed Access Type) = 객체의 속성의 타입에 접근하는 방식
- key의 타입 = keyof 객체\_타입
- value의 타입 = 객체\_타입[key의 타입]

**참고**

---

1. 객체의 메서드를 선언하는 방법(2.19)
   - 메서드(매개변수): 반환값
   - 메서드:(매개변수) ⇒ 반환값
   - 메서드:{(매개변수): 반환값}

### 2.10.2 매핑된 객체 타입

- 기존의 다른 타입으로부터 새로운 객체 속성을 만들어 내는 타입
- in 연산자를 사용
- 인터페이스에서 사용(❌) 타입 별칭에서만 사용 가능 (⭕)

```jsx
type HelloAndHi = {
	[key in 'hello' | 'hi ]: string;
}
```

- 수식어(readonly 또는 ?) 앞에 - 붙이면 해당 수식어를 제거할 수 있음

### 2.11 타입을 집합으로 생각하자 (유니언, 인터섹션)

- 합집합 = 유니언(|)
- 교집합 = 인터섹션(&)
- 타입스크립트에서 전체 집합은 unknown, 가장 좁은 집합은 never(공집합) 임
- never 와의 & 연산은 무조건 never

<aside>
💡 타입스크립트에서는 좁은 타입을 넓은 타입에 대입 가능(⭕)
넓은 타입은 좁은 타입에 대입 불가능(❌)

</aside>

### 2.12 타입도 상속이 가능하다

- 인터페이스, 타입 별칭 둘 다 상속이 가능하다.. 서로도 가능

```jsx
type Animal = { name: string };
type Dog = Animal & { bark(): void };
type Cat = Animal & { meow(): void };
type Name = Cat["name"];
```

- 상속 받는다는 것은 더 좁은 타입이 된다는 것을 의미

### 2.13 객체 간에 대입할 수 있는지 확인하는 법을 배우자

- 추상적인 타입 = 넓은 타입, 구체적인 타입 = 좁은 타입
- 좁은 타입을 넓은 타입에 대입 ⭕ 넓은 타입을 좁은 타입에 대입 ❌
  여러 예시 👀
  - 튜플은 배열보다 좁은 타입이다 ⇒ 배열에 튜플 대입 ⭕, 튜플에 배열 대입 ❌
  - readonly 가 붙은 배열이 일반 배열보다 넓은 타입
  - 두 객체가 있고 속성 동일 ⇒ 속성이 옵셔널인 객체가 옵셔널이지 않은 객체보다 더 넓은 타입
    - 배열과 다르게 객체에서는 속성에 readonly 가 붙어도 서로 대입 가능

### 2.13.1 구조적 타이핑(structural typing)

- 모든 속성이 동일하면 객체 타입의 이름이 다르더라도 동일한 타입으로 취급 → 객체를 어떻게 만들었든 간에 구조가 같으면 같은 객체이다 ~~

```jsx
interface Money {
  amount: number;
  unit: string;
}

interface Liter {
  amount: number;
  unit: string;
}

const liter: Liter = { amount: 1, unit: "liter" };
const circle: Money = liter;
```

- 서로 다른 객체로 취급하고 싶으면 ?!? **브랜딩** 기법을 사용하자

```jsx
interface Money {
  __type: "money"; // 브랜딩 기법
  amount: number;
  unit: string;
}

interface Liter {
  __type: "liter"; // 브랜딩 기법
  amount: number;
  unit: string;
}

const liter: Liter = { amount: 1, unit: "liter" };
const circle: Money = liter;
```

### 2.14 제네릭으로 타입을 함수처럼 사용하자

- 제네릭을 쓰면 중복을 제거할 수 있다

```jsx
interface Person<N, A> {
  type: "human";
  race: "yellow";
  name: N;
  age: A;
}

interface Zero extends Person<"zero", 28> {}
interface Nero extends Person<"nero", 32> {}
```

- 하나의 타입을 여러 방법으로 재사용할 수 있다
- 타입 매개변수는 기본값(default) 사용할 수도 있으나… ⇒ 추론이 되므로 굳이 안써도 됨
- 타입 매개변수 앞에 const 수식어를 추가하면 타입 매개변수 T 를 추론할 때 as const 를 붙인 값으로 추론된다

```jsx
function values<const T>(initial:T[]){
	return {
		hasValue(value:T) { return initial.includes(value) },
	}
}

const savedValues = values(['a','b','c']);
savedValues.hasValue('x') // 😡 -> as const로 만들었기 때문에 에러남.. 'a' | 'b' | 'c' 만 가능
```

**참고**

---

제네릭은 다음과 같은 위치에 사용할 수 있다

1. interface 이름<타입 매개변수들>{…}
2. type 이름<타입 매개변수들> = {…}
3. class 이름<타입 매개변수들> {…}
4. function 이름<타입 매개변수들> (…) {…}
5. const 함수 이름 = <타입 매개변수들> (…) ⇒ {…}

### 2.14.1 제네릭에 제약 걸기

- 타입 매개변수의 범위에 제약을 걸 수 있다 ⇒ extends 사용
- 타입 매개변수와 제약을 동일하게 생각하면 안된다…

```jsx
interface VO{
	value: any;
}

const returnVO = <T extends VO>() : T => {
	return { value: 'test' }; // T는 VO에 대입할 수 있는 모든 타입 😡
}

```

- never는 모든 타입에 대입할 수 있으므로 never extends boolean 은 참이다..

```jsx
function onlyBoolean<T extends boolean>(arg: T = false):T{
	return arg;
} // T는 false가 될 수 없음... 왜냐하면 T가 never 일 수도 있기 때문... 😡
```

**참고**

---

제네릭에서 제약

1. <T extends object> // 모든 객체
2. <T extends any[]> // 모든 배열
3. <T extends (…args: any) ⇒ any > // 모든 함수
4. <T extends abstract new (…args: any) ⇒ any> // 생성자 타입
5. <T extends keyof any> // string | number | symbol

### 2.15 조건문과 비슷한 컨디셔널 타입이 있다

<aside>
💡 특정 타입 extends 다른 타입 ? 참일 때 타입 : 거짓일 때 타입

</aside>

- 특정 타입이 다른 타입의 부분 집합일 때, 참이 된다

```jsx
type OmitByType<O,T> = {
	[K in keyof O as O[K] extends T ? never : K ] : O[K];
}

type Result = OmitByType<{
	name: string;
	age : string;
	married: boolean;
	rich: boolean;
},boolean> // type Result = { name : string; age: number;}

```

### 2.15.1 🔴 컨디셔널 타입 분배 법칙

- **검사하려는 타입이 제네릭이면서 유니언이면, 분배 법칙이 실행**

```jsx
type Start = string | number;
type Result<Key> = Key extends string ? Key[] : never;
let n: Result<Start> = ['hi'];
//답:string[] => 분배법칙이 일어나서 Result<string> | Result<number> 가 됨
```

- 다만, boolean 에 분배 법칙이 적용될 때에는 조심해야 함 🧐
  - boolean 이 true | false 가 된다…

```jsx
type Start = string | number | boolean;
type Result<Key> = Key extends string | boolean ? Key[] : never;
let n:Result<Start> = ['hi']; // string[] | false[] | true[]
n = [true];
```

### 분배 법칙을 막고 싶다면?

- 배열로 제네릭을 감싸면 분배 법칙이 일어나지 않는다

```jsx
type IsString<T> = [T] extends [string] ? true : false;
type Result = IsString<'hi' | 3>; // false 😡
```

- **never도 분배 법칙의 대상이 된다…😣 ⇒ 그냥 never 는 유니언이라고 생각하자**
  - **유니언으로 보이지는 않지만 유니언이다…**
  - **never는 공집합이므로, 공집합에서 분배 법칙을 실행하는 것은 아무것도 실행하지 않는 것돠 같다…**

```jsx
type R<T> = T extends string ? true : false;
type RR = R<never>; // 결과는 never..!

```

- never에서도 분배 법칙을 막을 수 있다…

```jsx
type IsNever<T> = [T] extends [never] ? true : false;
type T = IsNever<never> = true;
type F = IsNever<'never'> = false;
```

### **타입스크립트는 제네릭이 들어있는 컨디셔널 타입을 판단할 때, 값의 판단을 뒤로 미룬다..**

```jsx
function test<T>(a:T){
	type R<T> = T extends string ? T : T;
	const b:R<T> = a;
}
```

- 배열로 제네릭을 감싸자

```jsx
function test<T extends([T] extends [string] ? string :never)>(a: T){
	type R<T> = [T] extends [string] ? T : T;
	const b:R<T>= a;
}
```

### 2.16 함수와 메서드를 타이핑하자

- 옵셔널 수식어(?) ⇒ 선택 매개변수
- 매개변수에서의 나머지 문법
  - 배열이나 튜플만 가능
  - … 문법으로 사용
  - 매개변수의 마지막 자리에만 위치
- 함수 내부에서의 this
  - 명시적으로 표기해야 함
  - 매개변수의 첫 번째 자리에 표기
  - 일반적으로 this는 객체 자신으로 추론 ⇒ but this가 바뀔 수 있을 때는 명시적으로 타이핑

### 2.17 같은 이름의 함수를 여러 번 선언할 수 있다

- 타입스크립트에서는 매개변수에 어떤 타입과 값이 들어올지 미리 타입을 선언해야 한다
- 오버로딩(overloading) ⇒ 호출할 수 있는 동일한 이름을 가진 함수의 타입을 미리 여러 개 타이핑 해두기

```jsx
function add(x:number,y:number):number;
function add(x:string,y:string):string;
function add(x:any,y:any){
	return x + y;
}

add(1,2); // 3
add('1','2') // 12
add(1,'2'); // 맞는 타입이 없어서 오류
add('1',2); // 맞는 타입이 없어서 오류
```

- tip ✨ \*\*\*\*
  - **오버로딩의 순서는 좁은 타입부터 넓은 타입 순으로 오게 해야 문제가 없다.**
  - 유니언이나 옵셔널 매개변수를 활용할 수 있는 경우, 오버로딩을 쓰지 않는 게 좋다.

### 2.18 콜백 함수의 매개변수는 생략 가능하다

- 함수가 콜백 함수로 사용될 때, 인수로 제공하는 콜백함수의 매개변수에는 타입을 표기 하지 않아도 된다
  - 문맥적 추론(Contextual Typing) ⇒ 매개변수 e는 Error 타입이, 매개변수 r은 string 타입이 된다

```jsx
function example(callback: (error: Error, result: string) => void) {}
example((e, r) => {});
example(() => {});
example(() => true);
```

### 2.19 🔴 공변성과 반공변성을 알아야 함수끼리 대입할 수 있다

- **공변성 ⇒ 좁은 타입을 넓은 타입에 대입할 수 있다**
- **반공변성 ⇒ 넓은 타입을 좁은 타입에 대입할 수 있다**
- **이변성 ⇒ 공변성, 반공변성 둘다 가능하다**

### 타입스크립트는 공변성을 가지고 있지만, 함수의 매개변수는 반공변성을 가지고 있다 (함수의 반환값에 대해서는 공변성임)

```jsx
function a(x: string): number {
  return 0;
}

type B = (x: string) => number | string;
let b: B = a; // 공변성(넓은 타입에 좁은 타입을 대입할 수 있다)

function a(x: string | number): number {
  return 0;
}
type B = (x: string) => number;
let b: B = a; // 반공변성(좁은 타입에 넓은 타입을 대입할 수 있다)
```

- 매개변수는 strict 옵션일 때 반공변성, strict 옵션이 아닐 때는 이변성을 가진다
  - b → a 일 때, T<b>→ T<a>, T<a> → T<b> 도 가능

```jsx
function a(x: string): number {
  return 0;
}
type B = (x: string | number) => number;
let b: B = a;
```

### 2.20 클래스는 값이면서 타입이다

- 인터페이스와 implements 예약어를 사용하면, 클래스 멤버가 제대로 들어가 있는지 검사할 수 있다
- 클래스 자체의 타입 ⇒ **typeof 클래스 이름** 사용하자
- 클래스 멤버로는 옵셔널,readonly,public,protected,private 수식어 사용 가능

### 2.20.1 추상 클래스

- abstract class로 선언
  - 반드시, abstract 속성이나 메서드를 구현해야 함
  - implements 와 다르게 abstract 클래스는 실제 자바스크립트 코드로 변환

### 2.21 enum은 자바스크립트에서도 사용할 수 있다

- 자바스크립트의 값으로 사용할 수 있는 특이한 타입
- 값으로 사용하기 보다는 **타입**으로 사용

```jsx
enum Level{
	NOVICE,
	INTERMEDIATE,
	ADVANCED,
	MASTER
}
```

- 브랜딩을 위해 사용하면 좋음
- const enum 사용하면 자바스크립트 코드가 생성되지 않게 할 수 있음.

```jsx
const enum Money{
	WON,
	DOLLAR,
}

Money.WON;
Money[Money.WON];

```

### 2.22 🔴 infer로 타입스크립트의 추론을 직접 활용하자

- infer 활용해 타입을 추론하자
  - 추론하려는 부분을 infer로 만들면 됨
  ```jsx
  type MyParameters<T> = T extends (...args:infer P) => any ? P : never; // 매개변수
  type MyConstructorParameters<T> =
  T extends abstract new (...args: infer P) => any ? P : never; // 생성자 파라미터
  type MyReturnType<T> = T extends (...args:any) => infer R ? R : any; // 반환값
  type MyInstanceType<T> =
  T extends abstract new (...args:any) => infer R ? R : any; // 인스턴스 타입

  ```
- 합집합 타입과 교집합 타입
  - **기본적으로 같은 이름의 타입 변수는 서로 유니언이 되지만, 매개변수인 경우에는 다르다**
  - **매개변수는 반공변성을 가지고 있으므로 매개변수의 경우에는 인터섹션이 된다**
  ```jsx
  type Union<T> = T extends { a: infer U, b: infer U } ? U : never;
  type Result1 = Union<{ a: 1 | 2, b: 2 | 3}>;

  type Intersection<T> = T extends {
  a: (pa: infer U) => void,
  b: (pb: infer U) => void
  } ? U : never;

  type Result2 = Intersection<{a(pa:1|2):void,b(pb:2|3):void}>
  ```
  - 매개변수에 같은 타입 변수를 선언하면 인터섹션이 된다
  ```jsx
  type UnionToIntersection<U> = (U extends any ? (p:U)=>void :never) extends
  (p: infer I)=>void ? I : never;

  1)type Result5 = UnionToIntersection<{a:number}|{b:string}>
  2)type Result6 = UnionToIntersection<boolean|true>;

  ```
  - 1. U는 제네릭이자 유니언이므로 컨디셔널 타입에서 분배 법칙이 실행된다. 분배 법칙에 따라 UnionToIntersection<{a:number}>,UnionToIntersection<{b:string}>이 된다. 최종적으로 I는 매개변수이므로 인터섹션이 실행되어 {a:number}&{b:string}이 된다.
  - 2. UnionToIntersection<boolean | true>는 분배 법칙이 실행되어 UnionToIntersection<true|false|true>가 되어 true&false&true가 되므로 never가 된다.

### 2.23 타입을 좁혀 정확한 타입을 얻어내자

<aside>
💡 자바스크립트에서는 **typeof null** 이 ‘**object**’이다 ⇒ 객체와 typeof 결과가 똑같아서 typeof 만으로 null을 구분할 수 없다

</aside>

[예시]

```jsx
function strOrNullOrUndefined(param: string | null | undefined) {
  if (typeof param === "undefined") {
    param; // undefined
  } else if (param) {
    param; // string
  } else {
    params; // ' '(빈문자열)string | null
  }
}
```

- 타입 좁히기는 자바스크립트 문법을 사용해서 진행해야 한다. 자바스크립트에서도 실행할 수 있는 코드여야 하기 때문이다.
  - ex) instanceof (자바스크립트 문법(x))

### 타입 서술 함수(Type Predicate)

- param is Money = 00 은 00 타입이다~
  - 이렇게 하면 isMoney의 반환값이 true 이면, 매개변수의 타입도 is 뒤에 적은 타입으로 좁혀짐

```jsx
function isMoney(param: Money | Liter): param is Money{
	if(param.__type === 'money'){
		return true;
	}else{
		return false;
	}
}

function moneyOrLiter(param: Money | Liter){
	if(isMoney(param)){
		param; // Money
	}else{
		param; // Liter
	}
}
```

### 2.24 자기 자신을 타입으로 사용하는 재귀 타입이 있다

- 재귀 함수 : 자기 자신을 다시 호출하는 함수
- 타입스크립트에 재귀 타입이 있다…

```jsx
type JSONType =
  | string
  | boolean
  | number
  | null
  | JSONType[]
  | { [key: string]: JSONType };

const c: JSONType = {
  prop: null,
  arr: [{}],
};
```

- Reverse 타입

```jsx
type Reverse<T> = T extends [...infer L, infer R] ? [R,...Reverse<L>]:[];
const reverse:Reverse<[1,2,3]> // [3,2,1];

type FlipArguments<T> = T extends (...args: infer A) => infer R ?
(...args: Reverse<A>) => R : never;

type Flipped = FlipArguments<(a:string,b:number,c:boolean)=>string>;
const flipped:Flipped<('1',2,true)=>{return ''}> // (true,2,'1')=>{return ''};
```

### 2.25 정교한 문자열 조작을 위해 템플릿 리터럴 타입을 사용하자

- 템플릿 리터럴 타입 = 문자를 활용한 타입

```jsx
type Template = `template ${string}`;
let str:Template = 'template ';
str = 'template hello';
str = 'template 123' // 123이 number 이므로 안됨
str = 'template';
```

- 문자열을 조합할 때 편리

```jsx
type City = 'seoul' | 'suwon' | 'busan';
type Vehicle = 'car' | 'bike' | 'walk';
type ID = `${City}:${Vehicle}`;
const id = 'seoul:walk';
```

- 심화
  - 템플릿 리터럴 타입은 제네릭 및 infer 와 함께 사용하면 더 강력함

```jsx
type RemoveX<Str> = Str extends `x${infer Rest}` ? RemoveX<Rest> :
Str extends `${infer Rest}x` ? RemoveX<Rest> : Str;

type Removed = RemoveX<'xxxtestxx'> // 'test'
```

### 2.26 🟡 추가적인 타입 검사에는 satisfies 연산자를 사용하자

- satisfies : 있는 타입을 그대로 활용하면서 추가로 세부적인 타입 검사를 하고 싶을 때

```jsx
const universe ={
	sun: "start",
	sriius: "start", // 여기의 오타를 세부적으로 찾아낼 수 있음
	earth: { type:"planet", parent:"sun"}
} satisfies {[key in 'sun' | 'sirius' | 'earth']:{type;string,parent:string} | string }
```

- 이렇게 universe 선언하면 universe 타입이 이렇게 깔끔하게 나온다

```jsx
const universe:{
	sun: string;
	sriius: string;
	earth:{
		type: string;
		parent: string;
	}
}
```

### 2.27 🔴 타입스크립트는 건망증이 심하다

- error는 unknown 타입이다. unknown은 if문을 통과하면 {} 타입이 된다
- as 로 주장하는 것은 일시적이다… 주장한 타입을 계속 기억할 수 있게 변수에 담자

```jsx
try {} catch (error) {
	const err = error as Error;
	if(err as Error){
		err.message;
	}
}
```

- 제일 좋은 방법은 as 쓰지 않는 것

```jsx
try {
} catch (error) {
  if (error instanceof Error) {
    error.message;
  }
}
```

### 2.28 ⚪ 원시 자료형에도 브랜딩 기법을 사용할 수 있다

- 원시자료형에도 **브랜딩** 속성을 추가할 수 있다 ⇒ 타입을 정밀하게 사용할 수록 안정성이 올라간다..

```jsx
type Brand<T,B> = T & { __brand : B };
type KM = Brand<number,'km'>;
type Mile = Brand<number,'mile'>;

function kmToMile(km:KM){
	return km * 0.62 as Mile;
}

const km = 3 as KM;
const mile = kmToMile(km);
const mile2 = 5 as Mile;

```

### 2.29 🔴 배운 것을 바탕으로 타입을 만들어보자

### IsNever

- never인지 판단하는 타입
  - never는 유니언이므로 **분배법칙**이 일어남

```jsx
type IsNever<T> = [T] extends [never] ? true : false;
```

### IsAny

- any 타입인지 판단

```jsx
type IsAny<T> = string extends (number & T) ? true : false;
```

### IsArray

- 배열인지 판단

```jsx
type IsArray<T> = IsNever<T> extends true ? false :
T extends readonly unknown[] ?
IsAny<T> extends true ? false : true : false;
```

### IsTuple

- 튜플인지 판단
  - 튜플이 아닌 배열은 length 가 number 임

```jsx
type IsTuple<T> = IsNever<T> extends true ? false :
T extends readonly unknown[] ?
number extends T["length"] ? false : true : false;
```

### IsUnion

- 유니언인지 판단
  - **유니언의 경우, 컨디셔널 타입과 타입 제네릭이 만나면 분배 법칙이 발생**

```jsx
type IsUnion<T,U=T> = IsNever<T> extends true ? false :
T extends T ?
[U] extends [T] ? false : true : false;
```

### 2.29.2 🟡 집합 관련 타입 만들기

- 전체집합은 unknown, 공집합은 never

### omit

- 특정 객체에서 지정한 속성을 제거하는 타입

```jsx
type Diff<A,B> = Omit<A&B,keyof B>;
type R1 = Diff<{name:string,age:number},{name:string,married:boolean}>
```

### Diff

- 대칭차집합(합집합 - 교집합)

```jsx
type SymDiff<A,B> = Omit<A&B, keyof(A|B)>;
type R2 = SymDiff<{name:string,age:number},{name:string,married:boolean}>
```

### Exclude

- 어떤 타입에서 다른 타입을 제거하는 타입

```jsx
type SymDiffUnion<A, B> = Exclude<A | B, A & B>;
type R3 = SymDiffUnion<1 | 2 | 3, 2 | 3 | 4>; // 1|4
```

### IsSubset

- A는 B의 부분집합인지 알아내는 타입

```jsx
type IsSubset<A,B> = A extends B ? true : false;

type R1 = IsSubset<string,string | number>; // true;
type R2 = IsSubset<{name:string,age:number},{name:string}>; // true
type R3 = IsSubset<symbol,unknown>; // true
```

### Equal

- 두 타인이 동일한지 알아내는 타입

```jsx
1)type Equal<A,B> = A extends B ? B extends A ? true : false : false;

2) type Equal2<X,Y> = (<T>()=> T extends X ? 1:2) extends (<T>()=> T extends Y ? 1 :2) ?
true: false;

```

- 1. 이 Equal 타입은 any와 다른 타입을 구별하지 못함
  - type R3 = Equal<any,1>// true;
- 2. 이 Equal2<any, unknown> 의 경우 extends를 false로 만드는 경우가 없음에도 false가 됨..

### NotEqual

- Equal 과는 반대

```jsx
type NotEqaul<X,Y> = Equal<X,Y> extends true ? false : true
```

### 2.30 타입스크립트의 에러 코드로 검색하자

- **타입스크립트의 에러 코드와 해결책을 정리한 사이트**(https://typescript.tv/errors/)

### 2.31 함수에 기능을 추가하는 데코레이터 함수가 있다

- 클래스에서 공통적으로 쓰이는 기능을 공통모듈처럼 만들어 함수에 장식한다 🎄

```jsx
function startAndEnd(start='start',end='end'){
	return function RealDecorator<This,Args extends any[],Return>(
	originalMethod: (this:This, ...args: Args) => Return,
	context: ClassMethodDecoratorContext<This,(this:This,...args:Args)=>Return>
){
	 function replacementMethod(this:This,...args:Args):Return{
	console.log(context.name,start);
	const result = originalMethod.call(this,...args);
	console.log(context.name,end);
	return result;
}
return replacementMethod;
}
}

function log<Input extends new (...args:any[])=>any>(
	value: Input,
	context: ClassDecoratorContext
){
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
	return value;
}

@log
export class C{
	@startAndEnd
	eat(){
		console.log('Eat');
	}
}
```

### 2.32 🔴 앰비언트 선언도 선언 병합이 된다

- 타입스크립트에서 남의 라이브러리를 사용할 때 그 라이브러리가 자바스크립트라면 직접 타이핑해야하는 경우가 있다 ⇒ **앰비언트 선언(ambient declaration)**
- **declare** 예약어 사용
  - 단, 인터페이스와 타입 별칭은 declare로 선언하지 않아도 동일하게 작동되므로 안써도 됨

```jsx
declare namespace NS{
	const v:string;
}
declare enum Enum{
	ADMIN = 1
}
declare function func(param:number):string;
declare const variable:number;
declare class C{
	constructor(p1:string,p2:string);
}

new C(func(variable),NS.v);
```
