---
layout: post
title: 모던 리액트 Deep Dive 2장 - part 1.
description: >
  React에 Redux를 적용하기 위한 공부
image:
sitemap: false
---

## 02장, 리액트 햄심 요소 깊게 살펴보기 📑

1. JXS란? 👩‍🎤

> A syntax extension to JavaScript = 자바스크립트 확장 문법

JSX란 JavaScript + XML / HTML 합친것이라 볼 수 있다.
JSX는 **트랜스파일**이라는 과정을 거쳐 자바스크립트(ECMAScript)가 이해할 수 있는 코드로 변경된다.

### JSX는 어떻게 자바스크립트에서 변환될까?

JSX는 XML,HTML코드를 JS로 변환한다. 이는 React가 내부적으로 React.createElement 를 사용하여 JSX 코드를 변환하기 때문이다.

```js
    React.createElement(
      type:React element,
      [props]:속성,
      [...children]:자식 element
    );
```

## cf. 참고

React.createElement 로 변환되면서 해당 element의 정보를 담고 있는 자바스크립트 객체가 생성된다.

```js
const element = {
  type: "h1",
  props: {
    className: "greeting",
    children: "Hello World",
  },
};
```

React에서 JSX를 사용하는 것이 필수는 아니나, JSX를 사용하면 쉽게 React 를 작성 할 수 있다.
