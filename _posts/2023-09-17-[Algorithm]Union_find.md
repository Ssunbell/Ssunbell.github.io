---
title:  "[Algorithm] Union Find 구현"
date: '2023-09-17 16:59:00 +09:00'
category: [CS, Algorithm]
tags: [union, find, weighted union find, union find, rank, path compression]
use_math: true
---

# Union Find

> Union Find 알고리즘이란 Union과 Find 함수를 구현한 알고리즘이다.

너무나도 당연한 말이지만, 이 용어에 핵심이 다 들어있다.
- Find : 두 개의 Object가 같은 요소(연결되어 있는 요소)인지 확인하는 함수
- Union : 두 개의 Object를 하나의 요소로 만드는(연결하는) 함수. 만약, Object에 이미 연결되어 있는 Object가 있다면 그 연결되어 있는 Objecct 또한 하나의 요소로 연결된다.

> Union Find 알고리즘은 시간 복잡도에 따라 Quick-find, Quick-union, Weighted-union find 방식이 존재한다.

- Quick-find 방식은 find의 함수 시간 복잡도가 O(1)인 반면에 union의 함수 시간 복잡도가 O(N^2)인 Union Find 알고리즘이다.
- Quick-union 방식은 union의 함수 시간 복잡도가 O(N)인 반면에 find의 함수 시간 복잡도가 O(N)인 Union Find 알고리즘이다.

> 각 장단점이 있기 때문에 문제가 무엇이냐에 따라 해결 방법이 달라진다.

```python
from typing import *

class QuickUnionUF():
    def __init__(self, array):
        self.id = [i for i in range(len(array))]

    def root(self, i):
        while i != self.id[i]:
            i = self.id[i]
        return i

    def find(self, p, q):
        return self.root(p) == self.root(q)

    def union(self, p, q):
        i = self.root(p)
        j = self.root(q)
        self.id[i] = j

def solution(n, computers):
    uf = QuickUnionUF(computers)

    # upper matrix (lower matrix로 해도 동일)
    for p in range(len(computers)):
        for q in range(p+1, len(computers[0])):
            if (
                computers[p][q] == 1 and    # 만약 연결되어 있는 컴퓨터인데
                not uf.find(p,q)            # 연결상태를 찾아봤는데 연결되어 있지 않다면
               ):
                uf.union(p, q)              # union을 실행해서 연결 상태를 반영해줌

    answer = len(set(uf.id))                # 고유한 id값들을 set을 통해 찾음

    return answer
```

> Weighted-union find 방식은 경로 압축을 사용하여 시간복잡도를 개선한 방식이다.

- 값이 작을수록 루트 노드 중에서도 가장 상위의 루트 노드이다.
- 이것 또한 경우에 따라 위의 quick find or union보다 빠를 수도 있고 느릴 수도 있다.

```python
class WeightedUF():
    def __init__(self, array):
        self.parent = [-1 for i in range(len(array))]

    def find(self, x):
        if self.parent[x] < 0: # 루트 노드일 경우
            return x
        else:
            y = self.find(self.parent[x]) # 경로 압축
            self.parent[x] = y # 경로 갱신
            return y

    def union(self, x, y):
        x = self.find(x)
        y = self.find(y)

        if x == y: return

        if self.parent[x] < self.parent[y]: # 값이 작을수록 더 높은 노드
            self.parent[x] += self.parent[y]
            self.parent[y] = x
        else:
            self.parent[y] += self.parent[x]
            self.parent[x] = y
```
