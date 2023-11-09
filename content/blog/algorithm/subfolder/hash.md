---
title: "해쉬"
date: 2023-11-09
mainSectionTitle: "hugo"
---

Hash 문제 나왔을때 Sorting 하고 포문 도는 것과 비슷하게 빠를 때가 많으니 소팅을 잘하자!

hash -> 의상. 입지 않은 경우 None 을 넣어서 표현하기. 

Iterator<Class Type> variable_name = iterating object(list, tuple…).iterator()
```
while (variable_name.hasNext()):  → returns boolean
	variable_name.next() → returns next iterator
	variable_name.next().intValue() → returns next iterator’s int value
```

문제 해결 방법을 찾을때는 더 쉽게 쉽게 생각하고, 코딩을 실제로 하기 시작할때 빨리 생각하자. 처음 생각할때 복잡하게 생각할 필요가 없음.

### 프로그래머스 순위 검색

https://school.programmers.co.kr/learn/courses/30/lessons/72412

코드를 짤때 좀더 읽기 쉽게, 어려운 알고리즘이라도 가독성이 좋게 쓰기.
해쉬맵에서는 해쉬를 먼저 완벽하게 만든다음 query 처리를 하는게 좋다. 해쉬만드는게 오래걸려도 query 하나 할때 속도가 빨라지니까.  해쉬는 sorting 과 잘어울림.
```
def solution(info, query):
    #hashmap
    hashmap = {}
    finalResult = []
    #parse input
    for i in info:
        info_list = i.split(" ")
        languages = [info_list[0], "-"]
        roles = [info_list[1], "-"]
        grades = [info_list[2], "-"]
        foods = [info_list[3], "-"]
        score = info_list[4]
        #make hashmapß out of all combinations of info including -
        for language in languages:
            for role in roles:
                for grade in grades:
                    for food in foods:
                        arrayForKey = [language, role, grade, food]
                        key = " ".join(arrayForKey)
                        arrayForValue = hashmap.get(key, [])
                        arrayForValue.append(int(score))
                        hashmap[key] = arrayForValue
    #sort each array of scores
    for elem in hashmap.values():
        elem.sort()
        
    #get query and find corresponding list from that query key 
    for getQuery in query:
        #parse query
        queryList = getQuery.split(" and ")
        target = int(queryList[3].split(" ")[1])
        queryList[3] = queryList[3].split(" ")[0]
        key = " ".join(queryList)
        
        if hashmap.get(key, -1) != -1:
            arrayForScores = hashmap[key]
            left = 0
            right = len(arrayForScores)
            while(left < right):
                mid = (left + right) // 2
                if(arrayForScores[mid] >= target):
                    right = mid
                else:
                    left = mid + 1
            numOfScore = len(arrayForScores) - left
            finalResult.append(numOfScore)
    return finalResult

binary search 를 자주 써먹자.
```

https://school.programmers.co.kr/learn/courses/30/lessons/72411?language=java
메뉴 정렬.

문자 combination 만들때 재귀 함수 참고.까먹지 말자.
```
public void combination(String order, String others, int count){
        if(count ==0){
            hashMap.put(order, hashMap.getOrDefault(order, 0) + 1);
            return;
        }
        for(int i=0; i<others.length(); i++){
            combination(order + others.charAt(i), others.substring(i+1), count - 1);
        }
    }

```