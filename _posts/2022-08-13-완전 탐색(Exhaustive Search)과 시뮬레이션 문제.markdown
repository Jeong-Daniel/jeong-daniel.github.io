---
title: 완전 탐색(Exhaustive Search)과 시뮬레이션 구현문제
date:   2022-08-13 15:05:41 +0900
categories: [Coding_Test, Others]
tags: [coding, python, algorithm, search, simulation]
---

### 완전탐색 (Brute-Force Search / Exhaustive Search)
완전 탐색은 모든 경우를 빠짐없이 다 계산하는 해결 방법

완전 탐색 문제는 모든 경우의 수를 다 계산해야 하기 때문에 반복문 혹은 재귀함수를 적절히 사용해서 예외 케이스를 모두 확인해야 합니다. 그러무로 DFS/BFS 알고리즘을 이용해서 문제를 해결합니다.

![Exhaustive Search](https://user-images.githubusercontent.com/85277660/210991842-7b879e6d-0379-4e8a-8811-da9b1158d7f7.png)

### 시뮬레이션
시뮬레이션은 문제에서 제시하는 논리나 동작 과정을 그대로 코드로 옮겨야 하는 유형을 의미합니다.

![simulation example](https://user-images.githubusercontent.com/85277660/210992151-8a52647b-e6c2-40c1-ae48-cf87b69a6458.png)


원소를 나열하는 모든 경우의 수를 고려해야 상황에서 보통 순열이나 조합 라이브러리를 사용해야 합니다. 이때 파이썬은 표준 라이브러리인 itertools로 구현할 수 있습니다. 

이외에도 소스코드를 구현하기 까다롭거나 까다로운 문자열 처리, 소수코드양이 많은 경우에도 구현 유형으로 구분