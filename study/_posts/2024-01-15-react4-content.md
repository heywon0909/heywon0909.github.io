---
layout: post
title: 모던 리액트 Deep Dive 2장 - part 1.
description: >
  React에 Redux를 적용하기 위한 공부
image:
sitemap: false
---

## 02장, 리액트 햄심 요소 깊게 살펴보기 📑

2. 가상 DOM과 리액트 파이버
3. 렌더링은 어떻게 일어나는가?

### 1. 리액트 렌더링

브라우저(화면) UI를 그릴 DOM 트리를 만드는 과정

리액트 렌더링은 최초 한번, 그 이후에 발생하는 모든 렌더링(리렌더링)이 있다...

렌더링은 상태(state 나 props)의 변화에 따라 일어난다.

1. 클래스형 컴포넌트

- setState가 실행
- foreUpdate로 강제 렌더링

2. 함수형 컴포넌트

- useState의 두 번째 배열 요소인 setter 실행
- useReducer의 두 번째 배열 요소인 dispatch 실행

### 2. 리액트 렌더링 프로세스

1. render


    - 컴포넌트를 실행해 이전 가상DOM과 비교해 바뀔 필요 있는 부분 파악(type,props,key)

2. Fiber reconciler


    - 변경 사항을 가상DOM에 반영한 뒤 렌더링 요청

3. commit


    - 바뀐 부분 실제 DOM 에 적용

4. update

단, 항상 이 프로세스를 끝마치는 것은 아니다...<br/>
렌더링 프로세스를 진행하다가 변경 사항이 없다면 commit까지 안가고 중간에 멈출수도 있다.

#### VDOM

React Element가 업데이트 될 때, 실제 브라우저 DOM에 바로 반영되지 않고,
가상 DOM에 반영된 후, DOM 업데이트가 이뤄진다.

가상 DOM을 사용함으로써, 개발자는 사용자의 인터렉션에 따른 DOM의 변경사항을 수동으로 추적하지 않아도 되며, 여러번 발생하는 렌더링 과정을 줄일 수 있게 된다.

> DOM: Document Object Model로 브라우저가 웹페이지의 콘텐츠와 구조를 어떻게 보여줄지에 대한 정보를 담고 있음

#### 리액트 파이버 트리

가상 DOM은 current 파이버 트리, workInProgress 트리 총 2개로 만들어진다.

파이버 트리는 React fiber로 구성된 트리를 말하며,
파이버 트리는 current 파이버 트리, workInProgress 트리 총 2개 존재한다.
React Fiber의 작업이 끝나면 리액트는 workInProgress 트리를 현재 트리로 바꿔치기한다.(더블 버퍼링)<br/>
보이지 않는 곳에서 작업을 마무리 한 다음, 완성되면 새로운 그림으로 갈아끼우는 것이다.
<br/><br/>
업데이트가 발생하면, 앞서 만든 current 트리가 존재하는 상황에서, workInProgress트리를 다시 빌드한다. 이때 같은 React Fiber를 활용하여 변경 작업을 처리한다.<br/> 기존 Fiber를 재활용해 내부 속성값만 변경하여 트리를 업데이트한다.
<br/><br/>
이를 기반으로 실제 DOM과 비교하여 실제 DOM에 필요한 변경사항을 최소한으로 적용한다.

#### React Fiber

리액트는 React Fiber를 활용해 VDOM을 만든다.

리액트는 babel을 통해 React element를 React.createElement로 변환하고, 그 결과로 javascript 객체(React Fiber)가 생성된다.

React Fiber는 하나의 React Element에 하나 생성된다. (1:1 관계)

Fiber reconciler는 VDOM과 실제DOM을 비교해 변경된 부분을 파악하여 화면에 렌더링을 요청한다.

##### 리액트 조정 알고리즘

과거에는 스택 알고리즘을 활용하여 동기적으로 처리.
스택에 쌓이는 작업이 많아질수록 동기적으로 처리하는데 오래 걸림

비동기적으로 작업을 처리하는 스택 조정자 대신 React Fiber로 대체

파이버의 작업 원리

- 리액트는 하나의 작업 단위로 구성된 파이버를 생성한다.

1. 작업 수행: beginWork() 수행
2. 작업 완료: finishedWork() 수행
3. 커밋: commitWork() 실행

생성된 파이버는 state가 변경되거나 생명주기 메서드가 실행되거나, DOM의 변경이 필요한 시점에 실행된다.

이 과정은 바로 처리되거나, 스케쥴링된다.

이러한 React Fiber가 모여 가상DOM 형성한다.

> 가상DOM은 React Fiber(자바스크립트 객체)로 관리된다. 리액트 개발 팀은 리액트는 가상DOM이 아닌 Value UI, 즉 값을 가지고 있는 UI를 관리하는 라이브러리라는 내용을 피력한 바 있다. 즉, 핵심 원칙은 UI를 문자열, 숫자, 배열과 같은 값으로 관리한다는 것이다.

#### 동시성 렌더링

리액트 18에서 도입되어 의도된 우선순위로 컴포넌트를 렌더링할 수 있다.<br/>
렌더링 중 render 단계가 **비동기**로 작동해 특정 렌더링의 우선 순위를 낮추거나,
필요하다면 중단하거나 재시작, 포기도 가능하다.

브라우저의 동기 작업을 차단하지 않고, 백그라운드에서 새롭게 리액트 트리를 그릴 수 있다. 이를 통해 매끄러운 사용자 경험을 할 수 있다.
