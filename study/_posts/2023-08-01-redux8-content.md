---
layout: post
title: Redux 제대로 공부하기 - Ducks 패턴
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. Ducks pattern 이란

2. Ducks pattern 사용법

### Ducks pattern 🐤

Ducks pattern은 디자인 패턴의 하나이다. 패턴이란 소프트웨어 설계할때 쓰이는 효과적인 정형화된 방법을 의미한다.
그 중, Ducks pattern이란 Redux를 쓸때, 쓰이는 디자인패턴이다. 
애플리케이션의 규모가 커질수록, Redux Store도 그만큼 복잡해지게 된다. 이때 이를 효율적으로 정리하고 관리할 수 있게 해주는 규칙이 바로 Ducks pattern이다.


#### 📌 Duck

Ducks pattern은 Redux Reducer Bundle이라고도 불린다. Bundle은 꾸러미라는 의미로 Action,Reducer,Action Creator,외부로부터 하나의 특정한 기능에 대한 데이터를 받아오는 행위인 Side Effects 를 하나로 모아놓는 것을 의미한다.

Duck file은 Actions,Reducer,Action Creators,Side effects 순으로 적어햐하며, 다음과 같은 규칙을 따라야 한다.

1. reducer는 export default로 export 할 수 있어야한다.
2. action creator는 export 시 자기 자신의 name으로 export 해야한다.
3. actions type은 app 이름/reducer/ACTION_TYPE 이런식으로 적어햐한다.
4. actions type은 UPPER_SNAKE_CASE를 따뤄야하며 영어 대문자로 적고 export 할 수 있어야 한다.


```js
// 1. ACTIONS(ACTION TYPES)
const ACTION_TYPE_ADD_GREETINGS='my-app/greetings/ADD_GREETINGS'
// 2. Redcuer
const initialState = [];
export default function greetingsReducer(state=initialState,action){
    switch (action.type) {
        case ACTION_TYPE_ADD_GREETINGS:
            return state.concat(action.text);
        default:
            return state;
    }
}

// 3. Action Creators

export function addGreetingsCreator(text){
    return {
        type: ACTION_TYPE_ADD_GREETINGS, text:text
    }
}

// side-effects
//...서버 데이터 불러오는 내용 적기
export function getGreetings(){
    return dispatch => get('/greetings').then(greeting=>dispatch(....))
}

```