---
layout: post
title: Redux 제대로 공부하기 - container
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. Container란 

2. React-Redux
- 2-1. React-Redux 주요 API

3. React와 Redux 연동 과정

### Container란

Redux 공식 개념은 아니지만, React와 Redux를 연결시켜주는 Container 컴포넌트라고 생각하면 된다.
Redux의 모든 state와 연결되어있다는 개념이 아니라,
일부 state와 dispatcher를 포함하는 리액트 컴포넌트이다. 쉽게 생각하면 Context API처럼 컴포넌트를 우산처럼 감싸고 있는 느낌이라고 생각하면 된다.


### React-Redux

Redux는 독립적인 자바스크립트 라이브러리로, Redux는 UI와 관계없이 독립적으로 쓰일 수 있다. Redux는 Vue, Angular와도 쓰일 수 있으나, React와 가장 호환성이 좋다.
React에서 Redux를 쓰기 위해서는 React와 Redux를 이어주는 라이브러리(Binding Library)인 React-Redux를 설치해야한다.

> Binding Library = Bindings = 어떤 라이브러리를 특정 환경에서 사용하기 위해서 이어주는 라이브러리

#### 주요 API

1. Provider : Context API Provider처럼 하위 컴포넌트들이 Redux store에 접근할 수 있게 우산을 씌워주는 컴포넌트(Higher Order Component)이다.

두 함수는 Redux에 내장되어있는 함수가 아닌 개발자가 임의로 구현하여 써야하는 함수들이다. 이 함수들을 통해 props로 Redux의 state,dispatch 일부를 내려줄 수 있다.

2. mapStateToProps() : mapping Redux state to target Component props
3. mapDispatchToProps() : mapping Redux dispatch to target Component props

4. connect() : mapStateToProps와 mapDispatchToProps를 인자로 받으며 Redux와 React 컴포넌트를 연결시켜준다. connect를 통해 container Component가 생성된다.


###  React와 Redux 연동 과정

React와 Redux를 연동하기 위해서는

1. Redux Store을 만든다.
2. Redux Action Type을 정의하고 Action Creator와 Reducer를 생성한다.
3. React Component를 만든다.
4. Redux와 React Component를 연결하여 Container를 만든다.
5. Provider를 사용해 Redux Store를 전달해준다.
7. React Component가 아닌 Container Component를 실제로 렌더링 시켜준다.