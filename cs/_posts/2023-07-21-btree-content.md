---
layout: post
title: B-트리(B-Tree)
path:
description: >
  [2023.07.01 ~ 2023.07.06] 동안 공부한 트라이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
  이미지는 블로그 가져온 이미지 파일이다.
  1. https://gyoogle.dev/blog/computer-science/data-structure/Trie.html
sitemap: false
hide_last_modified: false
---

📌 B-트리(B-Tree)
<br>
<br>

## B-트리(B-Tree)

B 트리는 트리 자료 구조중 하나로 일정 높이를 유지하는 트리이다. 

<figure>
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBLi5L%2FbtrdInhxyVP%2Febff3uYkmyoEty5lULR8kK%2Fimg.png" width="600"  alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

B 트리에 대해서 알아보자 ✨

### B 트리의 규칙

B 트리가 되기 위해서는 규칙을 만족해야한다.

1. node의 key는 2개 이상들어 갈 수 있으며 항상 정렬된 상태로 저장된다.
2. 최대 자식이 m 개 일때, m 차 B트리라 부르는데, 이때 내부 node들은 m/2 ~ m 개의 자식 node를 가질 수 있다.
3. node의 데이터(key)가 k 개라면 자식 node의 개수는 k+1 이다.
4. 특정 node의 왼쪽 서브 트리는 루트 노드의 key보다 작은 값, 오른쪽 서브트리는 루트 노의 key보다 큰 값들로 구성된다. 
5. node내의 데이터(key)는 floor(m/2)-1개 ~ m-1개까지 가능하다.
6. 모든 레벨에 비어있는 부분(자식)이 존재하지 않는다..! 


##### B-트리 탐색 과정

> B-트리는 루트 노드에서 시작하여 하향식으로 탐색을 진행한다.

1. 루트노드에서 탐색 시작
2. 값을 찾으면 탐색 끝 
3. K와 node의 데이터(key)값을 비교하여 트리클링
4. 리프노드에 도달할때까지 3번 반복한다.
5. 찾지 못하면 트리에 값이 존재하지 않는 것이다.

#### B-트리 삽입 과정 

> B-트리 삽입은 상향식으로 진행된다. B-트리의 데이터 삽입은 항상 리프 노드에서 시작한다.

B-트리 삽입 과정은 분리가 일어나는 경우와 분리가 일어나지 않는 경우로 나눌 수 있다.

##### 분리가 일어나지 않는 경우

> 리프노드란 자식이 없는 노드를 의미한다.

1. 데이터를 삽입할 리프노드 찾아 데이터를 삽입한다.
2. node 내에서 올바른 위치에 정렬한 후, 종료한다.

#### 분리가 일어나는 경우

분리가 일어나는 경우란, 삽입 후 B-트리의 규칙을 위반해 B-트리의 규칙에 맞게 정렬하는 것을 의미한다.

1. 데이터를 삽입할 리프노드를 찾은 후, 해당 node에 데이터를 삽입한다.
2. B-트리의 규칙에 맞지 않다면, 분리를 진행하여 
   적절한 상태로 변경한다.


16이라는 k값을 B-트리에 삽입한다고 하자.

<figure>
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5OD0u%2FbtrBiHpVDoe%2FFlK7Dw0fahejkwokWLqNo1%2Fimg.webp" 
    width="900"
    alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

리프 노드를 탐색하여 적절한 위치에 해당 값을 넣는다.

<figure>
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEPnAb%2FbtrBiIbjyMn%2F4HZcRWkczEaSBvClUuXDiK%2Fimg.webp" 
    width="900"
    alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

해당 트리는 3차트리이므로 한 node에 floor(m/2)-1개 ~ m-1개이므로 1개 ~ 2개이므로 규칙에 어긋난다.

<figure>
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHnwn5%2FbtrBgE2SnqP%2FLfoPQh6kU5N2KgqafGScM0%2Fimg.webp" 
    width="900"
    alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

규칙에 어긋나므로 분리를 진행한다.
이렇게 하면 부모 node 가 규칙에 어긋나므로 다시 분리를 진행한다.

<figure>
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbnYL3M%2FbtrBhuE97RC%2FSDP7trISsSUnZ1jKDc2RK0%2Fimg.webp" 
    width="900"
    alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

B-트리 규칙에 어긋나지 않으므로 삽입을 종료한다.


#### B-트리 삭제 과정 