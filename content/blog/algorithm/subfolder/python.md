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

### 파이토닉 문법들
enumerate 으로 index, element 동시에 가져올 수 있다
```
animals = [cat, dog, sheep]
for i, animal in enumerate(animals):
    print(i, animal)
```

file 열때 with 를 사용해 read, write를 구분해 준다.
```
with open('spam'.txt, w) as fileObj:
    fileObj.write('helloworld')
```

None 을 비교할때 == 말고 is 를 쓰면 예외처리가 가능하다.
```
if spam is None:
    #do something
```

원시문자열 r 에서는 백슬래시를 이스케이프 문자로 취급하지 않으니 directory 주소를 원시문자열로 표현하자.
```
print( r 'users\hashtag\something\myfolder\file.txt')
```

f-string
```
width, length = 10, 12
print( f' a{width} by {length} room has an area of {width * height})
```

copy 모듈에서 카피 메서드로 객체 복사를 해준다.
```
import copy
lst = [a, b, c]
egg = copy.copy(lst)
```

Dictionary 메서드 중 get, setdefault 를 쓰면 if문을 제거하고 깔끔하게 해쉬를 업데이트 할 수 있다.
```
num = {'dog' : 1}
>> num.get('cat', 0) #returns 0
>> num.setdefault('cat', 0) # does nothing if cat exists
>> num[cat] += 10
```

Switch 문 대신 dictionary 를 사용하자
```
if season == "winter":
    holiday = 'a'
elif season == "spring":
    holiday = 'b'
else:
    holiday = 'c'
...

holiday = {"winter" : 'a',
           "spring" : 'b',
            ...}.get(season, 'c')
```

체이닝 할당, 체이닝 연산자를 사용하자
```
if 10 < num < 20:
    score = F

spam = egg = bacon = "breakfast"
print(spam, egg, bacon)

if(spam == egg == bacon):
    print("breakfast")
```

List Comprehension 으로 map, filter 를 대채할 수 있다.
```
[n for n in [1,2,3,4] if n % 2 == 0]
```