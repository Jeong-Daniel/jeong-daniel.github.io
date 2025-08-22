---
title: 어두운 길 University of Ulm Local Contest 그래프
date:   2022-08-24 20:57:30 +0900
categories: [Coding_Test, Others]
tags: [coding, python, algorithm, graph]
---

> 한 마을은 N개의 집과 M개의 도로로 구성되어 있습니다. 각 집은 0번부터 N-1번까지의 번호로 구분됩니다. 모든 도로에는 가로등이 구비되어 있는데 특정한 도로의 가로등을 하루 동안 켜기 위한 비용은 해당 도로의 길이와 동일합니다. 예를 들어 2번 집과 3번 집 사이를 연결하는 길이가 7인 도로가 있다고 해봅시다. 하루 동안 이 가로등을 켜기 위한 비용은 7이 됩니다.
> 
> 정부는 일부 가로등을 비활성화하되, 마을에 있는 임의의 두 집에 대하여 가로등이 켜진 도로만으로도 오갈 수 있도록 만들고자 합니다. 결과적으로 일부 가로등을 비활성화하여 최대한 많은 금액을 절약하고자 합니다. 마을의 집과 도로 정보가 주어졌을 때 일부 가로등을 비활성화하여 절약할 수 있는 최대 금액을 출력하는 프로그램을 작성하세요
> 
> 입력 예시
> N M (집의수 N, 도로의수 M)
> X Y Z (X번집과 Y번집 사이의 도로(비용) Z
> 일부 가로등을 비활성화하여 절약할 수 있는 최대 금액을 구하는데


제 생각에는 보자마자 신장트리 문제 같았습니다. 임의의 두 집에 대하여 가로등이 켜진 도로만으로 오갈 수 있으니 이제 크루스칼 알고리즘을 사용하면 됩니다. 하지만 도로의 전체 비용을 구하는 것이 아닌 절약하는 비용입니다.

그러니 전체 가로등을 켜는 비용에 최소 비용을 빼면 되겠네요

```py
#특정 원소가 속한 집합을 찾기
def find_parent(parent, x):
    #로트 노드가 아니라면, 로트 노드를 찾을 때까지 재귀적으로 호출
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

#두 원소가 속한 집합을 찾기
def union_parent(parent,a,b):
    a = find_parent(parent,a)
    b = find_parent(parent,b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

#노드의 개수와 간선의 개수 입력받기
n,m = map(int,input().split())
parent = [0]*(n+1) #부모 테이블 초기화

#모든 간선을 담을 리스트와 최종 비용을 담을 변수
edges = []
result = 0

#부모 테이블상에서, 부모를 자기 자신으로 초기화
for i in range(1,n+1):
    parent[i] = i
    
#모든 간선에 대해서 정보를 입력받기
for _ in range(m):
    x, y, z = map(int,input().split())
    #비용순으로 정렬하기 위해서 튜플의 첫 번째 원소를 비용으로 설정
    edges.append((z,x,y))
```

시작은 전과 비슷합니다. 일단 원소가 속한 집합(루트노드)를 찾고, 두 원소가 속한 집합을 합치는 함수를 만들고

값을 입력받고 부모 노드를 자기 자신으로 초기화를 합니다.

이때 간선을 입력받을때 비용을 기준으로 하기 위해서 z를 먼저 받습니다.

```py
#간선을 비용순으로 정렬
edges.sort()
total = 0 #전체 가로등 비용
```

그러면 간선은 z를 기준으로 먼저 정렬이 이루어지고 전체 입력받을 가로등 비용은 0으로 초기화를 합시다.

그 다음 최소비용으로 이루어지는 값 result는 위에서 초기화를 합니다.

```py
#간선을 하나씩 확인하며
for edge in edges:
    cost, a, b = edge
    total += cost
    #사이클이 발생하지 않는 경우에만 집합에 포함
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
        
print(total - result)
```

그리고 간선을 하나씩 확인하면서 사이클이 발생하지 않는 경우에만(두 집합의 부모가 같지 않을 경우)에만 합치고 비용을 계산하여 result를 갱신을 하고


최종적으로 전체 비용에서 result를 빼는 것으로 출력을 합니다.