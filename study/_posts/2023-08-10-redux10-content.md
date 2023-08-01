---
layout: post
title: Redux 제대로 공부하기 - 비동기 
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. 비동기란(asynchronous)
2. Side Effects
3. Async Logic

### 비동기란

동기는 어떤 요청을 수행하다가 다른 요청이 들어오면 기존의 요청을 멈추고 다른 요청을 수행한 다음, 요청이 완료되면 잠시 멈췄던 요청을 다시 실행시킨다.

비동기는 어떤 요청을 수행하다가 다른 요청이 들어와도 요청이 멈추지 않고 실행되며, 완료되면 완료되었다는 결과를 응답으로 알린다.


<figure>
    <img src="https://images.velog.io/images/dat0802/post/32583695-bf93-4d6e-856b-fe04e1cd2f1f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-18%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%203.36.46.png" width="600"  alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>


### Side Effects

Side Effects는 Reducer와 상관없이 Reducer 외부에서 보여질 수 있는 상태의 변경이나 동작이 일어나는 것을 의미한다. 예를 들면 서버와의 통신으로 인한 데이터 변경을 들 수 있다. 즉, pure function에서의 리턴 값과 관련이 없는 모든 동작을 일컫는다.

> Reducer는 Reducer의 외부와 내부를 철저하게 분리하고, 리턴 값과 관련없는 동작을 철저하게 막음으로서 완벽한 pure function을 구현하려고 한다.

### Async Logic

Redux에서 비동기 로직을 구현하려면 middleware를 사용해야한다.
middleware는 Redux에 원하는 기능을 구현할 수 있게 해주는 함수로, 대표적으로 Redux-thunk,Redux-saga가 있다.

#### Async Logic 직접 구현해보기

Async Logic 을 직접 구현해보자. Async Logic을 직접 만들어서 쓸 수 있으나 이렇게 하면, 많은 middleware를 직접 만들어서 써야하는 불편함이 있다...

```js
const timeSettingActionMiddleware = store => next => action => {
    if(action.type==='my-app/post/delete'){
        setTimeout(()=>{
            next(action);
        },1000)
    }
    return next(action);
}

const fetchUpdateMiddleware = store => next => action => {
    if(action.type==='my-app/user/update'){
        apiClient.get('update').then(user=>{
            store.dispatch({type:'my-app/user/UPDATE',payload:user});
        })
    }
}

```

#### Async function Middleware

좀 더 범용적으로 쓰이는 middleware를 살펴보자

```js
// 미들웨어 함수 구현
const asyncLogicMiddleware = store => next => action => {
    if(typeof action==='function'){ 
    // action의 type이 함수이면 비동기 로직이므로 action함수에 인자로 store.dispatch와 store.getState 기능을 넘김
        return action(store.dispatch,store.getState);
    }
    return next(action);
}
export default asyncLogicMiddleware;


// store에 미들웨어 연결

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION__COMPOSE__ || compose;
// compose -> Redux와 middleware를 연결해줌

const store = createStore(rootReducer,composeEnhancers(applyMiddleware(asyncFunctionMiddleware)));

// mapDispatchToProps 함수에 return 함수로 설정
function mapDispatchToProsp(dispatch,ownProps){
    return {
        onAsyncFunction:(asyncFunction) => dispatch(asyncFunction) // action 객체가 아닌 함수를 받아옴
    }
}


```