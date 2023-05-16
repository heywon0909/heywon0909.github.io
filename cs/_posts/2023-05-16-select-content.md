---
layout: post
title: Selectsort(선택정렬)
path:
description: >
  [2023.05.12 ~ 2023.05.18] 동안 공부한 선택정렬이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
sitemap: false
hide_last_modified: false
---

✔ 정렬알고리즘 중 선택정렬에 대해 정리해보자 💥
<br>
<br>

## Selectsort

정렬되지 않은 배열이 주어졌을 때, 배열을 정렬시킬 수 있는 방법중 하나이다.

### 선택정렬단계

> 가장 작은 값을 '선택 ✔' 해서 정렬한다

1. 배열의 각 셀을 오른쪽 방향으로 가면서 가장 작은 값(최솟값)을 찾는다.
2. 찾은 최솟값을 배열의 첫번째 인덱스와 교환한다.이때 첫번째 인덱스에는 제일 작은 값이 들어가므로 첫번째 인덱스는 정렬되었다‼(첫번째 패스스루 🏈)
3. 두번째 인덱스부터 1->2를 실행한다.
4. 배열의 마지막 패스스루까지 1->2를 실행한다.
   <br/>
   <br/>
   > 마지막 패스스루에서는 배열이 완벽히 정렬되어있다.✨
   > <br/>

### 선택정렬 만들어보기

> 선택정렬에서 기억해야할 점은, 패스스루를 돌면서 배열의 값을 비교하면서 최솟값을 찾아간다는 점이다.

```js
function solution(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
    let lowestNumberIndex = i;
    for (let j = i; j < arr.length; j++) {
      if (arr[j] < arr[lowestNumberIndex]) {
        lowestNumberIndex = j;
      }
      if (lowestNumberIndex !== i) {
        [arr[i], arr[lowestNumberIndex]] = [arr[lowestNumberIndex], arr[i]];
      }
    }
  }
  return arr;
}
let arr = [13, 5, 11, 7, 23, 15];
console.log(solution(arr));
// 답: [5, 7, 11, 13, 15, 23]
```

### 선택정렬의 효율성

> 선택정렬은 비교 + 교환 이라는 단계를 거친다.

원소 N개가 있을 때, (N-1)+(N-2)+(N-3)+ ... 비교를 해야한다.
또한, 선택정렬은 버블 정렬과 달리 각 패스스루마다 최대 교환을 한번 해야한다.

선택정렬과 버블정렬은 O(N^2) 단계수로 같지만, 선택 정렬은 버블 정렬보다 단계 수가 반 정도 작다고 한다...
