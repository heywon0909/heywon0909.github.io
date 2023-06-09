---
layout: post
title: Quicksort(퀵정렬)
path:
description: >
  [2023.04.28 ~ 2023.05.04] 동안 공부한 퀵정렬이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
sitemap: false
hide_last_modified: false
---

✔ 퀵정렬의 동작 방식을 배움으로써 재귀가 어떻게 알고리즘의 속도를 향상시키는지 배울 수 있다.
<br>
<br>

## Quicksort

버블 정렬,선택 정렬과 같은 많은 정렬 알고리즘이 있지만 실제로 배열을 정렬할때는 쓰지 않는다. 컴퓨터 언어에는 내장 정렬 함수로 정렬을 한다. 컴퓨터 언어중 대다수가 사용하는 정렬 알고리즘이 바로
`**퀵정렬(Quicksort)**`이다.

### 퀵정렬(Quicksort)은 분할(partitioning)에 기반한다?!?

퀵정렬이 진행되면서 임의의 값의 왼쪽에는 작은 값이, 임의의 값의 오른쪽에는 큰 값이 위치하게 된다. (단, 정렬이 끝나지 않은 경우에는 완전 순서대로 정렬되었다고 보기는 어렵다❗)

> 퀵정렬은 임의의 수(pivot:피벗)을 기준으로 정렬한다.

퀵정렬은 해당 단계를 반복하면서 점점 하위 배열로 파고든다.
임의의 깊이로 파고들면서 동일한 루프를 `**반복**`하므로 재귀에 적합하다.
<br/>
<br/>

### 퀵정렬은 어떻게 재귀적으로 분할을 진행하는가?

퀵정렬은 다음과 같은 단계를 반복한다.

피벗을 고른다. 가장 오른쪽에 있는 값을 피벗으로 고르겠다.
두 `**포인터**`를 사용해 하나는 배열 제일 왼쪽에, 하나는 피벗을 제외한 제일 오른쪽에 놓는다.

1. 왼쪽 포인터를 한 셀씩 계속 오른쪽으로 옮기면서 피벗보다 크거나 같은 값에 도달하면 멈춘다.
2. 다음에 오른쪽 포인터를 한 셀씩 계속 왼쪽으로 옮기면서 피벗보다 작거나 같은 값에 도달하면 멈춘다. (배열 맨 앞에 도달해도 멈춘다.)
3. if(왼쪽 포인터>= 오른쪽 포인터) [왼쪽 포인터 값, 피벗]=[피벗,왼쪽 포인터];
   else [왼쪽 포인터값, 오른쪽 포인터값] = [오른쪽 포인터값, 왼쪽 포인터 값];
4. 1 ~ 3 단계 계속 반복

분할이 끝나면 피벗 왼쪽에 값들은 피벗보다 작고, 피벗 오른쪽에 있는 값들은 피벗보다 크게 된다❗(피벗 자체는 배열 내에서 알맞은 위치에 있는 것이다.)

### 퀵정렬 만들어보기

책에 나와있는 퀵정렬 클래스와 메서드를 javascript 함수로 바꿔 보았다.

```js
function solution(arr) {
  let n = arr.length;
  // 분할메서드
  function partition(left_pointer, right_pointer) {
    // 피벗 인덱스
    let pivot_index = right_pointer;
    // 피벗 값
    let pivot = arr[pivot_index];
    // 오른쪽 포인터
    right_pointer -= 1;

    while (true) {
      // 피벗보다 크거나 같은 값을 가리킬때까지 왼쪽 포인터 이동
      // 왼쪽 포인터가 배열의 길이를 벗어나면 while문 벗어남
      while (arr[left_pointer] < pivot) {
        left_pointer += 1;
      }
      // 피벗보다 작거나 같은 값을 가리킬때까지 오른쪽 포인터 이동
      // 오른쪽 포인터가 배열의 길이를 벗어나면 while문 벗어남
      while (arr[right_pointer] > pivot) {
        right_pointer -= 1;
      }
      // 왼쪽 포인터가 오른쪽 포인터를 넘거나 도달했으면 while 문 끝내고 교환 시작
      if (left_pointer >= right_pointer) {
        break;
      } else {
        // 그렇지 않으면 왼쪽 포인터와 오른쪽 포인터가 가리키고 있는 값 계속 교환
        [arr[left_pointer], arr[right_pointer]] = [
          arr[right_pointer],
          arr[left_pointer],
        ];
        // 교환이 끝난 후 왼쪽 포인터 이동 -> 다시 while 문 맨앞으로 가서 비교
        left_pointer += 1;
      }
    }

    // 최종적으로 while 문이 끝나면 왼쪽 포인터와 피벗의 값을 교환한다.
    [arr[left_pointer], arr[pivot_index]] = [
      arr[pivot_index],
      arr[left_pointer],
    ];
    return left_pointer;
  }
  // 재귀 메서드로 분할메서드 재 호출
  function DFS(start, last) {
    if (last - start <= 0) return;
    else {
      // 한번의 분할이 끝나면 평균적으로 피벗은 배열의 한가운데에 위치해있으므로 피벗 왼쪽 하위 배열과 피벗 오른쪽 하위배열의 분할을 시작한다.
      let pivot_index = partition(start, last);
      // 피벗 왼쪽 하위 배열
      DFS(start, pivot_index - 1);
      // 피벗 오른쪽 하위 배열
      DFS(pivot_index + 1, last);
    }
  }
  DFS(0, arr.length - 1);
  return arr;
}
console.log("solution", solution([0, 5, 2, 1, 6, 3]));
```

