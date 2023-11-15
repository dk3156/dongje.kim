---
title: "Python"
date: 2023-11-05
mainSectionTitle: "hugo"
---
## 귀납
귀납 제한 풀기.
python 은 기본적으로 귀납 제한이 1000 이기 때문에, 귀납을 더 반복하고 싶으면 제한을 풀어줘야한다.
```
>> sys.setrecursionlimit(100000)
```

## 우선순위큐
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

## 파이토닉한 문법들
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

## 파이썬에서 순열, 조합 모듈가져오기
from itertools import permutations

from itertools import combinations
```
from itertools import permutations

def getMax(lst1, lst2):
    total = 0
    for i in range(len(lst1)):
        total += i * (lst2[i] - lst1[i])
    return total
                
def solution2(arr1, arr2):
    sumOfPerm = []
    gen1 = permutations(arr1)
    gen2 = permutations(arr2)
        
    for perm1 in gen1:
        for perm2 in gen2:
            sumOfPerm.append(getMax(perm1, perm2))
            
    return max(sumOfPerm)

arr1 = [1,2,3,4]
arr2 = [1,2,3,4]
print(solution2(arr1, arr2))
```

## 정렬
함수를 사용해 배열 정렬을 하기 위해서는 functools 모듈의 functools.cmp_to_key() 메서드를 가져다가 쓰면된다.
sort 함수에 key 파라미터에 함수를 패스해준다.
sort 함수는 negative value, 0, positive value 셋 중 하나를 리턴해야 한다.
예를 들어 item1 , item2 를 비교할때 item1 < item2 라면 마이너스 값을 리턴하고, 
item1 > item2 라면 플러스 값을 리턴해 준다. 함수에서 비교한 더 작은 값을 정렬도중 앞으로 보내게 되어있다.
return item1 - item2 또한 같은 역할을 한다.
```
import functools
def compare(item1, item2):
    global preferences
    preferred_item1 = 0
    preferred_item2 = 0
    for pref in preferences:
        if pref.index(item1) < pref.index(item2):
            preferred_item1 += 1
        else:
            preferred_item2 += 1
        
    if preferred_item1 == preferred_item2:
        #smaller id beats larget id
        return item1 - item2
    elif preferred_item1 > preferred_item2:
        #item1 beats item2
        return -1
    else:
        #item2 beats item 1
        return 1
        
def solution(preferences):
    n = len(preferences)
    m = len(preferences[0])

    songList = [x for x in range(m)]
    songList.sort(key=functools.cmp_to_key(compare))
    return songList

preferences = [[2, 1, 0], [0, 2, 1], [0, 2, 1]]
print(solution(preferences))
```

### lambda key 를 이용한 정렬
프로그래머스 오답풀이: 
https://school.programmers.co.kr/learn/courses/30/lessons/42579 

lambda key x : criteria(x) 로 정렬을 할때, () 를 사용하면 두 가지 이상의 criteria 로 정렬을 할 수 있다.
(criteria1(x), criteria2(x), criteria3(x))  형식으로 3개의 함수를 wrap 해서 lambda key 로 넘겨주면 3 가지 조건 전부 적용이 되서 정렬된다.
```
temp = [[genres[i], plays[i], i] for i in range(len(genres))]   
temp = sorted(temp, key=lambda x: (x[0], -x[1], x[2]))  
```
