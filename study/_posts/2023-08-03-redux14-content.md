---
layout: post
title: Redux 제대로 공부하기 - hook
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. useSelector
2. useDispatch
3. useStore

## Redux hook

react의 hook처럼 react-redux에서 제공하는 hook에 대해서 배워보자

### useSelector

Redux Store의 state 중 원하는 state값만 추출할 수 있게 해주는 훅이다. connect()함수의 mapStateToProps()와 동일한 역할을 한다.

|비교|useSelector|mapStateToProps|
|------|---|---|
|위치|함수형 컴포넌트 내에 존재(o)|(x)|
|재렌더링|Action이 Dispatch되어 useSelector의 이전값과 달라지게 되면 컴포넌트 재렌더링|props로 내려준 state가 바뀌게 되면 컴포넌트 재렌더링|


### useDispatch

Redux의 Action을 Dispatch할 수 있게 해주는 훅으로, Redux Store의 dispatch함수에 대한 레퍼런스이다. 
connect() 함수의 mapDispatchToProps() 함수와 동일한 역할을 한다.

useEffect훅에서 사용시 dependency에 useDispatch훅을 넣어주어야한다. 이렇게 해야 eslint에서의 에러가 나지 않는다. 또한 Redux Store의 인스턴스가 변경되지 않는 이상 useDispatch 훅이 바뀔 일은 없다. Redux Store의 인스턴스는 거의 변하지 않기때문에 useEffect훅이 dependency로 인해 호출되는 경우는 거의 없다.


### useStore

Redux Stroe에 접근할 수 있게 해주는 훅으로 Redux Store에 대한 레퍼런스를 반한한다. 
Redux Store에 직접 접근해서 뭔가를 하면 Side Effect가 생겨서 오류가 발생할 수 있기 때문에 만약에 Store의 데이터 값을 가져오고 싶은 거라면 useStore 훅 사용을 자제하고 useSelector로 가져와야한다. 