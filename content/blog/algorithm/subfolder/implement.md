---
title: "구현 & 계산"
date: 2023-11-09
mainSectionTitle: "hugo"
---

선분의 길이

https://school.programmers.co.kr/learn/courses/30/lessons/120876

```
class Solution {
    public int solution(int[][] lines) {
        // 1. arr 배열 및 변수 초기화
        int[] arr = new int[200];
        int answer = 0;
        
        // 2. lines 정보를 arr 배열에 적용
        for(int i = 0; i < lines.length; i++)
            for(int j = lines[i][0] + 100; j < lines[i][1] + 100; j++)
                arr[j]++;
        
        // 3. arr 배열에서 겹친 부분 세기
        for(int i = 0; i < 200; i++)
            if(arr[i] > 1)
                answer++;
        
        return answer;
    }
  }
```

array 를 사용하기. <0 값이 있을때 배열에 넣기 위해서 최솟값을 더해서 인덱스를 구하기.
1. array 초기화. 
2. lines 정보를 arr 에 적용
3. arr 배열에 겹친 부분 세기

최대공약수

https://www.youtube.com/watch?v=8upKVJc1tgo&list=PLlV7zJmoG4XJtqWjB6pPCe5i7b064dfvB&index=5
```
int gcdA = arrayA[0];
        int gcdB = arrayB[0];
        
        // 1. 각 배열의 최대공약수 구하기
        for(int i =1; i< arrayA.length;i++){
            gcdA = gcd(gcdA, arrayA[i]);
            gcdB = gcd(gcdB, arrayB[i]);
        }
```

3 개 숫자의 최대공약수를 구할 때( A, B, C) 
f = gcd(A, B) 라고 했을때 gcd(f, C) 가 A B C 의 최대 공약수가 되는것.
gcd(A B C) 를 동시에 구하는게 아니라 한쌍의 최대공약수를 구하고 나머지 쌍과 다시 최대 공약수를 구하는것.

## gcd 재귀함수 외워버리기
```
int gcd(int a, int b){
if (a % b == 0) return b;
	return gcd(b, a % b)
}
```

마지막 두개의 원소에 2를 곱하고 그 원소를 다시 리스트에 넣는다…
score[:] = 이렇게 되는거였어?? 충격…
String.isnumeric() → if it is number or not
```
scores[-2:] = [x * 2 for x in scores[-2:]]
```