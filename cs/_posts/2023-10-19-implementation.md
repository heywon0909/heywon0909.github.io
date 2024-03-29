---
layout: post
title: implementation (구현)
description: >
  [2023.00.11 ~ 2023.09.21] 동안 공부한 구현 내용이다.<br>
  해당 포스트는 `이것이 취업을 위한 코딩테스트다`책을 읽고 정리한 내용이다.
sitemap: false
hide_last_modified: false
---

이렇게 풀어야 되는지 감은 오지만 막상 실제로 짜려고 하니 어려운 문제에 대해 알아보자.

## 구현(implementation)

> 구현(implementaion)은 '풀이를 떠올리는 것은 쉽지만 소스 코드로 옮기기 어려운 문제'를 의미한다.

구현을 좀 더 구체적으로 설명해보면, 알고리즘은 간단한데 코드가 엄청 길어질 수 있는 문제를
의미한다. 예를 들면, 특정 소수점 자리까지 출력해야하는 문제, 문자열이 입력으로 주어졌을 때, 한 문자 단위로 끊어서 리스트를 넣어야하는 문제등을 들 수 있다.

이 책에서는 **완전탐색** 과 **시뮬레이션** 유형을 모두 '구현' 으로 정의하고 있다.
완전 탐색은 모든 경우의 수를 다 계산해보는 방법을 의미하고, 시뮬레이션은 문제에서 제시한 알고리즘을 한 단계씩 실행해나가는 문제이다.

### 구현 문제에 접근해보자

구현 유형의 문제는 입력 조건을 문제에서 정의해주며, 문제 길이도 상당히 길다.
문제 길이를 보고 겁을 먹는데, 고차원적인 사고력을 요하는 문제는 아니므로 긴장을 풀고 푼다면 쉽게 접근할 수 있다.

### 구현에서의 완전 탐색

완전 탐색 알고리즘은 엄청난 시간 복잡도를 가지고 있으므로 데이터의 개수가 너무 많은 경우, 정상 동작하지 않을 수 있다. 그래서 일반적으로 알고리즘 문제를 풀 때, 탐색해야할 전체 데이터의 갯수가 100만개 이하인 경우, 완전탐색을 사용하면 적절하다.
