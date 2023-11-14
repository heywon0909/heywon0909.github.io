# 7장, React 타입 분석하기

### tsconfig

```jsx
"jsx":"react"
```

- jsx 속성이 JSX 문법을 지원할지 결정하는 옵션
  - 속성을 react로 입력하면 웹용 React에서 실행하는 문법으로 변환

```jsx
"jsx":"react-native"
```

- JSX문법이 그대로 유지되어 React Native에서 실행되는 코드가 됨

### React는 CommonJS 모듈을 따른다

```jsx
export = React;

export as a namespace React;
declare namespace React{

}
```

- React를 .tsx 파일에서 쓸 때, React를 import 함 ⇒ ECMAScript 모듈 시스템을 따르는 것으로 보임
  - but… **export=React**이므로 React도 CommonJS 모듈 시스템을 따른다

### 🔴 React Hooks 분석하기

```jsx
declare namespace React{
	function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];
  function useState<S = undefined>(): [S | undefined, Dispatch<SetStateAction<S | undefined>>];
	function useRef<T>(initialValue: T): MutableRefObject<T>;
	function useRef<T>(initialValue: T|null): RefObject<T>;
	function useRef<T = undefined>(): MutableRefObject<T | undefined>;
	function useEffect(effect: EffectCallback, deps?: DependencyList): void;
	function useCallback<T extends Function>(callback: T, deps: DependencyList): T;
	function useMemo<T>(factory: () => T, deps: DependencyList | undefined): T;
}
```

- **useState, useRef는 오버로딩이 존재하고,** useEffect 와 useCallback,useMemo는 하나

<aside>
💡 오버로딩(Overloading) ⇒ 같은 이름을 가진 메소드가 있더라도, 매개변수의 개수 또는 타입이 다르면, 같은 이름을 사용해서 메소드를 정의할 수 있다.

</aside>

### useState

```jsx
	function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];
  function useState<S = undefined>(): [S | undefined, Dispatch<SetStateAction<S | undefined>>];

	type SetStateAction<S> = S | ((prevState:S)=>S);
	type Dispatch<A> = (value:A) => void;
```

- useState의 오버로딩은 매개변수의 유무로 구분
  - 매개변수가 있느냐(첫번째)
  - 매개변수가 없느냐(두번째)

### UseRef

```jsx
function useRef<T>(initialValue: T): MutableRefObject<T>;
function useRef<T>(initialValue: T|null): RefObject<T>;
function useRef<T = undefined>(): MutableRefObject<T | undefined>;
```

- 3가지 오버로딩이 있음
  - MutableObject
    - current 속성 수정 가능
    ```jsx
    interface MutableRefObject<T> {
      current: T;
    }
    ```
  - RefObject
    - current 속성 readonly