### 퀵정렬의 효율성

퀵정렬의 효율성을 알아내려면 분할의 과정을 살펴봐야한다.
분할은 비교와 교환 두 단계로 이루어진다.
<br>
비교 : 포인터와 피벗을 비교한다.
교환 : 왼쪽과 오른쪽 포인터가 가리키는 값을 교환한다.

평균적으로 각 분할마다 비교하는데 최소 N번, 교환하는데 최대 N/2번 걸린다.

빅오 관점에서 퀵정렬을 분류하면 평균적으로 분할하는데 배열을 하위 배열 2개로 나누고 비교와 교환을 수행하므로 (N \* logN) 정도 걸린다.

#### 퀵정렬의 최악의 시나리오?!?

일반적으로 분할 후 피벗이 하위 배열의 한가운데 있을때, 알고리즘 효율이 높다. 가장 최악의 시나리오는 피벗이 분할이 진행될때마다 배열의 한가운데가 아닌 한쪽 끝에 있을때이다.

각 분할마다 교환은 한번이지만 비교가 너무 많아진다.

> 공식으로 표현하면, 원소가 N개 일때 N + (N-1) + (N-2) + (N-3) ... + 1 단계가 걸린다. 빅오는 O(N^2) 이다.

<br>
<br>

### 퀵셀렉트

퀵정렬보다 빠른 방식으로 배열에서 N번째 크거나 작은 값을 검색할 수 있는 방법이다. 퀵셀렉트는 분할에 기반하며, 퀵정렬과 이진정렬에 걸쳐있다고 볼 수 있다.

1. 퀵정렬에 따라 전체 배열을 분할한다.
2. 피벗이 배열에 중간에 있다고 가정했을 때,
   if(N> 피벗인덱스) 피벗 오른쪽 하위 배열 분할
   if(N< 피벗인덱스) 피벗 왼쪽 하위 배열 분할
3. 분할이 끝나면 N번째 크거나 작은 값이 알맞은 위치에 있게된다.

### 퀵셀렉트 만들어보기

책에 나와있는 퀵셀렉트 클래스와 메서드를 javascript 함수로 바꿔 보았다.

```js
function solution(arr, find) {
  let n = arr.length;
  // 분할메서드
  function partition(left_pointer, right_pointer) {
    // 피벗 인덱스
    let pivot_index = right_pointer;
    // 피벗 값
    let pivot = arr[pivot_index];
    // 오른쪽 포인터
    right_pointer -= 1;

    while (true) {
      // 피벗보다 크거나 같은 값을 가리킬때까지 왼쪽 포인터 이동
      // 왼쪽 포인터가 배열의 길이를 벗어나면 while문 벗어남
      while (arr[left_pointer] < pivot) {
        left_pointer += 1;
      }
      // 피벗보다 작거나 같은 값을 가리킬때까지 오른쪽 포인터 이동
      // 오른쪽 포인터가 배열의 길이를 벗어나면 while문 벗어남
      while (arr[right_pointer] > pivot) {
        right_pointer -= 1;
      }
      // 왼쪽 포인터가 오른쪽 포인터를 넘거나 도달했으면 while 문 끝내고 교환 시작
      if (left_pointer >= right_pointer) {
        break;
      } else {
        // 그렇지 않으면 왼쪽 포인터와 오른쪽 포인터가 가리키고 있는 값 계속 교환
        [arr[left_pointer], arr[right_pointer]] = [
          arr[right_pointer],
          arr[left_pointer],
        ];
        // 교환이 끝난 후 왼쪽 포인터 이동 -> 다시 while 문 맨앞으로 가서 비교
        left_pointer += 1;
      }
    }

    // 최종적으로 while 문이 끝나면 왼쪽 포인터와 피벗의 값을 교환한다.
    [arr[left_pointer], arr[pivot_index]] = [
      arr[pivot_index],
      arr[left_pointer],
    ];
    return left_pointer;
  }
  // 재귀 메서드로 분할메서드 재 호출
  function DFS(start, last) {
    if (last - start <= 0) return;
    else {
      // 한번의 분할이 끝나면 평균적으로 피벗은 배열의 한가운데에 위치해있으므로 피벗 왼쪽 하위 배열과 피벗 오른쪽 하위배열의 분할을 시작한다.
      let pivot_index = partition(start, last);
      // 피벗 인덱스가 구하고자 하는 인덱스보다 작으면
      if (pivot_index > find) {
        // 피벗 왼쪽 하위 배열
        DFS(start, pivot_index - 1);
        // 피벗 인덱스가 구하고자 하는 인덱스이면
      } else if (pivot_index === find) {
        return arr[find];
      } else {
        // 피벗 인덱스가 구하고자 하는 인덱스보다 크면
        // 피벗 오른쪽 하위 배열
        DFS(pivot_index + 1, last);
      }
    }
  }
  DFS(0, arr.length - 1);
  return arr[find - 1];
}
console.log("solution", solution([0, 5, 2, 1, 6, 3], 2));
```
