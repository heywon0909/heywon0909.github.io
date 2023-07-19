---
layout: post
title: typescript 제대로 공부하기 3
description: >
  typescript 를 SPA 프레임워크에 적용하기위한 공부
image:
sitemap: false
---

제로초의 강의[웹 게임을 만들며 배우는 React에 TypeScript 적용하기] 를 들으면서 중요한 개념을 정리한 문서이다.

## topic 🚀

1. type을 모를때는 React defintion 문서를 살펴보자
2. typescript를 적용했을 시, styled component 와 React 는 충돌이 날 수 있다...
3. eslint 를 적용하려면 typescript-eslint를 사용하자

### type을 모를때는 React defintion 문서를 살펴보자

type 을 모르는 부분이 존재할 때, 오른쪽 마우스를 클릭하여 Go to Definition 을 클릭하면, 해당 문서 내용(index.d.ts)을 확인해 볼 수 있다.

#### ReactNode

ReactNode는 컴포넌트 생성시 return 부분에 해당하는 즉, JSX 칸에 넣을 수 있는 모든 것을 아우르는 개념이다.

```js
 type ReactNode =
        | ReactElement -> HTML
        | string
        | number
        | Iterable<ReactNode> -> React Fragment
        | ReactPortal
        | boolean
        | null
        | undefined
        | DO_NOT_USE_OR_YOU_WILL_BE_FIRED_EXPERIMENTAL_REACT_NODES[keyof DO_NOT_USE_OR_YOU_WILL_BE_FIRED_EXPERIMENTAL_REACT_NODES];


```

#### ReactElement

html tag 를 의미한다. -> props, key를 가질 수 있는 JSX element 를 의미한다.

```js
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
        type: T;
        props: P;
        key: Key | null;
    }
```

#### ReactText

태그 사이에 text 나 숫자를 의미한다.

```js
type ReactText = string | number;
```

### typescript를 적용했을 시, styled component 와 React 는 충돌이 날 수 있다...

typescript를 적용했을 시, styled component 와 React는 우선순위 충돌로 인해 오류가 발생할 수 있다.

### typescript에 eslint를 적용하고 싶다면 typescript-eslint를 사용하자

typescript에 eslint를 적용하고 싶다면 typescript-eslint를 사용해 더 엄격하게 체크할 수 있다.

<타입스크립트eslint>
https://typescript-eslint.io/
