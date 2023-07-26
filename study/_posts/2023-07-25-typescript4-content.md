---
layout: post
title: typescript 제대로 공부하기 4
description: >
  typescript 를 SPA 프레임워크에 적용하기위한 공부
image:
sitemap: false
---

제로초의 강의[웹 게임을 만들며 배우는 React에 TypeScript 적용하기] 를 들으면서 중요한 개념을 정리한 문서이다.

## topic 🚀

1. react router에서는 history,location,match만 기억하자
2. class Component 와 function Component 문법에서 사용하는 방식이 다르다.(type은 같음!)

📌해당 강의에서 적용하는 react-router는 v5버전으로 @types/react-router와 @types/react-router-dom을 설치해야한다.

📌react-router v6버전에서는 @types/react-router @types/react-router-dom을 설치할 필요가 없다...!

### react router에서는 history,location,match만 기억하자

브라우저와 react 의 라우터를 연결하게 되면, 라우터가 history library에 접근할 수 있게 되며, 각각의 Route와 연결된 컴포넌트에 props로 match,location,history라는 객체를 전달한다.

#### Match

match 객체에는 <Route path>와 url에 대한 정보가 담겨있다.

```js
params => url path로 전달된 파라미터 객체 
export interface match<Params extends { [K in keyof Params]?: string } = {}> {
    params: Params;
    isExact: boolean; -> true 일 경우 전체 경로가 완전히 매칭될 경우에만 요청 수행 
    path: string;
    url: string;
}

```

#### location

location 객체에는 현재 페이지에 대한 정보를 가지고 있다. 

location.search로 현재 url의 쿼리 스트링에 대한 정보를 가져올 수 있다.

```js
export interface Location<S = LocationState> {
    pathname: Pathname;
    search: Search;
    state: S;
    hash: Hash;
    key?: LocationKey | undefined;
}

```


#### history

스택(stack)에 현재까지 이동한 url 경로들이 담겨있는 형태로, history namespace 값에 접근하여 주소를 변경하거나 되돌릴 수 있게 해준다.

```js
export interface RouteChildrenProps<Params extends { [K in keyof Params]?: string } = {}, S = H.LocationState> {
    history: H.History;
    location: H.Location<S>; -> H === history 라이브러리 
    match: match<Params> | null;
}

----------------------------------------------------
history 라이브러리안에 있는 설정 

export interface History<HistoryLocationState = LocationState> {
    length: number;
    action: Action;
    location: Location<HistoryLocationState>;
    push(location: Path | LocationDescriptor<HistoryLocationState>, state?: HistoryLocationState): void;
    replace(location: Path | LocationDescriptor<HistoryLocationState>, state?: HistoryLocationState): void;
    go(n: number): void;
    goBack(): void;
    goForward(): void;
    block(prompt?: boolean | string | TransitionPromptHook<HistoryLocationState>): UnregisterCallback;
    listen(listener: LocationListener<HistoryLocationState>): UnregisterCallback;
    createHref(location: LocationDescriptorObject<HistoryLocationState>): Href;
}

```

### class Component 와 function Component 문법에서 사용하는 방식이 다르다.(type은 같음!)


클래스 컴포넌트에서는 제네릭으로 RouteProps를 정의한다음 this.props로 match,location,history에 접근할 수 있는 반면, 함수형 컴포넌트는 hooks인 useLocation, useRouteMatch, useHistory 를 활용하여 접근이 가능하다.

```js
// 클래스형 컴포넌트
class test extends Component<RouteComponentProps<{name:string}>>{
    render(){
      const {match,location} = this.props;
      
      
    }

}
// 함수형 컴포넌트
const test = () => {
    const match = useRouteMatch();
    const location = useLocation();
    const history = useHistory();
}

```