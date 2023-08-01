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

1. Redux는 state-persistence가 아닌 state-management 라이브러리이다.

2. Redux 관련 함수들에 대해서 알아보자 👩‍🎤

### Redux는 state-persistence가 아닌 state-management 라이브러리

> state-management : 실제 저장장치에 상태를 저장하지 않고, 상태를 관리함(상태가 영구적이지 않음)
> state-persistence : 실제 저장장치에 상태를 저장하므로 상태가 지속됨 

Redux는 웹브라우저 새로고침, 컴퓨터 재부팅시 Redux-store에서 데이터가 모두 날아가므로 state-management 라이브러리이다.

### Redux 관련 함수들에 대해서 알아보자 👩‍🎤

1. createStore() : Redux store 생성
2. applyMiddleware(): Redux에 원하는 기능을 추가하고 싶을때 middleware를 사용한다. 해당 함수는 middleware를 Redux에 넣을때 사용한다.
3. getState() : Redux는 getState를 통해서만 상태에 접근 가능하다.
4. dispatch() : Action을 실제로 실행시킨다.


