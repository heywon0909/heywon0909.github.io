---
layout: post
title: Redux 제대로 공부하기 - middleware 
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. Redux Thunk
- 1-1. Thunk
- 1-2. redux-thunk
- 1-3. Thunk 함수 작성 방법
- 1-4. Thunk 사용 패턴
2. Redux Saga
- 2-1. Saga
- 2-2. Generator
- 2-3. redux-saga
- 2-4. redux-saga 사용법
3. Thunk vs Saga

### Redux Thunk

Redux middleware 중 하나인 redux-thunk에 대해 배워보자.

#### 📌 Thunk

Thunk란 일부 지연된 작업을 수행하는 코드 조각이라는 의미로, Redux 관점에서 본다면 Side Effect 작업을 수행하는 Redux Store와 상호작용하는 함수이다.

#### 📌 Thunk 함수 작성 방법

인자로 Redux Store의 dispatch,getState 함수를 받아서 안에 dispatch와 getState에 접근하여 코드를 짤 수 있다.

```js
const thunkFunction = (dispatch,getState) => {

}

store.dispatch(thunkFunction);
```
이렇게 매번 작성해주는 방법보다는 Thunk Action Creator를 사용하여 작성한다.
```js
const fetchGreetingsByCountry = (country) => async (dispatch,getState) => {
    const response = await apiClient.get(`/api/greetings/${country}`);
    dispatch(greetingsLoaded(response.greetings));
}
```

```js
import {useDispatch} from 'react-redux';
function GreetingsComponent(props){
    const {country} = props;
    const fetchGreetings = () => {
        dispatch(fetchGreetingsByCountry(country))
    }
}
```

#### 📌 Thunk 사용 패턴

Thunk는 다른 Action을 Dispatch하거나 Redux Store의 state에 접근할 수 있다.

1. Dispatching Actions : Thunk 함수 내에서 다른 Actions을 Dispatch할 수 있다.
2. state 값에 접근
- 2-1. state 값에 따른 조건부 Action Dispatch
- 2-2. Action Dispatch 이후 state 접근
- 2-3. cross-slice-state 문제 해결 -> Redux store state가 주제별로 분리되어있을 시, 다른 state에 접근할 수 있다.
3. Async Logic과 Side Effect 구현
4. Thunk에서 값을 반환 : dispatch에서 action 객체를 반환하는데 미들웨어를 사용하여 반환하는 값을 원하는 대로 반환할 수 있다.



#### 📌 Redux Thunk

Redux Thunk란 Thunk middleware for Redux라는 말처럼 Redux에서 비동기 로직을 사용하기 위한 표준 방법을 정의한 것이다. 
사실 비동기로직을 컴포넌트 훅에서 useEffect훅안에서 정의해도 되지만, Redux와 middleware를 사용하여 정의하는 이유는 컴포넌트 UI로직과 비동기로직을 분리하기 위해서이다. 이를 통해 코드 재사용성을 높이고 유지보수 할 수 있게 된다.


### Redux Saga

Redux middleware 중 하나인 redux-saga에 대해 배워보자.

#### 📌 Saga 

Saga란 일련의 사건을 뜻하는 단어로, Side Effect를 활용하여 데이터의 흐름을 관리하는 방법을 말한다.

#### 📌 Generator

Generator는 es6의 문법으로, 중단없이 반복실행시키는 Iterator를 보완하고자 나왔다.
Generator는 Iterator의 levelup버전으로, 코드의 실행 흐름을 원하는 대로 제어할 수 있다.

> yield - Generator로부터 결과를 산출한다.


```js
function * generateStudyPlan(){
    yield English;
    yield Math;
    yield Science;
}

let generator = generateStudyPlan();
let english = generator.next();
console.log('english',english); // {value:'English',done:false}

```

#### 📌 redux-saga

redux-saga는 직관적인 Redux Side-Effect manager이다.

> Effect는 SideEffect를 말하는 게 아니라,Generator로부터 생성된 JavaScript객체를 의미한다.

1. Declarative Effect
Effect Creator를 통해 Effect를 만든다.

```js
import {take,takeEvery,takeLatest,takeLeading,put,call,select} from 'redux-saga/effects'

```
- take(pattern)
effect를 생성한다. 인자로 들어가는 pattern이란 실행시킬 action을 정하는 규칙이라고 생각하면된다. 규칙을 만족하는 아이들만 effect를 생성하는 것이다.

- takeEvery(pattern,saga,...args)
Redux Store에 Dispatch된 Action들중에 pattern에 맞는 Action만 saga를 생성한다.

- takeLatest(pattern,saga,...args)
Redux Store에 Dispatch된 Action들중에 pattern에 맞는 Action만 saga를 생성한다. 
단, 최근의 Action들을 수행한다.하지만 이전에 실행중인 saga가 있으면 자동으로 취소된다.

- takeEvery(pattern,saga,...args)
Redux Store에 Dispatch된 Action들중에 pattern에 맞는 Action만 saga를 생성한다.
단, 생성된 saga가 끝날때까지 새로 생성되는 saga는 차단한다.

- put(action)
Dispatch와 유사하다. Action객체를 파라미터로 받아서 Effect를 생성하고 saga에 집어넣는다.

- call(fn,...args)
fn함수를 호출한다.

-select(selector,...args)
Redux Store state에서 selector에 해당하는 아이들을 선택하는 effect 생성한다.

2. Dispatching Actions
Action을 Dispatch하기위해 Dispatch대신 put을 사용한다.


#### 📌 redux-saga 사용법

1. 각각의 Action 분류에 대해 각각 saga 파일을 만든다.
2. saga들을 all()함수에 하나로 묶어주는 rootSaga를 생성후 Redux Store에 middleware 연동해준다.
3. sagaMiddleware의 run을 실행하여 rootSaga를 실행시켜준다.


### Thunk vs Saga 👩‍🎤
둘다 Redux middleware로 SideEffect를 사용할 수 있게 해주는 역할을한다.

|비교|Thunk|Saga|
|:---|---:|:---:|
|Async Logic 처리방식|Promise|Generator|
|사용난이도|쉬움|어려움|
|코드의 양|적음|많음|
|복잡한 Async Logic|구현어려움|구현쉬움|

둘마다 장점과 단점이 있으므로 대체적으로,
규모가 크고 Async Logic이 복잡할 수록 Saga를 쓰고, 규모가 비교적 작고 Async Logic이 복잡함이 낮을 수록 Thunk를 쓴다고 한다.