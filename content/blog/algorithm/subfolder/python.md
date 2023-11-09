---
title: "Python"
date: 2023-11-05
mainSectionTitle: "hugo"
---
### 귀납
귀납 제한 풀기.
python 은 기본적으로 귀납 제한이 1000 이기 때문에, 귀납을 더 반복하고 싶으면 제한을 풀어줘야한다.
```
>> sys.setrecursionlimit(100000)
```

### 우선순위큐
파이썬은 min_heap 밖에 제공하지 않으므로, max_heap 을 구현하고 싶다면 원소를 음수로 바꿔 저장한 뒤, 꺼낼때 다시 양수로 바꾸면 된다.
```
from queue import PriorityQueue
que = PriorityQueue()
a = 100
que.put(0 - a)
print(0 - que.get())
→ 100

import heapq

lst = []
heapq.heappush(lst, 4)
heapq.heappop(lst)
lst[0]
heapq.heapify(lst)
```
