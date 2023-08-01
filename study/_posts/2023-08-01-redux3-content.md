---
layout: post
title: Redux 제대로 공부하기 - Flux & Redux
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---

생활코딩님의 redux 강의를 들은 후 redux 공식 문서를 보면서 react와 함께 써보려고 했다... 
하지만 redux를 쓸때 redux-toolkit를 추천한다는 문구와 함께 모든 예제가 reduxjs/toolkit를 활용한 것 밖에 없었다...!
redux를 배워야 redux-toolkit를 제대로 활용할 수 있겠다는 생각이 들어 redux를 soapple님의 [Redux]강의를 들으면서 정리하고자 한다.

## topic 🚀

1. Redux 를 왜 쓸까

2. Flux 란
- 1. Redux와 Flux의 관계
3. Redux의 3가지 원칙
4. 꼭 Redux를 써야할까

### Redux 를 왜 쓸까

애플리케이션의 규모가 커지고 컴포넌트의 규모가 많아지게되면서 각각의 상태를 관리하는 것이 매우 복잡해졌다. 
> 상태는 서버로부터 오는 data, 로컬 data, UI 컴포넌트의 상태를 포함한다.

이러한 상태를 효과적으로 관리하기위해 Redux가 나타나게 되었다.

### Flux란

Flux란 단방향 데이터 흐름을 나타낸 아키텍쳐이다.


어떠한 action (행위)가 일어나면 Dispatcher가 행위를 발송하여 store에 전달하고 전달된 상태로 바뀌어서 View(화면)에 뿌려지게된다. 

<figure>
    <img src="https://haruair.github.io/flux/img/flux-simple-f8-diagram-1300w.png" width="600"  alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>


#### Flux와 Redux의 관계

Redux는 Flux를 실제로 구현한 라이브러리이다...👩‍🎤

Flux의 단방향 데이터 흐름을 실제로 구현하였는데, action이 생기면 dispatcher가 store에 전달된 상태를 발송하고, reducer가 이전의 상태를 발송된 상태로 바꿔서 화면에 뿌려준다.

<figure>
    <img src="https://miro.medium.com/v2/resize:fit:1400/1*N62EZSeQNHtwVumCQOdU-Q.png" width="600"  alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

### Redux의 3가지 원칙

Redux의 3가지 원칙은 다음과 같다...

1. sigle source of the truth
2. state is read-only
3. changes are made with pure functions 


#### 📌single source of the truth

single source of the truth (진실의 원천은 단 하나..)
redux store라는 곳에서만! 진실로 상태값을 관리하고 있다는 의미이다.

#### 📌state is read-only

state는 읽기전용이므로 Redux store안의 상태 값은 직접 변경할 수 있는 것이 아니라 정해진 어떤 규칙에 의해서만 상태값을 바꿀 수 있다.

#### 📌changes are made with pure functions

pure 함수란, 입력값이 함수안에서 변경되지 않고, 입력값이 같으면 같은 출력값을 얻게 될 수 있는 함수를 말한다.
pure functions인 reducer라는 함수를 통해서만 상태를 변경하는데 이때 reducer에 들어오는 입력값은 동일한 원본 데이터 값을 직접 변경하지 않으며, 입력값이 같은 경우, 같은 출력값을 줄 수 있다.