---
layout: post
title: Redux 제대로 공부하기
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---

Redux를 공부해보자🍀

## topic 🚀

1. Redux 란

2. Redux 내 store,state,action,dispatch,subscribe,getState 용어 정리

### Redux 란

Redux란 자바스크립트 생태계 내에서 쓰이는 상태관리 라이브러리이다. 주로 React와 함께 쓰이며, 상당한 양의 데이터를 쉽게 다룰 수 있게 해준다.

1. state로 상태를 관리하여 application의 복잡성을 줄여준다.
2. 외부로부터 state에 직접 접근을 차단한다. state에 접근하기 위해서는 getState를 통해서 접근 가능 🙆‍♀️, state 값을 바꾸기위해서는 dispatcher와 reducer를 사용해서 변경가능하다. => data가 바뀔때마다 각각의 부품 별로 본인의 일에 충실하면 된다...!
3. undo(원상태로 돌리다),redo(무엇을 다시하다) 가 쉽다 👏 => state 값을 변경할때 원본을 수정하는 것이 아니라 ‼ 복제한 원본을 수정하여 새로운 원본으로 만드므로 이전 상태 recording이 가능하다.4. hot module Reloading => 자동으로 바뀐 state 가 application에 반영된다(그냥 되는게 아님)

### Redux 용어 정리

#### store

store는 정보가 저장되는 은행 즉, state가 모여있는 저장소라고 생각하면 된다.

#### state

변경가능한 데이터의 집합체

#### render

state값을 활용해서 UI를 만들어준다.

#### action

state를 바꾸기 위한 명령어(객체)
이 객체에 바꿔주고 싶은 값들을 넣어서 dispatch를 실행한다.

#### dispatch

dispatch는 2가지 기능을 수행한다.

1. reducer를 호출해서 state 값을 변경한다.
2. subscribe를 활용해 render를 실행한다.

##### 1. reducer를 호출해서 state 값을 변경한다.

reducer에 현재 state,action객체를 전달하면 reducer는 반환값으로 새로운 state를 return 한다. 이때 return 한 값이 새로운 state값으로 변경된다.

##### 2. subscribe를 활용해 render를 실행한다.

subscribe는 말그대로 구독이다. dispatch를 통해 reducer를 실행하여 state값을 변경하고, subscribe가 state 변경을 감지해 render를 호출하게 한다.
