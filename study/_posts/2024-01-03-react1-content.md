---
layout: post
title: 모던 리액트 Deep Dive 1장 - part 1.
description: >
  React에 Redux를 적용하기 위한 공부
image:
sitemap: false
---

## 01장, 리액트 개발을 위해 꼭 알아야 할 자바스크립트 📑

1. 자바스크립트의 동등 비교

2. 함수

3. 클래스

4. 클로저

### 1. 동등 비교

1. 리액트의 함수형 컴포넌트와 훅의 의존성 배열(dependencies) 가 어떻게 작동하는지
2. 리액트 컴포넌트의 렌더링이 일어나는 이유 중 하나
3. 함수의 메모제이션
4. 리액트의 가상 DOM과 실제 DOM의 비교

이 모든 것이 자바스크립트의 동등 비교를 기반으로 한다.

#### 자바스크립트 데이터 타입

자바스크립트의 모든 값은 데이터 타입을 가지고 있다.

1. 원시 타입

   - boolean
   - null
   - undefined
   - number
   - string
   - symbol
   - bigint(**number**가 다룰 수 있는 숫자 크기 범위 제외)

2. 객체 타입 - object
   자바스크립트를 이루고 있는 대부분의 타입이 객체 타입이다.
   **배열, 함수, 정규식, 클래스** 등이 포함된다.

#### 값을 저장하는 방식의 차이

1. 원시 타입
   값을 저장할 때 변하지 않는 형태의 값으로 저장된다. 이 때 이 값은 변수 할당 시점에 메모리 영역을 차지하고 저장된다.

2. 객체 타입
   객체는 프로퍼티를 삭제,추가,수정이 가능하므로 원시 값과 다르게 변경 가능한 형태로 저장된다.
   값을 복사할 때는 값이 아닌 참조를 전달하게 된다.

   ```
   var arrow = {cupid:true};
   var arrow-copy = {cupid:true};

   console.log(arrow === arrow-copy) // false; -> 동등비교하면 false
   console.log(arrow.cupid === arrow-copy.cupid) // true (속성 값은 일치)
   ```

#### 자바스크립트의 또 다른 비교 Object.is

== , === 처럼 두개의 인수를 받아 비교를 하는 메서드이다.

#### 리액트에서의 동등 비교

리액트에서 사용하는 동등 비교는 == 이나 === 이 아니라 **Object.is**이다.
리액트에서는 이 objectIS를 기반으로 동등비교를 하는 shallowEqual이라는 함수를 만들어 사용한다. 단순히 Object.is를 수행하는 것 뿐 아니라 객체 간의 키 유무,키 값 비교도 추가되어 있다.

먼저 Object.is로 비교를 수행 후, 객체 간 얕은 비교를 한번 더 수행한다.

> 객체 간 얕은 비교란, 객체의 첫 번째 값에 존재하는 값만 비교한다는 것을 의미한다.

리액트에서 사용하는 JSX props 는 객체의 형태를 띄고, props 만 일차적으로 비교하여 렌더링 여부를 결정한다.
props안에 객체를 담고 있는 경우, props의 변화를 예상하지 못해 컴포넌트 재렌더링이 발생하지 않을 수 있다.

### 2. 함수

입출력 기능을 하나의 블록으로 감싸서 실행 단위로 만들어 놓은 것을 의미한다.

#### 함수를 정의하는 방법

1. 함수 선언문
   함수를 표현하는 가장 일반적인 방식
2. 함수 표현식
   함수는 일급객체, 함수를 변수에 할당하는 것이 가능하다.
   함수를 변수에 할당하는 방식
   > 일급객체란 다른 함수의 매개변수가 될 수 있고 반환 값이 될 수도 있고 다른 변수에 할당 가능한 객체
3. Function 생성자

4. 화살표 함수
   => 키워드를 사용해서 함수를 만드는 방식

---

cf. 참고

함수 선언문과 함수 표현문의 가장 큰 차이는 호이스팅(hoisting)이다. 함수 호이스팅은 함수 선언문이 마치 코드 최상단으로 끌어올려진 것처럼 작동하는 것을 의미한다.<br/>
함수 선언문은 함수 선언문 이전에 함수를 호출하더라도 함수의 동작이 가능하다. 이는 함수에 대한 선언을 실행 전에 미리 메모리에 등록하였으므로, 코드의 순서에 상관없이 함수 호출이 가능한 것이다.<br/>
함수 표현식도 마찬가지로 호이스팅이 발생한다. 선언문과는 다르게 호이스팅이 되는 시점에서 var의 경우 undefined로 초기화된다.

#### 이외의 함수

1. 즉시 실행 함수
   함수를 정의하는 **즉시** 실행되는 함수로 단 한번만 호출되고 다시 호출이 불가능하다.<br/>
   즉시 실행 함수를 활용하면 독립적인 함수 스코프를 운영할 수 있다. 함수의 선언과 실행이 그 자리에서 끝나므로, 함수 내부에 있는 값은 함수 내부에서만 사용 및 접근이 가능하다.

