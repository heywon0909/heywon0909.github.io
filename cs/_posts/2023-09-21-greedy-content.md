---
layout: post
title: greedy(그리디)
description: >
  [2023.00.11 ~ 2023.09.21] 동안 공부한 그리디 내용이다.<br>
  해당 포스트는 `이것이 취업을 위한 코딩테스트다`책을 읽고 정리한 내용이다.
sitemap: false
hide_last_modified: false
---

루프를 사용할 수 있는 경우라면 거의 대부분 재귀도 쓸 수 있다. 재귀를 쓸 수 있다고 하여 무조건 재귀를 써야하는 것은 아니다. 재귀는 명쾌한 코드를 작성해 줄 수 있는 하나의 도구이다.

## 그리디(Greedy)

> 그리디(greedy)는 탐욕적 접근이다.

그리디란, 현재 상황에서 가장 좋은 것을 고르는 방법이다.
`현재 상황에서 가장 좋은 것`을 고를 수 있는 기준이 주어지는데, 일반적으로 가장 큰 순서대로 또는 가장 작은 순서대로 기준을 통해서 문제를 해결해 나갈 수 있다.
하지만, 항상 이 기준으로 뽑인 아이가 가장 좋은 solution이 아닐 수 있으므로, 최소한의 아이디어를 해당 기준으로 떠올리고, 이것이 정당한지 검토해나가는 과정이 필요하다. 🙏

### 하향식과 상향식

상향식과 하향식 둘다 재귀로 풀어나갈 수 있다. 하지만 상향식은 루프나 재귀나 별반 차이가 없다.
하지만 하향식에서는 재귀를 써야한다. 하향식에서는 재귀가 더 효율적이다.

### 그리디 문제

책에 나와있는 파이썬 코드의 문제를 javascript로 변환하여 정리하였다.

### 큰 수의 법칙

> 반복되는 수열을 파악하자

```js
function solution(data, m, k) {
  data.sort((a, b) => b - a); // 내림차순으로 정렬
  // 1번째로 큰 수
  const first = data[0];
  // 2번째로 큰 수
  const second = data[1];
  let count = 0;

  count = parseInt(m / (k + 1)) * k;
  count += m % (k + 1);

  let result = 0;
  result += count * first;
  result += (m - count) * second;

  return result;
}
solution([2, 4, 5, 4, 6], 8, 3);
```

### 숫자 카드 게임

> 각 행마다 가장 작은 수를 찾은 뒤, 그 수 중에서 가장 큰 수를 찾자

```js
function solution(arr) {
  let len = arr.length;

  let result = 0;
  let min_value = 10001;
  for (let i = 0; i < len; i++) {
    min_value = Math.min(...arr[i]);
    result = Math.max(min_value, result);
  }

  return result;
}
solution([
  [3, 1, 2],
  [4, 1, 4],
  [2, 2, 2],
]);
```

### 1이 될때까지

> 최대한 많이 나누자

```js
function solution(n, k) {
  let result = 0;
  while (true) {
    // N이 K로 나누어 떨어지는 수가 될 때까지만 1씩 빼기
    const target = Math.floor(n / k) * k;
    console.log("target1", target);
    result += n - target;
    console.log("result1", result);
    n = target;

    // N이 K보다 작을 때 (더 이상 나눌 수 없을 때) 반복문 탈출
    if (n < k) {
      console.log("더이상안나눠짐", n);
      break;
    }

    // K로 나누기
    result += 1;
    n = Math.floor(n / k);
  }

  // 마지막으로 남은 수에 대하여 1씩 빼기

  result += n - 1;
  return result;
}
solution(25, 3);
```
