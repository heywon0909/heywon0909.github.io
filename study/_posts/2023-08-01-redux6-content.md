---
layout: post
title: Redux 제대로 공부하기
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. Action 과 Action Creator

2. Reducer는 pure function 이다

3. combineReducers()

### Action 과 Action Creator

Action은 Redux state에 변화를 주기 위한 행위로
이는 javascript 객체로 구현되어 있다.
예) {type:'remove'}
Action은 애플리케이션에서 발생할 일을 설명하며, 객체로 작성할때 type이라는 키값을 꼭 넣어줘야한다‼

Action Creator는 Action을 만드는 javascript 함수로 리턴값으로 Action 객체를 반환한다.

```js
var ACTION_TYPE_ADD_TODO ='ADD_TODO';
function addTodoActionCreator(text){
    return {
        type: ACTION_TYPE_ADD_TODO,
        text: text
    }
}
```


### Reducer는 pure function 이다

> pure function 이란 받아온 입력값을 직접 수정하는 일이 없고, 만약 동일한 입력값이라면 동일한 결과값이 나올 수 있는 함수이다.

Reducer는 Action 이 발생하면 Action을 처리하여 Redux state 상태를 변화시켜준다.
이때 인자값으로 현재 State와 Action을 받아서 새로운 State를 리턴시킨다. 

#### Rules of Reducer

Reducer는 다음과 같은 원칙들을 지켜야한다.

1. only Calculate the new state value based on the state and action arguments
2. not allowed to modify the existing state. instead make immutable updates
3. not do any asynchronous login or other 'side-effects'

##### only Calculate the new state value based on the state and action arguments

인자값으로 현재 State와 Action을 받아서 새로운 State를 리턴시킨다. 


##### not allowed to modify the existing state. instead make immutable updates

Reducer는 현재 상태값과 action 객체를 인자로 받아서 immutable updates를 한다. 

> immutable updates 란 현재 state 값을 수정하지 않고(건드리지 않고), 새로운 state 값을 생성하여 state 값을 생성하는 것을 말한다.

##### not do any asynchronous login or other 'side-effects'

Reducer는 비동기 로직, console.log 와 같은 return 상태값과 관련이 없는 로직은 허용하지 않는다. 

이러한 3가지 원칙을 통해 Reducer는 pure function 이 될 수 있다. 

### combineReduers()

여러개의 Reducer가 존재할때 하나로 합치는 함수이다. 이때 만들어진 Reducer가 RootReducer가 된다. RootReducer 생성시 각각의 키값들은 RootState의 key로 해당 State에 접근할 수 있게 해준다. 

```js
  var rootReducer = Redux.combineReducers({
            todo: todoReducer,
            memo: memoReducer,
        })
  var store = Redux.createStore(rootReducer);      

```