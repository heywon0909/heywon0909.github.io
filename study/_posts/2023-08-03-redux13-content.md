---
layout: post
title: Redux 제대로 공부하기 - redux toolkit
description: >
  React에 Redux를 적용하기 위한 공부 
image:
sitemap: false
---


redux를 soapple님의 [Redux]강의를 들으면서 정리한 내용이다.

## topic 🚀

1. Redux Toolkit
- 1-1. Slice
- 1-2. Redux Toolkit API
- 1-3. immer 사용 패턴

### Redux Toolkit

현재 Redux에서 권장하는 Redux 개발을 위한 도구이다.

#### Slice 🧩

Redux를 구성하는 요소들의 조각으로 Action Types,Action Creators,Reducer 를 하나의 slice 안에 구성시켜놓는다고 생각하면 된다. 마치 Duck file과 비슷한 형태를 띄고 있는데 단, Action Types,Action Creator는 Reducer에 의해 자동으로 생성된다.

##### Redux Toolkit API

1. configureStore() <br>
Redux의 CreateStore와 비슷한 기능으로 Redux Store를 만들때 사용한다.
2. createAction() <br>
Action Type과 Action Creator를 만들어주는 함수이다.
3. createReducer() <br>
Reducer를 만들어 주는 함수로 쓰는 형태를 볼때 state를 직접 변경해서 쓰기도 하는데 이는, Redux toolkit이 immer를 내장하고 있기때문에 immutabe updates를 자동으로 해주기때문에 가능하다.
4. createSlice() <br/>
Slice 조각을 만들어주는 함수이다.
5. createAsyncThunk() <br>
비동기 로직을 위한 Thunk 함수를 만들어준다.
6. current() <br/>
redux toolkit이 내부적으로 immer를 쓰고 있기 때문에 state 값을 가져오려고 하면 프록시 인스턴스값이 나오므로 state 값에 접근하기 위해서는 current를 써야한다.

##### immer 사용 패턴 🔍

1. State 수정 또는 반환
- 기존 State값을 직접 수정하거나 새로운 State를 만들어서 반환한다.
2. State 교체 또는 초기화
- State = 새로운 값 이렇게 대입하지 않고, 새로운 State나 초기 State 값을 변화시켜 반환한다.
3. 디버깅 및 Drafted State 검사
- 현재 State를 console.log로 출력하기 위해서는 current 함수를 활용해서 출력해야 정상적으로 출력된다.
4. 중첩된 데이터 업데이트 
- 중접된 객체의 변수가 String,number,boolean등의 값일 경우, value 값의 업데이트를 트래킹하지못하기 때문에 변수를 선언해서 값을 대응시키는 것이 아니라 객체.변수 로 접근해아한다.


```js
const greetingSlice = createSlice({
    name:'greeting',
    initialState:[],
    reducers:{
        greetingsUpdate: (state,action) => {
            const itemId = action.payload;
            const greeting = state.find((greeting)=> greeting.id===itemId);
            if(greeting){
                // 💩 -> 값을 트래킹할 수 없음
                let {infashion} = greeting;
                infashion = !infashion
                // 👍
                greeting.infashion = !greeting.infashion
            }
        }
    
    }
})
```

- 중첩된 객체의 키값이 존재하지 않을 경우 오류가 발생할 수 있기때문에 확인한 후에 작업을 해야한다.


```js
const greetingSlice = createSlice({
    name:'greeting',
    initialState:[],
    reducers:{
        addGreeting: (state,action) => {
            const {itemId,country}= action.payload;
            
            // 💩 -> country에 해당하는 값이 존재하지 않을 경우 오류 발생...!
            state[country].push(item);
            
            // 👍
            if(!state[country]){
                state[country] = [];
            }

            state[country].push(item);
        }
    
    }
})
```