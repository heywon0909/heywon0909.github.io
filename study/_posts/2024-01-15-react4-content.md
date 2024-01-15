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

### 리액트 렌더링 과정

#### VDOM

React Element가 업데이트 될 때, 실제 브라우저 DOM에 바로 반영되지 않고,
가상 DOM에 반영된 후, DOM 업데이트가 이뤄진다.

가상 DOM을 사용함으로써, 개발자는 사용자의 인터렉션에 따른 DOM의 변경사항을 수동으로 추적하지 않아도 되며, 여러번 발생하는 렌더링 과정을 줄일 수 있게 된다.

> DOM: Document Object Model로 브라우저가 웹페이지의 콘텐츠와 구조를 어떻게 보여줄지에 대한 정보를 담고 있음

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

#### 리액트 파이버 트리

파이버 트리는 current 파이버 트리, workInProgress 트리 총 2개 존재한다.
React Fiber의 작업이 끝나면 리액트는 workInProgress 트리를 현재 트리로 바꿔치기한다.(더블 버퍼링)<br/>
보이지 않는 곳에서 작업을 마무리 한 다음, 완성되면 새로운 그림으로 갈아끼우는 것이다.

업데이트가 발생하면, 앞서 만든 current 트리가 존재하는 상황에서, workInProgress트리를 다시 빌드한다. 이때 같은 React Fiber를 활용하여 변경 작업을 처리한다.<br/> 기존 Fiber를 재활용해 내부 속성값만 변경하여 트리를 업데이트한다.

이를 기반으로 실제 DOM과 비교하여 실제 DOM에 필요한 변경사항을 최소한으로 적용한다.
