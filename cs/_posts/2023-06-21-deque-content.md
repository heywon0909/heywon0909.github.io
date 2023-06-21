---
layout: post
title: 데큐
path:
description: >
  [2023.06.09 ~ 2023.06.22] 동안 공부한 데큐이다.<br>
  chat gpt,블로그를 활용하여 작성되었다.
sitemap: false
hide_last_modified: false
---

📌 데큐(Deque)
<br>
<br>

## 데큐(Deque)

큐와 스택의 기능을 동시에 가지고 있는 데큐에 대해서 알아보자.

### 데큐

> 데큐는 "double-endede queue"의 약자로 양쪽 끝에서 삽입과 삭제가 모드 가능한 자료구조이다. 데큐는 큐(Queue)와 스택(Stack)의 속성을 모두 가지고 있다.

1. 큐(FIFO)
2. 스택(LIFO)

#### 데큐 구현하기

데큐를 배열을 활용하여 javascript로 구현해보았다.

```js
const deque = [];

// 데큐의 끝에 요소 추가
deque.push("마지막");
deque.push("두번째");

// 데큐의 앞쪽에 요소 추가
deque.unshift("첫번째");

// 데큐의 끝에서 요소 제거
const lastElement = deque.pop();

// 데큐의 앞쪽에서 요소 제거
const firstElement = deque.shift();
```

#### Deque의 시간복잡도 문제 해결

하지만 이렇게 unshift()를 사용하여 첫번째 요소를 제거하면 빈 첫번째 공간을 채우기 위해 각 배열의 값들이 왼쪽으로 시프트 되므로 시간복잡도가 O(N)이 될 수 있다....
이를 위해 첫번째 요소와 마지막 요소에 접근할 수 있는 이중연결리스트를 통해 구현해볼 수 있다.

```js
class Node {
  constructor(elem) {
    this.elem = elem;
    this.next = null;
    this.prev = null;
  }
}

class Deque {
  // 초기화
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }
  isEmpty() {
    return this.size === 0;
  }
  // 앞 쪽에 추가
  addFront(elem) {
    const newNode = new Node(elem);
    if (this.isEmpty()) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.next = this.head;
      this.head.prev = newNode;
      this.head = newNode;
    }
    this.size++;
  }
  // 끝 쪽에 추가
  addEnd(elem) {
    const newNode = new Node(elem);
    if (this.isEmpty()) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.prev = this.tail;
      this.tail.next = newNode;
      this.tail = newNode;
    }
    this.size++;
  }
  // 앞쪽에서 삭제
  removeFront() {
    if (this.isEmpty()) {
      return null;
    }
    const removeNode = this.head;

    if (this.size === 1) {
      this.head = null;
      this.tail = null;
    } else {
      this.head = removeNode.next;
      this.head.prev = null;
    }
    removeNode.next = null;
    this.size--;

    return removeNode.elem;
  }
  // 뒤에서 삭제
  removeEnd() {
    if (this.isEmpty()) {
      return null;
    }
    const removeNode = this.tail;
    if (this.size === 1) {
      this.head = null;
      this.tail = null;
    } else {
      this.tail = removeNode.prev;
      this.tail.next = null;
    }
    removeNode.prev = null;
    this.size--;

    return removeNode.elem;
  }
  // 앞쪽 요소를 반환
  getFront() {
    if (this.isEmpty()) {
      return null;
    }
    return this.head.elem;
  }
  // 끝 요소를 반환
  getEnd() {
    if (this.isEmpty()) {
      return null;
    }
    return this.tail.elem;
  }
  // 데큐의 길이 반환
  getSize() {
    return this.size;
  }
}

const deque = new Deque();
deque.addEnd("1");
deque.addEnd("2");
deque.addFront("3");

console.log(deque.getFront()); // 3
console.log(deque.getEnd()); // 2
console.log(deque.removeFront()); // 3
console.log(deque.getSize()); // 2
```

### 데큐는 언제 사용할까? ✨

1. 큐와 스택의 기능이 모두 필요한 경우 -> 양쪽에서 빠른 삽입과 삭제가 필요한 경우
2. 슬라이딩 윈도우(Sliding Window)알고리즘

- 슬라이딩 윈도우 알고리즘 == 투 포인터 알고리즘
- 배열이나 문자열 같은 선형 구조에서 일정 범위의 값을 비교할때 유용하다

참고

1. chat gpt
2. https://velog.io/@turtle601/Javascript%EB%A1%9C-Deque%EB%A5%BC-%EC%A7%81%EC%A0%91-%EA%B5%AC%ED%98%84%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0
3. https://window6kim.tistory.com/25
