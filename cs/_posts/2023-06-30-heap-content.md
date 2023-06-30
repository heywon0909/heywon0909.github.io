---
layout: post
title: 힙
path:
description: >
  [2023.06.22 ~ 2023.06.30] 동안 공부한 힙이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
sitemap: false
hide_last_modified: false
---

📌 힙(Heap)
<br>
<br>

## 힙(Heap)

힙을 알기 위해서는 우선순위 큐(priority queue)에 대해서 알아야한다.
우선순위 큐는 삽입과 삭제는 큐(queue)와 비슷하나 우선순위에 따라 큐를 정렬한다.
큐와 마찬가지로 앞쪽에서만 삭제가 가능하므로 삭제는 O(1)이지만 삽입은 O(N)이 걸릴 수 있다....
우선순위 큐에 항목이 많아지게 되면 O(N)으로 인해 작업을 수행하는 시간이 오래 걸리게 된다..
우선순위 큐룰 구현하는 또다른 방식으로 힙에 대해서 알아보자 ✨

<figure>
    <img src="https://i.namu.wiki/i/TRZ9q05c_vRQZeclKg1kJkLxTWjg-KJqftozbsM4cycBWQTZHRmnqlVPajBoRV5x3glx6_a_lqYskMV5hMV6T8N07ugpvKDpjIz1njYzTl7e2MrmPtL0Rmzuf3T3qbUR6f5YXyo2cKRutesrrsUKCA.webp" width="300">
</figure>

힙 중에서 이진 힙(binary heap)에 대해서 다뤄보자

### 이진 힙(binary heap)

이진 힙은 이진 트리의 구조를 띄고 있다. 이진힙은 루트 노드에 가장 큰 값, 가장 작은 값으로 위치시킴으로써 최대 힙, 최소 힙을 구현한다.

##### 힙 조건

힙은 다음의 조건을 따른다.

1. 각 노드의 값은 그 노드의 모든 자손 노드의 값보다 커야한다.
2. 트리는 완전(complete)해야한다.

> 트리는 왼쪽부터 차례대로 값을 채워서 구현한다. 이때, 바닥 레벨에서는 빈자리가 있을 수 있는데, 바닥 레벨에 빈 공간이 존재할 경우, 빈 자리의 오른쪽으로 노드가 아예 없을 경우, 트리가 완전하다고 말한다.

##### 힙의 속성

힙에서 삽입과 삭제를 수행하기 위해서는 마지막 노드(last node)가 중요한 역할을 한다.‼ 마지막 노드란, 바닥 레벨에서 가장 오른쪽에 위치한 노드를 의미한다.

#### 삽입

힙에서 새로운 값을 삽입하려면 다음 단계를 거친다.

1. 새값을 가지는 노드를 생성, 마지막 노드의 옆에 노드를 삽입한다.
2. 새로 삽입한 노드와 부모 노드를 비교하여 새로 삽입한 노드가 더 크면 새 노드와 위치를 교환한다.(트리클링)
3. 2단계를 반복하며 올바른 위치에 노드를 위치시킨다.

> 트리클링: 새 노드를 다른 노드들과 자리를 바꾸면서 위로 올리는 과정

※ 해당 이미지 자료는 최소 힙을 구현한 것으로 루트 노드값이 다른 노드의 값에 비해 제일 작은 값으로 이루어져 있다. 이때 새로 삽입한 노드가 부모 노드 값과 비교했을때 더 작으면 트리클링을 통해 위치를 바꾼다.

<figure>
    <img src="https://i.namu.wiki/i/C8aYz2ekN73rkfsARa2yTXpBh2WVv1uVCY2fskURIfrKuKCabcJ_8soyhO2VUoMWHApqnM92nUuGgk7e9lAr7ydjzKTX96vCSo96PjRm0z6ZQPae2H6XK8b6FUWz5SuSdn1hdlMTPBGxYU2eBY_bNw.webp"  width="200">
</figure>

### 삭제

힙에서 새로운 값을 삭제하려면 다음 단계를 거친다.

1. 마지막 노드를 루트 노드 자리로 위치시킨다(결과적으로 원래 루트 노드는 삭제된다.)
2. 루트 노드를 위가 아닌! 아래로 트리클링한다.
   ※아래로 트리클링하는 방법은 다음과 같다.

   - 루트 노드 아래 두 자식을 확인해 어느 쪽이 더 큰지 본다.
   - 루트 노드가 두 자식 노드 중 가장 큰 노드보다 작으면 큰 노드와 루트 노드의 위치를 바꾼다.
   - 루트 노드보다 큰 자식 노드가 없을때까지 아래로 트리클링을 반복한다.
     참고

   ※ 해당 이미지 자료는 최소 힙을 구현한 것으로 루트 노드값이 다른 노드의 값에 비해 제일 작은 값으로 이루어져 있다. 이때 새로 삽입한 노드가 자식 노드 값과 비교했을때 더 작을때 아래로 트리클링을 통해 위치를 바꾼다.
   <figure>
       <img src="https://i.namu.wiki/i/T1BI7HOHG0Ox6GJqjKAKRU_tlL0YSwVC8hReNpyo_GBv1s-FHFtQcI298iPJV_HOLiJctNN_dSFDatBoZjONrqhKjAbUa2qFPHi2QjKub2_ENxRHd7V9B2L5dtvieXYK1PfvMfGt2hvSaqC1aT_EyA.webp"  width="200">
   </figure>

### 힙 구현하기

힙을 구현할때에는 주로 배열을 사용해 힙을 구현한다. 배열을 사용해 힙을 구현하면 배열의 마지막 인덱스의 값이 마지막 노드가 되며, 레벨에 따라 노드를 배치하기 쉽게 된다.

1.  마지막 노드 : 배열의 마지막 인덱스
2.  어떤 노드의 부모 : parseInt((자신의 노드 인덱스 - 1) /2)
3.  어떤 노드의 자식 노드
    - 왼쪽 자식 : (자신의 노드 인덱스 \* 2) + 1
    - 오른쪽 자식: (자신의 노드 인덱스 \* 2) + 2

  <figure>
       <img src="https://i.namu.wiki/i/UrWjmKYIwZNefx3D7jl6nuo0C0EG5x61RLPih7HraMmNPOJd-Wv7Dd166TeNsU0yxJBr6WFlwR3Y2npV4vyr00HGGm_hwv40IvxUyc5C56eB1cF4851U6rgfuxHqFmLkPyGXdVaGVJRntUB0QCFI7g.webp"  width="400">
   </figure>
