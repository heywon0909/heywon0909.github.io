---
layout: post
title: Insertsort(삽입정렬)
path:
description: >
  [2023.05.19 ~ 2023.05.25] 동안 공부한 삽입정렬이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
sitemap: false
hide_last_modified: false
---

✔ 정렬알고리즘 중 삽입정렬에 대해 정리해보자 💥
<br>
<br>

## Insertsort

정렬되지 않은 배열이 주어졌을 때, 배열을 정렬시킬 수 있는 방법중 하나이다.

### 삽입정렬단계

> 값을 꺼내서 올바른 위치에 넣는다.

1. 인덱스 1의 값을 삭제하고 임시 변수에 저장한다.(인덱스 1의 값은 공백)
2. 인덱스 1 이전의 값을 임시 변수의 값과 비교한다.
   if(인덱스 1이전 값 > 임시변수){
   오른쪽 방향으로 값을 한 셀 옮긴다.
   }
3. 2의 단계를 임시변수보다 작거나 같은 값을 만날때까지 반복한다.
4. 비어있는 공백에 임시 변수의 값을 넣는다. (첫번째 패스스루 🏈)
5. 배열의 마지막 인덱스까지 패스스루를 실행한다.

   <br/>
   <br/>
   > 마지막 패스스루에서는 배열이 완벽히 정렬되어있다.✨
   > <br/>

### 삽입정렬 만들어보기

> 책에 나와있는 파이썬 코드를 자바스크립트로 구현해보았다.

```js
function solution(arr) {
  for (let i = 1; i < arr.length; i++) {
    let temp_value = arr[i];
    let position = i - 1;
    while (position >= 0) {
      if (arr[position] > temp_value) {
        arr[position + 1] = arr[position];
        position = position - 1;
      } else break;
    }
    arr[position + 1] = temp_value;
  }
  return arr;
}
let arr = [13, 5, 11, 7, 23, 15];
console.log(solution(arr));
// 답: [5, 7, 11, 13, 15, 23]
```
