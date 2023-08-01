---
layout: post
title: Redux 제대로 공부하기 - FSA
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. FSA 란

2. Redux-actions 주요 API

### FSA 란

Redux Action 객체를 만들때 standard 기준을 말한다. 애플리케이션의 규모가 커지고, Redux의 규모가 커지면 Action,Action type이 많아지게 된다. 이때, FSA를 활용하여 만들면 정형화된 action를 만들 수 있다.

#### Rules of Flux Standard Action(FSA)

1. action은 항상 자바스크립트 객체 형태이고, type 프로퍼티를 가져야한다.
2. action은 optional 프로퍼티로 error,payload,meta를 가질 수 있다.



### Redux-actions

Redux Action에 FSA를 적용할 수 있게 해주는 도구가 Redux-actions 라이브러리이다.

##### Redux-actions API

1. createAction(s) : Action Creator(s)를 생성
2. handleAction(s) : Action(s)에 대한 Reducer를 생성
3. combineAction(s) : Action Type(s),Action Creator(s)를 하나로 합쳐준다.

```js
import {createActions,handleActions} from 'redux-actions';

export const {addGreetings,removeGreetings} = createActions({
    // key: Action type, value: Action Creator
    ADD_GREETINGS: (text) => {return {text}},
    REMOVE_GREETINGS:() =>{return {}}
});

const reducer = handleActions({
    // key: Action creator, value : state 값을 변경해주는 함수
    [addGreetings]:(state,action) =>{
        return state.concat(action.payload.text)
    },
    [removeGreetings]:(state,action)=>{
        return state.slice(0,-1);
    },
    initialState
});

export default reducer;

```