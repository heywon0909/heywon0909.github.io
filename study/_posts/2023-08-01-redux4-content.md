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

1. immutablitiy

2. Redux vs Context API

### immutablitiy

> immutablitiy = immutable = 변할 수 없는

React에서 immutability란,
1. state(상태)는 Redux store에서 생성 후 값을 직접 변경할 수 없다는 점,
2. pure function인 reducer를 사용할때 인자값으로 받은 원래 상태값을 입력값으로 변경하지 않고 새로운 객체를 생성해서 상태값을 변경하고, 입력값이 동일할때는 동일한 결과값을 가지게 된다는 것을 의미한다.
=> 원본값을 변경하는게 아니라 새로운 값으로 바꿔주기때문에 불변성을 침해하지 않는다... 하지만 이렇게 불변성을 지키며 짜는 것이 어렵기 때문에 immerjs 라는 라이브러리를 같이 쓰기도 한다.

redux-toolkit도 immerjs 속성을 내재하고 있다고 한다.


### Redux vs Context API


> props drilling : 데이터를 조상에서 자식컴포넌트로 데이터를 뿌려주는 로직을 의미한다.
하지만 조상에서 자식으로 아래로 아래로 계속 내려주다보면 props 추적이 어렵고 데이터 관리도 어려워지게 된다.


Redux와 Context API는 둘다 props drilling을 해결하기위한 방안이다. 둘다 props로 데이터를 내려주는 것이 아니라 component tree 를 통해 특정 계층의 컴포넌트에 데이터를 뿌려줄 수 있게 한다.

Redux는 내부적으로 Context API를 사용하고 있어 Redux와 Context API간의 차이가 존재하지 않다고 생각할 수 있다...

하지만, Redux 는 redux-devtools를 활용해 데이터 관리를 시각적으로 확인해볼 수 있으며, time-travel 기능을 통해 상태값을 이전 history내역으로 debugging을 해볼 수 있다. Redux는 action과 reducer를 통해서만 상태값을 변경할 수 있다.
Conext API는 Redux와 관리 store라는 별도 공간에서 데이터를 관리하고 있지 않으며, 단순히 컴포넌트 간의 state 전달 통로의 역할을 가지고 있다. 



