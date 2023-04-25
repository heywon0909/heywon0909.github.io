---
layout: post
title: HW-shoppy api 모듈
image:
  path: /assets/img/mini/hw-shoppy/proceduralprogramming.png
description: >
  [hw-shoppy] api 모듈 정의에 대하여
sitemap: false
---

[hw-shoppy] 프로젝트를 진행하면서 api 모듈을 어떻게 분리하고 작성할지에 대해 고민해보았다.✍
{:.lead}

- api 기능별 함수로 분리하여 정의하기
  {:toc .large-only}
- 함수형 api 에서 객체 api 모듈로
  {:toc .large-only}

## api 기능별 함수로 분리하여 정의하기

기능별 함수로 분리하여 사용하는 것은 기능이 생길때마다 함수를 계속 생성해줘야했다.
함수로 생성하다보니 코드가 커지게 되면서 여러 함수를 관리하는 것이 복잡해졌다.

firebase로 api 호출을 하다보니 firebase database를 반복적으로 조회하는 부분들이 생겨났다.
예를 들면 나의 관심 목록 전체를 가져오는 api 와 상품 상세 화면에서 동일하게 collection(db,"interest") 를 생성하여 조회하고 있었다...

또한,상품 id를 통한 나의 관심 상품인지 파악하는 것과 나의 관심목록 전체를 가져오는 함수를 따로 빼는 것보다 하나의 함수에서 id를 flag 값으로 조회하는 것이 훨씬 코드의 양을 줄이고 가독성을 높일 수 있을 것이다.

이러한
함수 단위로 코드를 분리하고 재사용하는 형태인 [절차적(구조적) 프로그래밍](https://yozm.wishket.com/magazine/detail/1396/) 보다 비슷한 유형의 코드를 하나의 group으로 묶어서 생각하는 방식 [객체 재향 프로그래밍](https://yozm.wishket.com/magazine/detail/1396/)으로 변경의 필요성을 느끼게 되었다.

## 함수형 api 에서 객체 api 모듈로

함수형 api 에서 공통적으로 쓰이는 부분은 1. firebase database 에서 데이터를 조회 2. 조회해온 데이터를 포맷팅 이다.

데이터만을 조회해오는 class 인 ShopClient 와 ShopClient에서 조회해온 데이터를 포맷팅하는 Shop class로 나눠서 구현하였다.

## firebase 사용하기

> [파이어베이스는](https://blog.wishket.com/%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4firebase%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%8B%AC%EC%B8%B5-%ED%83%90/)는 "앱을 개발하고 개선하고, 키워갈 수 있는" 도구 모음 이며 이러한 도구가 없다면 개발자들은 일반적으로 서비스의 상당 부분을 직접 만들어내야만 한다.

1. firebase 가져오는 js
2. firebase 데이터 읽고 쓰는 api 모듈
3. api 모듈에서 가져온 데이터 포맷팅

총 3가지 파일로 나눠보았다.

### firebase.js

```js
// Javascript code with syntax highlighting.
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: process.env.REACT_APP_API_KEY,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN,
  databaseURL: process.env.REACT_APP_DB_URL,
  projectId: process.env.REACT_APP_PROJECT_ID,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_MS_SENDER_ID,
  appId: process.env.REACT_APP_APP_ID,
  measurementId: process.env.REACT_APP_MEASUREMENT_ID,
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
```

firebase config에 필요한 값 설정들은 env 파일을 만들어 환경변수로 관리하였다.

### firebase 저장소 사용하기

나의 firebase 저장소의 구성은 이러하다

| 컬렉션            | 문서     | 컬렉션     | 문서          |
| ----------------- | -------- | ---------- | ------------- |
| shop              | list     | items      | 쇼핑아이템 id |
| buy(산 아이템)    | 유저 uid | 유저데이터 |
| buying(장바구니)  | 유저 uid | 유저데이터 |
| interest(찜 목록) | 유저 uid | 유저데이터 |

<br><br>
공통적으로 쓰는 기능은

1. 데이터 추가
2. 데이터 가져오기

이정도로 추려볼 수 있었다.

- 문서

```js
doc(db, 컬렉션, 문서, 추가할 데이터);
```

- 문서 쓰기

```js
await setDoc(doc(db, 컬렉션, 쓸 문서));
```

- 문서 조회

```js
await getDoc(doc(db, 컬렉션, 가져올 문서));
```

공통적으로 쓰는 3가지를 추려 ShopClient class에 private method로 만들어서 내부에서만 호출가능하게 만들었다.

```js
  // 데이터 쓰기
  #setFirebaseDoc(name, user, params) {
    return setDoc(doc(db, name, user?.uid), params);
  }
  // 문서 조회
  #getFirebaseDoc(name, user) {
    return doc(db, name, user.uid);
  }
  // buy 컬렉션에 찾는 유저의 데이터가 있는 지
  async #initBuyCollection(user) {
    return await getDoc(this.#getFirebaseDoc("buy", user));
  }

```

#### firebase api 모듈

이런식으로 shopClient로 변경하였다.

```js
export default class ShopClient {
  // private 변수
  #user = null;
  #auth = getAuth();
  constructor() {
    // class 생성시 item목록 collection 생성
    this.itemsCollection = collection(db, "shop", "list", "items");
  }
  #문서조회,데이터쓰기
  유저 로그인,로그아웃
  찜하기 추가,삭제
  장바구니 추가,삭제
}
```

해당 firebase에서 가져온 데이터들을 Shop class에서 포맷팅해서 보내주도록 하였다.

```js
export default class Shop {
  // apiClient로 ShopClient를 받아와서 생성
  constructor(apiClient) {
    this.apiClient = apiClient;
  }
  // 상품 조회 (이런식으로 포맷팅...)
  async getItem(id) {
    return this.apiClient.getItem().then((result) => {
      if (id) {
        return result.filter((item) => item.id === id)?.[0];
      }
      return result;
    });
  }....
}
```

## Context 로 전역에서 이용하게끔

해당 Shop(ShopClient를 가진) 클래스를 한번 호출하면 프로퍼티를 호출할 수 있도록 하기위해 ShopContext 를 만들어주었다.

> [context api](https://ko.legacy.reactjs.org/docs/context.html)는 react에서 기본적으로 제공되는 툴로 context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있다.

- 장점

* 매우 간단
* 추가 패키지 없어도 됨

2.  단점

- 비지니스 로직에 따라 Provider를 계속 생성..
- 코드가 매우 복잡해질 수 있음

```js
// context
export function ShopApiProvider({ children }) {
  const client = new ShopClient();
  const shop = new Shop(client);

  return (
    <ShopContext.Provider value={{ shop }}>{children}</ShopContext.Provider>
  );
}

export function useShopApi() {
  return useContext(ShopContext);
}
```

쓸 때 useQuery callback 에서 shop을 가져와서 프로퍼티를 쓰도록 만들어주었다.

```js
// useQuery
const { isLoading, data: items } = useQuery(["myInterest"], () => {
  const stored = JSON.parse(sessionStorage.getItem("shoppy"));
  return shop.getInterest(stored);
});
```
