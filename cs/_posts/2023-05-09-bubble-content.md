---
layout: post
title: Bubblesort(버블정렬)
path:
description: >
  [2023.05.04 ~ 2023.05.11] 동안 공부한 버블정렬이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
sitemap: false
hide_last_modified: false
---

✔ 정렬알고리즘 중 단순정렬이라고 불리는 버블정렬에 대해 정리해보자 💥
<br>
<br>

## Bubblesort

정렬되지 않은 배열이 주어졌을 때, 배열을 정렬시킬 수 있는 방법중 하나이다.

`**퀵정렬(Quicksort)**`이다.

### 버블정렬단계

1. 배열내에서 연속된 두 항목(포인터 두개)을 바라본다.(맨처음에는 배열의 첫번째,두번째 원소)
2. 두 항목을 비교해서 순서가 올바르지 않으면 교환, 올바르면 두포인터를 오른쪽으로 한 셀씩 옮긴다.
3. 배열이 끝날때까지 1-2를 반복한다.(첫번째 패스스루 🏈)
4. 두 포인터를 다시 배열의 첫번째,두번째 원소로 옮겨서 1-3를 반복한다.
5. 교환이 일어나지 않는 패스스루🏈가 반복될때까지 패스스루를 실행한다.
   <br/>
   <br/>
   > 교환이 없다는 것은 배열이 완전히 정렬되었다는 것을 의미한다.
   > <br/>

### 버블정렬 만들어보기

> 버블정렬에서 기억해야될 것은, 각 패스스루를 반복할때마다 정렬되지 않은 값 중 가장 큰 값,"버블"이 올바른 위치에 있게된다는 점이다.

```js
function solution(arr) {
  let answer = arr;
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return answer;
}
let arr = [13, 5, 11, 7, 23, 15];
console.log(solution(arr));
// 답: [5, 7, 11, 13, 15, 23]
```

### 버블정렬의 효율성

> 버블정렬은 비교 + 교환 이라는 단계를 거친다.

만약 버블정렬이 내림차순으로 정렬된 최악의 시나리오라면 매번 비교할때마다 교환이 일어나므로 O(N^2) 단계가 될 수 있다.
원소의 수가 증가할 수록, 단계수가 기하급수적으로 늘어나게된다. O(N^2)은 데이터가 증가할때 단계수가 급격히 늘어나므로 비효율적인 알고리즘이다... ‼

📢참고로, O(N^2)을 이차시간(duration time)이라고 부른다.