```
    (function(a,b){
        return a + b;
    }(10,24));

    ((a,b)=>{
        return a + b;
    },(10,24))
```

2. 고차 함수

일급객체라는 특성을 활용하면 함수를 인수로 받거나 새로운 함수를 반활 할 수 있다.

```
    const tripleArray = [1,2,3].map((item)=>item*item*item);
```

#### 함수를 만들 때 주의해야 할 사항

1. 함수의 부수 효과를 최대한 억제하라

함수 내의 작동으로 인해 함수 외부에 영향을 끼치는 것을 의미한다.
부수 효과 여부에 따라 순수 함수, 비 순수 함수로 나뉜다.

- 순수 함수: 동일한 입력값이면 같은 출력 값이 나와야 함.
- 비순수 함수: 입력 값을 내부에서 변경 가능함.

##### 리액트도 순수 함수이다.

리액트의 function Component도 순수 함수이다. Props를 받아서 React Element를 만드는 것이 입출력 기능을 가진 함수와 비슷하다.
Props는 Read-only 이므로 동일한 Props에 대해서는 동일한 React Element 가 나와야 한다.

###### 부수 효과를 억제하자

리액트에 관점에서 보면, useEffect는 부수 효과를 처리하므로 useEffect의 사용을 최소화 하는 것이 함수의 역할을 좁히는 방법이다.

예측 가능한 단위의 부수 효과가 적은 함수를 설계하면 유지보수 하는데 용이한 함수를 만들 수 있다.

2. 가능한 함수를 작게 만들어라

함수의 크기를 작게 하면, 무슨 역할을 하는지 이해하기 쉬워지고, 재사용성이 높아진다.

### 3. 클래스

16.8 버전이후 리액트에 모든 컴포넌트가 클래스에서 함수형으로 작성되기 시작되었다.<br/>
과거에 작성된 코드를 이해하기 위해서는 자바스크립트의 클래스가 어떻게 동작하는지 파악해야한다.

#### 클래스란?

객체를 만들기 위한 일종의 템플릿이다.

#### 인스턴스 메서드

클래스 내부에서 선언한 메서드이며, 실제로 자바스크립트의 prototype에 선언되므로 프로토타입 메서드라고 불린다.

<br/>

> 프로토타입 체이닝: 모든 객체는 프로토타입을 가지고 있는데, 특정 속성을 찾을 때 자기 자신부터 시작해서 이 프로토타입을 타고 최상위 객체인 object까지 훑는다. 이를 프로토타입체이닝을 거쳤다라고 표현한다.

#### 정적 메서드

클래스의 인스턴스가 아닌 클래스 name 명으로 호출할 수 있는 메서드이다.<br/>
정적 메서드 내부의 this는 클래스 자신을 가리킨다.

### 클래스와 함수의 관계 🤔 (추가 공부중...)

클래스를 바벨로 트랜스파일하면, 클래스를 생성자 함수와 유사하게 변환된다. 즉, 클래스는 객체지향을 사용하던 프로그래머가 자바스크립트에 쉽게 접근할 수 있게 해주는 문법적 설탕(syntax sugar)의 역할을 한다고 볼 수 있다.

### 4. 클로저

클로저란 **함수와 함수가 선언된 렉시컬 스코프와의 조합**이다.
<br/>
함수형 컴포넌트에 대한 이해, 함수형 컴포넌트의 구조와 작동방식, 훅의 원리, 의존성 배열등이 클로저에 의존하고 있다.

<br/> 
좋은 함수형 컴포넌트를 만들기 위해서는 클로저를 알고 있어야 한다.

선언된 어휘적 환경 === 선언된 렉시컬 스코프란, 변수가 코드 내부에서 어디에 선언되었는지를 의미한다.

> 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향을 주지 않는다. (모던 자바스크립트 Deep Dive p.199)

#### 클로저의 활용 👀

클로저를 활용하면 내부 상태 값을 전역 스코프에서 숨기고, 내부에서만 접근 가능하게 하여 상태 값 관리가 용이해질 수 있다.

이러한 클로저의 원리를 활용하여 만들어진 리액트 함수형 컴포넌트 훅이 useState이다.<br/> 🧐 setState가 useState 내부의 최신값을 계속해서 기억할 수 있는 점은, 클로저가 useState 내부에서 활용이 되어서 외부 함수(useState)가 반환한 내부 함수(setState)는 외부 함수(useState)의 호출이 끝났음에도 자신이 선언된 환경(state가 저장되어 있는 곳)을 기억하므로 state 값을 사용할 수 있는 것이다.

#### 클로저에 대해 주의할 점

클로저는 외부 함수를 기억하고, 내부 함수에서 가져다 쓰는 구조로 외부 함수의 정보를 메모리에 기억해놓으므로, 성능에 영향을 미칠 수 있다. 그러므로, 클로저에서는 꼭 필요한 작업만 남겨두어야 한다.
