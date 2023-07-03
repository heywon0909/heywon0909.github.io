---
layout: post
title: 트라이
path:
description: >
  [2023.07.01 ~ 2023.07.06] 동안 공부한 트라이다.<br>
  해당 포스트는 누구나 자료구조와 알고리즘 개정 2판을 참고하여 작성되었다.
  이미지는 블로그 가져온 이미지 파일이다.
  1. https://gyoogle.dev/blog/computer-science/data-structure/Trie.html
sitemap: false
hide_last_modified: false
---

📌 트라이(Trie)
<br>
<br>

## 트라이(Trie)

트라이는 검색어 자동완성이나 자동 수정같은 기능을 구현할때 효율적이다. 트라이는 특수한 트리 형태 자료 구조로 O(logN)보다 좋은 성능을 가지고 있다.

<figure>
    <img src="https://t1.daumcdn.net/cfile/tistory/24354E335833A7CF17" width="400" alt="https://gyoogle.dev/blog/computer-science/data-structure/Trie.html">
</figure>

특수한 트리 기반 자료인 트라이에 대해서 알아보자 ✨

### 트라이(Trie)

트라이도 트리형태의 자료구조이지만, 이진 트리가 아니다 ‼
이진트리는 최대 2개의 자식 노드를 가질 수 있지만, 트라이노드는 자식 노드 개수에 제한이 없다.

##### 트라이(Trie)의 효율성

> 트라이는 검색하고자하는 문자열의 문자 갯수 만큼의 단계가 걸린다.

트라이는 빅 오로 나타내기에는 어렵다. 단계 수를 좌지우지 하는것이 자료구조 내 데이터 (N)이 아니라 검색 문자열의 길이에 따라 달라지기 때문이다. 그러므로 트라이의 복잡도를 O(K)라고 부른다.

결국, O(K) 라는 것은 트라이가 엄청나게 방대해지더라도 검색하는 속도에는 영향을 미치지 않는다는 것이다. 문자가 7개로 된 문자열이라면, 항상 복잡도는 7단계가 걸린다는 의미이다.

> 알고리즘의 속도에 영향을 미치는 요인은 사용 가능한 전체 데이터가 아니라 입력 크기뿐이다.

#### 트라이 자바스크립트로 구현해보기

chat gpt를 활용하여 트라이를 구현해보았다.

```js
// 노드
class TireNode {
  constructor(value = "") {
    this.value = value;
    this.children = {};
    this.isEndOfWord = false;
  }
}

class Trie {
  constructor() {
    this.root = new TireNode();
  }
  // 트라이에 넣기
  insert(word) {
    let current = this.root;
    for (let i = 0; i < word.length; i++) {
      const char = word[i];
      if (!current.children[char]) {
        current.children[char] = new TireNode(current.value + char);
      }
      current = current.children[char];
    }
    current.isEndOfWord = true;
  }
  // 검색
  search(word) {
    let current = this.root;
    for (let i = 0; i < word.length; i++) {
      const char = word[i];
      if (!current.children[char]) {
        return false;
      }
      current = current.children[char];
    }
    return current.value;
  }
  // prefix로 시작하는지
  startsWith(prefix) {
    let current = this.root;

    for (let i = 0; i < prefix.length; i++) {
      const char = prefix[i];
      if (!current.children[char]) {
        return false;
      }

      current = current.children[char];
    }
  }
}
```
